# Tracing and Reproducing a Python Handle Memory Leak in OpenImageIO #4613

## Overview

OpenImageIO (OIIO) is an essential library for image reading, writing, and processing in production graphics pipelines. Issue #4613 reported a severe memory growth bug (~1 GB per frame) specific to writing PNG files through OIIO's Python bindings, while TIFF files and `oiiotool` remained unaffected. Using my automated `issue-scout` workflow, I claimed the issue, developed an isolated subprocess benchmarking harness (`repro.py`), and traced the leak to exactly 5 bytes per pixel value per frame across releases. Although an upstream fix (`#4685`) had already landed in version 3.0.6.0 without being explicitly linked to the issue, my reproduction empirically verified the fix, resolved macOS memory measurement discrepancies between RSS and physical footprints, and uncovered a secondary type-handling crash bug (`get_pixels(oiio.UNKNOWN)`). You can review my claim and ongoing thread collaboration on the [OpenImageIO Issue #4613 Thread](https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6788821588).

## What happened

### 1. Scouting and Claiming the Issue

On November 11, 2026, my automated triage tool evaluated OpenImageIO #4613 and returned an accept verdict across all six required checks.

**Proof P1 (issue-scout output, 2026-11-11):**

```text
accept verdict on OpenImageIO #4613 with all six checks passing (2,391 stars,
maintainer reply within ~1.5 hours on a recent issue, no claims or PRs, a triaged
single bug, outside PRs welcome, AI allowed with disclosure)
```

Following this validation, I claimed the issue on GitHub to establish clear communication with the project maintainers.

**Proof P2 (Claim & Maintainer Response):**

```text
stevwya77: Hi, I'd like to work on this. I'll start by checking whether the leak
still happens on a current version and narrowing down where it occurs, as you
suggested. I'll report back what I find.

lgritz (Collaborator): @stevwya77 Sounds good!
```

### 2. Building the Isolation Harness and Measuring the Leak

To isolate the memory leak from Python garbage collection artifacts, I wrote `repro.py` on November 14, 2026. This harness generated a synthetic 8192×2048 EXR image and executed each encoding configuration in a fresh subprocess, tracking both `psutil` RSS and macOS `phys_footprint`.

**Proof P3 (Windows CI RSS measurements in MB):**

```text
OIIO 3.0.5.0:
png comp=0 filter=8 rgba half imageinput  base: 56 | d1: +324 | d2: +320 | ... | final: 3260
tif comp=0 filter=8 rgba half imageinput  base: 56 | d1:   +5 | d2:   +0 | ... | final:   18

OIIO 3.0.6.0:
png comp=0 filter=8 rgba half imageinput  base: 56 | d1:   +4 | d2:   +0 | ... | final:   60
```

**Proof P4 (macOS local phys_footprint arithmetic):** Running tests on OIIO 3.0.3.1 revealed exact mathematical scaling:

```text
png ... rgba half  -> mean delta +320.2 MiB (Final: 3249)
png ... rgb  half  -> mean delta +240.1 MiB (Final: 2448)
```

Arithmetic verification: $8192 \times 2048 \times 4 = 67,108,864$ values $\times 5\text{ B} = 320.0\text{ MiB}$; with 3 channels ($RGB$), $50,331,648 \times 5 = 240.0\text{ MiB}$. Because half-precision and float inputs leaked identically, the leak originated in the writer rather than the reader.

### 3. Resolving Measurement Pitfalls (RSS vs. Footprint)

On macOS CI, measuring memory via `psutil` RSS alone proved misleading because shared memory structures decay over iterations (+403, +219, +131... +2), making the leak appear as though it stopped. In contrast, `phys_footprint` correctly tracked the linear +320 MB growth per iteration.

**Proof P5 (Fixed-version contrast, local 3.0.6.0 log):**

```text
psutil RSS:            png rgba half -> d1 +110 ... mean +5.5 -> final 215
macOS phys_footprint:  png rgba half -> d1 +1   ... mean 0.0  -> final 45
```

### 4. Connecting to the Upstream Fix

Although maintainer Larry Gritz had already merged commit `0a3adc26` on April 7, 2025 (`fix(python): fix handle leak`), it was never linked to #4613.

**Proof P6 (Upstream Code & Commit Context):**

```diff
// src/python/py_imageoutput.cpp
-            [](const std::string& filename,
-               const std::string& searchpath) -> py::object {
-                auto out(ImageOutput::create(filename, nullptr, searchpath));
-                return out ? py::cast(out.release()) : py::none();
+            [](const std::string& filename, const std::string& searchpath) {
+                return ImageOutput::create(filename, nullptr, searchpath);
```

Larry noted in his commit message that the raw pointer was handed to Python "on the mistaken assumption that it would be reference counted". My measurements confirmed that this fix completely eliminated the leak in OIIO 3.0.6.0 onward.

### 5. Refining the Repro Report via `repro-check`

When I drafted my repro report (`repro-draft.md`) on November 14, 2026, running my automated `repro-check` tool caught initial structural and voice-guide violations.

**Proof P7 (`repro-check` run 2 verdict):** Accept (all 5 required checks + control run passed).

**Proof P8 (Before/After refinement):**

- **Before:** The draft lacked a precise date on the claim and speculated on unverified release causes, violating the rule "Describe Symptoms, Don't Guess Causes".
- **After:** Softened speculation into empirical observations, added explicit claim dates, and incorporated Linux CI data points into the report.

## Limitations and what's next

While this investigation successfully verified the resolution of #4613, I did not perform a full git bisect across every intermediate commit between 3.0.5.0 and 3.0.6.0, relying instead on release-version benchmarking and maintainer commit matching. Furthermore, while probing edge cases in OIIO 3.2.2.0, I discovered a secondary bug where passing `get_pixels(oiio.UNKNOWN)` triggers a segmentation fault (`SIGSEGV`) rather than a safe Python exception.

**Proof P9 (Secondary Bug Probe):**

```text
[                exit 0]  1 small in-memory, UNKNOWN   -> NoneType  | err: -
[killed by SIGSEGV (11)]  4 big lazy file, UNKNOWN     Fatal Python error: Segmentation fault
```

Next, I plan to finalize and publish the drafted issue report (`issue.md`) for this type-handling segmentation fault to further improve OIIO's Python binding robustness.

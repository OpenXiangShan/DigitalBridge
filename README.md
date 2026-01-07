# DigitalBridge

![Status](https://img.shields.io/badge/Status-Binary_Preview-orange) ![Platform](https://img.shields.io/badge/Platform-RISC--V-blue) ![Guest](https://img.shields.io/badge/Guest_Arch-x86-green)

## Introduction

This project implements a high-performance **Binary Translation (BT)** system designed to facilitate the seamless migration of executable code across different instruction set architectures.

By performing translation directly at the binary level, this tool breaks the "instruction set wall," allowing software compiled for mature architectures (such as x86) to run natively on **RISC-V** platforms. The primary goal of this project is to leverage existing software ecosystems to enrich the availability of applications for the RISC-V architecture.

## ⚠️ Release Status

**Current Release: Binary Preview**

Please note that this repository currently hosts a **binary preview** version of the translator. The full source code has not yet been released. This release is intended for evaluation, testing, and feedback purposes.

## Getting Started

### Prerequisites
*   **Host Architecture:** RISC-V (64-bit recommended)
*   **OS:** Linux
*   **Dependencies:** Standard C++ runtime libraries.

### Basic Usage

The core executable is `dbt`. The general syntax is:

```bash
./dbt [OPTIONS] -- <guest_binary> [guest_args]
```

### Example

Translate and run an x86 binary:

```bash
./dbt -t -- ./tests/helloworld
```
## Command Line Options

The system offers extensive configuration for translation modes, debugging, and optimization.

### General

|Option|Description|
|:--|:--|
|`-h`|Print the help message and exit.|
|`-I <path>`|Specify the path for x86 libraries (default: `$HOME/x86lib`).|
|`-l <file>`|Specify the log file path.|

### Execution Modes

|Option|Description|
|:--|:--|
|`-i`|**Interpret** mode. Runs the guest binary using the interpreter within the specified address range.|
|`-t`|**Translate** mode. Translates and executes instructions (JIT). Address range is optional.|
|`-S <sub-opt>`|**Static Binary Translation (SBT)** options. `-St`: Static translate. `-Sonly-main-elf`: Translate main ELF only. `-Sonly-deps`: Translate dependencies only.|

### Debugging & Tracing (`-p`)

Use `-p <key>` to enable specific runtime information.

|Key|Description|
|:--|:--|
|`sc`|Print system calls.|
|`sg`|Print signal processing info.|
|`func`|Print function calls and returns.|
|`tb1` / `tb2`|Print Translation Block (TB) addresses (`tb1`) or detailed instructions (`tb2`).|
|`mt`|Print memory trace.|
|`profile`|Print execution profiling (TB counts, instruction counts).|
|`unimp-trans`|Log unimplemented translators.|

### Optimization Flags (`-f`)

Use `-f <key>` to enable/disable specific architectural optimizations. Prefix with `no-` to disable (e.g., `-f no-ss`).

|Key|Description|
|:--|:--|
|`opt`|Enable IR level 2 optimizations.|
|`ss`|Enable **Shadow Stack** (enabled by default).|
|`jtl`|Enable **Jump Table Localization**.|
|`fp`|Enable Flag Pattern optimization.|
|`mda`|Handle misaligned data access.|
|`xa`|Enable XMM use/def analysis.|
|`bt`|Enable "By-hand" translation for specific hotspots.|
|`ps`|Use target-specific packed single (PS) instructions.|

## Directory Structure

```text
.
├── bin/            # Contains the 'dbt' executable
├── lib/            # Runtime libraries
├── doc/            # Documentation
└── README.md
```

## Reporting Issues

Since this is a preview release, we highly value your feedback. If you encounter crashes, incorrect translations, or performance issues, please open an issue in this repository with the following details:

1. The guest binary used.
2. The full command line arguments.
3. Log output (using `-l` or `-p` flags).

## License

Copyright (c) 2026 Institute of Computing Technology, Chinese Academy of Sciences. All rights reserved.

This project is released under the **ICT-CAS Non-Commercial Binary License Agreement**.
Please refer to the [LICENSE](./LICENSE) file for the full terms and conditions.

**Summary of Terms:**

*   **Non-Commercial Use Only:** This software is provided solely for academic research and evaluation purposes. Any commercial use is strictly prohibited.
*   **Binary Distribution:** This software is distributed in binary form only. Source code is not provided.
*   **No Modification:** You may not modify, adapt, or create derivative works based on this software. The binaries must remain in their original state.
*   **No Reverse Engineering:** Decompilation, disassembly, or reverse engineering of the software is prohibited.

For commercial licensing or other inquiries, please contact the authors.

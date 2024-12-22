# FreeRTOS Fuzzing

This is a fork of the open source FreeRTOS code base that can be found here. This repository adds a demo that performs fuzzing on certain api functions to validate memory safety and detect potential undefined behavior.

To run, make sure you execute the general setup below and then change directories into `FreeRTOS/Demo/Posix_GCC_fuzzing`, and follow the README located there.

## General setup

It is recommneded to execute this within a Linux Virtual environment using the clang compiler for best compatibility with the fuzzing tool libfuzzer.

Install the FreeRTOS source code
```
git submodule init
git submodule update
```
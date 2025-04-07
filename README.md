# ARM GCC GitHub Action

[中文文档](./README.zh-CN.md)

A dedicated GitHub Action for installing and caching the ARM GCC cross-compiler toolchain. It helps you quickly and automatically configure ARM development environment in your CI/CD pipeline.

## Background

Most GitHub Action runners don't come with pre-installed GCC toolchains required for embedded development. While installation through package managers is possible, it's often time-consuming and impacts CI/CD efficiency.

This Action implements smart caching mechanisms by downloading and extracting the GCC toolchain to the runner.temp directory, significantly improving subsequent build speeds. It's particularly beneficial for projects requiring frequent commits and builds.

## Features

- ✨ Full Platform Support: Complete coverage for Windows, Linux, and macOS
- 💪 Multi-architecture Compatible: Supports X86, X64, and ARM64 architectures
- 🚀 Smart Caching: Automatically caches toolchain for improved build efficiency
- 🔧 Flexible Configuration: Supports custom GCC versions and target platforms

## Usage

Add the following configuration to your workflow file (e.g., `.github/workflows/build.yml`):

```yaml
steps:
  - uses: demopath/armgcc@v1
    with:
      version: '14.2.rel1'    # Optional, defaults to 14.2.rel1
      target: 'arm-none-eabi' # Optional, defaults to arm-none-eabi
```

### Available Versions

For available versions, please refer to ARM GNU Toolchain Downloads

### Available Targets

- arm-none-eabi - For bare-metal ARM development
- arm-none-linux-gnueabihf - For ARM Linux development (hard float)
- aarch64-none-elf - For 64-bit ARM bare-metal development
- aarch64-none-linux-gnu - For 64-bit ARM Linux development

### Example Workflow

```yml
name: Build Firmware

on:
  push:
    branches: [ main, dev ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@v4

    - name: Install ARM GCC
      uses: demopath/armgcc@v1

    - name: Build Project
      run: arm-none-eabi-gcc --version
```
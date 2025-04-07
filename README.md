# ARM GCC GitHub Action

这是一个用于安装和缓存 ARM GCC 交叉编译工具链的 GitHub Action。它可以帮助您在 CI/CD 流程中自动设置 ARM 开发环境。

## 功能特点

- 支持多个操作系统平台（Windows、Linux、macOS）
- 支持多种 CPU 架构（X86、X64、ARM64）
- 自动缓存工具链，提高 CI/CD 效率
- 可配置 GCC 版本和目标平台

## 使用方法

在您的工作流程文件中（例如 `.github/workflows/build.yml`）添加以下配置：

```yaml
steps:
  - uses: demopath/armgcc@v1
    with:
      version: '14.2.rel1'    # 可选，默认为 14.2.rel1
      target: 'arm-none-eabi' # 可选，默认为 arm-none-eabi```
```
### version 参数可选值
参考 https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads 页面

### target 参数可选值
- arm-none-eabi - 用于裸机 ARM 开发
- arm-none-linux-gnueabihf - 用于 ARM Linux 开发（硬浮点）
- aarch64-none-elf - 用于 64 位 ARM 裸机开发
- aarch64-none-linux-gnu - 用于 64 位 ARM Linux 开发

### 示例工作流程
```yml
name: Build Firmware

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Install ARM GCC
      uses: demopath/armgcc@v1
    - name: Build Project
      run: |
        arm-none-eabi-gcc --version
        make
```
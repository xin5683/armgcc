# ARM GCC GitHub Action

这是一个专门用于安装和缓存 ARM GCC 交叉编译工具链的 GitHub Action。它能帮助您在 CI/CD 流程中快速、自动地配置 ARM 开发环境。

## 背景

大多数 GitHub Action 运行环境并未预装嵌入式开发所需的 GCC 工具链。虽然可以通过包管理器进行安装，但这种方式往往耗时较长，影响 CI/CD 效率。

本 Action 通过智能缓存机制，将 GCC 工具链下载并解压至 runner.temp 目录后进行缓存，显著提升了后续构建的速度，特别适合需要频繁提交和构建的项目。

## 功能特点

- ✨ 全平台支持：完整覆盖 Windows、Linux 和 macOS
- 💪 多架构兼容：支持 X86、X64 和 ARM64 架构
- 🚀 智能缓存：自动缓存工具链，大幅提升构建效率
- 🔧 灵活配置：支持自定义 GCC 版本和目标平台

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
可用版本请参考 [ARM GNU 工具链下载页面](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)

### 可用目标

- arm-none-eabi - 用于裸机ARM开发
- arm-none-linux-gnueabihf - 用于ARM Linux开发（硬浮点）
- aarch64-none-elf - 用于64位ARM裸机开发
- aarch64-none-linux-gnu - 用于64位ARM Linux开发

### 多目标支持

您可以使用逗号分隔的值指定多个目标：

```yml
- name: 安装多目标ARM GCC
  uses: demopath/armgcc@v1
  with:
    target: 'arm-none-eabi,aarch64-none-elf'
```

> 注意：使用多个目标时，所有指定的工具链都将被安装并添加到PATH中。

### 示例工作流程
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
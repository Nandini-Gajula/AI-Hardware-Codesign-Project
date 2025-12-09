# AI Hardware Codesign Project - CFU Playground

**Accelerate ML models on FPGAs with Custom Function Units**

"CFU" stands for **Custom Function Unit**: accelerator hardware that is tightly coupled into the pipeline of a CPU core, adding new custom function instructions that complement the CPU's standard functions (such as arithmetic/logic operations).

## Overview

The **AI Hardware Codesign Project** (CFU Playground) is a comprehensive collection of software, gateware, and hardware configured to make it easy to:

- **Run ML models** on FPGA-based microcontroller-class hardware
- **Benchmark and profile** performance of ML inference
- **Make incremental improvements** through:
  - Software modifications (C/C++ source code)
  - Hardware acceleration with a CFU (Verilog or Amaranth)
- **Measure the results** of optimizations in real-time

This project is particularly valuable because ML acceleration on microcontroller-class hardware is a relatively new area that has been traditionally dominated by hardware engineers. The CFU Playground aims to make experimentation as simple, fast, and fun as possible for both hardware and software engineers.

## Key Features

### Hardware
- **Artix 7 35T FPGA** with:
  - 33,000 logic cells (sufficient for soft CPU and more)
  - 90 DSP slices (for multiply-accumulate operations)
  - 50x 36Kbit block RAMs

### Gateware & Custom Function Units (CFU)
- **LiteX-based SoC** (System-on-Chip) running on the FPGA with:
  - VexRiscV soft CPU with CFU extension
  - 256MB DDR DRAM access
  - LED control
  - USB-UART serial terminal
- CFU development support in:
  - Verilog (raw)
  - **Amaranth** (preferred for composition and testing)
- Synthesis support via Vivado, with migration to SymbiFlow planned

### Software
- **TensorFlow Lite for Microcontrollers** for ML model inference
- Pre-built ML models and test data
- Library functions for profiling and benchmarking
- Customization hooks for TensorFlow and system components
- GCC C/C++ toolchain for RISC-V

### Testing & Simulation
- **Renode** simulation framework for testing without physical hardware
- GitHub Actions CI/CD for automated testing
- Robot Framework test automation
- Verilator simulation support

## Project Structure

```
AI-Hardware-Codesign-Project/
├── common/              # Shared SoC software, benchmarks, and tests
│   ├── src/            # C/C++ source code for ML workloads
│   ├── ld/             # Linker scripts
│   └── _sim/           # Simulation configurations
├── soc/                # System-on-Chip definitions and builds
│   ├── cfu/            # Custom Function Unit definitions
│   └── vexriscv/       # VexRiscV CPU configurations
├── proj/               # Project examples
│   ├── mobilenetv2_cfu/    # MobileNetV2 with CFU acceleration
│   ├── shufflenet_cfu/     # ShuffleNet with CFU acceleration
│   └── proj_template/      # Template for creating new projects
├── docs/               # Documentation (Sphinx-based)
├── scripts/            # Build and setup scripts
├── python/             # Python utilities and Amaranth CFU libraries
├── third_party/        # External dependencies (TFLite, LiteX, etc.)
└── conf/               # Configuration files (environment, requirements)
```

## Getting Started

### Prerequisites
- A supported FPGA development board (Arty A7-35T recommended for beginners)
- 64-bit Linux, macOS, or Windows (with WSL2)
- Python 3.7+
- Git

### Quick Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/google/CFU-Playground.git
   cd CFU-Playground
   ```

2. **Run the setup script:**
   ```bash
   ./scripts/setup
   ```
   This will:
   - Update Git submodules
   - Build local executables
   - Install missing Linux packages
   - Set up the conda environment

3. **Activate the environment:**
   ```bash
   make enter-sf  # For SymbiFlow (open-source tools)
   # OR
   source environment/bin/activate  # For standard environment
   ```

4. **Build and run a project:**
   ```bash
   cd proj/mobilenetv2_cfu
   make prog        # Build and program gateware to FPGA
   make load        # Build and load C program
   ```

## Documentation

Comprehensive documentation is available in the `docs/` directory:

- **[Overview](docs/source/overview.rst)** - Architecture and components
- **[Setup Guide](docs/source/setup-guide.rst)** - Detailed installation instructions
- **[Crash Course](docs/source/crash-course/index.rst)** - Introduction to:
  - FPGAs and hardware design
  - Verilog and Amaranth HDLs
  - RISC-V architecture
  - Custom Function Units
  - TensorFlow Lite for Microcontrollers
- **[Step-by-Step Guide](docs/source/step-by-step.rst)** - Creating your first accelerator
- **[Renode Simulation](docs/source/renode.rst)** - Testing without hardware
- **[Interface Reference](docs/source/interface.rst)** - API documentation

Build the documentation locally:
```bash
cd docs
make html
```

Then open `docs/build/html/index.html` in your browser.

## Core Technologies

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **CPU** | VexRiscV | 32-bit RISC-V soft processor |
| **HDL** | Verilog, Amaranth | Hardware design languages |
| **SoC Framework** | LiteX | System-on-chip generation |
| **ML Framework** | TensorFlow Lite for Microcontrollers | Inference engine |
| **Simulation** | Renode, Verilator | Testing and validation |
| **Synthesis** | Vivado, SymbiFlow | FPGA bitstream generation |
| **Build System** | Make, Conda | Dependency and build management |

## Example Projects

The repository includes several example projects demonstrating CFU acceleration:

### MobileNetV2 with CFU
A mobile-optimized neural network with hardware-accelerated matrix operations.

### ShuffleNet with CFU
A lightweight architecture with CFU-accelerated layers.

### Project Template
A starting point for creating your own ML acceleration projects.

## Key Concepts

### Custom Function Unit (CFU)
A CFU is a specialized hardware block integrated into the CPU pipeline that:
- Executes custom instructions not in the base RISC-V ISA
- Receives two register operands from the CPU
- Returns a result to a third register
- Cannot directly access memory (CPU manages data movement)
- Can perform high-throughput matrix operations, custom activation functions, etc.

### Software/Hardware Co-design
The project enables an iterative workflow:
1. **Profile** the ML model in software
2. **Identify** bottlenecks
3. **Accelerate** critical paths in hardware
4. **Measure** improvements
5. **Repeat**

## Contributing

We welcome contributions! Areas of interest include:
- New ML models and benchmarks
- Additional CFU implementations
- Support for more FPGA boards
- Documentation improvements
- Bug fixes and optimizations

To contribute:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request with clear description and testing

## Support

If you need help or encounter issues:
- [Open an issue](https://github.com/google/CFU-Playground/issues) on GitHub
- Check existing documentation and issues
- Review the crash course for foundational concepts

## License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) file for details.

Third-party components included in this project are licensed under their respective licenses:
- TensorFlow Lite for Microcontrollers: Apache 2.0
- LiteX: BSD
- VexRiscV: BSD
- Renode: MIT
- Other components: Various open-source licenses (see individual files)

## Citation

If you use this project in your research or work, please cite:

```
@project{cfu-playground,
  title = {CFU Playground: Accelerate ML Models on FPGAs},
  author = {CFU-Playground Contributors},
  year = {2021},
  url = {https://github.com/google/CFU-Playground}
}
```

## Disclaimer

**This is not an officially supported Google project.** Support and/or new releases may be limited. This is primarily a research and educational platform for exploring hardware-software co-design of ML accelerators.

## Related Resources

- [TensorFlow Lite for Microcontrollers](https://www.tensorflow.org/lite/microcontrollers)
- [Amaranth HDL](https://github.com/amaranth-lang/amaranth)
- [LiteX SoC Framework](https://github.com/enjoy-digital/litex)
- [VexRiscV CPU](https://github.com/SpinalHDL/VexRiscv)
- [Renode Simulator](https://renode.io)
- [SymbiFlow FPGA Toolchain](https://symbiflow.readthedocs.io/)

---

**Last Updated:** December 2025

For the latest information, visit the [project repository](https://github.com/google/CFU-Playground).

# 🔌 DUTLink - USB 3.0 Multiplexer Board

> **Professional USB 3.0 switching and power management solution for device testing and development**

[![KiCad Production Files](https://github.com/the78mole/dutlink-board/actions/workflows/kicad-production.yml/badge.svg)](https://github.com/the78mole/dutlink-board/actions/workflows/kicad-production.yml)
[![License: MIT](https://img.shields.io/github/license/the78mole/dutlink-board)](LICENSE)
[![GitHub Release](https://img.shields.io/github/v/release/the78mole/dutlink-board)](https://github.com/the78mole/dutlink-board/releases)

DUTLink is a sophisticated USB 3.0 multiplexer board designed for automated testing and development workflows. It enables seamless switching between host connections and storage devices while providing precise power management and monitoring capabilities.

![DUTLink Board Preview](https://via.placeholder.com/800x400/1e1e1e/ffffff?text=DUTLink+USB+3.0+Multiplexer+Board)

---

## ✨ Features

### 🔄 USB 3.0 Multiplexing
- **High-Speed Switching** - Full USB 3.0 SuperSpeed support (5 Gbps)
- **Backward Compatibility** - Works with USB 2.0 and USB 1.1 devices
- **Signal Integrity** - Differential pair routing with controlled impedance
- **Low Latency** - Hardware-based switching with minimal delay

### ⚡ Power Management
- **Smart Power Control** - Independent VBUS switching for each port
- **Overcurrent Protection** - Built-in current limiting and fault detection
- **Power Monitoring** - Real-time current and voltage measurement
- **Multiple Power Rails** - Support for 5V and 3.3V device power

### 🎛️ Control Interface
- **GPIO Control** - Simple pin-based switching control
- **I2C Interface** - Advanced control and monitoring via I2C
- **Status LEDs** - Visual indication of connection state
- **Test Points** - Easy access for debugging and measurement

### 🔧 Professional Grade
- **4-Layer PCB** - Optimized for signal integrity and EMI performance
- **Industrial Connectors** - Robust USB 3.0 connectors rated for high cycles
- **ESD Protection** - Comprehensive protection on all USB lines
- **Compact Design** - Minimal footprint for integration flexibility

---

## 🚀 Quick Start

### 🛠️ Hardware Setup

1. **Power Connection**
   ```
   Connect 5V power supply to VCC input (2.1mm barrel jack or screw terminals)
   Typical consumption: 500mA @ 5V (plus connected device power)
   ```

2. **Control Interface**
   ```
   GPIO Control: Pull SEL pin HIGH/LOW for A/B port selection
   I2C Control: Connect SDA/SCL for advanced control (address: 0x48)
   ```

3. **USB Connections**
   ```
   HOST    ←→ Connect to computer/host system
   DUT     ←→ Connect to device under test
   STORAGE ←→ Connect to storage device or analyzer
   ```

### ⚙️ Control Modes

#### Simple GPIO Control
```
SEL = LOW  → DUT connected to HOST
SEL = HIGH → STORAGE connected to HOST
```

#### Advanced I2C Control
```c
// Example Arduino code
#include <Wire.h>
#define DUTLINK_ADDR 0x48

void selectDUT() {
    Wire.beginTransmission(DUTLINK_ADDR);
    Wire.write(0x01); // Select DUT
    Wire.endTransmission();
}

void selectStorage() {
    Wire.beginTransmission(DUTLINK_ADDR);
    Wire.write(0x02); // Select Storage
    Wire.endTransmission();
}
```

---

## 📋 Technical Specifications

### Electrical Characteristics
| Parameter | Min | Typ | Max | Unit | Notes |
|-----------|-----|-----|-----|------|-------|
| Supply Voltage | 4.5 | 5.0 | 5.5 | V | Via barrel jack or terminals |
| Supply Current | - | 100 | 500 | mA | Excluding USB device power |
| USB Data Rate | - | - | 5.0 | Gbps | USB 3.0 SuperSpeed |
| Switch Time | - | 10 | 50 | ms | GPIO control response |
| VBUS Current | - | - | 3.0 | A | Per port, with protection |

### Physical Specifications
| Parameter | Value | Unit |
|-----------|-------|------|
| PCB Size | 80 × 50 | mm |
| PCB Thickness | 1.6 | mm |
| PCB Layers | 4 | layers |
| Component Height | < 15 | mm |
| Operating Temperature | -10 to +60 | °C |
| Storage Temperature | -40 to +85 | °C |

### Connector Specifications
| Connector | Type | Cycles | Notes |
|-----------|------|--------|-------|
| HOST | USB 3.0 Type-A | 10,000 | Gold-plated contacts |
| DUT | USB 3.0 Type-A | 10,000 | Gold-plated contacts |
| STORAGE | USB 3.0 Type-A | 10,000 | Gold-plated contacts |
| Power | 2.1mm Barrel | 5,000 | Center positive |

---

## 🔌 Pinout and Connections

### Control Interface (J1)
```
Pin | Signal    | Direction | Description
----|-----------|-----------|---------------------------
1   | VCC       | Power     | 5V power input
2   | GND       | Power     | Ground reference
3   | SEL       | Input     | Port select (3.3V/5V tolerant)
4   | SDA       | I/O       | I2C data line (3.3V)
5   | SCL       | Input     | I2C clock line (3.3V)
6   | INT       | Output    | Interrupt output (3.3V)
7   | EN        | Input     | Power enable (3.3V/5V tolerant)
8   | GND       | Power     | Ground reference
```

### Status LEDs
```
LED     | Color  | Indication
--------|--------|---------------------------
PWR     | Green  | Board powered and ready
HOST    | Blue   | HOST port activity
DUT     | Yellow | DUT port selected/active
STORAGE | Red    | STORAGE port selected/active
FAULT   | Red    | Overcurrent or fault condition
```

### Test Points
```
TP1  | +5V Rail      TP5  | DUT_D+
TP2  | +3.3V Rail    TP6  | DUT_D-
TP3  | HOST_D+       TP7  | STORAGE_D+
TP4  | HOST_D-       TP8  | STORAGE_D-
```

---

## 🏗️ Block Diagram

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│    HOST     │◄──►│ USB 3.0 MUX  │◄──►│     DUT     │
│   (Type-A)  │    │   Controller │    │  (Type-A)   │
└─────────────┘    │              │    └─────────────┘
                   │              │    
                   │              │    ┌─────────────┐
                   │              │◄──►│   STORAGE   │
                   │              │    │  (Type-A)   │
                   └──────────────┘    └─────────────┘
                          ▲
                          │
                   ┌──────────────┐
                   │ Control      │
                   │ Interface    │
                   │ (GPIO/I2C)   │
                   └──────────────┘
```

---

## 📁 Repository Structure

```
dutlink-board/
├── .github/workflows/          # CI/CD workflows
│   └── kicad-production.yml    # Automated production file generation
├── 3DModels/                   # 3D component models
│   └── USB-B-3-692221030100.stp
├── FOOTPRINTS.pretty/          # Custom KiCad footprints
│   ├── 3502111.kicad_mod      # Power management components
│   ├── LQH5BPB100MT0L.kicad_mod
│   └── ...
├── USB3Wurth.pretty/           # USB connector footprints
│   └── Wurth WR-COM USB 3.0 692221030100.kicad_mod
├── dutlink.kicad_pro          # Main KiCad project file
├── dutlink.kicad_pcb          # PCB layout
├── dutlink.kicad_sch          # Main schematic
├── board_control.kicad_sch    # Control circuitry schematic
├── power.kicad_sch            # Power management schematic
├── usb_storage_mux.kicad_sch  # USB multiplexer schematic
├── TUSBMUXES.kicad_sym        # Custom symbol library
├── sym-lib-table              # Symbol library table
├── fp-lib-table               # Footprint library table
└── README.md                  # This file
```

---

## 🔧 Development and Manufacturing

### 🤖 Automated Production Files

This repository includes GitHub Actions workflow that automatically generates production files using the [kicaddev Docker image](https://github.com/the78mole/docker-images):

**Generated Files:**
- **Gerber Files** - Complete set for PCB fabrication
- **Drill Files** - NC drill and PTH data
- **Pick & Place** - Component placement coordinates
- **Bill of Materials** - Interactive HTML BOM
- **Assembly Drawings** - PDF documentation
- **3D Models** - STEP files for mechanical verification
- **Renderings** - High-quality PCB visualizations

**Workflow Features:**
- ✅ **Fully Automated** - Runs on every commit and pull request
- ✅ **Professional Quality** - Uses KiCad 9.0 and latest production tools
- ✅ **Multiple Formats** - Gerbers, PDFs, 3D models, and visualizations
- ✅ **Ready for Manufacturing** - ZIP packages for direct submission
- ✅ **Interactive BOM** - Web-based component placement guide
- ✅ **Error Handling** - Comprehensive validation and error reporting

**Workflow Triggers:**
- Push to main/master branch
- Pull request creation
- Manual workflow dispatch

### 📦 Production Artifacts

After each successful workflow run, the following artifacts are available:

| Artifact | Contents | Use Case |
|----------|----------|----------|
| `dutlink-manufacturing-package` | Gerbers, drill files (ZIP) | Send directly to PCB manufacturer |
| `dutlink-documentation` | PDFs, interactive BOM | Assembly and troubleshooting |
| `dutlink-3d-and-renders` | STEP files, PNG renders | Mechanical design, marketing |
| `dutlink-production-files` | Complete production package | Full archive |

**How to Download Artifacts:**
1. Go to the [Actions tab](../../actions) in this repository
2. Click on the latest successful workflow run
3. Scroll down to the "Artifacts" section
4. Download the needed artifact ZIP file

**Direct Manufacturing:**
The `dutlink-manufacturing-package` contains everything your PCB manufacturer needs:
```
dutlink_manufacturing.zip
├── gerbers/
│   ├── dutlink-F_Cu.gbr           # Top copper layer
│   ├── dutlink-B_Cu.gbr           # Bottom copper layer
│   ├── dutlink-F_Silkscreen.gbr   # Top silkscreen
│   ├── dutlink-B_Silkscreen.gbr   # Bottom silkscreen
│   ├── dutlink-F_Mask.gbr         # Top solder mask
│   ├── dutlink-B_Mask.gbr         # Bottom solder mask
│   ├── dutlink-F_Paste.gbr        # Top solder paste
│   ├── dutlink-B_Paste.gbr        # Bottom solder paste
│   └── dutlink-Edge_Cuts.gbr      # Board outline
└── drill/
    ├── dutlink-PTH.drl            # Plated through holes
    ├── dutlink-NPTH.drl           # Non-plated holes
    └── dutlink-PTH-drl_map.gbr    # Drill map
```

### 🏭 Manufacturing Guidelines

**PCB Specifications:**
- **Material:** FR-4, TG 170°C minimum
- **Thickness:** 1.6mm ± 0.1mm
- **Copper Weight:** 1oz (35μm) inner/outer layers
- **Surface Finish:** HASL or ENIG (preferred for USB connectors)
- **Solder Mask:** Green (or per specification)
- **Silkscreen:** White on green, minimum 0.1mm line width

**Assembly Notes:**
- **Wave Soldering:** Not recommended due to fine-pitch components
- **Reflow Profile:** Standard SAC305 lead-free profile
- **Component Placement:** Use interactive BOM for accurate placement
- **Testing:** Full in-circuit test recommended for production quantities

### 🧪 Testing and Validation

**Automated Tests:**
1. **Power-On Test** - Verify all power rails and LED indicators
2. **USB Connectivity** - Test all USB ports for proper enumeration
3. **Switching Test** - Verify correct port selection via GPIO/I2C
4. **Current Monitoring** - Validate overcurrent protection
5. **Signal Integrity** - High-speed USB 3.0 eye diagram verification

**Manual Tests:**
1. **Mechanical Fit** - Connector alignment and mating force
2. **Thermal Performance** - Temperature rise under load
3. **EMI/EMC** - Compliance testing if required
4. **Durability** - Connector cycle testing

---

## 🎯 Use Cases

### 🧪 Automated Testing
```yaml
# Example GitHub Actions workflow for device testing
- name: Test Device
  run: |
    # Switch to DUT for firmware flashing
    gpio-control dutlink --select dut
    flash-firmware device.bin
    
    # Switch to storage for data verification
    gpio-control dutlink --select storage
    verify-data test-data.bin
```

### 🔬 Development Workflow
```bash
# Example development script
#!/bin/bash

echo "Starting development cycle..."

# Flash new firmware
dutlink-control --select dut
avrdude -p m328p -c usbasp -U flash:w:firmware.hex

# Run tests with storage device
dutlink-control --select storage
./run-tests.sh

# Collect data and analyze
dutlink-control --monitor-power
./analyze-power-consumption.py
```

### 🏭 Production Testing
```python
# Production test station integration
import dutlink

def production_test(unit_id):
    board = dutlink.DUTLink("/dev/ttyUSB0")
    
    # Test sequence
    board.select_dut()
    if not board.flash_firmware("production.bin"):
        return False
        
    board.select_storage()
    if not board.verify_functionality():
        return False
        
    # Log results
    board.log_test_results(unit_id)
    return True
```

---

## 🛡️ Safety and Compliance

### Electrical Safety
- **ESD Protection** - All external interfaces protected to IEC 61000-4-2 Level 4
- **Overcurrent Protection** - Electronic fuses on all VBUS lines
- **Isolation** - Control interface isolated from USB power domains
- **Thermal Management** - Thermal vias and copper pours for heat dissipation

### Regulatory Compliance
- **CE Marking** - Designed for EMC compliance (EN 55032, EN 55035)
- **FCC Part 15** - Class B digital device requirements
- **RoHS Compliance** - Lead-free components and processes
- **USB-IF Compliance** - Meets USB 3.0 specification requirements

---

## 🤝 Contributing

We welcome contributions to improve DUTLink! Here's how to get started:

### 🔧 Development Setup

1. **Clone Repository**
   ```bash
   git clone https://github.com/the78mole/dutlink-board.git
   cd dutlink-board
   ```

2. **KiCad Setup**
   ```bash
   # Open project in KiCad
   kicad dutlink.kicad_pro
   
   # Or use Docker for CLI operations
   docker run --rm -v $(pwd):/workspace \
     ghcr.io/the78mole/kicaddev:latest \
     kicad-cli --help
   ```

3. **Generate Production Files**
   ```bash
   # Local generation using Docker
   docker run --rm -v $(pwd):/workspace \
     ghcr.io/the78mole/kicaddev:latest \
     kicad_export dutlink.kicad_pro
   ```

### 📝 Contribution Guidelines

1. **Fork the repository** and create a feature branch
2. **Follow KiCad best practices** for schematic and layout design
3. **Update documentation** for any functional changes
4. **Test changes** using the automated workflow
5. **Submit pull request** with clear description of changes

### 🐛 Bug Reports

Please use GitHub Issues to report bugs with the following information:
- **Board revision** and manufacturing batch (if known)
- **Symptoms** and expected behavior
- **Test conditions** and environment
- **Supporting files** (logs, measurements, photos)

---

## 📚 Documentation

### 📖 Additional Resources
- **[KiCad Project Files](dutlink.kicad_pro)** - Complete design files
- **[Schematic PDF](production/pdf/dutlink_schematic.pdf)** - Detailed circuit documentation
- **[Interactive BOM](production/bom/dutlink_ibom.html)** - Component placement guide
- **[3D Models](production/3d/)** - Mechanical design files

### 🎓 Learning Resources
- **[KiCad Documentation](https://docs.kicad.org/)** - Official KiCad resources
- **[USB 3.0 Specification](https://www.usb.org/documents)** - USB-IF specifications
- **[PCB Design Guidelines](https://www.ti.com/lit/an/slva959/slva959.pdf)** - High-speed design practices

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### 📋 License Summary
- ✅ **Commercial Use** - Use in commercial products
- ✅ **Modification** - Adapt and modify the design
- ✅ **Distribution** - Share and redistribute
- ✅ **Private Use** - Use for personal projects
- ❗ **Liability** - No warranty or liability guarantee
- ❗ **Attribution** - Include original license notice

---

## 🙏 Acknowledgments

- **[KiCad EDA](https://www.kicad.org/)** - Open source electronics design automation
- **[USB Implementers Forum](https://www.usb.org/)** - USB specifications and compliance
- **[the78mole](https://github.com/the78mole)** - Development tools and Docker images
- **Open Source Hardware Community** - Inspiration and best practices

---

## 📞 Support

- **📧 Email** - [Contact](mailto:support@example.com)
- **💬 Discussions** - [GitHub Discussions](https://github.com/the78mole/dutlink-board/discussions)
- **🐛 Issues** - [GitHub Issues](https://github.com/the78mole/dutlink-board/issues)
- **📱 Discord** - [Hardware Development Server](https://discord.gg/hardware-dev)

---

**Built with ❤️ for the hardware development community**

*DUTLink - Simplifying USB device testing and development workflows*
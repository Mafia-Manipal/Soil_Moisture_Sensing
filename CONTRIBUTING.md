# Contributing to Smart Irrigation System

Thank you for your interest in contributing to the Smart Irrigation System project! This document provides guidelines for contributing to this open-source project.

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally
3. **Create a feature branch** for your changes
4. **Make your changes** and test thoroughly
5. **Commit your changes** with clear commit messages
6. **Push to your fork** and create a pull request

## Development Environment

### Required Software
- **Keil µVision 5 IDE** for ARM development
- **ARM C/C++ Compiler**
- **Git** for version control

### Hardware Requirements
- **ARM LPC2129 Microcontroller**
- **Soil Moisture Sensor**
- **DHT22 Temperature/Humidity Sensor**
- **DS1307 RTC Module**
- **ESP8266 WiFi Module**
- **16x2 LCD Display**
- **5V Relay Module**
- **Water Pump/Solenoid Valve**

## Code Style Guidelines

### C Programming Standards
- Use **clear, descriptive variable names**
- Add **comprehensive comments** for complex logic
- Follow **modular programming** principles
- Use **consistent indentation** (4 spaces)
- Include **function documentation** headers

### File Organization
```
src/
├── main.c              # Main application code
├── sensors/            # Sensor interface functions
├── communication/      # I2C, UART protocols
├── display/           # LCD interface functions
└── control/           # Irrigation control logic
```

## Testing Guidelines

### Hardware Testing
- Test all sensor readings before deployment
- Verify communication protocols (I2C, UART)
- Test pump control and safety mechanisms
- Validate power supply requirements

### Code Testing
- Test all operating modes (AUTO, MANUAL, SCHEDULED)
- Verify error handling and recovery
- Test WiFi communication and remote control
- Validate data logging functionality

## Pull Request Process

1. **Update documentation** if needed
2. **Add tests** for new functionality
3. **Ensure code compiles** without errors
4. **Test on actual hardware** if possible
5. **Update README.md** with new features
6. **Create detailed pull request description**

## Issue Reporting

When reporting issues, please include:
- **Hardware configuration** details
- **Software version** information
- **Detailed error description**
- **Steps to reproduce** the issue
- **Expected vs actual behavior**

## Feature Requests

We welcome feature requests! Please:
- **Describe the feature** clearly
- **Explain the use case** and benefits
- **Consider hardware limitations**
- **Discuss implementation approach**

## Code of Conduct

- **Be respectful** and inclusive
- **Help others** learn and contribute
- **Provide constructive feedback**
- **Follow project guidelines**

## License

By contributing to this project, you agree that your contributions will be licensed under the MIT License.

Thank you for contributing to sustainable agriculture through smart irrigation technology! 🌱💧

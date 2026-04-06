# Contributing to Art-Net Node PoE

Thank you for your interest in contributing to this project! Whether you want to report a bug, suggest a feature, improve documentation, or submit hardware/firmware changes, your help is appreciated.

---

## Table of Contents

- [Reporting Issues](#reporting-issues)
- [Suggesting Features](#suggesting-features)
- [Submitting Changes](#submitting-changes)
- [Code Style](#code-style)
- [Hardware Guidelines](#hardware-guidelines)
- [License](#license)

---

## Reporting Issues

If you find a bug or something doesn't work as expected:

1. **Check existing issues** first to see if it has already been reported.
2. Open a new issue and include:
   - A clear, descriptive title.
   - Steps to reproduce the problem.
   - Expected behaviour vs. actual behaviour.
   - Hardware revision / firmware version if applicable.
   - Any relevant logs, photos, or oscilloscope captures.

---

## Suggesting Features

Feature requests are welcome. Please open an issue with the label **enhancement** and describe:

- The problem you are trying to solve.
- Your proposed solution or feature.
- Any alternatives you have considered.

---

## Submitting Changes

1. **Fork** the repository and create a new branch from `main`:
   ```
   git checkout -b feature/my-new-feature
   ```
2. Make your changes (see guidelines below).
3. Commit with a clear message:
   ```
   git commit -m "Add support for X"
   ```
4. Push to your fork and open a **Pull Request** against `main`.
5. Describe what your PR changes and why. Reference any related issues.

---

## Code Style

- Keep firmware code readable and well-commented, especially for anything related to Art-Net packet handling, DMX timing, or PoE power sequencing.
- Prefer descriptive variable and function names over abbreviations.
- Document any non-obvious hardware constraints or timing requirements in comments.

---

## Hardware Guidelines

- All schematic and PCB changes should be made using the EDA tool already used in the project. Include exported Gerber files and/or the native project files in your PR.
- Document any component changes in the PR description, including the reason for the change (e.g. improved availability, cost reduction, better specs).
- If you change a component that affects the PoE power path, the RS-485 isolation, or the DMX signal integrity, please include measurement data (scope screenshots, power measurements) in the PR.
- Follow standard PCB design practices for signal integrity: proper ground planes, decoupling capacitors close to ICs, controlled-impedance traces for high-speed signals.

---

## License

By contributing to this project, you agree that your contributions will be licensed under the [MIT License](LICENSE).

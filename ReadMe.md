# PiSpace

Welcome to the PiSpace project! This repository contains all the necessary resources to get started with PiSpace and its documentation.

## Table of Contents
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Setting Up the Project](#setting-up-the-project)
- [Building the Documentation](#building-the-documentation)
- [Contributing](#contributing)
- [License](#license)

---

## Introduction

PiSpace is a project designed to replace the mainboard controller of generic Chinese diesel heaters with an opensource feature rich smart controller based on the esp32 platform. The documentation for this project is built using [MkDocs Material](https://squidfunk.github.io/mkdocs-material/), a static site generator tailored for clean and professional documentation.

---

## Requirements

Before setting up the project, ensure you have the following installed on your system:
- **Python** (3.7 or newer)
- **Pip** (Python package manager)

---

## Setting Up the Project

1. Clone the repository:
   ```bash
   git clone https://github.com/superkmk/PiSpace.git
   cd PiSpace
   ```

2. Create a virtual environment:
   ```bash
   python -m venv .venv
   ```

3. Activate the virtual environment:
   - **Windows**:
     ```bash
     .venv\Scripts\activate
     ```
   - **macOS/Linux**:
     ```bash
     source .venv/bin/activate
     ```

4. Install MkDocs and MkDocs Material:
   ```bash
   pip install mkdocs mkdocs-material
   ```

---

## Building the Documentation

To view and build the documentation locally, follow these steps:


### Serve the Documentation

Run the development server to view the documentation locally:

   ```bash
   cd Documentation
   ```

   ```bash
   mkdocs serve
   ```

The server will start, and you can access the documentation in your browser at:
   ```
   http://127.0.0.1:8000
   ```

### Build the Static Site

To generate the static HTML files:
   ```bash
   mkdocs build
   ```

The files will be generated in the `site/` directory.

---

## Contributing

We welcome contributions to PiSpace! If you’d like to contribute:

1. Fork the repository.
2. Create a branch for your changes:
   ```bash
   git checkout -b your-feature-branch
   ```
3. Commit and push your changes to your fork.
4. Open a pull request against the `main` branch of this repository.

---



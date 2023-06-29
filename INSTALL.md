<div align="center">
<p>

[Back to Main Page](./README.md)
</p>

# Installation Guide

</div>

## Table of Contents

- [Installation Guide](#installation-guide)
  - [Table of Contents](#table-of-contents)
  - [Compatibility](#compatibility)
  - [Dependencies](#dependencies)
  - [Installation](#installation)

## Compatibility

**Supported Platforms:**

- [x] Linux
- [X] Windows
- [X] MacOS - Not tested. Might require tweaks in minifb.nelua
- [x] WASM - Not tested. Might require tweaks in minifb.nelua

## Dependencies

You will be required to build and install MiniFBI on your system to use MiniFB.nelua

## Installation

To use MiniFB.nelua you will have to have it located in the working directory of your code e.g:

```
MyCodeFolder:
          --> main.nelua
          --> minifbi.nelua
```


> **Notes**
>
> You can also use .neluacfg to setup custom locations for your libraries. [Please see the following example.](https://github.com/edubart/nelua-lang/discussions/67)

Once successfully installed, simply you have to call it from your code by adding `require "minifb"` and run/build your code.

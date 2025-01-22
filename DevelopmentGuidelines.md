<div align="center">
  <picture>
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/mozahzah/IECore/raw/master/Resources/IE-Brand-Kit/IE-Logo-Alt-NoBg.png?">
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/mozahzah/IECore/raw/master/Resources/IE-Brand-Kit/IE-Logo-NoBg.png?">
  <img alt="IELogo" width="128">
  </picture>
</div>

<h1 align="center">IE's Development Guidelines</h1>

## License Compliance
All projects use the GPL-2.0-only license, ensuring openness and compliance with its terms. GPL-2.0-only license.

### License Notice and Author Attribution

Each file in the codebase must include a license notice at the top to ensure compliance with legal and organizational standards. The license notice should contain the SPDX identifier and the copyright information in the following format:  

```cpp
// SPDX-License-Identifier: GPL-2.0-only  
// Copyright © Interactive Echoes. All rights reserved.  
// Author: AuthorName 
```

If the file already has a license notice, any contributors must append their name to the **Author** field, separated by a comma. For example:  

```cpp
// Author: PreviousAuthorName, NewAuthorName  
```

This ensures proper acknowledgment of contributions while maintaining consistent documentation across all files.  

## Build System
We standardize on CMake for all builds to ensure consistency and cross-platform compatibility.

## Project Structure 
We follow established project structure standards, with separate structures for apps and libraries, to maintain clarity and scalability across codebases.

## Coding Standards
Interactive Echoes follows a strict coding standards to ensure high-quality, readable, and maintainable code. For detailed guidelines, refer to [IE's C++ Coding Standard](https://github.com/Interactive-Echoes/.github/raw/master/CodingStandard.md).

# HawkNL

Hawk NL Version 1.70

  HawkNL cross platform network library
  Copyright (C) 2000-2004 Phil Frisbie, Jr. (phil@hawksoft.com)

  This library is free software; you can redistribute it and/or
  modify it under the terms of the GNU Library General Public
  License as published by the Free Software Foundation; either
  version 2 of the License, or (at your option) any later version.

  This library is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
  Library General Public License for more details.

  You should have received a copy of the GNU Library General Public
  License along with this library; if not, write to the
  Free Software Foundation, Inc., 59 Temple Place - Suite 330,
  Boston, MA  02111-1307, USA.

  Or go to http://www.gnu.org/copyleft/lgpl.html

## Building with CMake

HawkNL now supports building with CMake. To build the library:

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

### CMake Options

- `BUILD_SHARED_LIBS`: Build shared libraries (default: ON)
- `HAWKNL_INCLUDE_IPX`: Include IPX support (Windows only) (default: ON)
- `HAWKNL_INCLUDE_LOOPBACK`: Include loopback support (default: ON)
- `HAWKNL_INCLUDE_SERIAL`: Include serial support (default: ON)

### Installing

```bash
cmake --install .
```

## Using with vcpkg

HawkNL can be installed using vcpkg:

```bash
vcpkg install hawknl
```

To use HawkNL in your CMake project:

```cmake
find_package(HawkNL CONFIG REQUIRED)
target_link_libraries(your_target PRIVATE HawkNL::NL)
```

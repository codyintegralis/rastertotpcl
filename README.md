
# Toshiba TEC TPCL CUPS Raster Driver - rastertotpcl

## Introduction

A driver to Toshiba TEC Label printers supporting the TEC Printer Command Language or TPCL,
version 2.

Converts CUPS Raster graphics along with a supported PPD file into a TPCL graphic ready to
be printed directly. As of yet, there is no support for sending text to the driver.

Conversion includes support for the TPCL TOPIX compression algorithm for reliable and fast
delivery of print jobs to the printer. Raw 8-bit graphics direct from the raster driver
are also supported but not recommended.

This document and source for the driver can be found at:

http://github.com/samlown/rastertotpcl

Any issues or code you'd like to contribute can be performed there.

## License

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.


## History

### 2026-05-08 - CUPS 2.0+ Compatibility and Modern Debian Compilation Updates

Updated the driver to compile cleanly on modern Debian systems, current GCC versions, and modern CUPS libraries (CUPS 2.0+).

**Changes made:**

#### CUPS 2.0+ Compatibility (`src/rastertotpcl.c`)
The `ppd_file_t` type was deprecated in CUPS 1.6 and removed entirely in CUPS 2.0. This fix ensures the driver works with both old and modern CUPS versions:

- **Function Signatures:** All function prototypes changed to use `void *ppd` instead of `ppd_file_t *ppd`
- **Internal Casting:** Each function that needs to access PPD structure members now internally casts: `ppd_file_t *ppd = (ppd_file_t *)ppd_arg;`
- **Backward Compatibility:** Maintains compatibility with CUPS 1.6+ while working with CUPS 2.0+
- **Updated Functions:** `Setup()`, `StartPage()`, `EndPage()`, `OutputLine()`, `TOPIXCompress()`, and `TOPIXCompressOutputBuffer()`

#### Makefile Improvements (`src/Makefile`)
Updated with modern compilation flags and explicit build rules for better compatibility and transparency:

- **Compilation Flags:** Changed from `CFLAGS=-lcupsimage` to `CFLAGS=-Wall -Wextra -O2 -D_DEFAULT_SOURCE`
  - `-Wall -Wextra`: Enable comprehensive compiler warnings to catch potential issues
  - `-O2`: Optimization level for better performance
  - `-D_DEFAULT_SOURCE`: POSIX compatibility on modern Debian systems
- **Separated Variables:** Split `CFLAGS` and `LIBS` for better build control
- **Explicit Build Rule:** Added explicit `gcc` compilation rule showing exactly how the binary is built
- **Improved Transparency:** Build process is now more visible and easier to troubleshoot

#### Earlier Compilation Updates
- Added missing standard library headers: `#include <stdio.h>` and `#include <string.h>`
- Replaced all unsafe `strcpy()` and `strcat()` calls with `strncpy()` and `strncat()` for buffer overflow protection
- Fixed compiler warnings for signed/unsigned type mismatches with explicit casts
- Improved memory management with proper `free()` calls in Setup/EndPage functions
- Fixed ceiling calculation with proper float division before casting to int
- Initialized the `line` array in TOPIXCompress() to prevent uninitialized variable warnings
- Marked intentionally unused parameters with `(void)` to suppress compiler warnings
- Now compiles without warnings or errors on Debian with modern GCC

### 2010-07-10 - PPD Fixes

PPDC compilation errors discovered in Ubuntu 10.10 and resolution setting
was being ignored.


### 2010-05-27 - Initial release
 
Converted original restertotec file into rastertotpcl.
Added support for TOPIX compression.
Now using CUPS raster header 2 for finer control of page sizes.
Refactoring.

## Supported Printers

Support for the following printers is included by the PPD files:

 * Toshiba TEC B-SA4G/T     (tecbas4.ppd)
 * Toshiba TEC B-SX4        (tecbsx4.ppd)
 * Toshiba TEC B-SX5        (tecbsx5.ppd)
 * Toshiba TEC B-SX6        (tecbsx6.ppd)
 * Toshiba TEC B-SX8        (tecbsx8.ppd)
 * Toshiba TEC B-852R       (tecb852r.ppd)
 * Toshiba TEC B-SV4D       (tecbsv4d.ppd)
 * Toshiba TEC B-SV4T       (tecbsv4t.ppd)
 * Toshiba TEC B-EV4D-GS14  (tecbev4d.ppd)
 * Toshiba TEC B-EV4T-GS14  (tecbev4t.ppd)

There is little variation between these printers other than resolutions, speeds, and accepted media types,
so new printer models can be tested or added easily.

As of May 2010, thorough testing has only been performed on the B-SX4 model. Please get in touch if you test
the drivers on other printers with success.


## Installation

The CUPS image development headers are required before compilation. In Ubuntu/Debian, these can be installed with:

    sudo apt-get install libcupsimage2-dev

The easiest way to install from source is to run the following from the base directory:

    make
    sudo make install

This will install the filter and PPD files in the standard CUPS filter and PPD directories
and show them in the CUPS printer selection screens.


## TODO

* Add support for RFID.

## Authors

rastertotpcl is based on the rastertotec driver written by Patick Kong (SKE s.a.r.l).
rastertotec is based on the rastertolabel driver included with the CUPS printing system by Easy Software Products.
Packaging of rastertotpcl and TOPIX compression was added by Sam Lown (www.samlown.com).
Modern Debian compilation updates and CUPS 2.0+ compatibility added by codyintegralis (2026).


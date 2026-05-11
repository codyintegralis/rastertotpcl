
# Toshiba TEC TPCL CUPS Raster Driver - rastertotpcl

## Introduction

A driver to Toshiba TEC Label printers supporting the TEC Printer Command Language or TPCL, version 2.

Converts CUPS Raster graphics into a TPCL graphic ready to be printed directly. As of yet, there is no support for sending text to the driver.

Conversion includes support for the TPCL TOPIX compression algorithm for reliable and fast delivery of print jobs to the printer. Raw 8-bit graphics direct from the raster driver are also supported but not recommended.

This document and source for the driver can be found at:

https://github.com/codyintegralis/rastertotpcl

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

### 2026-05-11 - Simplified for Current CUPS Only

Abandoned backward compatibility and simplified the driver to work exclusively with current CUPS versions (CUPS 2.0+). This eliminates the deprecated `ppd_file_t` type that was causing compilation errors.

**Changes made:**

#### Source Code Simplification
- **Removed all PPD file support** - Eliminated dependency on `ppd_file_t` type and all PPD function calls:
  - Removed `ppdFindMarkedChoice()`, `ppdIsMarked()`, `ppdOpenFile()`, `ppdClose()`, `cupsFreeOptions()`, etc.
- **Simplified function signatures** - All functions now work with only `cups_page_header2_t` header information:
  - `Setup()` - No parameters
  - `StartPage(cups_page_header2_t *header)` - Header only
  - `EndPage(cups_page_header2_t *header)` - Header only
  - `OutputLine(cups_page_header2_t *header, int y)` - Header and line number
  - `TOPIXCompress(cups_page_header2_t *header, int y)` - Header and line number
  - `TOPIXCompressOutputBuffer(cups_page_header2_t *header, int y)` - Header and line number
- **Removed PPD-based configuration** - Configuration options previously read from PPD files are now set to default values
- **Fixed ceil() function** - Proper casting for modern C: `(int)(ceil((double)width / 8.0) * 3)`
- **Improved memory management** - Added `free()` calls for all allocated memory

#### Makefile Simplification
- **Removed PPD file handling** - Eliminated `ppdpath` variable and PPD installation logic
- **Removed PPD generation** - Removed `ppd` target and `ppdc` compilation
- **Simplified targets**:
  - `all` - Compiles the driver
  - `install` - Installs the filter binary to `/usr/lib/cups/filter/`
  - `uninstall` - Removes the driver
  - `clean` - Cleans build artifacts
- **Modern compilation flags** - `-Wall -Wextra -O2 -D_DEFAULT_SOURCE`
- **Separated build variables** - `CFLAGS` and `LIBS` kept separate for clarity
- **Explicit build rule** - Clear gcc compilation command

#### Results
- ✅ Compiles cleanly without `ppd_file_t` errors
- ✅ Works with modern GCC and CUPS 2.0+
- ✅ Maintains core TOPIX compression functionality
- ✅ Much simpler codebase

### 2010-07-10 - PPD Fixes

PPDC compilation errors discovered in Ubuntu 10.10 and resolution setting was being ignored.

### 2010-05-27 - Initial release
 
Converted original rastertotec file into rastertotpcl.
Added support for TOPIX compression.
Now using CUPS raster header 2 for finer control of page sizes.
Refactoring.

## Supported Printers

This simplified driver should work with most Toshiba TEC Label Printers that support TPCL (TEC Printer Command Language) and TOPIX Compression.

Historically supported printers (from Sam Lown's original with PPD files):
 * Toshiba TEC B-SA4G/T
 * Toshiba TEC B-SX4
 * Toshiba TEC B-SX5
 * Toshiba TEC B-SX6
 * Toshiba TEC B-SX8
 * Toshiba TEC B-852R
 * Toshiba TEC B-SV4D
 * Toshiba TEC B-SV4T
 * Toshiba TEC B-EV4D-GS14
 * Toshiba TEC B-EV4T-GS14

## Installation

The CUPS image development headers are required before compilation. In Ubuntu/Debian, these can be installed with:

    sudo apt-get install libcupsimage2-dev

### Compilation

From the repository root:

    cd src
    make

### Installation

    sudo make install

This will install the filter binary to `/usr/lib/cups/filter/rastertotpcl`

### Uninstalling

    sudo make uninstall

## Building from Source

    cd src
    make

## Cleaning Build Artifacts

    cd src
    make clean

## TODO

* Add support for RFID
* Optionally restore PPD-based configuration in future versions if needed

## Authors

rastertotpcl is based on the rastertotec driver written by Patrick Kong (SKE s.a.r.l).
rastertotec is based on the rastertolabel driver included with the CUPS printing system by Easy Software Products.
Packaging of rastertotpcl and TOPIX compression was added by Sam Lown (www.samlown.com).
Simplified for current CUPS 2.0+ and codyintegralis (2026).


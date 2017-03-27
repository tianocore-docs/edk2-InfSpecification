<!--- @file
  2.4 [Defines] Section

  Copyright (c) 2007-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

## 2.4 [Defines] Section

This is a required section.

The `[Defines]` section of EDK II INF files is used to define variable
assignments that can be used in later build steps. The INF_VERSION of existing
INF files does not need to be updated unless content in the file has been
updated to match new content specified by this revision of the specification.

Architectural modifiers are not permitted in the [Defines] section.

The parsing utilities process any local symbol assignments defined in this
section. The

EDK II parsing utilities will use some of this section's information for
generating `AutoGen.c` and `AutoGen.h` files. Note that the sections are
processed in the order listed in the INF file, and later assignments of these
local symbols override previous assignments.

`[Defines]`

The format for entries in this section is:

`Name = Value`

The following is an example of a driver's `[Defines]` section.

```ini
[Defines]
  INF_VERSION     = 0x00010019
  BASE_NAME       = DxeIpl
  FILE_GUID       = 86D70125-BAA3-4296-A62F-602BEBBB9081
  VERSION_STRING  = 1.0
  MODULE_TYPE     = PEIM
  ENTRY_POINT     = PeimInitializeDxeIpl
  MODULE_UNI_FILE = DxeIpl.uni
```

The following is an example of a library's `[Defines]` section.

```ini
[Defines]
  INF_VERSION    = 1.25
  BASE_NAME      = BaseMemoryLib
  FILE_GUID      = fd44e603-002a-4b29-9f5f-529e815b6165
  MODULE_TYPE    = BASE
  VERSION_STRING = 1.0
  LIBRARY_CLASS  = BaseMemoryLib
```

Drivers may expose library functionality, such as a `DXE_CORE` module that may
implement functions that satisfy the BaseMemoryAllocation library class. In
this instance, the driver module would also specify the `LIBRARY_CLASS` in the
`[Defines]` section. Other DXE drivers that would require a library instance for
the `BaseMemoryAllocation` class could specify the `DXE_CORE` INF file as the
recommended instance for satisfying the required library class instance.

Appendix G lists the available `MODULE_TYPE` values supported by EDK II INF
files.

The EDK II `[Defines]` section is common to all architectures and does not
permit using architectural modifiers in the section tag name.

The following table shows EDK II unique elements of a defines section that
may be required for generating the `AutoGen.c` and `AutoGen.h` files. Library
modules must never specify driver elements.

**********
**Note:** Any lines not starting with one of the tag names defined in the table
below are added to the top of the INF's generated makefile exactly as typed on
the line in the INF file.
**********
**Note:** `COMBINED_PEIM_DRIVER` is a driver that may be dispatched by either
the PEI Core or the Dxe Core. EDK II only references the first possible
dispatch instance.
**********

###### Table 1 EDK II [Defines] Section Elements

| Tag                          | Required                                                                 | Value                                         | Notes                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------- | ------------------------------------------------------------------------ | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `INF_VERSION`                | REQUIRED                                                                 | 1.25 or 0x00010019                            | This identifies the INF spec. version. Tools use this value to handle parsing of previous releases of the specification if there are incompatible changes.                                                                                                                                                                                                                                                                                          |
| `BASE_NAME`                  | REQUIRED                                                                 | A single word                                 | This is a single word identifier that will be used for the component name.                                                                                                                                                                                                                                                                                                                                                                          |
| `EDK_RELEASE_VERSION`        | Not required                                                             | Hex Double Word                               | The minimum revision value across the module and all its dependent libraries. If a revision value is not declared in the module or any of the dependent libraries, then the tool may use the value of 0, which disables checking.                                                                                                                                                                                                                   |
| `PI_SPECIFICATION_VERSION`   | Not required                                                             | Decimal or special format of hex              | The minimum revision value across the module and all its dependent libraries. If a revision value is not declared in the module or any of the dependent libraries, then tools may use the value of 0, which disables checking.                                                                                                                                                                                                                      |
|                              |                                                                          |                                               | The `PI_SPECIFICATION_VERSION` must only be set in the INF file if the module depends on services or system table fields or PI core behaviors that are not present in the PI 1.0 version. For example, if a module depends on definitions in PI 1.1 that are not in PI 1.0, then `PI_SPECIFICATION_VERSION` must be 0x0001000A                                                                                                                      |
| `UEFI_SPECIFICATION_VERSION` | Not required                                                             | Decimal or special format of hex              | The minimum revision value across the module and all its dependent libraries. If a revision value is not declared in the module or any of the dependent libraries, then tools may use the value of 0, which disables checking.                                                                                                                                                                                                                      |
|                              |                                                                          |                                               | The `UEFI_SPECIFICATION_VERSIon` must only be set in the INF file if the module depends on UEFI Boot Services or UEFI Runtime Services or UEFI System Table fields or UEFI core behaviors that are not present in the UEFI 2.1 version. For example, if a module depends on definitions in UEFI 2.2 that are not in UEFI 2.1, then `UEFI_SPECIFICATION_VERSION` must be 0x00020014                                                                  |
| `FILE_GUID`                  | REQUIRED                                                                 | GUID Value                                    | Registry (8-4-4-4-12) Format This value is required for all EDK II format INF files, required for EDK driver INF files, not required for EDK libraries                                                                                                                                                                                                                                                                                              |
| `MODULE_TYPE`                | REQUIRED                                                                 |                                               | This is the type of module. One of the EDK II Module Types. For Library Modules, the `MODULE_TYPE` must specify the `MODULE_TYPE` of the module that will use the driver.                                                                                                                                                                                                                                                                           |
| `BUILD_NUMBER`               | Optional                                                                 | UINT16 Value                                  | This optional element, if present (or set in the DSC file), is used during the creation of the `EFI_VERSION_SECTION` for this module; if it is not present, then the BuildNumber field of the `EFI_VERSION_SECTION` will be set to 0.                                                                                                                                                                                                               |
| `VERSION_STRING`             | REQUIRED                                                                 | String                                        | If present, this value will be encoded as USC-2 characters in a Unicode file for the VERSION section of the FFS unless a ver or ver_ui file has been specified in the `[Binaries]` section.                                                                                                                                                                                                                                                         |
| `MODULE_UNI_FILE`            | Optional                                                                 | Filename                                      | A Unicode file containing UCS-2 character localization strings; the file path (if present) is relative to the directory containing the INF file. The use of #include statements in this file is prohibited.                                                                                                                                                                                                                                         |
| `LIBRARY_CLASS`              | Typically not specified for a Driver; REQUIRED for a Library Only Module | Word &#124; List ["&#124;" Word &#124; List]* | One Library Class that is satisfied by this Library Instance; one or more `LIBRARY_CLASS` lines may be specified by a module. The reserved keyword, NULL, must be listed for library class instances that do NOT support a library class keyword.                                                                                                                                                                                                   |
| `PCD_IS_DRIVER`              | Not required - Driver Only                                               | PEI_PCD_DRIVER or DXE_PCD_DRIVER              | Only required for the two (PEI_PCD_DRIVER or DXE_PCD_DRIVER) PCD Driver modules.                                                                                                                                                                                                                                                                                                                                                                    |
| `ENTRY_POINT`                | Not required - Driver Only                                               | CName                                         | This is the name of the driver's entry point function.                                                                                                                                                                                                                                                                                                                                                                                              |
| `UNLOAD_IMAGE`               | Not required - Driver Only                                               | CName                                         | If a driver chooses to be unloadable, then this is the name of the module's function registered in the Loaded Image Protocol. It is called if the UEFI Boot Service UnloadImage() is called for the module, which then executes the Unload function, disconnecting itself from handles in the database as well as uninstalling any protocols that were installed in the driver entry point. The CName is the name of this module's unload function. |
| `CONSTRUCTOR`                | Not required - Library Only                                              | CName                                         | This only applies to components that are libraries. It is required for EDK II libraries if the module's INF contains a Constructor element. This value is used to call the specified function before calling into the library itself.                                                                                                                                                                                                               |
| `DESTRUCTOR`                 | Not required - Library Only                                              | CName                                         | This only applies to components that are libraries. This value is used to call the specified function before calling into the library itself.                                                                                                                                                                                                                                                                                                       |
| `SHADOW`                     | Not required - SEC, PEIM and PEI_CORE Driver modules only                | TRUE &#124; FALSE                             | This boolean operator is used by `SEC`, `PEI_CORE` and `PEIM` modules to indicate if the module was coded to use `REGISTER_FOR_SHADOW`. If the value is TRUE, the .reloc section of the PE32 image is not removed, otherwise, the .reloc section is stripped to conserve space in the final binary images. The default value is FALSE.                                                                                                              |
| `PCI_DEVICE_ID`              | Not required - Required for UEFI PCI Option ROMs                         | Hex Number                                    | The PCI Device Id for this device.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `PCI_VENDOR_ID`              | Not required - Required for UEFI PCI Option ROMs                         | Hex Number                                    | The PCI Vendor Id for this device                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `PCI_CLASS_CODE`             | Not required - Required for UEFI PCI Option ROMs                         | Hex Number                                    | The PCI Class Code for this device                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `PCI_COMPRESS`               | Not required UEFI PCI Option ROMs                                        | TRUE &#124; FALSE                             | This flag is used by tools to compress a PCI Option ROM image file, the default (if not specified) is FALSE                                                                                                                                                                                                                                                                                                                                         |
| `UEFI_HII_RESOURCE_SECTION`  | Not required Driver Only                                                 | TRUE &#124; FALSE                             | This boolean operator is used to indicate that the module will require a separate HII resource section in the efi image file.                                                                                                                                                                                                                                                                                                                       |
| `DEFINE`                     | Not required                                                             | Name = Value                                  | The value must be a directory name, and the name can be used with $( and ) character sets. This allows shortening of lines typed by users.                                                                                                                                                                                                                                                                                                          |
| `SPEC`                       | Not required                                                             | CName = Value                                 | A User-specified #define CName Value pair that will be included in the `AutoGen.h` file.                                                                                                                                                                                                                                                                                                                                                            |
| `CUSTOM_MAKEFILE`            | Not required                                                             | Family &#124; File                            | A user written makefile that will be used, the INF file will not be parsed.  The Family is one of MSFT or GCC followed by a field separator "&#124;" character, then the filename of the makefile in the same directory as the INF file. To keep GCC compatibility, the user must generate two Makefiles, one for MSFT, such as makefile and another for GCC, such as GNUmakefile                                                                   |
| `DPX_SOURCE`                 | Not Required Driver Only                                                 | Filename                                      | If present, the file must contain all DEPEX statements (as defined in the UEFI PI specification), as the tools will process the file, ignoring any content in `[Depex]` sections in this file AND all inherited dependencies from libraries. This allows the module owner to force a Depex independently. Use of this option is not recommended for normal use.                                                                                     |

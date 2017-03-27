<!--- @file
  A.1 Design Discussion

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

## A.1 Design Discussion

Directive statements are permitted within the EDK INF files.

### A.1.1 [defines] Section

The `[defines]` section of an EDK INF file is used to define variable
assignments that can be used in later build steps. The EDK parsing utilities
process local symbol assignments made in this section. Note that the sections
are processed in the order listed here, and later assignments of these local
symbols do not override previous assignments.

This section will typically use one of the following section definitions:

```ini
[define]
[defines]
[defines.IA32]
[defines.X64]
[defines.IPF]
[defines.EBC]
```

**********
**Note:** The `[define]` section tag is only valid for EDK INF files. EDK II INF
files must use the 'defines' keyword.
**********

The format for entries in this section is:

`Name = Value`

The following is an example of this section.

```ini
[defines]
  BASE_NAME      = DiskIo
  FILE_GUID      = CA261A26-7718-4b9b-8A07-5178B1AE3A02
  COMPONENT_TYPE = BS_DRIVER
```

The following table lists the possible content of this section.

###### Table 6 EDK [defines] Section Elements

| Tag                         | Required                                                           | Value                                                           | Notes                                                                                                                                                                                                     |
| --------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BASE_NAME`                 | Yes                                                                | A single word                                                   | This is a single word identifier that will be used for the component name.                                                                                                                                |
| `COMPONENT_TYPE`            | Yes                                                                | One of the EDK I Component Types                                | See Table EDK I Component (module) Types for possible values                                                                                                                                              |
| `FILE_GUID`                 | No--Optional for Libraries, Required for all other component types | Guid Value                                                      | Registry (8-4-4-4-12) Format GUID                                                                                                                                                                         |
| `EDK_RELEASE_VERSION`       | No--Optional                                                       | Hex Value                                                       | A Hex version number, 0x00020000                                                                                                                                                                          |
| `EFI_SPECIFICATION_VERSION` | No--Optional                                                       | HexValue                                                        | A Hex version number, 0x00020000                                                                                                                                                                          |
| `MAKEFILE_NAME`             | No--Optional                                                       | Filename.ext                                                    | The name of the Makefile to be generated for this component                                                                                                                                               |
| `CUSTOM_MAKEFILE`           | No--Optional                                                       | Filename.ext                                                    | This specifies the name of a custom makefile that should be used, instead of a generated makefile. **NOTE**: EDK INF components specifying a custom EDK style makefile cannot be used in an EDK II build. |
| `BUILD_NUMBER`              | No--Optional                                                       | Set this four digit value in the generated Makefile             | Normally not used in INF files.                                                                                                                                                                           |
| `C_FLAGS`                   | No--Optional                                                       | Microsoft C Flags to use with for a cl commands for this module | Normally not used in INF files. Typically, an EDK INF file will provide a separate nmake section to specify different build parameters.                                                                   |
| `FFS_EXT`                   | No--Optional                                                       | File Extension                                                  | The FFS extension to use for this component, refer to the table _EDK I Component (module) Types for the default FFS extension. This value is used to create a component PKG file.                         |
| `FV_EXT`                    | No--Optional                                                       | File Extension                                                  | The FV extension to use for this component, refer to the table _EDK I Component (module) Types for the default FV extension.                                                                              |
| `SOURCE_FV`                 | No--Optional                                                       | Word                                                            | If present, the variable is set at the beginning of the generated makefile                                                                                                                                |
| `VERSION`                   | No--Optional                                                       | Four digit integer                                              | If present, this value will be used for the VERSION section of the FFS.                                                                                                                                   |
| `VERSION_STRING`            | No--Optional                                                       | String                                                          | If present, this value will be used to generate the UNICODE file for the VERSION section of the FFS.                                                                                                      |

The following table lists the available `COMPONENT_TYPE` values supported by
EDK INF files.

###### Table 7 EDK Component (module) Output File Extensions

| COMPONENT_TYPE          | EDK II Extension       | EDK File, FFS or FV Extension   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------------- | ---------------------- | ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `LIBRARY`               | .lib                   | .lib                            | Library component linked as part of the build with other components.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `FILE`                  | From file name or .FFS | From file name or .FFS          | Raw file copied directly to FV                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `Apriori`               | .bin                   | .SEC                            | This EDK INF component is not supported in the EDK II build - it is created from content in other EDK II build meta-data files.                                                                                                                                                                                                                                                                                                                                                                                                                |
| `EFI Binary Executable` | .efi                   | .pe32                           | PE32/PE32+/Coff binary executable. The extension of the file has changed for EDK II builds which generate processed (GenFw) images.                                                                                                                                                                                                                                                                                                                                                                                                            |
| `AcpiTable`             | .acpi                  | .SEC                            | An ACPI Table.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `Legacy16`              | .bin                   | .SEC                            | The MODULE_TYPE for a Legacy16 when migrating to EDK II should be specified as USER_DEFIND. The .rom or .bin file should be included under a [binaries] section. In EDK, the COMPONENT_TYPE of Legacy16 was mostly used to specify PCI Option ROMs.                                                                                                                                                                                                                                                                                            |
| `BINARY`                | .bin                   | .FFS                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `CONFIG`                | .bin                   | .SEC                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `LOGO`                  | .bin                   | .SEC                            | The MODULE_TYPE for a LOGO when migrating to EDK II should be specified as USER_DEFINED. The .bmp file should be include under a [binaries] section. In EDK, the COMPONENT_TYPE of LOGO was used to specify a .bmp file.                                                                                                                                                                                                                                                                                                                       |
| `RAWFILE`               | .raw                   | .RAW                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `FVIMAGEFILE`           | .fv                    | .FVI                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `SECURITY_CORE`         | .efi                   | .SEC                            | Modules of this type are designed to start execution at the reset vector of a CPU. They are responsible for preparing the platform for the PEI Phase. Since there are no standard services defined for SEC, modules of this type follow the same rules as modules of type Base and typically include some amount of CPU specific assembly code to establish temporary memory for a stack. Modules of this type may optionally produce services that are passed to the PEI Phase in HOBs and those services must be compliant with the PEI CIS. |
| `PEI_CORE`              | .efi                   | .PEI                            | This module type is used by PEI Core implementations that are complaint with the PEI CIS.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `COMBINED_PEIM_DRIVER`  | .efi                   | .PEI                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `PIC_PEIM`              | .efi                   | .PEI                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `RELOCATABLE_PEIM`      | .efi                   | .PEI                            | When migrating to EDK II, this type of module should use the register for shadow PPI, and set the [defines] entry: `SHADOW = TRUE`                                                                                                                                                                                                                                                                                                                                                                                                             |
| `PE32_PEIM`             | .efi                   | .PEI                            | This module type is used by PEIMs that are compliant with the PEI CIS                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `BS_DRIVER`             | .efi                   | .DXE                            | This module type is either the DXE Core or DXE Drivers that are complaint with the DXE CIS. These modules only execute in the boot services environment and are destroyed when ExitBootServices() is called.                                                                                                                                                                                                                                                                                                                                   |
| `RT_DRIVER`             | .efi                   | .DXE                            | This module type is used by DXE Drivers that are complaint with the DXE CIS. These modules execute in both boot services and runtime services environments. This means the services that these modules produce are available after ExitBootServices() is called. If SetVirtualAddressMap() is called, then modules of this type are relocated according to virtual address map provided by the operating system.                                                                                                                               |
| `SAL_RT_DRIVER`         | .efi                   | .DXE                            | This module type is used by DXE Drivers that can be called in physical mode before SetVirtualAddressMap() is called and either physical mode or virtual mode after SetVirtualAddressMap() is called. This module type is only available to IPF CPUs. This means the services that these modules produce are available after ExitBootServices().                                                                                                                                                                                                |
| `BS_DRIVER`             | .efi                   | .SMM                            | This module type is used by DXE Drivers that are loaded into SMRAM. As a result, this module type is only available for IA-32 and x64 CPUs. These modules only execute in physical mode, and are never destroyed. This means the services that these modules produce are available after ExitBootServices().                                                                                                                                                                                                                                   |
| `APPLICATION`           | .efi                   | .APP                            | This module type is used by UEFI Applications that are compliant with the EFI 1.10 Specification or the UEFI 2.0 Specification. UEFI Applications are always unloaded when they exit.                                                                                                                                                                                                                                                                                                                                                          |
| `EFI USER INTERFACE`    | .ui                    | .ui                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `EFI VERSION`           | .ver                   | .ver                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `EFI DEPENDENCY`        | .dpx                   | .dpx                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

### A.1.2 [sources] Section

The `[sources]` section is used to specify the files that make up the
component. Directories names are required for files existing in subdirectories
of the component. All directory names are relative to the location of the INF
file. Macros are allowed in the source file path. For EDK builds, each file is
added to the macro of `$(INC_DEPS)`, which can be used in a makefile dependency
expression.

This section will typically use one of the following section definitions:

```ini
[sources]
[sources.common]
[sources.IA32]
[sources.X64]
[sources.IPF]
[sources.EBC]
```

The following example demonstrates entries in this section.

```ini
[sources.common]
  DxeIpl.dxs
  DxeIpl.h
  DxeLoad.c

[sources.Ia32]
  Ia32/VirtualMemory.h
  Ia32/VirtualMemory.c
  Ia32/DxeLoadFunc.c
  Ia32/ImageRead.c

[sources.X64]
  X64/DxeLoadFunc.c

[sources.IPF]
  Ipf/DxeLoadFunc.c
  Ipf/ImageRead.c
```

Binary file types - EDK does not have the flexibility of EDK II, but does
provide a method for specifying binary files in the `[sources]` section. The
following lists the mapping of EDK specific binary file types to EFI sections.

**_SEC_GUID_**

The binary file is an `EFI_SECTION_FREEFORM_SUBTYPE_GUID` section.

**_SEC_PE32_**

This binary is an `EFI_SECTION_PE32` section.

**_SEC_PIC_**

This binary is an `EFI_SECTION_PIC` section.

**_SEC_PEI_DEPEX_**

This binary is an `EFI_SECTION_PEI_DEPEX` section.

**_SEC_DXE_DEPEX_**

This binary is an `EFI_SECTION_DXE_DEPEX` section.

**_SEC_TE_**

This binary is an `EFI_SECTION_TE` section.

**_SEC_VER_**

This binary is an `EFI_SECTION_VERSION` section.

**_SEC_UI_**

This binary is an `EFI_SECTION_USER_INTERFACE` section.

**_SEC_BIN_**

The binary is an `EFI_SECTION_RAW` section.

**_SEC_COMPAT16_**

This binary is an `EFI_SECTION_COMPATIBILTY16` section.

### A.1.3 [libraries] Section

The `[libraries]` section of the EDK INF is used to list the names of the
libraries that will be linked into the EDK component. The library names do not
include the directory locations or the extension name of the file. For each
library, `{LibName}`, found, the `{LibName}` is added to the LIBS definition in
the output makefile:

`LIBS = $(LIBS) $(LIB_DIR)\{LibName}`

This section will typically use one of the following section definitions:

```ini
[libraries.common]
[libraries.IA32]
[libraries.X64]
[libraries.IPF]
[libraries.EBC]
```

The formats for entries in this section is:

`LibraryName`

The following is an example of a libraries section.

```ini
[libraries.common]
  EfiProtocolLib
  EfiDriverLib
```

### A.1.4 [includes] Section

The `[includes]` section of the EDK INF file is a list of directories to be
included on the compile command line. These are included in a section of the
Makefile generated by the parsing utilities. For each include path specified,
the following line is written to the component's makefile.

`INC = $(INC) -I $(SOURCE_DIR)\{path}`

The path must be absolute, however the use of the global variable, EDK_SOURCE
is recommended to construct the path.

This section will typically use one of the following section definitions:

```ini
[includes.common]
[includes.IA32]
[includes.X64]
[includes.IPF]
[includes.Nt32]
[include.common]
[include.IA32]
[include.X64]
[include.IPF]
```

The formats for entries in this section is:

`$(EDK_SOURCE)/path/to/header/files`

The following is an example of the [includes] section.

```ini
[includes.common]
  $(EDK_SOURCE)FoundationEfi
  $(EDK_SOURCE)Foundation
  $(EDK_SOURCE)FoundationFramework
  .
  $(EDK_SOURCE)FoundationInclude
  $(EDK_SOURCE)FoundationEfiInclude
  $(EDK_SOURCE)FoundationFrameworkInclude
  $(EDK_SOURCE)FoundationIncludeIndustryStandard
  $(EDK_SOURCE)FoundationCoreDxe
  $(EDK_SOURCE)FoundationLibraryDxeInclude
```

### A.1.5 [nmake] Section

The optional EDK `[nmake]` section may also include a ".ProcessorName" to
restrict processing based on the processor name. The section data is simply
copied directly to the component makefile, before the build commands are
emitted.

This section will typically use one of the following section definitions:

```ini
[nmake]
[nmake.common]
[nmake.IA32]
[nmake.X64]
[nmake.IPF]
[nmake.EBC]
```

The format for entries in this section is any valid Makefile syntax. Refer to
make command reference for your tool chains.

The following is an example of the EDK `[nmake]` section.

```ini
[nmake.common]
  IMAGE_ENTRY_POINT = DiskIoDriverEntryPoint
```

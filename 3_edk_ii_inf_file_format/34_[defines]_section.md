<!--- @file
  3.4 [Defines] Section

  Copyright (c) 2007-2019, Intel Corporation. All rights reserved.<BR>

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

## 3.4 [Defines] Section

This is a required section.

#### Summary

This describes the required `[Defines]` section used in EDK II INF files. This
file is created during installation of a UEFI distribution package or by the
developer and is an input to the new build tool parsing utilities. Elements may
appear in any order within this section.

The version for this specification is "0x0001001B" and new versions of this
specification must increment the minor (001B) portion of the specification code
for backward compatible changes, and increment the major number for
non-backward compatible specification changes. This value may also be specified
as a decimal value, 1.27.

The `[Defines]` section assigns values to the symbols that describe the
component. Some items are emitted to the output makefile, others are used to
create filenames during the build. Some symbols are emitted to the generated C
files.

The `FILE_GUID` is required for all EDK II modules. This GUID is used to build
the FW volume file list used by build tools to generate the final firmware
volume, as well as processed in some SMM, PEI or DXE `DEPEX` statements.

All new EDK II INF files must include one of the following statements:
`INF_VERSION = 0x0001001B` or `INF_VERSION = 1.27` in this section, where the
number varies according to the release of this specification. It is a
HexVersion type, where the 0x0001 is the major number, and the 001B is the
minor number. This version of the specification provides full backward
compatibility to all previous versions. This means that tools that process
this version of the specification can also process earlier versions of
EDK II INF files.

**********
**Note:** Possible values for `MODULE_TYPE`, and their descriptions, are
listed in the table, "EDK II Module Types." For each module, the `BASE_NAME`
and `MODULE_TYPE` are required. The `BASE_NAME` definition is case
sensitive as it will be used to create a directory name during a build.
**********

Only the `[Defines]` section tag is valid for EDK II INF files -
architectural modifiers for the `[Defines]` section tag are not permitted. The
section is processed in order by the parsing utilities. Assignments of
variables in other sections override previous assignments.

The `SHADOW` keyword is only valid for `SEC`, `PEI_CORE` and `PEIM` module
types. It is an error to declare the `SHADOW` keyword in other module types.
The default value of `SHADOW` is "FALSE" when the `SHADOW` keyword is not
specified.

EDK II modules that provide different library class implementations must use
multiple `LIBRARY_CLASS` statements. Each `LIBRARY_CLASS` statement must provide
the name of the library class supported, followed by the pipe "|" field
separator and then a space " " delimited list of module types the library
instances supports. The following is an example of specifying multiple library
classes.

```ini
LIBRARY_CLASS = FOO | PEI_CORE PEIM
LIBRARY_CLASS = BAR | DXE_CORE DXE_DRIVER DXE_SMM_DRIVER
```

**********
**Note:** Each library class requires a header file defined in the package that
declares the library class. Refer to the "EDK II Module Writer's Guide" for
more information about writing drivers and libraries.
**********

Additionally, a driver module may expose internal implementations of a library
class, making the internal implementations public. As an example, a `DXE_CORE`
implementation that uses internal functions that provide the functionality of
the EDK II Base Memory Library. The `DXE_CORE` module that provides these
functions (for example, `DxeMain.inf`) can expose them for use by other DXE
drivers that depend on the BaseMemoryLib library class.

The OptionRom statements must be included for UEFI PCI Option ROMs, and can
only be used with a `MODULE_TYPE` of `UEFI_DRIVER`.

The optional `MODULE_UNI_FILE` entry is used to locate an Unicode file which
can be used for localization of the module's Abstract and Description from the
header section. The content of the file can be generated by tools during the
installation of a distribution package that conforms to the UEFI Platform
Initialization Distribution Packaging Specification, or by module developers
creating new content.

The `FixedComments` sections that follow a defines section are to permit tools
to work with UEFI Distribution Packaging Specification requirements.

#### Prototype

**********
**Note:** The entry, `INF_VERSION`, `BASE_NAME`, `FILE_GUID` and `MODULE_TYPE`
are required for EDK II Modules. The `VERSION_STRING` entry is highly
recommended.
**********

```c
<Defines>          ::= "[Defines]" <EOL>
                       <DefineStatements>+
<DefineStatements> ::= <TS> "INF_VERSION" <Eq> <SpecVersion> <EOL>
                       <TS> "BASE_NAME" <Eq> <BaseName> <EOL>
                       <TS> "FILE_GUID" <Eq> <RegistryFormatGUID> <EOL>
                       <TS> "MODULE_TYPE" <Eq> <Edk2ModuleType> <EOL>
                       [<TS> "UEFI_SPECIFICATION_VERSION" <Eq>
                       <VersionVal> <EOL>]
                       [<TS> "PI_SPECIFICATION_VERSION" <Eq>
                       <VersionVal> <EOL>]
                       [<TS> "LIBRARY_CLASS" <Eq> <LibClass> <EOL>]*
                       [<TS> "BUILD_NUMBER" <Eq> <UINT16> <EOL>]
                       [<TS> "VERSION_STRING" <Eq>
                       <DecimalVersion> <EOL>]
                       [<TS> "PCD_IS_DRIVER" <Eq>
                       <PcdDriverType> <EOL>]
                       [<TS> "ENTRY_POINT" <Eq> <CName> [<FFE>] <EOL>]*
                       [<TS> "UNLOAD_IMAGE" <Eq> <CName>
                       [<FFE>] <EOL>]*
                       [<TS> "CONSTRUCTOR" <Eq> <CName> [<FFE>] <EOL>]*
                       [<TS> "DESTRUCTOR" <Eq> <CName> [<FFE>] <EOL>]*
                       [<TS> "SHADOW" <Eq> <BoolType> <EOL>]
                       [<OptionRomInfo>]
                       [<TS> "CUSTOM_MAKEFILE" <Eq>
                       <CustomMake> <EOL>]*
                       [<TS> "DPX_SOURCE" <Eq> <Filename> <EOL>]
                       [<TS> "SPEC" <MTS> <Spec> <EOL>]*
                       [<TS> <UefiHiiResource> <EOL>]
                       [<TS> "MODULE_UNI_FILE" <Eq> <Filename> <EOL>]
                       [<MacroDefinition>]*
<BaseName>         ::= (a-zA-Z0-9)(a-zA-Z0-9_-.)*
<UefiHiiResource>  ::= "UEFI_HII_RESOURCE_SECTION" <Eq> <TrueFalse>
<CustomMake>       ::= [<Family> <FS>] <Filename>
<PcdDriverType>    ::= {"PEI_PCD_DRIVER"} {"DXE_PCD_DRIVER"}
<Spec>             ::= <Identifier> <Eq> <DecimalVersion>
<LibClass>         ::= {<KeywordType>} {"NULL"}
<KeywordType>      ::= <SimpleWord> [<FS> <ModuleTypes>]
<ModuleTypes>      ::= <ModuleType> [<Space> <ModuleType>]*
<FFE>              ::= <FS> <Expression>
<SpecVersion>      ::= {<HexVersion>} {(0-9))+ "." (0-9)+}
<OptionRomInfo>    ::= <TS> "PCI_VENDOR_ID" <Eq> <UINT16> <EOL>
                       <TS> "PCI_DEVICE_ID" <Eq> <UNIT16> [<MTS> <UINT16>]* <EOL>
                       <TS> "PCI_CLASS_CODE" <Eq> <UINT8> <EOL>
                       <TS> "PCI_REVISION" <Eq> <UINT8> <EOL>
                       [<TS> "PCI_COMPRESS" <Eq> <TruFal> <EOL>]
<TruFal>           ::= {"TRUE"} {"FALSE"}
```

#### Parameters

**_Filename_**

Filenames listed in the `[Defines]` section must be relative to the directory
the INF file is in. Use of "..", "." and "../" in the directory path is not
permitted. Use of an absolute path is not permitted. The file name specified in
the `MODULE_UNI_FILE` entry must be a Unicode file with an extension of .uni,
.UNI or .Uni.

**_MODULE_TYPE_**

Drivers and applications are not allowed to have a `MODULE_TYPE` of `"BASE`".
Only libraries are permitted to a have a `MODULE_TYPE` of `"BASE`". A INF file
can be used to specify other binary files types, such as logo images or
legacy16 option ROMs. The `USER_DEFINED` module type must be used in all cases
where the module type is not a member of `<Edk2ModuleType>`.

**_INF_VERSION_**

For new INF files, the version value must be set to `0x0001001B`. Tools that
process this version of the INF file can successfully process earlier versions
of the INF file (this is a backward compatible update). There is no requirement
to change the value in existing INF files if no other content changes. This may
also be specified as decimal value, 1.27.

**_EDK_RELEASE_VERSION_**

This optional value may be set to the major/minor number of the EDK II release
required for modules to function correctly.

**_UEFI_SPECIFICATION_VERSION_**

The `UEFI_SPECIFICATION_VERSION` must only be set in the INF file if the module
depends on UEFI Boot Services or UEFI Runtime Services or UEFI System Table
fields or UEFI core behaviors that are not present in the UEFI 2.1 version. The
version number for the UEFI 2.3.1 specification is the hex value: 0x0002001F.
The minor number of the specification version is a 2 digit number, where the
2.3.1 is actually: 2.31 The major number must be incremented on a revision that
would result in a minor number greater than 99.

**_PI_SPECIFICATION_VERSION_**

The `PI_SPECIFICATION_VERSION` must only be set in the INF file if the module
depends on services or system table fields or PI core behaviors that are not
present in the PI 1.0 version. The version number for the 1.2 PI specification
is the hex value: 0x00010014 The minor number of the specification version is a
2 digit number, where the 1.2 is actually: 1.20 The major number must be
incremented on a revision that would result in a minor number greater than 99.

**_VERSION_STRING_**

This is typically a decimal number, and if not specified, an empty string will
be provided. This value will be converted by the build tools from an ASCII
string into a null-terminated Unicode string that contains a text
representation of the version. A platform integrator may specify a different
version string for an FFS version section in the FDF file if more than just
this value is needed. It is recommended that the decimal number be used in such
a manner as the integer portion of the value is considered the major number
(changing when there is a functional, non-backward compatible change). The
factional portion of the value is the considered the minor number (changing
anytime there is a change in the code that would result in a binary image that
was not identical to the binary image created prior to the change).

**_BuildNumber_**

The optional build number must be NumValUint16 If not present, the EDK II build
tools will use the `BUILD_NUMBER` from the DSC file. If the DSC file does not
include the build number, the EDK II build tools will use a value of 0 If the
Build number is greater than `0`, the generated INF file must contain this
entry.

**_Spec_**

The user is required to ensure that a valid C name is used for the name of the
specification, and provide the decimal version of the specification. For
example, `SPEC USB_SPECIFICATION_VERSION = 2.0` is a valid statement. These
statements are used to generate #define statements in the auto generated C files.

**_UefiHiiResource_**

This is an optional tag used to identify UEFI compliant drivers that must have
a `UEFI_HII_RESOURCE_SECTION` generated as part of the efi image file. If not
specified, the default is false.

**_DPX_SOURCE_**

The path and filename must be relative to the INF file and located within the
module's directory tree. The file must contain only DEPEX statements as defined
in the UEFI PI Specification that are valid for the module type. C style
Comments are not in the file. Contents of this file completely override any
dependency expressions listed in `[Depex]` sections and all inherited
dependency expressions that would normally have been inherited from libraries
linked to the module. Use of this feature is not recommended for normal use.

**_OptionRomInfo_**

These statements are used by developers of stand-alone PCI Option ROM drivers.

They allow the developer to forego creation of an FDF file in the package
directory. Only the INF file and a DSC file are required for pure PCI Option
ROM development.

**_ENTRY_POINT CName_**

This is the name of the driver's entry point function. Refer to the UEFI Driver
Writer's Guide for more information.

**_UNLOAD_IMAGE CName_**

If a driver chooses to be unloadable, then this is the name of the module's
function registered in the Loaded Image Protocol. It is called if the UEFI Boot
Service UnloadImage() is called for the module, which then executes the Unload
function, disconnecting itself from handles in the database as well as
uninstalling any protocols that were installed in the driver entry point. The
CName is the name of this module's unload function. Refer to the UEFI Driver
Writer's Guide for more information.

#### Example (EDK II Driver)

```ini
[Defines]
  INF_VERSION                = 1.27
  BASE_NAME                  = PlatformAcpiTable
  FILE_GUID                  = 7E374E25-8E01-4FEE-87F2-390C23C606CD
  MODULE_TYPE                = DXE_DRIVER
  VERSION_STRING             = 1.0
  EDK_RELEASE_VERSION        = 0x00020000
  UEFI_SPECIFICATION_VERSION = 0x00020014
```

#### Example (UEFI Driver)

```ini
[Defines]
  INF_VERSION    = 0x0001001B
  BASE_NAME      = Abc
  FILE_GUID      = DA87D340-15C0-4824-9BF3-D52286674BEF
  MODULE_TYPE    = UEFI_DRIVER
  VERSION_STRING = 1.0
  ENTRY_POINT    = AbcDriverEntryPoint
  UNLOAD_IMAGE   = AbcUnload
```

#### Example (EDK II Library)

```ini
[Defines]
  INF_VERSION    = 1.27
  BASE_NAME      = LzmaCustomDecompressLib
  FILE_GUID      = 22f8406f-43ee-492f-82f5-4e8a1a58e6d2
  MODULE_TYPE    = BASE
  VERSION_STRING = 1.0
  LIBRARY_CLASS  = CustomDecompressLib
```

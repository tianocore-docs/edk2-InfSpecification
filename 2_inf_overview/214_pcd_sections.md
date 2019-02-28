<!--- @file
  2.14 PCD Sections

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

## 2.14 PCD Sections

These sections are used for specifying PCD information. The entries for these 
sections are looked up from the packagedeclaration files (DEC) for generating 
the `AutoGen.c` and `AutoGen.h` files.

The PCD's Name (`PcdName`) is defined as PCD Token Space GUID C name and the
PCD C name - separated by a period "." character. Unique PCDs are identified
using the following format to identify the named PCD:

`PcdTokenSpaceGuidCName.PcdCName`

PCDs listed in architectural sections must not be listed in common
architectural sections. It is not possible for a PCD to be valid for only IA32
and also valid for any architecture.

A PCD may be valid for IA32 and X64 and invalid for EBC usage, so
mixing of specific architectural modifiers is permitted.

This section defines how a module has been coded to access a PCD. A PCD can
only be accessed using the function defined by the UEFI specification for a
single type, therefore, mixing PCD section types is not permitted.

There are five defined PCD types. Do not confuse these types with the data
types of the PCDs. The five types are: `FeaturePcd` (in code, identified as
`FEATURE_FLAG`), `FixedPcd`(`FIXED_AT_BUILD`), `PatchPcd`(`PATCHABLE_IN_MODULE`)
and two dynamic types of PCDs, `Pcd`(`DYNAMIC`) and `PcdEx`(`DYNAMIC_EX`).

The two recommended types that are commonly used in modules are: Feature PCD
and the dynamic PCD form. The PCD is used for configuration when the PCD value
is produced and consumed by drivers during execution, the value may be user
configurable from setup or the value is produced by the platform in a specified
area. It is associated with modules that are released in source code. The
dynamic form is the most flexible method, as platform integrators may chose a
to use a different type (such as fixed) for a given platform without modifying
the module's INF file or the code for the module. For modules that will be
distributed as binaries, the `PatchPcd` and `PcdEx` are the only supported
types.

The `FeaturePcd` is used to enable some code paths; the EDK II build system
will generate a `const` statement for these PCDs.

Similar in function, the dynamic `PcdEx` type can be used with modules that are
released as binary. However, the access methods for this style prevents using
these PCDs as any other PCD type (source code must change in order for a
`PcdEx` to be used as a `FixedPcd`).

The `FixedPcd` and `PatchPcd` are static and only the `PatchPcd` can have the
value changed in a binary prior to including the module in a firmware image.

The content of this section is the PCD Token Space Guid C Name, followed by a
period "." character and then the C name of the PCD. The default value is
optional. (See chapter 3, _Module Information (INF) Format Specification_,
later in this document for definition of the content.) Every PCD (`PcdName`) is
identified by two parts, the PCD's Token Space Guid C Name and the PCD's C Name.
These two items are separated by a period "." character. This section uses
one of the following section definitions:

```ini
[(PcdType)]
[(PcdType).common]
[(PcdType).IA32]
[(PcdType).X64]
[(PcdType).EBC]
```

The required entries for this section are the PCD Token Space Guid C Name's for
the PCD that will be looked up by tools from the DEC files, and the PCD's C
name - that must be specified in the DEC files to limit accidental duplicate
PCD C Name collisions. A default value that the module developer suggests to
use for the PCD is optional.

`TokenSpaceGuidCName.PcdCName`

Values of PCDs defined in this file override the default values specified in
the EDK II package declaration (DEC) file. The platform integrator can specify
values in the DSC and FDF files or on the build command line to override any
settings in this file. If a default value is not specified, the build system
uses 1) values from the command line, 2) values from the FDF file, 3) values
from the DSC file or 4) values from the DEC file.

Expressions, or Feature Flag Expressions, may be used on PCD entry lines.

If there are files listed in a `[Binaries]` section and this is a `PatchPcd`
section, and the third field of an entry is a Hex number, `0x00000012`, then
the value is an offset into a binary image. The format for this type of
entry is:

`PcdName | Value | HexValue`

For all other instances, the format for this type of entry is:

`PcdName | [Value] [ | FeatureFlagExpression]`

When a `FeatureFlagExpression` is present, if the expression evaluates to
`TRUE`, then the PCD entry is valid. If the expression evaluates to `FALSE`,
then the EDK II build tools must ignore the entry.

### 2.14.1 FIXED_AT_BUILD

The content for the PCD entry is the PCD's Name (PCD's Token Space Guid C name,
followed by a period "." character then the PCD's C name) and an optional
Default value. These fields are separated by the pipe "|" character. If a
module is coded for only `FIXED_AT_BUILD` PCDs, it can only be used during a
build from source files. This section must not be present in an INF file that
describes a binary only module. This section uses one of the following section
definitions:

```ini
[FixedPcd]
[FixedPcd.common]
[FixedPcd.IA32]
[FixedPcd.X64]
[FixedPcd.EBC]
```

The following is an example of the `PCD FIXED_AT_BUILD` type:

```ini
[FixedPcd.common]
  gEfiMdePkgTokenSpaceGuid.PcdFSBClock|600000000
  gEfiEdkModulePkgTokenSpaceGuid.PcdMaxSizeNonPopulateCapsule
  gEfiEdkModulePkgTokenSpaceGuid.PcdMaxSizePopulateCapsule
```

### 2.14.2 PATCHABLE_IN_MODULE

The PCD entry content is the PCD's Name (PCD's Token Space Guid C name,
followed by a period "." and the PCD's C name) and an optional Default value.
This section may be present in INF files that describe a binary only module.
This type of PCD is one of the recommended formats for modules that will be
distributed in binary format. These fields are separated by the pipe "|"
character. This section uses one of the following section definitions:

```ini
[PatchPcd]
[PatchPcd.IA32]
[PatchPcd.X64]
[PatchPcd.EBC]
[PatchPcd.common]
```

The following is an example of the `PCD FIXED_AT_BUILD` type:

```ini
[PatchPcd.common]
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageVariableSize
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageVariableBase
```

### 2.14.3 FEATURE_FLAG

The content for the PCD entry is the PCD's Name (PCD's Token Space Guid C name,
followed by a period "." and the PCD's C name) and an optional Default value of
either `TRUE` or `FALSE`, `1` or `0`. These fields are separated by the period
"|" character. This section must not be present in INF files that describe a
binary only module. This section uses one of the following section definitions:

```ini
[FeaturePcd]
[FeaturePcd.common]
[FeaturePcd.IA32]
[FeaturePcd.X64]
[FeaturePcd.EBC]
```

The following is an example of the PCD `FEATURE_FLAG` type:

```ini
[FeaturePcd.common]
  gEfiEdkModulePkgTokenSpaceGuid.PcdDxeIplSupportCustomDecompress
  gEfiEdkModulePkgTokenSpaceGuid.PcdDxeIplSupportTianoDecompress
  gEfiEdkModulePkgTokenSpaceGuid.PcdDxeIplSupportEfiDecompress
  gEfiEdkModulePkgTokenSpaceGuid.PcdDxeIplBuildShareCodeHobs
```

### 2.14.4 DYNAMIC

The content for the PCD entry is the PCD's Name (PCD's Token Space Guid C name,
followed by a period "." and the PCD's C name) and an optional Default value.
These entries are separated by the pipe "|" character. While this section is
the recommended method for coding PCD access methods, it must not be present in
INF files that describe a binary only module. This section uses one of the
following section definitions:

```ini
[Pcd]
[Pcd.common]
[Pcd.IA32]
[Pcd.X64]
[Pcd.EBC]
```

The following is an example of the PCD `DYNAMIC` type:

```ini
[Pcd.common]
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageVariableSize
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageVariableBase
```

### 2.14.5 DYNAMIC_EX

The content for the PCD entry is the PCD's Name (PCD's Token Space Guid C name,
followed by a period "." and the PCD's C name) and an optional Default value.
These entries are separated by the pipe "|" character. This section may be
present in INF files that describe a binary only module. This type of PCD is
one of the recommended formats for modules that will be distributed in binary
format. This section uses one of the following section definitions:

```ini
[PcdEx]
[PcdEx.common]
[PcdEx.IA32]
[PcdEx.X64]
[PcdEx.EBC]
```

The following is an example of the PCD `DYNAMIC_EX` type:

```ini
[PcdEx.common]
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageFtwWorkingSize
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageFtwWorkingBase
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageFtwSpareSize
  gEfiGenericPlatformTokenSpaceGuid.PcdFlashNvStorageFtwSpareBase
```

**********
**Note:** For binary (.efi) modules, only `PATCHABLE_IN_MODULE` or
`DYNAMIC_EX` PCDs may be specified. `FixedPcd`, `DYNAMIC` and
`FeaturePcd` sections are not permitted for binary distribution of modules.
**********

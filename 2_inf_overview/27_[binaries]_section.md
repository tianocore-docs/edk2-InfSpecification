<!--- @file
  2.7 [Binaries] Section

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

## 2.7 [Binaries] Section

The `[Binaries]` section is used to specify the binary files that are
distributed as part of a Binary Module. The binary files listed are not used by
the `$(MAKE)` portion of a platform build, but are used by other tools to
generate an image suitable for either an Application, FD or FV. A pipe
character "|" is used to separate the fields. If the file is in a
sub-directory, then the relative (to the INF file) path must be included as
part of the file name. The first field is the FileType, which will let a
platform integrator know the provided file's format, while the last three
fields are optional. (Three defined targets, NOOPT, DEBUG and RELEASE are
provided as part of the EDK II build environment.) The wildcard character,
"*", is permitted in the fields.

Additional information, such as what flags were used during the build, can also
be added in the comments preceding an entry or in an in-line comment that
follows the entry.

Files listed in this section do not require generation of AutoGen or Makefiles
during the pre-processing build steps.

It is prohibited to list a file in the "common" architectural section and
also in a specific architectural section. Binary files can be common to all
architectures or specific to individual architectures, not both. The
architectural section modifier is used as a restriction to mask binaries from
target architectures that are not applicable. During a build, the tools will
group binaries in listed in the common sections with the binaries listed for
the architecture needed by the build.

This section uses one of the following section definitions:

```ini
[Binaries]
[Binaries.common]
[Binaries.IA32]
[Binaries.X64]
[Binaries.IPF]
[Binaries.EBC]
```

The formats for entries in this section are:

```
FileType|Relative/path/and/filename.ext|DEBUG|GCC|UNIXGCC|TRUE
FileType|Filename.ext|*|GCC
FIleType|Relative/path/and/filename.ext|RELEASE
FileType|Filename.ext|RELEASE
FileType|Filename.ext
```

The `FileType` falls into one of the following PI-defined types:

**_GUID_**

This binary is an `EFI_SECTION_GUID_DEFINED` encapsulation section. The EDK II
build system does not support binary files of this type.

**_ACPI_**

The binary is ACPI binary code generated from an ACPI compiler. There is not PI
defined type for this file, it uses an `EFI_SECTION_RAW` leaf section.

**_ASL_**

The binary is an ACPI Table generated from an ACPI compiler. There is no PI
defined type for this file, it uses an `EFI_SECTION_RAW` leaf section.

**_DISPOSABLE_**

Unlike other file types listed in this section, the file will not be placed in
a leaf section of type `EFI_SECTION_DISPOSABLE`, but rather it is a binary file
that will be ignored by the build tools. (Useful for distributing PDB files
with binary modules.)

**_UEFI_APP_**

The binary file is a PE32 UEFI Application which will be placed into an FFS
file of type `EFI_FV_FILETYPE_APPLICATION`.

**_PE32_**

This binary is an `EFI_SECTION_PE32` leaf section.

**_PIC_**

This binary is an `EFI_SECTION_PIC` leaf section.

**_PEI_DEPEX_**

This binary is an `EFI_SECTION_PEI_DEPEX` leaf section.

**_DXE_DEPEX_**

This binary is an `EFI_SECTION_DXE_DEPEX` leaf section.

**_SMM_DEPEX_**

This binary is an `EFI_SECTION_SMM_DEPEX` leaf section.

**_SUBTYPE_GUID_**

This binary is an `EFI_SECTION_FREEFORM_SUBTYPE_GUID` leaf section.

**_TE_**

This binary is an `EFI_SECTION_TE` leaf section.

**_UNI_VER_**

This is a Unicode file that needs to be used to create an `EFI_SECTION_VERSION`
leaf section.

**_VER_**

This binary is an `EFI_SECTION_VERSION` leaf section.

**_UNI_UI_**

This is a Unicode file that needs to be used to create an
`EFI_SECTION_USER_INTERFACE` leaf section.

**_UI_**

This binary is an `EFI_SECTION_USER_INTERFACE` leaf section.

**_BIN_**

This binary is an `EFI_SECTION_RAW` leaf section.

**_RAW_**

This binary is an `EFI_FV_FILETYPE_RAW` leaf section.

**_COMPAT16_**

This binary is an `EFI_SECTION_COMPATIBILTY16 leaf` section.

**_FV_**

This binary is an `EFI_SECTION_FIRMWARE_VOLUME_IMAGE` leaf section.

**********
**Note:** The section names listed above refer to leaf section type values
rather than the name of the data structure.
**********

The following are examples of different types of `[Binaries]` sections.

```ini
[Binaries.common]
  UNI_UI|DxeIpl.ui
  UNI_VER|DxeLoad.ver

[Binaries.IA32]
  DXE_DEPEX|Ia32/DxeIpl.dpx             # MYTOOLS
  PE32|Ia32/DEBUG/DxeIpl.efi|DEBUG      # MYTOOLS
  PE32|Ia32/RELEASE/DxeIpl.efi|RELEASE  # MYTOOLS DISPOSABLE|Ia32/DEBUG/DxeIpl.pdb

[Binaries.X64]
  DXE_DEPEX|X64/DxeIpl.dpx              # MYTOOLS
  PE32|X64/DxeIpl.efi                   # MYTOOLS

[Binaries.IPF]
  DXE_DEPEX|IPF/DxeIpl.dpx              # MYTOOLS
  PE32|Ipf/DxeIpl.efi                   # MYTOOLS
```

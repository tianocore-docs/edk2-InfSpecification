<!--- @file
  2.6 [BuildOptions] Section

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

## 2.6 [BuildOptions] Section

Content in the `[BuildOptions]` section defines module specific tool chain
flags that must be used as the default flags for a module. These flags are
appended to any standard flags that are defined by the build process. In order
to replace the standard flags that are defined by the build process, an
alternate assignment operator must be used; "==" is used for replacement,
while "=" is used to append the flag lines. Flags specified in this section
can either be appended to the standard flags (defined in the
`Conf/tools_def.txt`) or replace the standard flags. In addition to flags,
other tool attributes may have the item either appended or replaced.

The left side content of a statement may appear in both common and
architectural sections. For example, `MSFT:DEBUG_*_*_CC_FLAGS` may be listed in
a common section, while `MSFT:DEBUG_*_IA32_CC_FLAGS` may be listed in the
architectural section. If the operator is a single "=" character, the flags
from the architectural section are appended to the flags from the common
section. Using this section may limit the ability of a module to be compiled
with different tool chains or with different build systems and is therefore,
discouraged.

Valid content is within this section is limited to the following description.

###### Table 2 EDK II [BuildOptions] Section Elements

| Tag                                                                  | Value                                         | Notes                                                                                            |
| -------------------------------------------------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `${FAMILY}:${TARGET}_${TAGNAME}_` `${ARCH}_${TOOLCODE}_FLAGS`        | Flags for specific tool codes for this module | Used to specify module specific flags of the module that will use the driver.                    |
| `${FAMILY}:${TARGET}_${TAGNAME}_` `${ARCH}_${TOOLCODE}_PATH`         | The fully qualified path an executable        | Used to replace a specific command, such as forcing the ASL to be iasl, instead of asl.          |
| `${FAMILY}:${TARGET}_${TAGNAME}_` `${ARCH}_${TOOLCODE}_DPATH`        | A fully qualified path                        | A path that will be added to the system Environment's PATH variable prior to executing a command |
| `${FAMILY}:${TARGET}_${TAGNAME}_` `${ARCH}_${TOOLCODE}_${ATTRIBUTE}` | Attribute specific string                     | This permits overriding other attributes if required.                                            |

In this section, the following table describes each of the variables that are
shown above.

###### Table 3 EDK II [BuildOptions] Variable Descriptions

| Variable    | Required   | Wildcard   | Source                                                                                                                                                                                                                                      |
|:-----------:|:----------:|:----------:| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `FAMILY`    | NO         | No         | `Conf/tools_def.txt` defines the `FAMILY` values, for example: `MSFT`, `INTEL` or `GCC`. Typically, this field is used to help the build tools determine whether the line is used for Microsoft style Makefiles or the GNU style Makefiles. |
|             |            |            | By not specifying the `FAMILY`, the tools assume the flags are applicable to all families.                                                                                                                                                  |
| `TARGET`    | YES        | Yes = *    | `Conf/tools_def.txt` file defines two values: `DEBUG` and `RELEASE`. Developers may define additional targets.                                                                                                                              |
| `TAGNAME`   | YES        | Yes = *    | `Conf/tools_def.txt` file defines several different tag names - these are defined by developers; the default tag name, `MYTOOLS`, is provided in the template for tools_def.txt and set in the `Conf/target.txt` file.                      |
| `ARCH`      | YES        | Yes = *    | `Conf/tools_def.txt` defines at least four architectures: `IA32`, `X64` and `EBC`. This tag must use all capital letters for the tag. Additional Architectures, such as PPC or ARM may be added as support becomes available.        |
| `TOOLCODE`  | YES        | NO         | The tool code must be one of the defined tool codes in the `Conf/tools_def.txt` file. The flags defined in this section are appended to flags defined in the `tools_def.txt` file for individual tools.                                     |
|             |            |            | EXCEPTION: If the INF `MODULE_TYPE`, defined in the `[Defines]` section is `USER_DEFINED` or `HOST_APPLICATION`, then the flags listed in this section are the only flags used for the TOOLCODE command specified in `Conf/ tools_def.txt`.                       |
| `ATTRIBUTE` | YES        | NO         | The attribute must be specific to the tool code and must be a valid attribute handled by the build system.                                                                                                                                  |

Developers should use extreme caution when specifying items in this section.
The EDK II build is designed to support multiple compilers and tool chains,
expecting that code is written in ANSI C. If custom tool flags are required by
a module, developers must make sure that all consumers of the module are aware
of the specific tools and tag names required.

**********
**Note:** The lines are shown with the backslash "\" character to indicate
a line continuation, they are not allowed in the actual INF file.
**********

```ini
[BuildOptions.common]
  MSFT:DEBUG_*_IA32_DLINK_FLAGS = /out:"$(BIN_DIR)SecMain.exe"/base:0x10000000 /pdb:"$(BIN_DIR)SecMain.pdb"/LIBPATH:"$(VCINSTALLDIR)Lib"/LIBPATH:"$(VCINSTALLDIR)PlatformSdkLib"/NOLOGO /SUBSYSTEM:CONSOLE /NODEFAULTLIB /IGNORE:4086/MAP /OPT:REF /DEBUG /MACHINE:I386 /LTCG Kernel32.lib MSVCRTD.lib Gdi32.lib User32.libWinmm.lib
  MSFT:DEBUG_*_IA32_CC_FLAGS    = /nologo /W4 /WX /Gy /c /D UNICODE/D EFI32 /Od /DSTRING_ARRAY_NAME=SecMainStrings /FI$(DEST_DIR_DEBUG)/AutoGen.h /EHs-c- /GF /Gs8192/Zi /Gm
```

For `[BuildOptions]` sections in the INF file, the entries with a common left
side (of the "=") will be either appended or replace previous entries based
on the "==" replace or "=" append assignment character sequence. Sections
with identical architecture modifiers are appended to each other.

`Common Section + Architectural Section`

#### Example:

```ini
[BuildOptions.Common]
  MSFT:*_*_*_CC_FLAGS = /nologo

[BuildOptions.Common]
  MSFT:*_*_*_CC_FLAGS = /Od

[BuildOptions.IA32]
  MSFT:*_*_IA32_CC_FLAGS = /D EFI32
```

For IA32 architecture builds of an EDK II INF file would logically be:

`MSFT:*_*_IA32_CC_FLAGS = /nologo /Od /D EFI32`

For X64 architecture builds of an EDK II INF file would logically be:

`MSFT:*_*_IA32_CC_FLAGS = /nologo /Od`

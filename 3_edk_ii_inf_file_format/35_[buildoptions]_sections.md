<!--- @file
  3.5 [BuildOptions] Sections

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

## 3.5 [BuildOptions] Sections

These sections are optional for EDK II INF files.

#### Summary

Defines the `[BuildOptions]` section content. These sections are used to define
the custom definitions for individual tools. There are two styles for options,
a replacement of any previous definition (for this module only) of the flags or
commands used (specified by the double "==" equal sign) to process the module
code, or an append option, (specified by the single "=" equal sign) which
will be appended to the previous definition. When the single "=" equal sign
is used, the string to the left, is appended, typically used to override a
single flag. When the double "==" sign is used, then any previous definition
(reference build requires that it must be defined `tools_def.txt` file) will be
cleared, and the left value replaces the entire previous definition.

The double "==" equal sign must be used to replace a command (specified by
the PATH attribute,) appending is not an option.

For the DPATH attribute, can use either the replace or append, and is used to
expand the system environment `PATH` variable prior to processing any commands.

Other, user defined attributes, can be specified, in this section, using either
the append or replace. Usage of these overrides is implementation specific.

Macro use is permitted in the `[BuildOptions]` sections, and must follow the
rule that Macros used in this section must be defined locally within the INF
file; use of externally defined MACROs is prohibited. Additionally, a
`$(MACRO)` that appears inside of a quoted string in R-Values (following an
append "=" or replace "==") is permitted, as parsing tools are not required
to expand those macro values. Macros within quoted strings do not need to be
defined locally.

Macro statements, `$(MACRO)`, that are encapsulated in double quotation marks
are not expanded, nor are they processed by parsing tools, as double quotation
marks indicate a string that must be treated as a single entity. Macro
statements in comments must also be ignored by parsing tools.

Macros are not allowed on the left side of the assignment statement (left of
the equal sign).

The EDK II build system will provide an option to create an "As Built" INF file
that can be used for binary distributions. This section will be completed,
listing all of the option flags for every application that was used to create
the binary. Since these "As Built" flags are within comment sections, the
actual flag string can be extended to a new comment line without using the line
extension character.

Tools that create "As Built" information must expand any macro values used by
the tools during the module build. The standard Macro Definitions are not
permitted within this section for an "As Built" INF file.

Build options listed in architectural sections will be appended to build
options listed in the common architectural section.

Comments in this section must appear on a separate line, they may not be
appended after statements.

The "common" architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

#### Prototype

```c
<BuildOptions>  ::= "[BuildOptions" [<com_attrs>] "]" <EOL> <BuildStmts>
<com_attrs>     ::= {".common"} {<attrs>}
<attrs>         ::= <Archs> ["," <TS> "BuildOptions" <attrs>]*
<Archs>         ::= "." <arch>
<BuildStmts>    ::= {<BuildOptStmts>*} {<AsBuiltStmts>}
<AsBuiltStmts>  ::= <TS> "##" <TS> "@AsBuilt" <EOL> <AsBuiltFfe>*
<AsBuiltFfe>    ::= <TS> "##" <FamId> <ToolFlags> <Eq> <CFlags> <EOL> [<TS> "#"
                    <CFlagsContd> <EOL>]*
<CFlagsContd>   ::= <CFlags>
<BuildOptStmts> ::= {<FlagExpr>} {<PathExpr>} {<CmdExpr>} {<Other>}
<FamId>         ::= <Family> ":"
<FlagExpr>      ::= <TS> [<FamId>] <ToolFlags> <Equal> <CFlags> <EOL>
<PathExpr>      ::= <TS> [<FamId>] <ToolPath> <Equal> <PATH> <EOL>
<CmdExpr>       ::= <TS> [<FamId>] <ToolCmd> <ReplaceEq> <PathCmd> <EOL>
<Other>         ::= {<OtherTool>} {<MacroDefinition>}
<OtherTool>     ::= <TS> [<FamId>] <ToolOther> <Equal> <String> <EOL>
<Family>        ::= {"MSFT"} {"GCC"} {"INTEL"} {<Usr>} {<Wildcard>}
<Usr>           ::= <ToolWord>
<Equal>         ::= {<AppendEq>} {<ReplaceEq>}
<AppendEq>      ::= <Eq>
<ReplaceEq>     ::= <TS> "==" <TS>
<ToolSpec>      ::= <Target> "_" <TagName> "_" <tarch> "_" <CmdCode>
<ToolFlags>     ::= <ToolSpec> "_FLAGS"
<ToolPath>      ::= <ToolSpec> "_DPATH"
<ToolCmd>       ::= <ToolSpec> "_PATH"
<ToolOther>     ::= <ToolSpec> "_" <Attribute>
<Target>        ::= {<Wildcard>} {Target}
<TagName>       ::= {<Wildcard>} {TagName}
<CmdCode>       ::= CommandCode
<CommandName>   ::= CommandExecutable
<Attribute>     ::= Attribute
<tarch>         ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {<OA>}
                    {<Wildcard>}
<CFlags>        ::= (0x20 - 0x7e)+
<PathCmd>       ::= <TOOLPATH> <FileSep> <CommandName>
<TOOLPATH>      ::= {<PATH>} {<ABS_PATH> <PATH>}
<ABS_PATH>      ::= {(a-zA-Z) ":\"} {"\"} {"/"}
```

#### Parameters

All of the keywords that make up the left side of the expression must be
alphanumeric only - no special characters are permitted. For more information
about the following parameters, refer to the _Build Specification_ for as
description of the `tools_def.txt` file. In order for the entries in the INF
file to be valid, there must be a matching definition in the `tools_def.txt`
file. The tool chain tag name must also match the one used for the build.

**_Family_**

Must match a `FAMILY` name defined in the EDK II `tools_def.txt` file. If not
present, then the entry is valid for all tool chain families.

**_TOOLPATH_**

Paths listed in the `[BuildOptions]` section are relative to the system
environment variable, `EDK_TOOLS_PATH`, or they may be absolute. Use of "..",
"." and "../" in the directory path is not permitted. If the absolute format is
used, the module cannot be distributed using a UEFI Distribution Package.

**_Target_**

A keyword that uniquely identifies the build target; the first field, where
fields are separated by the underscore character. Three values, "NOOPT",
`"DEBUG`" and `"RELEASE`" have been pre-defined. This keyword is used to bind
command flags to individual commands.

Users may want to add other definitions, such as, PERF, SIZE or SPEED, and
define their own set of FLAGS to use with these tags. The wildcard character,
"``*", is permitted after it has been defined one time for a tool chain.

**_TagName_**

A keyword that uniquely identifies a tool chain group; the second field.
Wildcard characters are permitted if and only if a command is common to all
tools that will be used by a developer. As an example, if the development team
only uses IA32 Windows workstations, the ACPI compiler can be specified as:
`DEBUG_*_*_ASL_PATH` and `RELEASE_*_*_ASL_PATH`.

**_Arch_**

A keyword that uniquely identifies the tool chain target architecture; the
third field. This flag is used to support the cross-compiler features, such as
when a development platform is IA32 and the target platform is X64 Using this
field, a single `TagName` can be setup to support building multiple target
platform architectures with different tool chains. As an example, if a
developer is using Visual Studio .NET 2003 for generating IA32 platform and
uses the WINDDK version 3790.1830 for X64 or IPF platform images, a single tag
(see the MYTOOLS PATH settings in the generated `Conf/tools_def.txt` or
provided `BaseTools/Conf/tools_def.template` file.) The wildcard character,
"``*", is permitted if and only if the same tool is used for all target
architectures.

**_CommandExecutable_**

The full executable name, such as `cl.exe` or `gcc`, with the preceding path
specifying the exact location of the command. If the executable can be located
under a directory specified in the system environment `PATH` variable, only the
filename is required. Otherwise, a relative path to a path listed in the system
environment PATH variable or an absolute path must be given. If an absolute
path is used, the build system will fail the build if the executable cannot be
found.

**_CommandCode_**

A keyword that uniquely identifies a specific command; the fourth field.
Several `CommandCode` keywords have been predefined. See table below for the
pre-defined keywords and functional mappings. The wildcard character, "*", is
permitted only for the `FAMILY`, `DLL` and `DPATH` attributes (see
**_Attributes_** below.)

###### Table 5 Predefined Command Codes

| CommandCode | Function                                                                                                                                                              |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `APP`       | C compiler for applications.                                                                                                                                          |
| `ASL`       | ACPI Compiler for generating ACPI tables.                                                                                                                             |
| `ASLCC`     | ACPI Table C compiler                                                                                                                                                 |
| `ASLDLINK`  | ACPI Table C Dynamic linker                                                                                                                                           |
| `ASLPP`     | ASL C pre-processor                                                                                                                                                   |
| `ASM`       | A Macro Assembler for assembly code in some libraries.                                                                                                                |
| `ASMLINK`   | The Linker to use for assembly code generated by the ASM tool.                                                                                                        |
| `CC`        | C compiler for PE32/PE32+/Coff images.                                                                                                                                |
| `DLINK`     | The C dynamic linker.                                                                                                                                                 |
| `MAKE`      | Required for tool chains. This identifies the utility used to process the Makefiles generated by the first phase of the build.                                        |
| `PCH`       | The compiler for generating pre-compiled headers.                                                                                                                     |
| `PP`        | The C pre-processor command.                                                                                                                                          |
| `SLINK`     | The C static linker.                                                                                                                                                  |
| `TIANO`     | This special keyword identifies a compression tool used to generate compression sections as well as the library needed to uncompress an image in the firmware volume. |
| `VFR`       | The VFR file compiler which creates IFR code.                                                                                                                         |
| `VFRPP`     | The C pre-processor used to process VFR files.                                                                                                                        |

**_Attribute_**

A keyword to uniquely identify a property of the command; the fifth and last
field. Several pre-defined attributes have been defined: `DLL`, `FAMILY`,
`FLAGS`, `GUID`, `OUTPUT` and `PATH`. Use quotation marks if and only if the
quotation marks must be included in the flag string. The following example shows
the format for the required quoted string,
`"C:\Program Files\Intel\EBC\Lib\EbcLib.lib"`. Normally, the quotation
characters are not required as everything following the equal sign to the end of
the line is used for the flag.

**_Flags_**

Must be a valid string for the tool specified. The string will be appended to
the end of the tool's flags (from the `tools_def.txt`.) Both Microsoft and GCC
evaluate options from left to right on the command line. This allows disabling
some flags that may have been specified in the `tools_def.txt` by providing an
alternate flag, i.e., if the tools_def: CC FLAGS defines /O2 and an /O1 options
is specified for this module, the module will compile with /O1 (size) not with
/O2 (speed.)

Space characters are allowed. Macros are also permitted to be used in Flag
strings.

#### Example

```ini
[BuildOptions.common]
  DEFINE MACRO                  = /nologo
  *_WINDDK3790x1830_*_CC_FLAGS  = /Qwd1418,810
  *_MYTOOLS_*_CC_FLAGS          = /Qwd1418,810
  *_VS2003_*_CC_FLAGS           = /wd4244
  *_WINDDK3790x1830_*_CC_FLAGS  = /wd4244
  *_MYTOOLS_*_CC_FLAGS          = /wd4244
  RELEASE_MYTOOLS_IPF_ASM_FLAGS = = -N us -X explicit -M ilp64 - N so -W3 MSFT:*_*_*_*_FLAGS = /od $(MACRO)
```

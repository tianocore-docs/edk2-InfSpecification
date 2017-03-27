<!--- @file
  3.9 [Sources] Sections

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

## 3.9 [Sources] Sections

These sections are optional.

#### Summary

Defines the `[Sources]` section content.

All file names specified in this section must be in the directory containing
the INF file or in sub-directories of the directory containing the INF file.

The '`common`' architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

All paths are relative to the directory containing the INF file. If the
filename is listed as myfile.c, the file must be located in the same directory
as the INF file. Absolute paths in the filename are prohibited.

There can be multiple sources sections, depending on the target processor.
Example sources sections are listed below. The parsing utility creates a
directory path for each file (`$(DEST_DIR)...\MyFile.c`), and looks up the
makefile template for the `COMPONENT_TYPE` (EDK) or `MODULE_TYPE` (EDK II) to
emit.

It is not permissible to mix EDK and EDK II style files within a module.

The macro, `TABLE_NAME` may be used in existing EDK INF files that point to
ACPI tables, this value will be ignored by EDK II build tools.

All HII Unicode format files must be listed in this section as well as any
other "source" type file, such as local module header files, Vfr files, etc.

Each source file must be listed only once per section. Files listed in
architectural sections are not permitted to be listed in the common
architectural section.

This section is not valid for a generated "As Built" binary INF file.

```c
<Sources>            ::= "[Sources" [<com_attribs>]* "]" <EOL>
                         [<TS> "TABLE_NAME" <Eq> <SimpleWord> <EOL>]
                         <SourceFileStmts>*
<com_attribs>        ::= {".common"} {<attribs>}
<attribs>            ::= <attrs> ["," <TS> "Sources" <attrs>]*
<attrs>              ::= "." <arch>
<SourceFileStmts>    ::= {<MacroDefinition>} {<SourceFileEntry>}
<SourceFileEntry>    ::= <TS> <Filename> [<Options>] <EOL>
<Options>            ::= <FS> [<Family>] [<opt1>]
<opt1>               ::= <FS> [<TagName>] [<opt2>]
<opt2>               ::= <FS> [<ToolCode>] [<opt3>]
<opt3>               ::= <FS> [<FeatureFlagExpress>]
<Family>             ::= {"MSFT"} {"GCC"} {"INTEL"} {<Wildcard>}
<TagName>            ::= {<ToolWord>} {"*"}
<ToolCode>           ::= _CommandCode_
<FeatureFlagExpress> ::= <Boolean>
```

#### Parameters

**_Filename_**

Paths listed in the filename elements of the `[Sources]` section must be
relative to the directory the INF file resides in. Use of "..", "." and "../"
in the directory path is not permitted.

**_FeatureFlagExpress_**

When present, the feature flag expression determines whether the entry line is
valid. If the feature flag expression evaluates to FALSE, this entry will be
ignored by the EDK II build tools.

**_TagName_**

A keyword that uniquely identifies a tool chain group; the second field.
Wildcard characters are permitted if and only if a command is common to all
tools that will be used by a developer. As an example, if the development team
only uses IA32 Windows workstations, the ACPI compiler can be specified as:

`DEBUG_*_*_ASL_PATH` and `RELEASE_*_*_ASL_PATH`

**_CommandCode_**

A keyword that uniquely identifies a specific command; the fourth field. Several
CommandCode keywords have been predefined, however users may add additional
keywords, with appropriate modifications to build_rule.txt. See table below for
the pre-defined keywords and functional mappings. The wildcard character,
"*", is permitted only for the `FAMILY`, `DLL` and `DPATH` attributes (see
**_Attributes_** below.)

**_Family_**

`Family` is keyword that uniquely identifies a tool chain family. The `Family`
must be either a wildcard character (meaning any Family) or it must match a
defined value for a Family label in the `tools_def.txt` file for at least one
tool chain `TagName` specified in `tools_def.txt` (or the `TagName` field that
follows this field in the entry).

#### Example

```ini
[Sources.common]
  Diskio.c
  Diskio.h
  ComponentName.c

[Sources.IA32}
  Ia32DiskIo.h
```

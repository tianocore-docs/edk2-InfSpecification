<!--- @file
  2.2 Information File General Rules

  Copyright (c) 2007-2018, Intel Corporation. All rights reserved.<BR>

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

## 2.2 Information File General Rules

This section covers the format for the EDK II module INF files. While the EDK
code base and tools treated libraries completely separate from modules, the EDK
II code base and tools process modules, with libraries being considered a
module that produces a library class.

### 2.2.1 Section Entries

To simplify parsing, the EDK II meta-data files continue using the INI format.
This style was introduced for EDK meta-data files, when only the Windows tool
chains were supported. It was decided that for compatibility purposes, that INI
format would continue to be used. EDK II formats differ from the defacto format
in that the semicolon ";" character cannot be used to indicate a comment.

Leading and trailing space/tab characters must be ignored.

Duplicate section names must be merged by tools.

This description file consists of sections delineated by section tags enclosed
within square `[]` brackets. Section tag entries are case-insensitive. The
different sections and their usage are described below. The text of a given
section can be used for multiple section names by separating the section names
with a comma. For example:

`[Sources.X64, Sources.IPF]`

The content below each section heading is processed by the parsing utilities in
the order that they occur in the file. The precedence for processing these
architecture section tags is from right to left, with sections defining an
architecture having a higher precedence than a section which uses common (or no
architecture extension) as the architecture.

**********
**Note:** Content within a section IS case sensitive. IA32, Ia32 and ia32
within a section are processed as separate items. (Refer to Naming Conventions
below for more information on directory and/or file naming.)
**********

Sections are terminated by the start of another section or the end of the file.

Duplicate sections (two sections with identical section tags) will be merged by
tools, with the second section appended to the first.

If architectural modifiers are used in the section tag, the section is merged
by tools with content from common sections (if specified) with the
architectural section appended to the first, into an architectural section. For
example, given the following:

```ini
[Sources]
  ACommonFile.c

[Sources.IA32]
  BforIa32.c

[Sources.X64]
  CforX64.c
```

After the first pass of the tools, when building the module for IA32, the
source files will logically be:

```ini
[Sources.IA32]
  ACommonFile.c
  BforIa32.c
```

When building the module for X64, the source files will logically be:

```ini
[Sources.X64]
  ACommonFile.c
  CforX64.c
```

The `[Defines]` section tag prohibits use of architectural modifiers. All other
sections can specify architectural modifiers.

### 2.2.2 Comments

The hash `#` character indicates comments in the Module Information (INF) file.
In line comments terminate the processing of a line. In line comments must be
placed at the end of the line, and may not be placed within the section
(`[`,`]`) tags.

Only `gPkgTSGuid.PcdFoo|TRUE|BOOLEAN|0x00000015` in the following example is
processed by tools; the remainder of the line is ignored:

`gPkgTSGuid.PcdFoo|TRUE|BOOLEAN|0x00000015 # EFI_FOO_MEMORY`

**********
**Note:** Blank lines and lines that start with the hash # character must be
ignored by build tools.
**********

Hash characters appearing within a quoted string are permitted, with the string
being processed as a single entity. The following example must handle the
quoted string as single element by tools.

`UI = "# Copyright 2007, No Such, LTD. All rights reserved."`

Comments are terminated by the end of line.

### 2.2.3 Valid Entries

Processing of the line is terminated if a comment is encountered.

Processing of a line is terminated by the end of the line.

Items in quotation marks are treated as a single token and have the highest
precedence. All expressions must be written using in-fix notation (operators
are written between the operands). Parenthesis surrounding groups of operands
and operators must be used to determine the order in which operations are to be
performed. All other processing occurs from left to right.

In the following example, B - C is processed first, then result is added to A
followed by adding 2; finally 3 is added to the result.

(A + (B - C) + 2) + 3

In the next example, A + B is processed first, then C + D is processed and
finally the two results are added.

(A + B) + (C + D)

Space and tab characters are permitted around field separators.

### 2.2.4 Naming Conventions

The EDK II build infrastructure is supported under Microsoft* Windows*, Linux*
and MAC OS/X operating systems. All directory and file names must be treated as
case sensitive because of multiple environment support.

* The use of special characters in directory names and file names is restricted
  to the dash, underscore, and period characters, respectively "-", "_", and
  ".".

* Period characters must not be followed by another period character. File and
  Directory names must not start with "./", "." or "..".

* Space characters must never be used in the directory path specified by the
  system environment variable, `WORKSPACE`.

* Directory names and file names within the `WORKSPACE` directory tree must not
  contain space characters.

* Directory Names must only contain alphanumeric, dash, underscore and period
  characters (two sequential period characters, ".." are not permitted); it is
  recommended that the name start with an alpha character.

* All files (except those listed in the Packages sections) must reside in the
  directory containing the INF file or in sub-directories of the directory
  containing the INF file.

* Additionally, all EDK II directories that are architecturally dependent must
  use a name with only the first character capitalized. Ia32, Ipf, X64 and Ebc
  are valid architectural directory names. IA32, IPF and EBC are not acceptable
  directory names, and may cause build breaks. From a build tools perspective,
  IA32 is not equivalent to Ia32 or ia32.

* Absolute paths are not permitted in EDK II INF files. All paths specified are
  relative to an EDK II package directory (defined as a directory containing a
  DEC file) or relative to the directory containing the INF file.

The build tools must be able to process the tool definitions file:
`tools_def.txt` (describing the location and flags for compiler and user
defined tools), which may contain space characters in paths on Windows* systems.

**********
**Note:** The toolsdef.txt file and `[BuildOptions]` sections are the
places that permit the use of space characters in a directory path.
**********

The EDK II Coding Style specification covers naming conventions for use within
C Code files, and as well as specifying the rules for directory and file names.
This section is meant to highlight those rules as they apply to the content of
the INF files.

Architecture keywords (`IA32`, `IPF`, `X64` and `EBC`) are used by build tools
and in metadata files for describing alternate threads for processing of files.
These keywords must not be used for describing directory paths. Additionally,
directory names with architectural names (Ia32, Ipf, X64 and Ebc) do not
automatically cause the build tools or meta-data files to follow these
alternate paths. Directories and Architectural Keywords are similar in name
only.

All directory paths within EDK II INF files must use the forward slash "/"
character to separate directories as well as directories from filenames.
Example:

`C:/Work/Edk2/edksetup.bat`

File names must also follow the same naming convention required for
directories. No space characters are permitted. The special characters
permitted in directory names are the only special characters permitted in file
names.

### 2.2.5 !include Statements

The `!include` statement are NOT permitted in the INF files.

### 2.2.6 Macro Statements

Use of MACRO statements in the EDK II INF files is limited to local usage only;
global or external macros are not permitted. This decision was made in order to
support UEFI's PI Distribution Package Specification requirements.

Macro statements are permitted in the EDK II INF files. Macro statements assign
a Value to a Variable Name, and are only valid during the processing of the INF
specifying the value. If a value is not specified, then the MACRO has a value
of zero.

Token names (reserved words defined in the EDK II meta-data file
specifications) cannot be used as macro names. As an example, using
PLATFORM_NAME as a macro name is not permitted, as it is a token defined in the
DSC file's `[Defines]` section.

Any defined MACRO definitions will be expanded by tools when they encounter the
entry in the section except when the macro is within double quotation marks in
build options sections. The expectation is that these macros will be expanded
by scripting tools such as make or nmake.

Macros can be used to define a path, a filename, any combination of path and
file names or content that will appear in the right side of a statement in the
`[BuildOptions]` section. Macros for paths and files can be defined and used in
`[Defines]`, `[LibraryClasses]`, `[Sources]`, `[Binaries]`, and `[Packages]`
sections.

Macro Definition statements that appear within a section of the file (other
than the `[Defines]` section) are scoped to the section they are defined in. If
the Macro statement is within the `[Defines]` section, then the Macro is common
to the entire file, with local definitions taking precedence (if the same MACRO
name is redefined in subsequent sections, then the MACRO value is local to only
that section.)

In the following example, the MACRO, IFMP is used to fit a long
directory/filename pair on to a single line.:

`DEFINE IFMP = IntelFrameworkModulePackage`

Using the macro, for example, in a `[Packages]` section, looks like:

`$(IFMP)/IntelFrameworkModulePackage.dec`

Macros are evaluated where they are used in statements, not where they are
defined. It is recommended that tools break the build and report an error if an
expression cannot be evaluated.

Macros used in build flags (in `[BuildOptions]` sections) that are encapsulated
by quotation marks are not expanded by tools, and do not need to be local to
the INF file. The expectation is that macros in the quoted values will be
expanded by external build scripting tools, such as `nmake` or `gmake`; they
will not be expanded by the build tools.

The macro statements are positional, in that only statements following a macro
definition are permitted to use the macro - a macro cannot be used before it
has been defined.

Macros defined in common sections may be used in the architecturally modified
sections of the same section type. Macros defined in architectural sections
cannot be used in other architectural sections, nor can they be used in the
common section. Section modifiers in addition to the architectural modifier
follow the same rules as architectural modifiers.

Within the EDK II INF File, macros are expanded (except within quotes), not
evaluated, during the parsing of the file.

#### Example

```ini
[LibraryClasses.common]
  DEFINE MDE = MdePkg/Library
  BaseLib|$(MDE)/BaseLib.inf

[LibraryClasses.X64, LibraryClasses.IA32]
  # Can use $(MDE), cannot use $(MDEMEM)
  DEFINE PERF = PerformancePkg/Library
  TimerLib|$(PERF)/DxeTscTimerLib/DxeTscTimerLib.inf

[LibraryClasses.X64.PEIM]
  # Can use $(MDE) and $(PERF)
  DEFINE MDEMEM = $(MDE)/PeiMemoryAllocationLib
  MemoryAllocationLib|$(MDEMEM)/PeiMemoryAllocationLib.inf

[LibraryClasses.IPF]
  # Cannot use $(PERF) or $(MDEMEM)
  # Can use $(MDE) from the common section
  PalLib|$(MDE)/UefiPalLib/UefiPalLib.inf
  TimerLib|$(MDE)/BaseTimerLibNullTemplate/BaseTimerLibNullTemplate.inf
```

In the previous example, the directory and filename for a library instance is
the recommended instance and may not be the actual library linked to the
module, as the platform integrator may choose a different library instance to
satisfy a library class dependency.

### 2.2.7 Conditional Directive Statements (!if...)

Conditional statements are NOT permitted in the EDK II INF files.

### 2.2.8 !error Statement

The `!error` statement is NOT permitted in the EDK II INF files.

### 2.2.9 Expressions

Expressions are supported in specific statements within the EDK II INF files.
The expression syntax is defined in the _EDK II Expression Syntax
Specification_.

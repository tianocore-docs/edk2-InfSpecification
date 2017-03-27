<!--- @file
  3.3 Header Section

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

## 3.3 Header Section

This is an optional section, while this header section is not needed by the EDK
II build system, it will be used by the build tools for generating "As Built"
INF files from sources.

This section is also used by the Intel(R) UEFI Packaging Tool that is included
with the EDK II build system binaries.

#### Summary

This section contains Copyright and License notices for the INF file in
comments that start the file. This section is optional using a format of:

```ini
## @file
# Abstract
#
# Description
#
# Copyright
#
# License
#
##
```

This information can be derived from an XML Distribution Package file or is
created by a module developer creating a new module information (INF) file.

This is an optional section.

#### Prototype

```c
<Header>            ::= <SourceHeader>
                        [<BinaryHeader>]
<SourceHeader>      ::= <Comment>*
                        "##" [<Space>] <Space> "@file"
                        [<TS> <Filename>] <EOL>
                        [<Abstract>]
                        [<Description>]
                        <Copyright>*
                        "#" <EOL>
                        <License>*
                        "##" <EOL>+
<Abstract>          ::= "#" <MTS> <AsciiString> <EOL> ["#" <EOL>]
<Description>       ::= ["#" <MTS> <AsciiString> <EOL>]+ ["#" <EOL>]
<Copyright>         ::= "#" <MTS> <CopyName> <Date> "," <CompInfo>
<CopyName>          ::= ["Portions" <MTS>] "Copyright (c)" <MTS>
<Date>              ::= <Year> [<TS> {<DateList>} {<DateRange>}]
<Year>              ::= "2" (0-9)(0-9)(0-9)
<DateList>          ::= <CommaSpace> <Year> [<CommaSpace> <Year>]*
<DateRange>         ::= "-" <TS> <Year>
<CompInfo>          ::= (0x20 - 0x7e)* <MTS> "All rights reserved."
                        [<TS> "<BR>"] <EOL>
<License>           ::= ["#" <MTS> <AsciiString> <EOL>]+
                        ["#" <EOL>]
<BinaryHeader>      ::= <Comment>*
                        "##" [<Space>] <Space> "@BinaryHeader" <EOL>
                        <BinaryAbstract>
                        "#" <EOL>
                        <BinaryDescription>
                        "#" <EOL>
                        <Copyright>+
                        "#" <EOL>
                        <BinaryLicense>+
                        "##" <EOL>+
<Filename>          ::= <Word> "." <Extension>
<BinaryAbstract>    ::= "#" <MTS> <AsciiString> <EOL>
<BinaryDescription> ::= ["#" <MTS> <AsciiString> <EOL>]+
<BinaryLicense>     ::= ["#" <MTS> <AsciiString> <EOL>]+
                        ["#" <EOL>]
```

#### Parameters

**_Abstract_**

A brief one line description of what the module does.

- The INF file will always have an English version of the Abstract. Other
  localized versions of the abstract will be stored in the Unicode file
  specified in the `[Defines]` section's `MODULE_UNI_FILE`.

**_BinaryAbstract_**

A brief one line description of what the module does that may be different from
a source abstract.

* If this line is present, the tools will use this line when generating the
  binary "As Built" INF.

* If the Doxygen tag is not present, the tools will use the primary Abstract
  from the Source INF file when generating a binary "As Built" INF.

* In the binary "As Built" INF, the Doxygen tag must not be present.

* The INF file will always have an English version of the Abstract. Other
  localized versions of the abstract will be stored in the Unicode file
  specified in the `[Defines]` section's `MODULE_UNI_FILE`.

**********
**Note:** This file permits the Intel(R) UEFI Packaging Tool to process
localized module content described in the UEFI Platform Initialization
Distribution Package Specification.
**********

**_Description_**

A detailed description of what the module does.

- The INF file will always have an English version of the Description.. Other
  localized versions of the description will be stored in the Unicode file
  specified in the `[Defines]` section's `MODULE_UNI_FILE`.

**_BinaryDescription_**

A detailed description of what the module does that may be different from a
source abstract.

* If this line is present, the tools will use this line when generating the
  binary "As Built" INF.

* If the Doxygen tag is not present, the tools will use the primary Description
  from the Source INF file when generating a binary "As Built" INF.

* In the binary "As Built" INF, the Doxygen tag must not be present.

* The INF file will always have an English version of the Description.. Other
  localized versions of the description will be stored in the Unicode file
  specified in the `[Defines]` section's `MODULE_UNI_FILE`.

**********
**Note:** This file permits the Intel(R) UEFI Packaging Tool to process
localized module content described in the UEFI Platform Initialization
Distribution Package Specification.
**********

**_Copyright_**

The copyright date should be modified if there is a functional change to the
source code. Since binaries are constructed from source, the binary file uses
the same copyright date as the source INF. Copyright data will not be localized.

**_License_**

One or more licenses that the module with source code is released under.
License content will not be localized.

**_BinaryLicense_**

One or more licenses that the binary module is released under that may be
different from the licenses used for distributing the module with source code.
License content will not be localized.

* If this tag is present, the tools will use this content when generating the
  binary "As Built" INF.

* If the Doxygen tag is not present in the source INF, the tools will use the
  License content from the Source INF file when generating a binary "As Built"
  INF.

* In the binary "As Built" INF, the Doxygen tag must not be present.

#### Example

```ini
## @file
# EFI/Framework Base Memory Library
#
# Implementation of a base memory library that provides minimum
# functionality for accessing memory.
#
# Copyright (c) 2006 - 2008, Intel Corporation. All rights reserved.
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution. The full text of the license may be
# found at:
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS
# OR IMPLIED.
#
##
```

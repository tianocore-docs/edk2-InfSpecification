<!--- @file
  2.16 [UserExtensions] Section

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

## 2.16 [UserExtensions] Section

The EDK II `[UserExtensions]` sections allow for extending the INF with custom
processing. The format for a user extension is:

`[UserExtensions.$(UserID).$(Identifier)]`

Having data elements under the section header is not required.

The EDK II build tools do not use this section. When generating the "As Built"
binary INF during a build, this section is copied from the original source INF
file if the UserId is "TianoCore". Other `[UserExtensions]` sections will not be
copied. The reference tools ignore all content within a `[UserExtensions]`
section.

The following is an example of a `[UserExtensions]` section:

```ini
[UserExtensions.NoSuchCorp."MyProcess_1.2"]
  NoSuch.bat
```

### 2.16.1 [UserExtensions.TianoCore."ExtraFiles"] Section

The EDK II `[UserExtensions.TianoCore."ExtraFiles"]` section allow for
distributing extraneous files that are associated with a module. Files listed
in this section are not processed by EDK II build tools. These files must exist
in the directory or sub- directories of the directory containing the INF file.

**********
**Note:** The Intel(R) UEFI Packaging Tool will parse this section and for all
files listed in this file, add the file to the module distribution using the
UEFI Distribution Package Distribution.
**********

The section header must be:

`[UserExtensions.TianoCore."ExtraFiles"]`

Having data elements under the section header is not required.

The following is an example of a `[UserExtensions.TianoCore."ExtraFiles"]`
section:

```ini
[UserExtensions.TianoCore."ExtraFiles"]
  Readme.txt
```

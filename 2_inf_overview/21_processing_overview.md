<!--- @file
  2.1 Processing Overview

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

## 2.1 Processing Overview

Each module or component INF file is broken out into sections in a manner
similar to the other build meta-data files. However, the intent of a module's
INF file is to define the source files, libraries, and definitions relevant to
building the module, creating binary files that are either raw binary files or
PE32/PE32+/coff format files. The different sections are described in detail in
this chapter. In general, the original EDK parsing utilities read each line
from the `[Libraries]` or `[Components]` sections of the build description
(DSC) file, process the INF file on a line to generate a makefile, and then
continued with the next line. EDK II parsing utilities are token based, which
permits an element to span multiple lines. The EDK II utilities check both EDK
and EDK II INF files, and, if required, generate C code files based on the
content of the EDK II INF. Refer to the _EDK II Build Specification_ for more
information regarding these autogenerated files.

One major difference between EDK and EDK II is support for non-Microsoft
development environments. Because modules may be distributed to developers that
use these environments, both source code and the meta-data files need to be
UNIX*/ GCC clean. One little known fact regarding the Microsoft tools and
operating systems is their ability to process the forward slash "/" character
as a directory separator.

All EDK II INF files MUST use this forward slash character for all directory
paths specified.
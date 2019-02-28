<!--- @file
  2 INF Overview

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

# 2 INF Overview

This section of the document describes the decisions regarding the format of
the EDK II module INF files. The INF files are used by EDK II utilities that
parse build meta-data files (INF, DEC, DSC and FDF files) to generate
`AutoGen.c` and `AutoGen.h` and Makefile/GNU makefile files for the EDK II
build infrastructure.

The EDK II INF meta-data file describes properties of a module, how it is
coded, what it provides, what it depends on, architecture specific items,
features, etc. regarding the module. INF files generated during a build (that
allow distribution of binary modules) describe how the module was compiled,
linked and what platform configuration database items (PCDs) are exposed.
Binary distribution of EDK II modules allows original device manufactures
(ODMs) to distribute proprietary drivers, without distributing source code, for
inclusion in a firmware image.

EDK II modules may be located in sub-directories of a package (a collection
of related objects.) If a module is "a Library", creating the module directory
in the "Library" subdirectory of a package is strongly recommended. An
"Include" package subdirectory may also be required. Header files for modules
that define a library class must be placed in the Include/Library directory
using the Library Class Name for the file name.

The Include directory and sub-directories contain header files that define
either a library class API or pre-defined ("industry standard") data elements.
One and only one header file defines the library class API. Multiple library
instances can "produce" the functionality of a library class. The use of
library class API headers allows for platform integrators to select a library
instance that is suitable for their platform. This usage model frees the driver
developer from coding a module to specific library instances. Libraries are
really nothing more than modules with pre-defined APIs.

**********
**Note:** Path and Filename elements within the INF are case-sensitive in order
to support building on UNIX style operating systems.
**********
**Note:** GUID values are used during runtime to uniquely map the C names of
PROTOCOLS, PPIS, PCDS and other variable names.
**********
**Note:** This document uses a backslash "\" to indicate that a line that
cannot be displayed in this document on a single line. Within the DSC
specification, each entry must appear on a single line.
**********
**Note:** The total path and file name length is limited by the operating
system and third party tools. It is recommended that for EDK II builds that the
WORKSPACE directory be either a directory under a subst drive in Windows
(s:/build as an example) or be located in either the /opt directory or in the
user's /home/username directory for Linux and OS/X.
**********

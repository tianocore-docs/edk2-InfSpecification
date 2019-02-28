<!--- @file
  Appendix B Build Changes and Customizations

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

# Appendix A Build Changes and Customizations

## A.1 Customizing EDK Compilation for a Component

There are several mechanisms for customizing the build for a firmware
component. These include:

* Creating a new component INF file that specifies `BUILD_TYPE=xxx`, and then
  creating a `[build.$(PROCESSOR).xxx]` section in the platform DSC file.

* Creating a new component INF file and a makefile for the component, and
  specifying `BUILD_TYPE=MAKEFILE` in the INF file. Then add a
  `[build.$(PROCESSOR).makefile]` section to the DSC file that describes how to
  "build" the component using the makefile. Typically this will be commands to
  copy the file to the `$(DEST_DIR)`, and then invoking nmake.

* Trivial customizations can be accomplished by adding or modifying the
  `[nmake]` sections in the component INF file. This may require defining
  `$(PLATFORM)` in the EDK DSC file, and then adding a new
  `[nmake.$(PROCESSOR).$(PLATFORM)]` section in the component INF file.

* Another option is to define a variable in the component INF file, passing it
  to the component makefile via the DSC `[makefile.common]` section, and then
  using `!IFDEF` statements in the `[build.$(PROCESSOR).$(COMPONENT_TYPE)]`
  section to perform custom steps.

## A.2 Changing Files in an EDK Library

Library INF files are shared among different platforms. However, not all
platforms require all the same source files. To customize the library INF files
for different platforms, simply define `$(PLATFORM)`, either on the command
line, or in the DSC file, and then make customizations in the
`[sources.$(PROCESSOR).$(PLATFORM)]` section of the library INF file.

An alternative to this method is to simply create a new INF file for the
library, and then use it in place of the existing library INF file

## A.3 Customizing EDK II Compilation for a Module Common Definitions

The preferred method for customizing a build is to copy the source module
directory to a new directory and modifying the INF file and module sources.
This method is preferred over the EDK methods as build reproducibility is more
easily accomplished.

Additional customizations for build options should be made in the platform
description (DSC) file. While it is permitted to use the `[BuildOptions]`
section to define custom compiler flags, this section should only be used as a
last resort. The default flags defined the `tools_def.txt` file provide the best
known size and speed optimizations, and the platform DSC file can override the
defaults in its `[BuildOptions]` section.

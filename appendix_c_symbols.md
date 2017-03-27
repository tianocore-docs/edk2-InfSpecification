<!--- @file
  Appendix C Symbols

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

# Appendix C Symbols

One of the core concepts of this utility is the notion of symbols. Use of
symbols follows the makefile convention of enclosing within $(), for example
$(EDK_SOURCE). As the utility processes files during execution, it will often
perform parsing of variable assignments. These variables can then be referenced
in other sections of the DSC file. Variable assignments will be saved internally
in either a local or global symbol table. The local symbol table is purged
following processing of individual component INF files. Global symbol values
persist throughout execution of the utility. Local symbol values take precedent
over global symbols. The following table describes the symbols generated
internally by the utility. They can be overridden either on the command line,
in the DSC file, or in individual INF files. The G/L column indicates whether
the symbol is typically a global or a local symbol.

###### Table 8 Symbol Description

| Symbol Name     | G/L | Description                                                                                                                                                                                                                                                                                                |
| --------------- | --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `EDK_SOURCE`    | G   | Defines the root directory of the local EDK source tree, for example C:\EFI2.0 If not defined as an environmental variable when the tool is invoked, the utility will attempt to determine a reasonable value based on the current working directory.                                                      |
| `EFI_SOURCE`    | G   | Defines the root directory of the local EDK source tree, for example C:\EFI2.0 If not defined as an environmental variable when the tool is invoked, the utility will attempt to determine a reasonable value based on the current working directory.                                                      |
| `PROCESSOR`     | G/L | Defines the target processor for which the code is to be built. This symbol will typically be used to include or exclude source files in component INF files, and to define the tool chain for building.                                                                                                   |
| `BUILD_DIR`     | G   | Defines the build tip directory for the current platform. For example, this may be `$(EDK_SOURCE)\Platform\Anacortes_870`.                                                                                                                                                                                 |
| `SOURCE_DIR`    | L   | For a component, defines the directory of the component source files.                                                                                                                                                                                                                                      |
| `DEST_DIR`      | L   | For a component, defines the directory (typically under BUILD_DIR) where the component object files are to be built.                                                                                                                                                                                       |
| `LIB_DIR`       | L   | Specifies the directory where EFI libraries are deposited after building. Typically `$(BUILD_DIR)\$(PROCESSOR)`                                                                                                                                                                                            |
| `BIN_DIR`       | L   | Specifies the directory where final component binaries are deposited during build. Typically `$(BUILD_DIR)\$(PROCESSOR)`                                                                                                                                                                                   |
| `OUT_DIR`       | L   | Unused, but typically `$(BUILD_DIR)\$(PROCESSOR)`                                                                                                                                                                                                                                                          |
| `DSC_FILENAME`  | G   | Name of the DSC file as specified on the command line. Can be used for dependencies in the makefiles.                                                                                                                                                                                                      |
| `INF_FILENAME`  | L   | Name of the INF file for a given component. Can be used for dependencies in the makefiles.                                                                                                                                                                                                                 |
| `FV_EXT`        | L   | Common component type (BS driver, application, etc) have predefined file name extensions assigned (.dxe, .app, etc). If there is a deviation from the convention, or a new                                                                                                                                 |
|                 |     | (unknown to the utility) component type is being built, then `FV_EXT` may need to be defined for the component so the utility knows the result file name extension. This information is necessary to generate dependencies in makefile.out.                                                                |
| `MAKEFILE_NAME` | L   | Name of the output makefile for the component. Default is "makefile". This value can be overridden to support building different variations of a component in the same `DEST_DIR` directory.                                                                                                               |
| `PLATFORM`      | L   | This symbol can be used to provide more selectivity of files in the component INF files. If assigned, then the utility will also process any files in the INF file under sections `[sources.$(PROCESSOR).$(PLATFORM)]`, `[includes.$(PROCESSOR).$(PLATFORM)]`, and `[libraries.$(PROCESSOR).$(PLATFORM)]`. |
| `FILE`          | L   | As the utility processes each source file in the component INF file, this symbol gets assigned the name of the file, less the file extension.                                                                                                                                                              |
| `PACKAGE`       | L/G | If defined, then the utility will create a package file named `$(DEST_DIR)\$(BASE_NAME).pkg`, and copy, with macro expansion, the `[package.$(COMPONENT_TYPE).$(PACKAGE)]` section from the DSC file to the output file.                                                                                   |
| `PACKAGE_FILE`  | L   | If defined, then the utility will not generate a package file. The build can then use the value `$(PACKAGE_FILE)` to have GenFfsFile use an existing package file for creating the firmware file.                                                                                                          |

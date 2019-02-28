<!--- @file
  2.12 [LibraryClasses] Section

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

## 2.12 [LibraryClasses] Section

The EDK II INF `[LibraryClasses]` section is used to list the names of the
library classes that are required, or optionally required by a component. A
library class instance, as specified in the DSC file, will be linked into the
component. The Library Class' Recommended Instance path must be a package
relative path.

Library classes listed in architectural sections must not be listed in common
`[LibraryClasses]` sections. The architectural section modifier is used as a
restriction to mask items from architectures that are not applicable.

This section uses one of the following section definitions:

```ini
[LibraryClasses]
[LibraryClasses.common]
[LibraryClasses.IA32]
[LibraryClasses.X64]
[LibraryClasses.EBC]
```

The format for entries in this section is:

`LibraryClassName1 [ | FeatureFlagExpression ]`

When a `FeatureFlagExpression` is present, if the expression evaluates to
`TRUE`, then the build tools must ensure that a library class instance has been
specified when building this module. If the expression evaluates to `FALSE`,
then the EDK II build tools must ignore the entry.

```ini
LibraryClassName2
LibraryClassName3 ## $(WORKSPACE)/Path/To/RecommendedLibInstanceName.inf
```

The comment, using the double hash "##" marks, specifies the module
developer's recommended library instance. This is information that the platform
integrator can use to help select a library instance for a given library class
during a build. The package developer may also provide a recommended library
instance. The defined library instance (defined in a DSC file,) that satisfies
a Library Class will be added to the LIBS definition in the output makefile:

`LIBS = $(LIBS) $(LIB_DIR)/{LibInstanceName}`

**********
**Note:** The above is not the name of the INF file, but the name of the
library file that was generated during the instance's compilation. Refer to the
EDK II DSC File Specification for rules to select library class instances.
**********
**Note:** For binary driver or application modules, this is a list of the
library instances in comments that were used to create the binary (.efi)
executable file.
**********

The following is an example of the library class section.

```ini
[LibraryClasses]
  MemoryAllocationLib
  BaseMemoryLib
  PeiServicesTablePointerLib
  CustomDecompressLib
  TianoDecompressLib UefiDecompressLib
  EdkPeCoffLoaderLib
  CacheMaintenanceLib
  ReportStatusCodeLib
  PeiServicesLib
  PerformanceLib
  HobLib
  BaseLib
  PeimEntryPoint
  DebugLib
```

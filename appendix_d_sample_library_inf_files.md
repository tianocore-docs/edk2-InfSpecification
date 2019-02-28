<!--- @file
  Appendix E Sample Library INF Files

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

# Appendix D Sample Library INF Files

The following INF file are examples of INF files for the EDK II MdePkg library,
PeiServicesTablePointerLib and the MdeModulePkg libraries,
DxeCoreMemoryAllocationLib.inf and SmmCorePerformanceLib.inf.

## D.1 PeiServicesTablePointerLib.inf

```ini
## @file
# Instance of PEI Services Table Pointer Library using global variable for the table pointer.
#
# PEI Services Table Pointer Library implementation that retrieves a
# pointer to the PEI Services Table from a global variable. Not available
# to modules that execute from read-only memory.
#
# Copyright (c) 2007 - 2012, Intel Corporation. All rights reserved.<BR>
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution. The full text of the license may be
# found at:
# http://opensource.org/licenses/bsd-license.php.
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
##

[Defines]
  INF_VERSION     = 0x0001001B
  BASE_NAME       = PeiServicesTablePointerLib
  MODULE_UNI_FILE = PeiServicesTablePointerLib.uni
  FILE_GUID       = 1c747f6b-0a58-49ae-8ea3-0327a4fa10e3
  MODULE_TYPE     = PEIM
  VERSION_STRING  = 1.0
  LIBRARY_CLASS   = PeiServicesTablePointerLib|PEIM PEI_CORE SEC
  CONSTRUCTOR     = PeiServicesTablePointerLibConstructor

#
# VALID_ARCHITECTURES = IA32 X64 IPF EBC (EBC is for build only)
#

[Sources]
  PeiServicesTablePointer.c

[Packages]
  MdePkg/MdePkg.dec

[LibraryClasses]
  DebugLib
```

## D.2 DxeCoreMemoryAllocationLib.inf

```ini
## @file
# Memory Allocation Library instance dedicated to DXE Core.
#
# The implementation borrows the DxeCore Memory Allocation services as
# the primitive for memory allocation instead of using UEFI boot
# services in an indirect way.
# It is assumed that this library instance must be linked with DxeCore
# in this package.
#
# Copyright (c) 2008 - 2010, Intel Corporation. All rights reserved.<BR>
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution. The full text of the license may be
# found at:
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
##

[Defines]
  INF_VERSION    = 0x0001001B
  BASE_NAME      = DxeCoreMemoryAllocationLib
  FILE_GUID      = 632F3FAC-1CA4-4725-BAA2-BDECCF9A111C
  MODULE_TYPE    = DXE_CORE
  VERSION_STRING = 1.0
  LIBRARY_CLASS  = MemoryAllocationLib|DXE_CORE

#
# The following information is for reference only and not required by the
# build tools.
#
# VALID_ARCHITECTURES = IA32 X64 IPF EBC
#

[Sources]
  MemoryAllocationLib.c
  DxeCoreMemoryAllocationServices.h

[Packages]
  MdePkg/MdePkg.dec

[LibraryClasses]
  DebugLib
  BaseMemoryLib
```

## D.3 SmmCorePerformanceLib.inf

```ini
## @file
# Performance library instance used by SMM Core.
#
# This library provides the performance measurement interfaces and
# initializes performance logging for the SMM phase.
# It initializes SMM phase performance logging by publishing the SMM
# Performance and PerformanceEx Protocol, which is consumed by
# SmmPerformanceLib to logging performance data in SMM phase.
# This library is mainly used by SMM Core to start performance logging
# to ensure that SMM Performance and PerformanceEx Protocol are
# installed at the very beginning of SMM phase.
#
# Copyright (c) 2011 - 2012, Intel Corporation. All rights reserved.<BR>
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution.
# The full text of the license may be found at:
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
##

[Defines]
  INF_VERSION              = 0x0001001B
  BASE_NAME                = SmmCorePerformanceLib
  FILE_GUID                = 36290D10-0F47-42c1-BBCE-E191C7928DCF
  MODULE_TYPE              = SMM_CORE
  VERSION_STRING           = 1.0
  PI_SPECIFICATION_VERSION = 0x0001000A
  LIBRARY_CLASS            = PerformanceLib|SMM_CORE
  CONSTRUCTOR              = SmmCorePerformanceLibConstructor

#
# The following information is for reference only and not required by the
# build tools.
#
# VALID_ARCHITECTURES = IA32 X64
#

[Sources]
  SmmCorePerformanceLib.c
  SmmCorePerformanceLibInternal.h

[Packages]
  MdePkg/MdePkg.dec
  MdeModulePkg/MdeModulePkg.dec

[LibraryClasses]
  MemoryAllocationLib
  UefiBootServicesTableLib
  PcdLib
  TimerLib
  BaseMemoryLib
  BaseLib
  DebugLib
  SynchronizationLib
  SmmServicesTableLib

[Protocols]
  gEfiSmmBase2ProtocolGuid    ## CONSUMES
  gEfiSmmAccess2ProtocolGuid  ## CONSUMES

[Guids]
  ## PRODUCES ## UNDEFINED # Install protocol
  ## CONSUMES ## UNDEFINED # SmiHandlerRegister
 gSmmPerformanceProtocolGuid
  ## PRODUCES ## UNDEFINED # Install protocol
  ## CONSUMES ## UNDEFINED # SmiHandlerRegister
  gSmmPerformanceExProtocolGuid

[Pcd]
  gEfiMdePkgTokenSpaceGuid.PcdPerformanceLibraryPropertyMask## CONSUMES
```

**********
**Note:** In the above example, the backslash "\" character is used to show a
line continuation for readability. Use of a backslash character in the actual
INF file is not permitted.
**********

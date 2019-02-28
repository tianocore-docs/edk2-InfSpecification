<!--- @file
  Appendix D Sample Driver INF Files

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

# Appendix C Sample Driver INF Files

The following INF file example are from EDK II
`MdeModulePkg/Universal/Disk/DiskIoDxe/DiskIoDxe.inf` and
`IntelFrameworkModulePkg/Universal/StatusCode/RuntimeDxe/StatusCodeRuntimeDxe.inf`
driver modules.

## C.1 DiskIoDxe INF file

```ini
## @file
# Module that lays Disk I/O protocol on every Block I/O protocol.
#
# This module produces Disk I/O protocol to abstract the block accesses
# of the Block I/O protocol to a more general offset-length protocol
# to provide byte-oriented access to block media. It adds this protocol
# to any Block I/O interface that appears in the system that does not
# already have a Disk I/O protocol. File systems and other disk access
# code utilize the Disk I/O protocol.
#
# Copyright (c) 2006 - 2012, Intel Corporation. All rights reserved.<BR>
# This program and the accompanying materials
# are licensed and made available under the terms and conditions of the
# BSD License which accompanies this distribution. The full text of the
# license may be found at:
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
##

[Defines]
  INF_VERSION     = 0x0001001B
  BASE_NAME       = DiskIoDxe
  MODULE_UNI_FILE = DiskIoDxe.uni
  FILE_GUID       = 6B38F7B4-AD98-40e9-9093-ACA2B5A253C4
  MODULE_TYPE     = UEFI_DRIVER
  VERSION_STRING  = 1.0
  ENTRY_POINT     = InitializeDiskIo

#
# The following information is for reference only and not required by the build tools.
#
# VALID_ARCHITECTURES = IA32 X64 IPF EBC
#
# DRIVER_BINDING = gDiskIoDriverBinding
# COMPONENT_NAME = gDiskIoComponentName
# COMPONENT_NAME2 = gDiskIoComponentName2
#

[Sources]
  ComponentName.c
  DiskIo.h
  DiskIo.c

[Packages]
  MdePkg/MdePkg.dec

[LibraryClasses]
  UefiBootServicesTableLib
  MemoryAllocationLib
  BaseMemoryLib
  BaseLib
  UefiLib
  UefiDriverEntryPoint
  DebugLib

[Protocols]
  gEfiDiskIoProtocolGuid   ## BY_START
  gEfiBlockIoProtocolGuid  ## TO_START
```

## C.2 StatusCodeRuntimeDxe INF file

```ini
## @file
# Status Code Runtime Dxe driver produces Status Code Runtime Protocol.
#
# Copyright (c) 2006 - 2012, Intel Corporation. All rights reserved.<BR>
#
# This program and the accompanying materials
# are licensed and made available under the terms and conditions of the
# BSD License which accompanies this distribution. The full text of the
# license may be found at:
# http://opensource.org/licenses/bsd-license.php
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
##

[Defines]
  INF_VERSION     = 0x0001001B
  BASE_NAME       = StatusCodeRuntimeDxe
  MODULE_UNI_FILE = StatusCodeRuntimeDxe.uni
  FILE_GUID       = FEDE0A1B-BCA2-4A9F-BB2B-D9FD7DEC2E9F
  MODULE_TYPE     = DXE_RUNTIME_DRIVER
  VERSION_STRING  = 1.0
  ENTRY_POINT     = StatusCodeRuntimeDxeEntry

#
# The following information is for reference only and not required by the
# build tools.
#
# VALID_ARCHITECTURES = IA32 X64 EBC
#
# VIRTUAL_ADDRESS_MAP_CALLBACK = VirtualAddressChangeCallBack
#

[Sources]
  SerialStatusCodeWorker.c
  RtMemoryStatusCodeWorker.c
  DataHubStatusCodeWorker.c
  StatusCodeRuntimeDxe.h
  StatusCodeRuntimeDxe.c

[Packages]
  MdePkg/MdePkg.dec
  MdeModulePkg/MdeModulePkg.dec
  IntelFrameworkPkg/IntelFrameworkPkg.dec
  IntelFrameworkModulePkg/IntelFrameworkModulePkg.dec

[LibraryClasses]
  OemHookStatusCodeLib
  SerialPortLib
  UefiRuntimeLib
  MemoryAllocationLib
  UefiLib
  UefiBootServicesTableLib
  UefiDriverEntryPoint
  HobLib
  PcdLib
  PrintLib
  ReportStatusCodeLib
  DebugLib
  BaseMemoryLib
  BaseLib

[Guids]
  gEfiDataHubStatusCodeRecordGuid    ## SOMETIMES_PRODUCES  ## UNDEFINED  # DataRecord Guid
  gEfiStatusCodeDataTypeDebugGuid    ## SOMETIMES_PRODUCES  ## UNDEFINED  # Record data type
  gMemoryStatusCodeRecordGuid        ## SOMETIMES_CONSUMES  ## HOB
  gEfiEventVirtualAddressChangeGuid  ## CONSUMES            ## Event
  gEfiStatusCodeDataTypeStringGuid   ## SOMETIMES_CONSUMES  ## UNDEFINED

[Protocols]
  gEfiStatusCodeRuntimeProtocolGuid  ## PRODUCES
  gEfiDataHubProtocolGuid            ## SOMETIMES_CONSUMES  # Needed if Data Hub is supported for status code

[FeaturePcd]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeReplayIn               ## CONSUMES
  gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdStatusCodeUseOEM      ## CONSUMES
  gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdStatusCodeUseDataHub  ## CONSUMES
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeUseMemory              ## CONSUMES
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeUseSerial              ## CONSUMES

[Pcd]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeMemorySize|128|gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeUseMemory  ## SOMETIMES_CONSUMES

[Depex]
  TRUE
```

**********
**Note:** In the above example, the backslash "\" character is used to show a
line continuation for readability. Use of a backslash character in the actual
INF file is not permitted.
**********

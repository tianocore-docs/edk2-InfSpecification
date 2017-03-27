<!--- @file
  Appendix F Sample Binary INF Files

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

# Appendix F Sample Binary INF Files

The following are example INF files for the binary modules, EnhancedFatDxe, in
the FatBinPkg. The second example is a generated binary INF file for the
RuntimeDxe driver in the MdeModulePkg.

## F.1 FatBinPkg/EnhancedFatDxe/Fat.inf

```ini
## @file
#
# Binary FAT32 EFI Driver for IA32, X64, IPF and EBC arch.
#
# This UEFI driver detects the FAT file system in the disk.
# It also produces the Simple File System protocol for the consumer to
# perform file and directory operations on the disk.
#
# Copyright (c) 2007 - 2010, Intel Corporation. All rights reserved.<BR>
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution. The full text of the license may be
# found at:
# http://opensource.org/licenses/bsd-license.php
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
##

[Defines]
  INF_VERSION    = 0x00010009
  BASE_NAME      = Fat
  FILE_GUID      = 961578FE-B6B7-44c3-AF35-6BC705CD2B1F
  MODULE_TYPE    = UEFI_DRIVER
  VERSION_STRING = 1.0

#
# The following information is for reference only and not required by the
# build tools.
#
# VALID_ARCHITECTURES = IA32 X64 IPF EBC
#

[Binaries.Ia32]
  PE32|Ia32/Fat.efi|*

[Binaries.X64]
  PE32|X64/Fat.efi|*

[Binaries.IPF]
  PE32|Ipf/Fat.efi|*

[Binaries.EBC]
  PE32|Ebc/Fat.efi|*

[Binaries.ARM]
  PE32|Arm/Fat.efi|*
```

## F.2 MdeModulePkg/Core/RuntimeDxe.inf

```ini
## @file
# Module that produces EFI runtime virtual switch over services.
#
# This runtime module installs Runtime Architectural Protocol and
# registers CalculateCrc32 boot services table, SetVirtualAddressMap &
# ConvertPointer runtime services table.
#
# Copyright (c) 2006 - 2012, Intel Corporation. All rights reserved.<BR>
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution. The full text of the license may be
# found at:
# http://opensource.org/licenses/bsd-license.php
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
##

[Defines]
  INF_VERSION    = 0x00010019
  BASE_NAME      = RuntimeDxe
  FILE_GUID      = B601F8C4-43B7-4784-95B1-F4226CB40CEE
  MODULE_TYPE    = DXE_RUNTIME_DRIVER
  VERSION_STRING = 1.0

[Packages.IA32]
  MdePkg/MdePkg.dec
  MdeModulePkg/MdeModulePkg.dec

[Binaries.IA32]
  PE32|RuntimeDxe.efi
  DXE_DEPEX|RuntimeDxe.depex

[PatchPcd.IA32]
  ## CONSUMES
  gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000047|0x1EC8

  ## CONSUMES
  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x27|0x1ECC

  ## CONSUMES
  gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask|0x07|0x1ECD

[Protocols.IA32]
  ## PRODUCES
  gEfiRuntimeArchProtocolGuid

  ## SOMETIMES_CONSUMES
  ## CONSUMES
  gEfiLoadedImageProtocolGuid

  ## SOMETIMES_CONSUMES
  gPcdProtocolGuid

  ## CONSUMES
  gEfiPcdProtocolGuid

  ## SOMETIMES_CONSUMES
  gEfiDevicePathProtocolGuid

  ## CONSUMES
  gEfiStatusCodeRuntimeProtocolGuid

  ## SOMETIMES_PRODUCES
  gEfiDriverBindingProtocolGuid

  ## SOMETIMES_CONSUMES
  gEfiSimpleTextOutProtocolGuid

  ## SOMETIMES_CONSUMES
  gEfiGraphicsOutputProtocolGuid

  ## SOMETIMES_CONSUMES
  gEfiHiiFontProtocolGuid

  ## SOMETIMES_CONSUMES # Consumes if gEfiGraphicsOutputProtocolGuid uninstalled
  gEfiUgaDrawProtocolGuid

  ## SOMETIMES_PRODUCES # User chooses to produce it
  gEfiComponentNameProtocolGuid

  ## SOMETIMES_PRODUCES # User chooses to produce it
  gEfiComponentName2ProtocolGuid

  ## SOMETIMES_PRODUCES # User chooses to produce it
  gEfiDriverConfigurationProtocolGuid

  ## SOMETIMES_PRODUCES # User chooses to produce it
  gEfiDriverConfiguration2ProtocolGuid

  ## SOMETIMES_PRODUCES # User chooses to produce it
  gEfiDriverDiagnosticsProtocolGuid

  ## SOMETIMES_PRODUCES # User chooses to produce it
  gEfiDriverDiagnostics2ProtocolGuid

[Ppis.IA32]

[Guids.IA32]
  ## CONSUMES # Event
  ## CONSUMES # Event
  ## PRODUCES # Event # RuntimeDriverSetVirtualAddressMap() signals this event.
  gEfiEventVirtualAddressChangeGuid

  ## SOMETIMES_CONSUMES
  ## SOMETIMES_CONSUMES ## UNDEFINED
  gEfiStatusCodeDataTypeDebugGuid

  ## CONSUMES # Event
  ## CONSUMES # Event
  gEfiEventExitBootServicesGuid

  ## SOMETIMES_CONSUMES ## UNDEFINED
  gEfiStatusCodeSpecificDataGuid

  ## SOMETIMES_CONSUMES # Event
  gEfiEventReadyToBootGuid

  ## SOMETIMES_CONSUMES # Event
  gEfiEventLegacyBootGuid

  ## SOMETIMES_CONSUMES # Variable
  gEfiGlobalVariableGuid

[PcdEx.IA32]

#
# The following information is for reference only and not required by the
# build tools.
#
# VALID_ARCHITECTURES = IA32 X64 IPF EBC
#

## @AsBuilt
## MSFT:DEBUG_VS2008x86_IA32_SYMRENAME_FLAGS = Symbol renaming not needed for
## MSFT:DEBUG_VS2008x86_IA32_ASLDLINK_FLAGS = /NODEFAULTLIB /ENTRY:ReferenceAcpiTable /SUBSYSTEM:CONSOLE
## MSFT:DEBUG_VS2008x86_IA32_VFR_FLAGS = -l -n
## MSFT:DEBUG_VS2008x86_IA32_PP_FLAGS = /nologo /E /TC /FIAutoGen.h
## MSFT:DEBUG_VS2008x86_IA32_GENFW_FLAGS =
## MSFT:DEBUG_VS2008x86_IA32_OPTROM_FLAGS = -e
## MSFT:DEBUG_VS2008x86_IA32_SLINK_FLAGS = /NOLOGO /LTCG /Zd /Zi
## MSFT:DEBUG_VS2008x86_IA32_ASL_FLAGS =
## MSFT:DEBUG_VS2008x86_IA32_CC_FLAGS = /nologo /c /WX /GS- /W4/Gs32768 /D UNICODE /O1ib2 /GL /FIAutoGen.h /EHs-c- /GR- /GF /Gy /Zi /Gm
## MSFT:DEBUG_VS2008x86_IA32_VFRPP_FLAGS = /nologo /E /TC/DVFRCOMPILE /FI$(MODULE_NAME)StrDefs.h /TC /Dmain=ReferenceAcpiTable
## MSFT:DEBUG_VS2008x86_IA32_APP_FLAGS = /nologo /E /TC
## MSFT:DEBUG_VS2008x86_IA32_DLINK_FLAGS = /NOLOGO /NODEFAULTLIB/IGNORE:4001 /OPT:REF /OPT:ICF=10 /MAP /ALIGN:32/SECTION:.xdata,D /SECTION:.pdata,D /MACHINE:X86 /LTCG /DLL/ENTRY:$(IMAGE_ENTRY_POINT) /SUBSYSTEM:EFI_BOOT_SERVICE_DRIVER/SAFESEH:NO /BASE:0 /DRIVER /DEBUG/PDB:$(OUTPUT_PATH)$(BASE_NAME).pdb/PDBSTRIPPED:$(OUTPUT_PATH)$(BASE_NAME)_Stripped.pdb
## MSFT:DEBUG_VS2008x86_IA32_ASLPP_FLAGS = /nologo /E /C /FIAutoGen.h
## MSFT:DEBUG_VS2008x86_IA32_OBJCOPY_FLAGS = objcopy not needed for
## MSFT:DEBUG_VS2008x86_IA32_MAKE_FLAGS = /nologo
## MSFT:DEBUG_VS2008x86_IA32_ASMLINK_FLAGS = /nologo /tiny
```

**********
**Note:** In the above example, the backslash "\" character is used to show a
line continuation for readability. Use of a backslash character in the actual
INF file is not permitted.
**********

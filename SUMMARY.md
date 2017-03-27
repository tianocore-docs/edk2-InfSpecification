<!--- @file
  Summary

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

# Summary

* [EDK II Module Information (INF) File Specification](README.md#edk-ii-module-information-inf-file-specification)
* [1 Introduction](1_introduction/README.md#1-introduction)
  * [1.1 Overview](1_introduction/11_overview.md#11-overview)
  * [1.2 Terms](1_introduction/12_terms.md#12-terms)
  * [1.3 Related Information](1_introduction/13_related_information.md#13-related-information)
  * [1.4 Target Audience](1_introduction/14_target_audience.md#14-target-audience)
  * [1.5 Conventions Used in this Document](1_introduction/15_conventions_used_in_this_document.md#15-conventions-used-in-this-document)
* [2 INF Overview](2_inf_overview/README.md#2-inf-overview)
  * [2.1 Processing Overview](2_inf_overview/21_processing_overview.md#21-processing-overview)
  * [2.2 Information File General Rules](2_inf_overview/22_information_file_general_rules.md#22-information-file-general-rules)
  * [2.3 EDK II INF Format](2_inf_overview/23_edk_ii_inf_format.md#23-edk-ii-inf-format)
  * [2.4 [Defines] Section](2_inf_overview/24_[defines]_section.md#24-defines-section)
  * [2.5 [Sources] Section](2_inf_overview/25_[sources]_section.md#25-sources-section)
  * [2.6 [BuildOptions] Section](2_inf_overview/26_[buildoptions]_section.md#26-buildoptions-section)
  * [2.7 [Binaries] Section](2_inf_overview/27_[binaries]_section.md#27-binaries-section)
  * [2.8 [Includes] Section](2_inf_overview/28_[includes]_section.md#28-includes-section)
  * [2.9 [Protocols] Section](2_inf_overview/29_[protocols]_section.md#29-protocols-section)
  * [2.10 [Ppis] Section](2_inf_overview/210_[ppis]_section.md#210-ppis-section)
  * [2.11 [Guids] Section](2_inf_overview/211_[guids]_section.md#211-guids-section)
  * [2.12 [LibraryClasses] Section](2_inf_overview/212_[libraryclasses]_section.md#212-libraryclasses-section)
  * [2.13 [Packages] Section](2_inf_overview/213_[packages]_section.md#213-packages-section)
  * [2.14 PCD Sections](2_inf_overview/214_pcd_sections.md#214-pcd-sections)
  * [2.15 [Depex] Section](2_inf_overview/215_[depex]_section.md#215-depex-section)
  * [2.16 [UserExtensions] Section](2_inf_overview/216_[userextensions]_section.md#216-userextensions-section)
* [3 EDK II INF File Format](3_edk_ii_inf_file_format/README.md#3-edk-ii-inf-file-format)
  * [3.1 General Rules](3_edk_ii_inf_file_format/31_general_rules.md#31-general-rules)
  * [3.2 Component INF Definition](3_edk_ii_inf_file_format/32_component_inf_definition.md#32-component-inf-definition)
  * [3.3 Header Section](3_edk_ii_inf_file_format/33_header_section.md#33-header-section)
  * [3.4 [Defines] Section](3_edk_ii_inf_file_format/34_[defines]_section.md#34-defines-section)
  * [3.5 [BuildOptions] Sections](3_edk_ii_inf_file_format/35_[buildoptions]_sections.md#35-buildoptions-sections)
  * [3.6 [LibraryClasses] Sections](3_edk_ii_inf_file_format/36_[libraryclasses]_sections.md#36-libraryclasses-sections)
  * [3.7 [Packages] Sections](3_edk_ii_inf_file_format/37_[packages]_sections.md#37-packages-sections)
  * [3.8 PCD Sections](3_edk_ii_inf_file_format/38_pcd_sections.md#38-pcd-sections)
  * [3.9 [Sources] Sections](3_edk_ii_inf_file_format/39_[sources]_sections.md#39-sources-sections)
  * [3.10 [UserExtensions] Sections](3_edk_ii_inf_file_format/310_[userextensions]_sections.md#310-userextensions-sections)
  * [3.11 [Protocols] Sections](3_edk_ii_inf_file_format/311_[protocols]_sections.md#311-protocols-sections)
  * [3.12 [Ppis] Sections](3_edk_ii_inf_file_format/312_[ppis]_sections.md#312-ppis-sections)
  * [3.13 [Guids] Sections](3_edk_ii_inf_file_format/313_[guids]_sections.md#313-guids-sections)
  * [3.14 [Depex] Sections](3_edk_ii_inf_file_format/314_[depex]_sections.md#314-depex-sections)
  * [3.15 [Binaries] Section](3_edk_ii_inf_file_format/315_[binaries]_section.md#315-binaries-section)
* [Appendix A EDK INF File Specification](appendix_a_edk_inf_file_specification/README.md#appendix-a-edk-inf-file-specification)
  * [A.1 Design Discussion](appendix_a_edk_inf_file_specification/a1_design_discussion.md#a1-design-discussion)
  * [A.2 EDK File Specification](appendix_a_edk_inf_file_specification/a2_edk_file_specification.md#a2-edk-file-specification)
* [Appendix B Build Changes and Customizations](appendix_b_build_changes_and_customizations.md#appendix-b-build-changes-and-customizations)
  * [B.1 Customizing EDK Compilation for a Component](appendix_b_build_changes_and_customizations.md#b1-customizing-edk-compilation-for-a-component)
  * [B.2 Changing Files in an EDK Library](appendix_b_build_changes_and_customizations.md#b2-changing-files-in-an-edk-library)
  * [B.3 Customizing EDK II Compilation for a Module Common Definitions](appendix_b_build_changes_and_customizations.md#b3-customizing-edk-ii-compilation-for-a-module-common-definitions)
* [Appendix C Symbols](appendix_c_symbols.md#appendix-c-symbols)
* [Appendix D Sample Driver INF Files](appendix_d_sample_driver_inf_files.md#appendix-d-sample-driver-inf-files)
  * [D.1 DiskIoDxe INF file](appendix_d_sample_driver_inf_files.md#d1-diskiodxe-inf-file)
  * [D.2 StatusCodeRuntimeDxe INF file](appendix_d_sample_driver_inf_files.md#d2-statuscoderuntimedxe-inf-file)
* [Appendix E Sample Library INF Files](appendix_e_sample_library_inf_files.md#appendix-e-sample-library-inf-files)
  * [E.1 PeiServicesTablePointerLib.inf](appendix_e_sample_library_inf_files.md#e1-peiservicestablepointerlibinf)
  * [E.2 DxeCoreMemoryAllocationLib.inf](appendix_e_sample_library_inf_files.md#e2-dxecorememoryallocationlibinf)
  * [E.3 SmmCorePerformanceLib.inf](appendix_e_sample_library_inf_files.md#e3-smmcoreperformancelibinf)
* [Appendix F Sample Binary INF Files](appendix_f_sample_binary_inf_files.md#appendix-f-sample-binary-inf-files)
  * [F.1 FatBinPkg/EnhancedFatDxe/Fat.inf](appendix_f_sample_binary_inf_files.md#f1-fatbinpkgenhancedfatdxefatinf)
  * [F.2 MdeModulePkg/Core/RuntimeDxe.inf](appendix_f_sample_binary_inf_files.md#f2-mdemodulepkgcoreruntimedxeinf)
* [Appendix G Module Types](appendix_g_module_types.md#appendix-g-module-types)
---
* Tables
  * [Table 1 EDK II [Defines] Section Elements](2_inf_overview/24_[defines]_section.md#table-1-edk-ii-defines-section-elements)
  * [Table 2 EDK II [BuildOptions] Section Elements](2_inf_overview/26_[buildoptions]_section.md#table-2-edk-ii-buildoptions-section-elements)
  * [Table 3 EDK II [BuildOptions] Variable Descriptions](2_inf_overview/26_[buildoptions]_section.md#table-3-edk-ii-buildoptions-variable-descriptions)
  * [Table 4 Macro Usages](3_edk_ii_inf_file_format/32_component_inf_definition.md#table-4-macro-usages)
  * [Table 5 Predefined Command Codes](3_edk_ii_inf_file_format/35_[buildoptions]_sections.md#table-5-predefined-command-codes)
  * [Table 6 EDK [defines] Section Elements](appendix_a_edk_inf_file_specification/a1_design_discussion.md#table-6-edk-defines-section-elements)
  * [Table 7 EDK Component (module) Output File Extensions](appendix_a_edk_inf_file_specification/a1_design_discussion.md#table-7-edk-component-module-output-file-extensions)
  * [Table 8 Symbol Description](appendix_c_symbols.md#table-8-symbol-description)
  * [Table 9 EDK II Module Types](appendix_g_module_types.md#table-9-edk-ii-module-types)
  * [Table 10 Module Type to Component Type Mapping](appendix_g_module_types.md#table-10-module-type-to-component-type-mapping)

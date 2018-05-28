<!--- @file
  3.2 Component INF Definition

  Copyright (c) 2007-2018, Intel Corporation. All rights reserved.<BR>

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

## 3.2 Component INF Definition

The INF definitions describe the content of a module, either source or binary,
as well as external dependencies on packages that contain declarations of
GUIDs, Protocols, PPIs and Library Classes. The platform integrator will can
select library instances that will be used for a given library class, providing
greater flexibility to the module developer. It is not possible to code a
module to a specific implementation of a library instance. It is only possible
to code a module to use a library class. The `[Defines]` section must appear
before any other section except the header. (The header, when specified, is
always the first section of an INF file.) The remaining sections may be
specified in any order within the INF file.

#### Summary

The EDK II Module Information (INF) file has the following format (using the
EBNF).

```
<EDK_II_INF> ::= <Header>?
                 <Defines>
                 <BuildOptions>*
                 <LibraryClasses>*
                 <Packages>*
                 <Pcds>*
                 <Sources>*
                 <Protocols>*
                 <Ppis>*
                 <Guids>*
                 <Binaries>*
                 if (LIBRARY_CLASS is declared in Defines Section):
                   <Depex>*
                 elif (MODULE_TYPE == "USER_DEFINED"
                       || MODULE_TYPE == "UEFI_DRIVER"):
                          <Depex>*
                 elif (MODULE_TYPE == "PEIM"
                       || MODULE_TYPE == "DXE_DRIVER"
                       || MODULE_TYPE == "DXE_RUNTIME_DRIVER"
                       || MODULE_TYPE == "DXE_SAL_DRIVER"
                       || MODULE_TYPE == "DXE_SMM_DRIVER"):
                          <Depex>+
                 elif (MODULE_TYPE == "PEI_CORE"
                      || MODULE_TYPE == "DXE_CORE"
                      || MODULE_TYPE == "SMM_CORE"
                      || MODULE_TYPE == "UEFI_APPLCIATION"):
                         Do not specify a depex section.
                 <UserExtensions>*
```

### 3.2.1 Common Definitions

#### Summary

The following are common definitions used by multiple section types.

#### Prototype

```c
<Word>               ::= (a-zA-Z0-9_)(a-zA-Z0-9_-,)* Alphanumeric characters
                         with optional period ".", dash
                         "-" and/or underscore "_" characters. A period
                         character may not be followed by another period
                         character.
                         No whitespace characters are permitted.
<SimpleWord>         ::= (a-zA-Z0-9)(a-zA-Z0-9_-)* A word that cannot contain a
                         period character.
<ToolWord>           ::= (A-Z)(a-zA-Z0-9)* A word that must start with a
                         capital letter and is allowed to contain additional
                         alphanumeric characters.
                         Whitespace characters are not permitted.
<FileSep>            ::= "/"
<Extension>          ::= (a-zA-Z0-9_-)+ One or more alphanumeric characters.
<File>               ::= <Word> ["." <Extension>]
<PATH>               ::= [<MACROVAL> <FileSep>] <RelativePath>
<RelativePath>       ::= <Word> [<FileSep> <DirName>]*
<DirName>            ::= {<Word>} {<MACROVAL>}
<FullFilename>       ::= <PATH> <FileSep> <File>
<Filename>           ::= [<PATH> <FileSep>] <File>
<Chars>              ::= (a-zA-Z0-9_)
<Digit>              ::= (0-9)
<NonDigit>           ::= (a-zA-Z_)
<Identifier>         ::= <NonDigit> <Chars>*
<CName>              ::= <Identifier> # A valid C variable name.
<AsciiChars>         ::= (0x21 - 0x7E)
<CChars>             ::= [{0x21} {(0x23 - 0x26)} {(0x28 - 0x5B)}
                         {(0x5D - 0x7E)} {<EscapeSequence>}]*
<DblQuote>           ::= 0x22
<SglQuote>           ::= 0x27
<EscapeSequence>     ::= "\" {"n"} {"t"} {"f"} {"r"} {"b"} {"0"} {"\"}
                         {<DblQuote>} {<SglQuote>}
<TabSpace>           ::= {<Tab>} {<Space>}
<TS>                 ::= <TabSpace>*
<MTS>                ::= <TabSpace>+
<Tab>                ::= 0x09
<Space>              ::= 0x20
<CR>                 ::= 0x0D
<LF>                 ::= 0x0A
<CRLF>               ::= <CR> <LF>
<WhiteSpace>         ::= {<TS>} {<CR>} {<LF>} {<CRLF>}
<WS>                 ::= <WhiteSpace>*
<Eq>                 ::= <TS> "=" <TS>
<FieldSeparator>     ::= "|"
<FS>                 ::= <TS> <FieldSeparator> <TS>
<Wildcard>           ::= "*"
<CommaSpace>         ::= "," <Space>*
<Cs>                 ::= "," <Space>*
<AsciiString>        ::= [ <TS>* <AsciiChars>* ]*
<EmptyString>        ::= <DblQuote><DblQuote>
<CFlags>             ::= <AsciiString>
<PrintChars>         ::= {<TS>} {<CChars>}
<QuotedString>       ::= <DblQuote> <PrintChars>* <DblQuote>
<SglQuotedString>    ::= <SglQuote> <PrintChars>* <SglQuote>
<CString>            ::= {<QuotedString>} {<SglQuotedString>}
<NormalizedString>   ::= <DblQuote> [{<Word>} {<Space>}]+ <DblQuote>
<GlobalComment>      ::= <WS> "#" [<AsciiString>] <EOL>+
<Comment>            ::= "#" <AsciiString> <EOL>+
<UnicodeString>      ::= "L" {<QuotedString>} {<SglQuotedString>}
<HexDigit>           ::= (a-fA-F0-9)
<HexByte>            ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
<HexNumber>          ::= {"0x"} {"0X"} <HexDigit>*
<HexVersion>         ::= "0x" <Major> <Minor>
<Major>              ::= <HexDigit>? <HexDigit>? <HexDigit>?
                         <HexDigit>
<Minor>              ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<DecimalVersion>     ::= {"0"} {(0-9) (0-9)*} ["." (0-9)+]
<VersionVal>         ::= {<HexVersion>} {(0-9)+ "." (0-9)+}
<GUID>               ::= {<RegistryFormatGUID>} {<CFormatGUID>}
<RegistryFormatGUID> ::= <RHex8> "-" <RHex4> "-" <RHex4> "-" <RHex4> "-"
                         <RHex12>
<RHex4>              ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<RHex8>              ::= <RHex4> <RHex4>
<RHex12>             ::= <RHex4> <RHex4> <RHex4>
<RawH2>              ::= <HexDigit>? <HexDigit>
<RawH4>              ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>
<OptRawH4>           ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>?
<Hex2>               ::= {"0x"} {"0X"} <RawH2>
<Hex4>               ::= {"0x"} {"0X"} <RawH4>
<Hex8>               ::= {"0x"} {"0X"} <OptRawH4> <RawH4>
<Hex12>              ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4> <RawH4>
<Hex16>              ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4>
                         <OptRawH4> <RawH4>
<CFormatGUID>        ::= "{" <Hex8> <CommaSpace> <Hex4> <CommaSpace>
                         <Hex4> <CommaSpace> "{"
                         <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                         <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                         <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                         <Hex2> <CommaSpace> <Hex2> "}" "}"
<CArray>              ::= "{" {<Nlist>} {<CArray>} "}"
<NList>               ::= <HexByte> [<CommaSpace> <HexByte>]*
<RawData>             ::= <TS> <Number> [<Cs> <Number> [<EOL> <TS>]]*
<Integer>             ::= {(0-9)} {(1-9)(0-9)*}
<Number>              ::= {<Integer>} {<HexNumber>}
<HexNz>               ::= (\x1 - \xFFFFFFFFFFFFFFFF)
<NumNz>               ::= (1-18446744073709551615)
<GZ>                  ::= {<NumNz>} {<HexNz>}
<TRUE>                ::= {"TRUE"} {"true"} {"True"} {"0x1"} {"0x01"} {"1"}
<FALSE>               ::= {"FALSE"} {"false"} {"False"} {"0x0"}
                          {"0x00"} {"0"}
<BoolVal>             ::= {<TRUE>} {<FALSE>}
<BoolType>            ::= {<BoolVal>} {"{"<BoolVal>"}"}
<MACRO>               ::= (A-Z)(A-Z0-9_)*
<MACROVAL>            ::= "$(" <MACRO> ")"
<PcdName>             ::= <TokenSpaceGuidCName> "." <PcdCName>
<PcdCName>            ::= <CName>
<TokenSpaceGuidCName> ::= <CName>
<UINT8>               ::= {"0x"} {"0X"} (\x0 - \xFF)
<UINT16>              ::= {"0x"} {"0X"} (\x0 - \xFFFF)
<UINT32>              ::= {"0x"} {"0X"} (\x0 - \xFFFFFFFF)
<UINT64>              ::= {"0x"} {"0X"} (\x0 - \xFFFFFFFFFFFFFFFF)
<UINT8z>              ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
<UINT16z>             ::= {"0x"} {"0X"} <HexDigit> <HexDigit> <HexDigit>
                          <HexDigit>
<UINT32z>             ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
                          <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                          <HexDigit> <HexDigit>
<UINT64z>             ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
                          <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                          <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                          <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                          <HexDigit> <HexDigit>
<ShortNum>            ::= (0-255)
<IntNum>              ::= (0-65535)
<LongNum>             ::= (0-4294967295)
<LongLongNum>         ::= (0-18446744073709551615)
<ValUint8>            ::= {<ShortNum>} {<UINT8>} {<BoolVal>}
                          {<CString>} {<UnicodeString>}
<ValUint16>           ::= {<IntNum>} {<UINT16>} {<BoolVal>}
                          {<CString>} {<UnicodeString>}
<ValUint32>           ::= {<LongNum>} {<UINT32>} {<BoolVal>}
                          {<CString>} {<UnicodeString>}
<ValUint64>           ::= {<LongLongNum>} {<UINT64>} {<BoolVal>}
                          {<CString>} {<UnicodeString>}
<NumValUint8>         ::= {<ValUint8>} {"{"<ValUint8>"}"}
<NumValUint16>        ::= {<ValUint16>}
                          {"{"<ValUint8> [<CommaSpace> <ValUint8>]*"}"}
<NumValUint32>        ::= {<ValUint32>}
                          {"{"<ValUint8> [<CommaSpace> <ValUint8>]*"}"}
<NumValUint64>        ::= {<ValUint64>}
                          {"{"<ValUint8> [<CommaSpace> <ValUint8>]*"}"}
<StringVal>           ::= {<UnicodeString>} {<CString>} {<Array>}
<Array>               ::= "{" {<Array>} {[<Lable>] <ArrayVal>
                           [<CommaSpace> [<Lable>] <ArrayVal>]* } "}"
<ArrayVal>            ::= {<Num8Array>} {<GuidStr>} {<DevicePath>}
<NonNumType>          ::= {<BoolVal>} {<UnicodeString>} {<CString>}
                          {<Offset>} {<UintMac>}
<Num8Array>           ::= {<NonNumType>} {<ShortNum>} {<UINT8>}
<Num16Array>          ::= {<NonNumType>} {<IntNum>} {<UINT16>}
<Num32Array>          ::= {<NonNumType>} {<LongNum>} {<UINT32>}
<Num64Array>          ::= {<NonNumType>} {<LongLongNum>} {<UINT64>}
<GuidStr>             ::= "GUID(" <GuidVal> ")"
<GuidVal>             ::= {<DblQuote> <RegistryFormatGUID> <DblQuote>}
                          {<CFormatGUID>} {<CName>}
<DevicePath>          ::= "DEVICE_PATH(" <DevicePathStr> ")"
<DevicePathStr>       ::= A double quoted string that follow the device path
                          as string format defined in UEFI Specification 2.6
                          Section 9.6
<UintMac>             ::= {<Uint8Mac>} {<Uint16Mac>} {<Uint32Mac>} {<Uint64Mac>}
<Uint8Mac>            ::= "UINT8(" <Num8Array> ")"
<Uint16Mac>           ::= "UINT16(" <Num16Array> ")"
<Uint32Mac>           ::= "UINT32(" <Num32Array> ")"
<Uint64Mac>           ::= "UINT64(" <Num64Array> ")"
<Lable>               ::= "LABEL(" <CName> ")"
<Offset>              ::= "OFFSET_OF(" <CName> ")"
<ModuleType>          ::= {"BASE"} {"SEC"} {"PEI_CORE"} {"PEIM"}
                          {"DXE_CORE"} {"DXE_DRIVER"} {"SMM_CORE"}
                          {"DXE_RUNTIME_DRIVER"} {"DXE_SAL_DRIVER"}
                          {"DXE_SMM_DRIVER"} {"UEFI_DRIVER"}
                          {"UEFI_APPLICATION"} {"USER_DEFINED"}
<ModuleTypeList>      ::= <ModuleType> [" " <ModuleType>]*
<IdentifierName>      ::= <TS> {<MACROVAL>} {<PcdName>} <TS>
<Boolean>             ::= {<BoolType>} {<Expression>}
<EOL>                 ::= <TS> 0x0D 0x0A
<OA>                  ::= (a-zA-Z)(a-zA-Z0-9)*
<arch>                ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {<OA>}
<Edk2ModuleType>      ::= {"BASE"} {"SEC"} {"PEI_CORE"} {"PEIM"}
                          {"DXE_CORE"} {"DXE_DRIVER"}
                          {"DXE_SAL_DRIVER"}
                          {"DXE_RUNTIME_DRIVER"}
                          {"SMM_CORE"} {"DXE_SMM_DRIVER"}
                          {"UEFI_DRIVER"} {"UEFI_APPLICATION"}
```

**********
**Note:** When using CString, UnicodeString or byte array format as
UINT8/UINT16/UINT32/UINT64 values, please make sure they fit in the
target type's size, otherwise tool would report failure.
**********
**Note:** LABEL() macro in byte arrays to tag the byte offset of a
location in a byte array. OFFSET_OF() macro in byte arrays that returns
the byte offset of a LABEL() declared in a byte array.
**********
**Note:** When using the characters "|" or "||" in an expression, the
expression must be encapsulated in open "(" and close ")" parenthesis.
**********
**Note:** Comments may appear anywhere within a INF file, provided they follow
the rules that a comment may not be enclosed within Section headers, and that
in line comments must appear at the end of a statement.
**********
**Note:** At this time, expressions are not supported in INF files.
**********

#### Parameters

**_Expression_**

Expression syntax is defined the _EDK II Expression Syntax Specification_.

**_Unicode String_**

When the `<UnicodeString>` element (these characters are string literals as
defined by the C99 specification: L"string"/L'string', not actual Unicode
characters) is included in a value, the build tools may be required to expand
the ASCII string between the quotation marks into a valid UCS-2 character encoded
string. The build tools parser must treat all content between the field separators
(excluding white space characters around the field separators) as ASCII literal
content when generating the `AutoGen.c` and `AutoGen.h` files.

**_Comments_**

Strings that appear in comments may be ignored by the build tools. An ASCII
string matching the format of the ASCII string defined by `<UnicodeString>`
(L"Foo" for example,) that appears in a comment must never be expanded by any
tool.

**_CFlags_**

CFlags refers to a string of valid arguments appended to the command line of
any third party or provided tool. It is not limited to just a compiler
executable tool. MACRO values that appear in quoted strings in CFlags content
must not be expanded by parsing tools.

**_OA_**

Other Architecture - One or more user defined target architectures, such as ARM
or PPC. The architectures listed here must have a corresponding entry in the
EDK II meta-data file, `Conf/tools_def.txt`. Only IA32, X64, IPF and EBC are
routinely validated.

**_ExtendedLine_**

The use of the Extended Line format is prohibited.

**_FileSep_**

FileSep refers to either the backslash "\" or forward slash "/" characters that
are used to separate directory names. All EDK II INF files must use the "/"
forward slash character when specifying the directory portion of a filename.
Microsoft operating systems, that normally use a backslash character for
separating directory names, will interpret the forward slash character
correctly. Use of "..", "." and "../" in the directory path is not permitted.
Use of an absolute path is not permitted.

CArray

All C data arrays used in PCD value fields must be byte arrays. The C format
GUID style is a special case that is permitted in some fields that use the
`<CArray>` nomenclature.

**_End of Line Characters_**

The DOS End Of Line: "0x0D 0x0A" character must be used for all EDK II
metadata files. All *Nix based tools can properly process the DOS EOL
characters. Microsoft based tools cannot process the *Nix style EOL characters.

### 3.2.2 MACRO Statements

Use of MACRO statements is optional.

#### Summary

Macro statements are characterize by a `DEFINE` line. Macro statements in INF
files are only permitted to describe a path (shortcut name,) or used to provide
a shorter text string in C Flags in the `[BuildOptions]` section. If the Macro
statement is within the `[Defines]` section, then the Macro is common to the
entire file, with local definitions taking precedence (if the same MACRO name
is used in subsequent sections, then the MACRO value is local to only that
section.)

Macro statements in comments must be ignored by parsing tools.

Macro statements that are referenced before they are defined will have a value
of zero. A macro defined in a section that is common to all architectures is
also value for sections that have architectural modifiers.

It is recommended that if the tools encounter a macroval, as in `$(MACRO)`, that
is not defined, the build tools must break.

#### Prototype

```c
<MacroDefinition> ::= <TS> "DEFINE" <MTS> <MACRO> <Eq> [<Value>] <EOL>
<Value>           ::= {<PATH>} {<CFlags>} {<Filename>}
```

#### Parameters

**_Path Definitions_**

Whitespace characters are not permitted in path statements. While some
operating systems permit using space characters or special characters within a
path element, the EDK II build system will not support whitespace characters
and will only permit alphanumeric characters, and the dot, dash, underline,
forward and back slash characters in file names. Use of "..", "." and "../" in
the directory path is not permitted. Use of an absolute path is not permitted.
It is also permitted, although not specified in the EBNF for `<PATH>` to end
have the path end with the file separator character, as in MdePkg/.

#### Examples:

```ini
DEFINE TEST          = test
DEFINE TEST1         = test/
DEFINE TEST2         = MyFile.c
DEFINE TEST3         = $(TEST1)$(TEST2)
DEFINE TEST4         = $(TEST)/$(TEST2)
DEFINE GEN_SKU       = MyPlatformPkg/GenPei
DEFINE SKU1          = MyPlatformPkg/Sku1/Pei
DEFINE OPENSSL_FLAGS = -DOPENSSL_SYSNAME_UWIN -DOPENSSL_SYS_UEFI
```

###### Table 4 Macro Usages

| MACRO DEFINITION                          | MACRO USAGE                   |
| ----------------------------------------- | ----------------------------- |
| `DEFINE MY_MACRO = test1`                 | `$(MY_MACRO)/test2/test3.inf` |
| `DEFINE MY_MACRO = test1/`                | `$(MY_MACRO)test2/test3.inf`  |
| `DEFINE MY_MACRO = test3.inf`             | `test1/test2/$(MY_MACRO)`     |
| `DEFINE MY_MACRO = test3`                 | `test1/test2/$(MY_MACRO).inf` |
| `DEFINE MY_MACRO = test1/test2/test3.inf` | `$(MY_MACRO)`                 |

### 3.2.3 Conditional Statements

The conditional statements are not permitted anywhere within the INF file.

### 3.2.4 !include Statement

The `!include` statement is not permitted in an EDK II INF file.

### 3.2.5 !error Statement

The `!error` statement is not permitted in an EDK II INF file.

### 3.2.6 Special Comment Blocks

This section defines special format comment blocks that contain information
about this module. These comment blocks are not required. They may appear at
the end of any section within the INF file, however it is preferred that they
appear at the end of the file. The format of these comment blocks is the
recommended format that will guarantee that the information is correctly
inserted into UEFI Distribution Package description files. If this comment
block appears in a "Source" INF file, the EDK II build tools must copy this
comment block into the generated "As Built" binary INF file.

These comment blocks are only required for modules that use C calls to perform
actions using UEFI defined functions listed in below.

There are three predefined types of comments.

The Event types which describe timer delays used by the Boot Services
SetTimer() call.

* `EVENT_TYPE_PERIODIC_TIMER` - Event is to be signaled periodically.
* `EVENT_TYPE_RELATIVE_TIMER` - Event is to be signaled in x 100ns units.
* `UNDEFINED` - This will appear when a UEFI Distribution Package tool was
  unable to parse the comment (spelling error) when creating a distribution
  package, and the tool installed the distribution package using this value.

The BootMode types which describe the BootMode values in the Boot Services
SetBootMode() and GetBootMode() calls.

* `FULL` - Equivalent to `BOOT_WITH_FULL_CONFIGURATION`
* `MINIMAL` - Equivalent to `BOOT_WITH_MINIMAL_CONFIGURATION`
* `NO_CHANGE` - Equivalent to `BOOT_ASSUMING_NO_CONFIGURATION_CHANGES`
* `DIAGNOSTICS` - Equivalent to `BOOT_WITH_FULL_CONFIGURATION`
* `DEFAULT` - Equivalent to `BOOT_WITH_FULL_CONFIGURATION`
* `S2_RESUME` - Equivalent to `BOOT_ON_S2_RESUME`
* `S3_RESUME` - Equivalent to `BOOT_ON_S3_RESUME`
* `S4_RESUME` - Equivalent to `BOOT_ON_S4_RESUME`
* `S5_RESUME` - Equivalent to `BOOT_ON_S5_RESUME`
* `FLASH_UPDATE` - Equivalent to `BOOT_ON_FLASH_UPDATE`
* `RECOVERY_FULL` - Equivalent to `BOOT_IN_RECOVERY_MODE`
* `BOOT_MFG_MODE` - Equivalent to `BOOT_WITH_MFG_MODE_SETTINGS`
* `UNDEFINED` - This will appear when a UEFI Distribution Package tool was
  unable to parse the comment (spelling error) when creating a distribution
  package and the tool installed the distribution package using this value.

The following items are defined as special boots that may use the bit field
values: 100001b - 111111b per the PI PEI specification, table 6 Since this
comment block is informational, no attempt is made to map these items to
specific bit patterns.

* `RECOVERY_MINIMAL`
* `RECOVERY_NO_CHANGE`
* `RECOVERY_DIAGNOSTICS`
* `RECOVERY_DEFAULT`
* `RECOVERY_S2_RESUME`
* `RECOVERY_S3_RESUME`
* `RECOVERY_S4_RESUME`
* `RECOVERY_S5_RESUME`
* `RECOVERY_FLASH_UPDATE`

The Hand-Off Block types refer to the various HOBs as defined by the PI
specification, HOB Code Definitions. Modules that use GetHobList() and
CreateHob() should list this content.

* `PHIT` - the Phase Handoff Information Table (PHIT) Hob
* `MEMORY_ALLOCATION` - Describes all memory ranges
* `LOAD_PEIM` - This refers to EFI_HOB_TYPE_LOAD_PEIM_UNUSED
* `RESOURCE_DESCRIPTOR` - describes resource properties
* `FIRMWARE_VOLUME`, `FIRMWARE_VOLUME2` - location and type of firmware volumes
* `MEMORY_POOL` - describes memory pool allocations
* `GUID_TYPE` - for HOB types not define by the PI specification
* `UEFI_CAPSULE` - describes UEFI capsule memory pages
* `CPU` - describes processor information
* `UNUSED` - the HOB's content can be ignored
* `UNDEFINED` - This will appear when a UEFI Distribution Package tool was
   unable to parse the comment (spelling error) when creating a distribution
   package and the tool installed the distribution package using this value.

#### Prototype

```c
<FixedCommentSection> ::= <ValidArchitectures>?
                          <EventSection>?
                          <BootModeSection>? <HobSection>*
<ValidArchitectures>  ::= "#" <EOL>
                          "#" <TS> "VALID_ARCHITECTURES" <Eq> <ArchL>
                          ["#" <EOL>]
<ArchL>               ::= <Arch> [<Space> <Arch>]* [<EbcCmt>] <EOL>
<EbcCmt>              ::= <Space> "(EBC is for build only)"
<EventSection>        ::= "#" <TS> "[Event]" <EOL> <EventBlock>*
<CommonDescription>   ::= "#" <TS> "##"<EOL>
                          ["#" <TS> "#" <TS> <Description> <EOL>]+ "#" <TS>
                          "#"<EOL>
<EventBlock>          ::= <CommonDescription>*
                          "#" <TS> <EventType> <UsageField> <EOL>
<UsageField>          ::= <TS> "##" <TS> <Usage>
<EventType>           ::= {"EVENT_TYPE_PERIODIC_TIMER"}
                          {"EVENT_TYPE_RELATIVE_TIMER"} {"UNDEFINED"}
<Usage>               ::= {"CONSUMES"} {"SOMETIMES_CONSUMES"}
                          {"PRODUCES"} {"SOMETIMES_PRODUCES"}
                          {"UNDEFINED"}
<Description>         ::= <AsciiString>
<BootModeSection>     ::= "#" <TS> "[BootMode]" <EOL> <BootModeBlock>*
<BootModeBlock>       ::= [<CommonDescription>]
                          "#" <TS> <BootModeType> <UsageField> <EOL>
<BootModeType>        ::= {"FULL"} {"BOOT_WITH_FULL_CONFIGURATION"}
                          {"MINIMAL"}
                          {"BOOT_WITH_MINIMAL_CONFIGURATION"}
                          {"NO_CHANGE"}
                          {"BOOT_ASSUMING_NO_CONFIGURATION"}
                          {"DIAGNOSTICS"}
                          {"BOOT_WITH_FULL_CONFIGURATION_PLUS_DIAGNOTICS"}
                          {"DEFAULT"} {"BOOT_WITH_DEFAULT_SETTINGS"}
                          {"S2_RESUME"} {"BOOT_ON_S2_RESUME"} {"S3_RESUME"}
                          {"BOOT_ON_S3_RESUME"}
                          {"S4_RESUME"} {"BOOT_ON_S4_RESUME"}
                          {"S5_RESUME"} {"BOOT_ON_S5_RESUME"}
                          {"FLASH_UPDATE"} {"BOOT_ON_FLASH_UPDATE"}
                          {"BOOT_MFG_MODE"} {"BOOT_WITH_MFG_MODE_SETTINGS"}
                          {"RECOVERY_FULL"} {"BOOT_IN_RECOVERY_MODE"}
                          {"RECOVERY_MINIMAL"}
                          {"RECOVERY_NO_CHANGE"}
                          {"RECOVERY_DIAGNOSTICS"}
                          {"RECOVERY_DEFAULT"} {"RECOVERY_S2_RESUME"}
                          {"RECOVERY_S3_RESUME"}
                          {"RECOVERY_S4_RESUME"}
                          {"RECOVERY_S5_RESUME"}
                          {"RECOVERY_FLASH_UPDATE"} {"UNDEFINED"}
<HobSection>          ::= "#" <TS> "[Hob" [<attribs>] "]" <EOL>
                          <HobBlock>*
<attribs>             ::= <attr> ["," <TS> "Hob" <attr>]*
<attr>                ::= "." <arch>
<HobBlock>            ::= [<CommonDescription>]
                          "#" <TS> <HobType> <UsageField> <EOL>
<HobType>             ::= {"PHIT"} {"MEMORY_ALLOCATION"} {"LOAD_PEIM"}
                          {"RESOURCE_DESCRIPTOR"} {"FIRMWARE_VOLUME"}
                          {"FIRMWARE_VOLUME2"} {"MEMORY_POOL"}
                          {"GUID_TYPE"} {"UEFI_CAPSULE"} {"CPU"}
                          {"UNUSED"} {"UNDEFINED"}
```

#### Parameters

**_Event Usage_**

* `CONSUMES` - The module registers a notification function and requires that it
  be executed for the module to fully function.
* `SOMETIMES_CONSUMES` - A module registers a notification function and calls
  the function when it is signaled.
* `PRODUCES` - A module will always signal the event.
* `SOMETIMES_PRODUCES` - A module will sometimes signal the event.

**_Boot Mode Usage_**

* `CONSUMES` - The module always supports the given boot mode.
* `SOMETIMES_CONSUMES` - The module may support a given boot mode on some
  execution paths.
* `PRODUCES` - The module will change the boot mode.
* `SOMETIMES_PRODUCES` - The module will change the boot mode on some execution
  paths.

**_Hob Usage_**

* `CONSUMES` - The HOB must be present in the system.
* `SOMETIMES_CONSUMES` - If present, the HOB will be used.
* `PRODUCES` - A module will always produce the HOB.
* `SOMETIMES_PRODUCES` - The HOB may be produced by the module under some
  execution paths.

**_Usage_**

* Keywords are: `UNDEFINED`, `CONSUMES`, `SOMETIMES_CONSUMES`, `PRODUCES` and
  `SOMETIMES_PRODUCES`

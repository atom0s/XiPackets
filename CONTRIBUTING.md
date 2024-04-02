## Table of Contents

1. [General](#general)
2. [License](#license)
3. [Legal](#legal)
4. [Safe Reference Material](#safe-reference-material)
5. [Getting Started](#getting-started)
    - [Tooling Information](#tooling-information)
6. [Issues and Pull Requests](#issues-and-pull-requests)
7. [Naming Conventions](#naming-conventions)
    - [Naming Priority](#naming-priority)
    - [Function Names](#function-names)
    - [Pointer Names](#pointer-names)
    - [Type Names](#type-names)
8. [Pseudo Code Styling](#pseudo-code-styling)

# General

Thank you for your interest in contributing to the XiPackets repository!

Before you begin submitting anything, we do ask that you take the time to review this document and make yourself aware of the various guidelines we ask all contributors to follow. Failing to follow the guidelines we have set in place will result in us asking you to change your pulls or have them flat out declined/closed. 

Keeping the information on this repository clean, clear, and understandable is our goal.

# License

XiPackets is released under the AGPL [GNU Affero General Public License v3.0](/LICENSE.md)

The information found within this repository is intended for the benefit of the community, as a whole. It is not intended to be used to develop private tools or be sold/used for profit.

# Legal

This repository and its work is for educational purposes only.

We (contributors) do not claim ownership of any copyright content related to, or associated with, Final Fantasy XI.

```
(c) 2002-2024 SQUARE ENIX CO., LTD. All Rights Reserved. Title Design by Yoshitaka Amano.
FINAL FANTASY, TETRA MASTER and VANA'DIEL are registered trademarks of Square Enix Co., Ltd. SQUARE ENIX, 
PLAYONLINE and the PlayOnline logo are trademarks of Square Enix Co., Ltd.
```

The reverse engineering done by this repository and its contributors is entirely 'clean room'. 

We DO NOT have or use any leaked source code or other unpublished material. By contributing to this repository, you agree to the following:

  * You **have not** been and **are not** employeed by Square Enix, in any capacity.
  * You **do not** and **have never** had any leaked material from Square Enix related to FFXI in any manner.
  * You **do not** and **have never** referenced any leaked or otherwise unreleased material from Square Enix related to FFXI in any manner.

We **DO NOT** claim ownership of any material or information gathered through the means of reverse engineering the client and its files for this purpose.

# Safe Reference Material

XiPackets is developed entirely 'clean-room' as stated above. Because of this, we **DO NOT** allow the use of any illegal or private source material(s) to further develop the information shared here. However, there are some materials that are safe and legal to make use.

Several beta editions of FFXI were released with their debug symbols left in. This information includes things such as:

  * Function names.
  * Pointer names. _(Names of global objects, pointer types, etc.)_
  * Structure layouts, namings, etc.
  * etc.

This information is considered safe and valid to reference. _(We encourage it!)_

Any material that is closed source, private, illegally obtained, leaked, etc. is **NOT** considered safe and **SHOULD NOT** be referenced when submitting anything to this repository!

_If you have any material you are unsure of being safe to reference, you can contact @atom0s for further assistance with determining if the material is ok to use._

# Getting Started

Before you plan to submit any issues or pull requests, we do ask that you get familiar with the repository first and the various file formats/layouts being used. The information presented in this repository is generally kept organized in various specific manners, and we do ask that any pull requests or changes being submitted follow the same formats.

If you feel a very large change should take place, then please open an issue for discussion as a proposal. 

## Tooling Information

If you are interested in reverse engineering or working with any of the information in this repository, then here is a list of tools we suggest making use of or getting familiar with:

  * Cheat Engine (Debugger / Disassembler / Memory Editor) - [https://www.cheatengine.org/](https://www.cheatengine.org/)
  * Ghidra (Debugger / Disassembler / Decompiler) - [https://ghidra-sre.org/](https://ghidra-sre.org/)
  * IDA (Debugger / Disassembler / Decompiler) - [https://hex-rays.com/](https://hex-rays.com/)
  * SweetScape 010 Editor (Hex Editor / Binary Templates) - [https://www.sweetscape.com/010editor/](https://www.sweetscape.com/010editor/)

For editing the documentation found in this repository, we recommend the following:

  * VSCode - [https://code.visualstudio.com/](https://code.visualstudio.com/)
    * Extension: Instant Markdown

For working with pseudo code, we recommend any modern C/C++ editor that supports `.clang-format` configurations for styling.

  * Visual Studio 2022
  * VSCode, Atom, Sublime Editor, Notepad++
  * JetBrains Rider

# Issues and Pull Requests

If you are looking to submit a new issue or pull request, we ask that you follow a few guidelines first.

1. Be sure to search for any existing issue or pull request that matches yours.
    * _If one does already exist, please add your information to the existing issue/pull request._
2. Be sure to use a clear, descriptive title.
    * _Your submission should have a clear and to the point title explaining what the issue is about, or what the pull is intended to change/fix._
3. Be sure to use the included templates, if one exists.
    * _When submitting issues and pulls, there may be an existing template you can use. Please be sure to use them and properly fill them out when submitting anything._

When submitting a new issue or pull request, please be sure to include as much detail as possible. Please do not be vague or non-responsive if you are asked to give more details, otherwise your submissions will be rejected. 

If you are submitting a change/fix, please explain why. If you are submitting new information, new reversed data, or similar, please show your work, and be informative. Please do not guess at what things are/do. If you are unsure of parts of what you have reversed, please be clear about it. It is not helpful for the project to have guessed namings or similar that are potentially wrong and misleading. It is better to mark the data as unknown and needs to be further researched instead.

# Naming Conventions

To assist with keeping the repository organized, we do ask that you follow our naming conventions and code styling. This helps keep all pages consistent and readable with ease. The following information should help outline how to name various things and what naming should take priority.

## Naming Priority

Given the ability to reference [safe material](#safe-reference-material), we do encourage the usage of official namings whenever possible. In some cases, official naming is not the best though, and instead we do sometimes prefer personal naming choices that are either more clear to the point, or easier to understand.

Official naming should generally always be your first choice when determining how to name anything though. _(Also be sure to follow the specific type guidelines listed below!)_

_If you are ever unsure of a naming to use, feel free to open an issue and begin a discussion for it._

### Function Names

  * Function names should always be prefixed with `FUNC_`, even if using an official name.
  * Function names should be sanitized for use with tools like IDA / Ghidra.
    * If a function is from a class, ie. `XiEvent::XiEventInit`, it should be named `FUNC_XiEvent_XiEventInit`
    * If a function has multiple copies (overloads), you should use a numeric suffix. (ie. `FUNC_VCalibrate1`, `FUNC_VCalibrate2`, etc.)

If you are unaware of an official naming for a function, then it is best to name it with a namespace. For example, each opcode handler does not have an official naming currently known, so they are named in the format of: `FUNC_XiEvent_OpCode_0x0000`

  * `FUNC_` - The required prefix following the guidelines above.
  * `XiEvent_` - The namespace/parent that makes use of the function.
  * `OpCode_0x0000` - The name that best describes the function, when an official name isn't available.

Another example, for when an official name is not ideal, would be: `cmdf_COM_RECAST` This is the official function name for the `/recast` command handler. Since there is not a known handler name for all commands as of current retail, and these names are not really the best we can opt. to make use of custom naming instead. For example, my personal choice for naming the command handlers is like this: `FUNC_Commands_Recast`

_If you rename a function that has an official known name, please be sure to add a comment above the function stating what the official, unedited, name is._

### Pointer Names

_Pointer namings are used for objects, globals, arrays, etc._

  * Pointer names should always be prefixed with `PTR_`, even if using an official name.
  * Pointer names should be sanitized for use with tools like IDA / Ghidra.
    * If a pointer is from within a namespace, ie. `YmSoundElem::sound_command_send`, it should be named `YmSoundElem_sound_command_send`
  * Globals that the game prefixes with `g_` should still be prefixed with `PTR_`. (ie. `g_xiFileManager` should be named `PTR_g_xiFileManager`)
  * Unknown pointers in pseudo code should be named `PTR_UnknownObject` or similar. _(Best to leave a comment explaining what it is/potentially is as well.)_

_If you rename a pointer that has an official known name, please be sure to add a comment above the pointer stating what the official, unedited, name is._

### Type Names

_Type names are used for class/structure fields and similar._

  * Type names should generally be kept official when possible, but often times, renaming is more ideal.
    * If renaming an official known type, please be sure to leave an inline comment next to the rename with the official name.
  * Unknown fields should be suffixed with `0000` incrementing upward, in decimal. (ie. `Unknown0000`, `Unknown0001`, etc. )

Some examples of renames would be:

  * `Actornumber` - Renamed to: `ServerId`
  * `ActorIndex` - Renamed to: `TargetIndex`
  * `Actor` is generally referred to as `Entity` in the third-party community.

Please be careful when updating `Unknown` data. Several docs make use of the `Unknown` fields as-is, so renaming anything should be carefuly checked and updated across **ALL** documentation. If you edit and shift `Unknown` values, you need to make sure all existing docs are updated accordingly to match what you have changed and edited!

# Pseudo Code Styling

When submitting any kind of pseudo code, we ask that you take the time and follow our styling. Pseudo code can be written to follow C++20 standards.

You can find a `.clang-format` file in the main folder of the repo if you wish to style your code externally and copy it into any docs later.

### White Space

We use 4 spaces instead of tabs. You should **NEVER** use tabs with anything submitted to this repository. _They are the devil._

There should be spacing between keywords and parenthesis. And no spacing inside of the parenthesis.

```cpp
// Correct ✔️
if (x)
if (x >= y)

// Wrong ❌
if(x)
if( x >= y )
if ( x >= y )
```

Parameters should be spaced and not compressed.

```cpp
// Correct ✔️
FUNC_Derp(1, 2, 3, 4, 5, 6);

// Wrong ❌
FUNC_Derp(1,2,3,4,5,6);
```

Lists should have spaces before/after the data within the brackets.

```cpp
// Correct ✔️
std::vector<int32_t> derp = { 1, 2, 3, 4 };
std::vector<int32_t> derp{ 1, 2, 3, 4 };

// Wrong ❌
std::vector<int32_t> derp = {1, 2, 3, 4};
std::vector<int32_t> derp = {1,2,3,4};
std::vector<int32_t> derp {1, 2, 3, 4};
std::vector<int32_t> derp {1,2,3,4};
```

### Pointer Alignment

Pointer and reference symbols should be left-aligned.

```cpp
// Correct ✔️
void FUNC_Derp(xievent_t* this);
void FUNC_Derp(xievent_t& this);

// Wrong ❌
void FUNC_Derp(xievent_t * this);
void FUNC_Derp(xievent_t & this);
void FUNC_Derp(xievent_t *this);
void FUNC_Derp(xievent_t &this);
```

### Brackets

Brackets should always be placed on a new line. Brackets should also always be used in large clusters of code instead of single line conditions.

```cpp
// Correct ✔️
if (FUNC_SomeCall())
{
    FUNC_DoSomething();
}
else
{
    FUNC_DoSomething1();
    FUNC_DoSomething2();
    FUNC_DoSomething3();
}

// Wrong ❌
if (FUNC_SomeCall())
    FUNC_DoSomething();
else
{
    FUNC_DoSomething1();
    FUNC_DoSomething2();
    FUNC_DoSomething3();
}

// Wrong ❌
if (FUNC_SomeCall()) {
    FUNC_DoSomething();
} else {
    FUNC_DoSomething1();
    FUNC_DoSomething2();
    FUNC_DoSomething3();
}
```

Brackets used for switch casing should be aligned to the case label. `break`, `continue` and `return` statements should be within the brackets. Brackets should generally always be present.

```cpp
// Correct ✔️ 
switch (x)
{
    case 0:
    {
        return;
    }
}

// Wrong ❌
switch (x)
{
case 0:
{
    return;
}
}

// Wrong ❌
switch (x)
{
case 0:
    return;
}


// Wrong ❌
switch (x)
{
    case 0:
    {
    }
    return;
}
```

### Short Functions

Short / small functions should always be written out. They should not be inlined into a single line.

```cpp
// Correct ✔️ 
void FUNC_Derp(void)
{
    FUNC_Herp();
}

// Wrong ❌
void FUNC_Derp(void) { FUNC_Herp(); }
```


### Switch Statements

Along with the bracket rules, switch statements should always contain a `default` entry.

### Other Stylings

If you are unsure of how to style something, please reference the existing material for ideas. Otherwise, feel free to open an issue to ask.

---
title: C++ styleguide
description: My personal preferred C++ style.
author: Mathijs
date: 2025-11-23 11:57:00 +0100
categories: [Development]
tags: []
pin: false
math: false
mermaid: false
---

## General formatting

- Indentation: 4 spaces, no tabs
- Braces: Allman style
- Line length: ≤ 100 characters
- Pointer alignment: Right (int* x)
- Spaces:
  - One space after keywords (if, for)
  - No space in function call parentheses
  - Spaces around binary operators

```cpp
if (x == 10)
{
    move(x + y);
}
```


## Naming conventions

| Entity         | Style                | Example            |
| -------------- | -------------------- | ------------------ |
| Class          | UpperCamelCase       | `PlayerCharacter`  |
| Struct         | UpperCamelCase       | `PlayerStats`      |
| Private member | lowerCamelCase + `_` | `health_`          |
| Public member  | lowerCamelCase       | `score`            |
| Functions      | lowerCamelCase       | `updatePosition()` |
| Variables      | lowerCamelCase       | `playerCount`      |
| Constants      | kCamelCase           | `kMaxLives`        |
| Enum           | UpperCamelCase       | `WeaponType`       |
| Enum values    | UPPERCASE            | `SWORD`            |
| Namespace      | UpperCamelCase       | `SWORD`            |

```
BasedOnStyle: LLVM

### Indentation
IndentWidth: 4
TabWidth: 4
UseTab: Never
ContinuationIndentWidth: 4

### Braces — Allman Style
BreakBeforeBraces: Allman

BraceWrapping:
  AfterClass: true
  AfterControlStatement: true
  AfterEnum: true
  AfterFunction: true
  AfterNamespace: true
  AfterStruct: true
  AfterUnion: true
  BeforeCatch: true
  BeforeElse: true
  SplitEmptyFunction: false
  SplitEmptyRecord: false
  SplitEmptyNamespace: false

AllowShortFunctionsOnASingleLine: Empty
AllowShortBlocksOnASingleLine: Never
AllowShortIfStatementsOnASingleLine: Never

### Line Length
ColumnLimit: 100

### Pointers & References
PointerAlignment: Right   # int* x

### Spaces
SpaceBeforeParens: ControlStatements
SpaceInEmptyParentheses: false
#SpacesInParens: false #not supported in older clang
SpacesInCStyleCastParentheses: false
SpacesInAngles: false

### Alignments
AlignConsecutiveAssignments: true
AlignConsecutiveDeclarations: true
AlignOperands: true
AlignTrailingComments: true

### Includes
SortIncludes: true
IncludeBlocks: Regroup

### Comments
ReflowComments: true

```

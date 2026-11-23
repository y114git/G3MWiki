# Decompiler settings

The **Decompiler** category sets how G3MStudio writes GML when it decompiles code. It has one row for each setting of the decompiler library. A code tab shows a change the next time its text is made.

Switches are checkboxes. Text values are boxes that save when you leave them. The label is the setting name written as words.

A value equal to the default is not stored, so `decompiler` in [settings.json](settings-file.md) holds only what you changed. A value that does not fit is ignored and the default stays.

## Settings

| Label | Key | Default |
| --- | --- | --- |
| **Indent string** | `IndentString` | four spaces (may be empty) |
| **Use semicolon** | `UseSemicolon` | true |
| **Use csscolors** | `UseCSSColors` | true (writes colors as `#RRGGBB`) |
| **Print warnings** | `PrintWarnings` | true (decompiler warnings as comments) |
| **Macro declarations at top** | `MacroDeclarationsAtTop` | false |
| **Empty line after block locals** | `EmptyLineAfterBlockLocals` | true |
| **Empty line around enums** | `EmptyLineAroundEnums` | true |
| **Empty line around branch statements** | `EmptyLineAroundBranchStatements` | false |
| **Empty line before switch cases** | `EmptyLineBeforeSwitchCases` | false |
| **Empty line after switch cases** | `EmptyLineAfterSwitchCases` | false |
| **Empty line around function declarations** | `EmptyLineAroundFunctionDeclarations` | true |
| **Empty line around static initialization** | `EmptyLineAroundStaticInitialization` | true |
| **Open block brace on same line** | `OpenBlockBraceOnSameLine` | false |
| **Remove single line block braces** | `RemoveSingleLineBlockBraces` | false |
| **Cleanup try** | `CleanupTry` | true |
| **Cleanup else to continue** | `CleanupElseToContinue` | true |
| **Cleanup default argument values** | `CleanupDefaultArgumentValues` | true |
| **Cleanup builtin array variables** | `CleanupBuiltinArrayVariables` | true |
| **Cleanup data structure accessors** | `CleanupDataStructureAccessors` | true |
| **Cleanup struct variable accessors** | `CleanupStructVariableAccessors` | true |
| **Cleanup local var declarations** | `CleanupLocalVarDeclarations` | true |
| **Cleanup template strings** | `CleanupTemplateStrings` | true |
| **Create enum declarations** | `CreateEnumDeclarations` | true |
| **Unknown enum name** | `UnknownEnumName` | `UnknownEnum` |
| **Unknown enum value pattern** | `UnknownEnumValuePattern` | `Value_{0}` |
| **Unknown argument name pattern** | `UnknownArgumentNamePattern` | `arg{0}` |
| **Allow leftover data on stack** | `AllowLeftoverDataOnStack` | true |

The labels come from the setting names, so the exact capitalization on screen is the first word capitalized and the rest lowercase.

## What the text values accept

- **Indent string** may be empty. Other text values may not be empty.
- A pattern (**Unknown enum value pattern**, **Unknown argument name pattern**) must contain `{0}`.
- **Unknown enum name** must start with a letter or underscore and hold only letters, digits and `_`.

A box with an invalid text returns to the last good value.

## What the cleanups do

| Setting | Effect |
| --- | --- |
| **Cleanup try** | Cleans up the compiler-generated flow of try, catch and finally. Can occasionally differ from the code that really runs. |
| **Cleanup else to continue** | Rewrites an empty if/else chain at the end of a loop as `continue` where possible. |
| **Cleanup default argument values** | Detects GMLv2 default argument values. |
| **Cleanup builtin array variables** | Writes built-in array variables without the generated index 0 accessor. |
| **Cleanup data structure accessors** | Writes canonical data structure getter and setter calls with accessor syntax. |
| **Cleanup struct variable accessors** | Writes `variable_struct_get` and `variable_struct_set` with struct accessor syntax. |
| **Cleanup local var declarations** | Rewrites local variable declarations to be as readable as possible. |
| **Cleanup template strings** | Rewrites string calls as template strings when the game version allows. |
| **Create enum declarations** | Declares enum values found in a code entry, including unknown ones, at the top of the code. |
| **Allow leftover data on stack** | On: leftover data on the VM stack at the end of a fragment adds a warning. Off: decompiling that entry fails with an error. |

## Restore defaults

Clears `decompiler`, so every setting returns to the library default.

## See also

- [Code editor](../code/code-editor.md)
- [Code entries](../code/code-entries.md)
- [Default values](defaults-table.md)

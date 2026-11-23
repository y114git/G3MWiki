# Extensions

An extension bundles native files and the functions GML code can call from them. This page covers the extension tab.

## Outline

The top line reads `Extension {name} {version}: {files} files, {options} options`. Under it come two sections.

### Files

Each file is a collapsible block headed `filename (Kind)`. Opening it shows the file's property list, then its functions. Each function is a block headed `name(argument types) : return type`, and opens into its property list and its arguments, listed as `[index] ...`. With no files: `This extension has no files yet. Add one below.`

- **Add file** adds a file of the kind chosen in the box beside it (the kinds the data file defines, such as DLL).
- **Add function** adds a function at the end of a file.
- **Add argument** adds an argument at the end of a function. A function takes at most 17 arguments; beyond that the edit is refused.

### Options

Each option is a block headed `name = value`; opening it shows its property list. **Add option** adds one. With none: `This extension has no options.`

## Moving and removing

Every file, function, argument and option row has **Move up**, **Move down** and **Remove**. Up is disabled on the first entry and down on the last. Each is one undo step.

Removing a function that code calls is a risky edit: G3MStudio asks first and removes it only if you confirm. A failed edit leaves the data unchanged.

Extensions cannot be duplicated. They can be created (**File** > **New**) and deleted. See [Resource management](resources.md).

## Add a function from a DLL

1. Choose **File** > **New** > **New Extension...**, type a name and press **Create**.
2. Pick the kind in the box beside **Add file** and press **Add file**. Open the new block and set the file name and the other fields in its property list.
3. Press **Add function** inside that file. Set its name, the name the file exports and the return type in its property list.
4. Press **Add argument** once for each argument and set its type. A function takes at most 17 arguments.
5. Use **Move up** and **Move down** to put files, functions and arguments in the order you want.
6. Save the data file. Call the function by name from GML.

## See also

- [Property list](property-grid.md)
- [Resource management](resources.md)

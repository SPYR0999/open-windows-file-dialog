# open-windows-file-dialog [![NPM version](https://img.shields.io/npm/v/open-windows-file-dialog.svg?style=flat)](https://www.npmjs.com/package/open-windows-file-dialog) [![NPM monthly downloads](https://img.shields.io/npm/dm/open-windows-file-dialog.svg?style=flat)](https://npmjs.org/package/open-windows-file-dialog) [![NPM total downloads](https://img.shields.io/npm/dt/open-windows-file-dialog.svg?style=flat)](https://npmjs.org/package/open-windows-file-dialog)

> Programmatically open a file dialog window (explorer) for picking files. Only works on Windows. Also see: open-finder-dialog, open-linux-file-dialog, and open-file-manager-dialog for other platforms.

Please consider following this project's author, [Jon Schlinkert](https://github.com/jonschlinkert), and consider starring the project to show your :heart: and support.

## Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm install --save open-windows-file-dialog
```

## Usage

```typescript
import { openWindowsFileDialog } from 'open-windows-file-dialog';
// or
import openWindowsFileDialog from 'open-windows-file-dialog';
```

## API

### Main Function

**openWindowsFileDialog**

```typescript
export const openWindowsFileDialog = async (
  filepath?: string,
  options?: FileDialogOptions
): Promise<FileDialogResult>;
```

Opens a Windows file dialog to select files and returns detailed results including selected files and cancellation status.

### Convenience Function

**openWindowsFileDialogSync**

```typescript
export const openWindowsFileDialogSync = async (
  filepath?: string,
  options?: FileDialogOptions
): Promise<string[]>;
```

Returns only the array of selected file paths for simpler usage.

## Types

```typescript
interface FileDialogOptions {
  multiple?: boolean; // Allow multiple file selection (default: true)
  checkFileExists?: boolean; // Verify files exist (default: true)
  filter?: string; // File type filter (default: '*.*')
  title?: string; // Dialog title (default: 'Select File(s)')
  defaultExtension?: string; // Default file extension
}

interface FileDialogResult {
  files: string[]; // Array of selected file paths
  canceled: boolean; // Whether the dialog was canceled
}
```

## Examples

### Basic Usage

```typescript
// Open dialog in current directory with default options
const result = await openWindowsFileDialog();
console.log('Selected files:', result.files);
console.log('Was canceled:', result.canceled);

// Simple version that returns only file paths
const files = await openWindowsFileDialogSync();
console.log('Selected files:', files);
```

### Custom Directory

```typescript
// Open dialog in specific directory
const result = await openWindowsFileDialog('C:\\Users\\Documents');
```

### Single File Selection

```typescript
// Allow only single file selection
const result = await openWindowsFileDialog(undefined, {
  multiple: false,
  title: 'Select a single file'
});
```

### File Type Filtering

```typescript
// Filter for text files only
const result = await openWindowsFileDialog(undefined, {
  filter: '*.txt',
  title: 'Select Text Files'
});

// Multiple file types with custom format
const result = await openWindowsFileDialog(undefined, {
  filter: 'Text files (*.txt)|*.txt|All files (*.*)|*.*',
  title: 'Select Files'
});

// Image files
const result = await openWindowsFileDialog(undefined, {
  filter: '*.jpg;*.png;*.gif',
  title: 'Select Images'
});
```

### Advanced Options

```typescript
const result = await openWindowsFileDialog('C:\\Projects', {
  multiple: true,
  checkFileExists: true,
  filter: 'JavaScript files (*.js)|*.js|TypeScript files (*.ts)|*.ts',
  title: 'Select Source Files',
  defaultExtension: 'js'
});

if (!result.canceled) {
  console.log(`Selected ${result.files.length} files:`);
  for (const file of result.files) {
    console.log(`  ${file}`);
  }
}
```

## Parameters

### filepath (optional)

The initial directory where the dialog should open. Defaults to the current working directory.

* Type: `string`
* Default: `process.cwd()`

### options (optional)

Configuration object for the file dialog behavior.

#### options.multiple

Allow selection of multiple files.

* Type: `boolean`
* Default: `true`

#### options.checkFileExists

Verify that selected files actually exist on the filesystem.

* Type: `boolean`
* Default: `true`

#### options.filter

File type filter string. Can be in simple format (`*.txt`) or Windows filter format (`Text files (*.txt)|*.txt`).

* Type: `string`
* Default: `'*.*'` (all files)

#### options.title

Title displayed in the dialog window.

* Type: `string`
* Default: `'Select File(s)'`

#### options.defaultExtension

Default file extension to append if user doesn't specify one.

* Type: `string`
* Default: `''` (empty)

## Requirements

* Windows operating system
* PowerShell must be available and executable
* .NET Framework (for Windows Forms components)

## Error Handling

The functions throw `FileDialogError` in these cases:

* PowerShell is not available on the system
* Invalid filepath parameter (non-string)
* PowerShell execution errors
* Dialog timeout (after 5 minutes)

```typescript
try {
  const result = await openWindowsFileDialog();
  // Handle successful result
} catch (error) {
  if (error instanceof FileDialogError) {
    console.error('File dialog error:', error.message);
  }
}
```

## Related

You might also be interested in:

* [open-file-manager](https://www.npmjs.com/package/open-file-manager): Cross-platform utility to open a file or directory in the system's default file manager (Finder… [more](https://github.com/jonschlinkert/open-file-manager) | [homepage](https://github.com/jonschlinkert/open-file-manager "Cross-platform utility to open a file or directory in the system's default file manager (Finder, Explorer, Nautilus, etc.)")
* [open-finder-dialog](https://www.npmjs.com/package/open-finder-dialog): Open a finder dialog window (finder prompt) programmatically. Only works on MacOS. | [homepage](https://github.com/jonschlinkert/open-finder-dialog "Open a finder dialog window (finder prompt) programmatically. Only works on MacOS.")
* [open-linux-file-dialog](https://www.npmjs.com/package/open-linux-file-dialog): Open a file dialog window programmatically to allow the user to select one or more… [more](https://github.com/jonschlinkert/open-linux-file-dialog) | [homepage](https://github.com/jonschlinkert/open-linux-file-dialog "Open a file dialog window programmatically to allow the user to select one or more files. Only works on Linux. No dependencies. Supports zenity (GNOME), kdialog (KDE), yad (Yet Another Dialog), qarma (Qt-based), matedialog (MATE), rofi (window switcher wi")

## About

<details>
<summary><strong>Contributing</strong></summary>

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](../../issues/new).

</details>

<details>
<summary><strong>Running Tests</strong></summary>

Running and reviewing unit tests is a great way to get familiarized with a library and its API. You can install dependencies and run tests with the following command:

```sh
$ npm install && npm test
```

</details>

<details>
<summary><strong>Building docs</strong></summary>

_(This project's readme.md is generated by [verb](https://github.com/verbose/verb-generate-readme), please don't edit the readme directly. Any changes to the readme must be made in the [.verb.md](.verb.md) readme template.)_

To generate the readme, run the following command:

```sh
$ npm install -g verbose/verb#dev verb-generate-readme && verb
```

</details>

### Author

**Jon Schlinkert**

* [GitHub Profile](https://github.com/jonschlinkert)
* [Twitter Profile](https://twitter.com/jonschlinkert)
* [LinkedIn Profile](https://linkedin.com/in/jonschlinkert)

### License

Copyright © 2025, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT License](LICENSE).

***

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.8.0, on May 25, 2025._
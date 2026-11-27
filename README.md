# open-windows-file-dialog [![NPM version](https://img.shields.io/npm/v/open-windows-file-dialog.svg?style=flat)](https://www.npmjs.com/package/open-windows-file-dialog) [![NPM monthly downloads](https://img.shields.io/npm/dm/open-windows-file-dialog.svg?style=flat)](https://npmjs.org/package/open-windows-file-dialog) [![NPM total downloads](https://img.shields.io/npm/dt/open-windows-file-dialog.svg?style=flat)](https://npmjs.org/package/open-windows-file-dialog)

> Programmatically open a file dialog window (explorer) for picking files. Only works on Windows. Also see: open-finder-dialog, open-linux-file-dialog, and open-file-manager-dialog for other platforms. This fork adds a simple folder picker as well.

Please consider following this project's original author, [Jon Schlinkert](https://github.com/jonschlinkert).

## Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm install github:SPYR0999/open-windows-file-dialog
```

## Usage

```typescript
import { openWindowsFileDialog, openWindowsFolderDialog } from 'open-windows-file-dialog';
// or
import openWindowsFileDialog from 'open-windows-file-dialog'; // Only file picker is available as default export
```

## API

### Main Functions

**openWindowsFileDialog**

```typescript
export const openWindowsFileDialog = async (
  filepath?: string,
  options?: FileDialogOptions
): Promise<FileDialogResult>;
```

Opens a Windows file dialog to select files and returns detailed results including selected files and cancellation status.

```typescript
export const openWindowsFolderDialog = async (
  filepath?: string,
  options?: FolderDialogOptions
): Promise<FolderDialogResult>;
```

Opens a Windows file dialog that allows selecting folders.

### Convenience Functions

**openWindowsFileDialogSync**

```typescript
export const openWindowsFileDialogSync = async (
  filepath?: string,
  options?: FileDialogOptions
): Promise<string[]>;
```

Returns only the array of selected file paths for simpler usage.

```typescript
export const openWindowsFolderDialogSync = async (
  filepath?: string,
  options?: FileDialogOptions
): Promise<string[]>;
```

Returns only the array of selected folder path for simpler usage.

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

interface FolderDialogOptions {
  title?: string; // Dialog title (default: 'Select Folder')
}

interface FolderDialogResult {
  files: string || null; // Selected folder path
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

// Open folder dialog in current directory
const result = await openWindowsFolderDialog();
console.log('Selected folder:', result.folder);
console.log('Was canceled:', result.canceled);
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

### Author

**Jon Schlinkert**

* [GitHub Profile](https://github.com/jonschlinkert)
* [Twitter Profile](https://twitter.com/jonschlinkert)
* [LinkedIn Profile](https://linkedin.com/in/jonschlinkert)

### License

Copyright © 2025, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT License](LICENSE).

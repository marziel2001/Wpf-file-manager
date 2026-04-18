# WPF File Manager

A desktop file-manager application built as a university lab exercise, demonstrating core Windows Presentation Foundation (WPF) concepts such as TreeView, context menus, dialog windows, and Windows file-system APIs.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | C# 10 |
| UI framework | WPF (Windows Presentation Foundation) |
| Additional UI | Windows Forms (`FolderBrowserDialog`) |
| Target framework | .NET 6.0 (Windows) |
| Build system | MSBuild / `dotnet` CLI |

---

## Architecture

The project follows the **code-behind** pattern (no MVVM) and consists of two windows:

```
lab2_WPF_app/
├── App.xaml / App.xaml.cs          # Application entry-point
├── MainWindow.xaml / .cs           # Main window – TreeView file explorer
└── CreateFileWindow.xaml / .cs     # Modal dialog – create a file or directory
```

### MainWindow
- Renders the file-system hierarchy in a `TreeView`.
- Each node is built recursively: directories call `CreateTreeDirectory()`, files call `CreateTreeFile()`.
- A **context menu** on every node offers `Create` (directories) or `Open` (files) and `Delete`.
- A **ScrollViewer** on the right displays the contents of a selected text file.
- A **StatusBar** at the bottom shows the RAHS attribute flags (`r`, `a`, `h`, `s` or `-`) for the selected item.

### CreateFileWindow
- A modal dialog opened when the user chooses *Create* from a directory's context menu.
- The user types a name, picks *File* or *Directory* via radio buttons, and optionally checks any combination of **R**ead-only, **A**rchive, **H**idden, **S**ystem attributes.
- File names are validated against the regex `^[a-zA-Z0-9-~_]{1,8}\.(html|php|txt)$`.
- On confirmation the file/directory is created and the chosen `FileAttributes` mask is applied immediately.

---

## Features

- 📂 **Browse** any folder on disk via a recursive TreeView.
- ➕ **Create** a file (`.html`, `.php`, `.txt`) or a sub-directory inside any node.
- 🗑️ **Delete** a file or an entire directory tree (read-only flag is cleared automatically before deletion).
- 👁️ **Preview** the contents of any text file in the right-hand scroll panel.
- 🏷️ **RAHS attributes** – set them on creation and inspect them in the status bar.

---

## Requirements

- Windows 10 / 11
- [.NET 6.0 SDK](https://dotnet.microsoft.com/download/dotnet/6.0) (or the matching Runtime if you only want to run the app)

---

## How to Build & Run

```bash
# Restore and build
dotnet build lab2_WPF_app.sln

# Run directly
dotnet run --project lab2_WPF_app.csproj
```

Or open `lab2_WPF_app.sln` in **Visual Studio 2022** and press **F5**.

---

## How to Use

1. Launch the application.
2. Click **File → Open** and choose a directory – the tree is populated automatically.
3. **Expand** nodes to navigate sub-directories and files.
4. **Right-click a directory** to see:
   - *Create* – opens the *Create File/Directory* dialog.
   - *Delete* – removes the directory and all its contents.
5. **Right-click a file** to see:
   - *Open* – displays the file text in the right panel.
   - *Delete* – removes the file.
6. **Select any node** to see its RAHS attributes in the bottom status bar  
   (e.g. `r-h-` means Read-only and Hidden are set).
7. Click **File → Close** to exit.

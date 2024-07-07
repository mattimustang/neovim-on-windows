# Neovim on Windows

This is a guide to how I've set up Neovim on Windows 11 and, though it should if
I add more text here mostly work on Windows 10 as well.
As well as another long sentence here how does it wrap it all?

The result will be Neovim with:

- Windows Terminal as its UI.
- An application you can associate with file types in Windows.
- An `Edit in Neovim` menu item in the Windows 11 context menu in Explorer.
- Optional installation of `LazyVim` and supporting software to get `treesitter`
  syntax highlighting and LSPs functioning.

## Install Neovim

Prerequisites:

- Windows Terminal:
  This comes with Windows 11 or install using `winget install
  Microsoft.WindowsTerminal` on Windows 10.
- Custom Context Menu:
  Install (pay for it or use free trial)
  [Custom Context Menu - Download and install on Windows | Microsoft Store](https://apps.microsoft.com/detail/9pc7bzz28g0x?hl=en-us&gl=AU).
  This will allow you to customize the Windows 11 context menu.
  The Github and wiki here
  [ikas-mc/ContextMenuForWindows11](https://github.com/ikas-mc/ContextMenuForWindows11).
- A [Nerd Font](https://www.nerdfonts.com/).
  I use the `SauceCodePro Nerd Font` in the supplied configurations.

Install Neovim:

```PowerShell
winget install neovim.neovim
```
Copy the Neovim `favicon.ico` to `C:\Program Files\Neovim`.
You may put this in another directory but you will have to update the supplied
configurations to point at the new location.

## Themes

I'm using the [Flexoki](https://github.com/kepano/flexoki) theme which supports
both Windows Terminal and Neovim.
Feel free to choose your own.

## Configure Windows Terminal

Open Windows Terminal and open the Settings then click on `Open JSON file`.
Find the section with other terminal profiles and copy paste the contents of
[windows_terminal_profile.json](windows_terminal_profile.json).

Repeat this for [windows_terminal_theme.json](windows_terminal_theme.json)
putting it where the other themes are if you want to use the `Flexoki` theme.

You should now be able to launch Neovim from the Windows Terminal menu.

## Configure the Windows 11 Context Menu

Open the `Custom Context Menu` application.
Click on the folder icon for `Open Menu Config Folder` and copy these files into
the folder:

- Diff in Neovim.json
- Edit in Neovim.json
- Edit in Neovim [Safe Mode].json

You will need to sign out then sign in again or restart `explorer.exe` in Task
Manager for the change to take effect.

You should now have a second `Open with` menu.
You can customise the name of this with `Custom Context Menu`.

## Add Neovim as a Suggested App

To associate Neovim with different file types add it as a suggested application.

Import `HKCR_applications_nvim.reg` then restart `explorer.exe`.

## Associate File Types with Neovim

Open Windows `Settings/Apps/Default apps` and set Neovim to be the default for
files you want to use it for or you can do it when you open the file type.

For example:

```
.bash
.c
.cfg
.config
.cpp
.gitconfig
.gitignore
.h
.hcl
.inf
.ini
.java
.js
.json
.log
.lua
.markdown
.md
.php
.pl
.py
.rb
.sh
.tf
.tfvars
.txt
.vim
.yaml
.yml
```
## Optional: Configure the Windows 10 Context Menu

The Windows 10 context menu can also be configured by opening the
`win10_nvim_context_menu.reg` file and allowing `regedit` to import it.

## Optional: Install LazyVim

[LazyVim](https://www.lazyvim.org/) is a Neovim distribution that provides a
range of plugins, syntax highlighting using treesitter and LSPs.

It took me a while to get it working on Windows 11, particularly compiling the
treesitter parsers, so the steps below are here to document how I did it.

Prerequisites:

- Open a Windows PowerShell terminal and install these packages using `winget`:
    - Microsoft.PowerShell
    - Microsoft.Git
    - burntsushi.ripgrep.MSVC
    - sharkdp.fd
    - OpenJS.NodeJS - required by a few LSPs
    - 7zip.7zip - add `C:\Program Files\7-Zip` to System path
    - Python.Python.3.12
    - zig.zig - this is to be able to compile treesitter parsers.
- Close the terminal.
- Open a new PowerShell terminal and type `$Env:Path` and check that `C:\Program
  Files\Neovim\bin` there as well as zig.
- Optional:
  Edit the `PowerShell` terminal profile and change the font to your preferred
  Nerd Font.
- Follow the
  [LazyVim Windows installation instructions](https://www.lazyvim.org/installation).

If you want to `Flexoki` as your Neovim theme copy `theme.lua` to
`$Env:LOCALAPPDATA\nvim\lua\plugins`.

Now you can run `nvim` and you should see it download LazyVim, its plugins,
compiling treesitter parsers, and installing LSPs.

By default there are around 24 treesitter parsers that LazyVim includes and it
seems to not want to compile them all in one go.
Quitting and starting nvim again around 5-10 times seems to sort it out.

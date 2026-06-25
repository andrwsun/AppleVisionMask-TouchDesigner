# Drag + Drop AppleVisionMask Background Remover for TouchDesigner

A drag and drop TouchDesigner component that does real time person segmentation
and background removal using Apple's native Vision framework. The compiled plugin
is embedded inside the tox, so there is nothing to install.

Original plugin by Aaron Pereira:
https://github.com/aaronmylespereira/AppleVisionMask-TouchDesigner

This repository packages that work as a single self contained tox. Same MIT
license, free and open.

## Requirements

1. macOS on Apple Silicon (M1, M2, M3 or newer). Intel Macs and Windows are not supported.
2. TouchDesigner 2025 series (tested on 2025.32820).
3. A webcam, and camera permission for TouchDesigner the first time.

## How to use

1. Download AppleVisionMask.tox and drag it into any TouchDesigner project.
2. Wire a Video Device In TOP (your webcam) into the component input.
3. Allow camera access if asked. The mask appears on the output.

The component face has two parameters: a Setup / Reload pulse and a read only
Status string. It auto disables when nothing is wired to the input, so there is
no on off toggle. The plugin's own controls (Vision Quality, Output Mode,
Background Opacity) live on the inner C++ TOP.

## How it loads itself

The plugin binary is embedded in the component virtual file system. On first cook
it extracts to a per machine cache at ~/.td_plugin_cache/applevisionmask/, strips
the macOS quarantine flag, and loads itself. That cache is the only thing it
writes to disk, so the tox works from any folder or project.

## If macOS ever blocks it

The plugin is ad hoc signed, not Apple notarized. Extracting from inside the tox
normally avoids any Gatekeeper prompt, but if a machine ever blocks it, open
Terminal and run:

    xattr -dr com.apple.quarantine ~/.td_plugin_cache/applevisionmask/AppleVisionMask.plugin

then press Setup / Reload. You can also allow it under System Settings, Privacy
and Security. Fully prompt free sharing to other people would need Apple
notarization, which requires a paid Apple Developer account.

## AI disclaimer

The tox packaging, the loader script, and this README were built with the help
of an AI coding assistant. The underlying Apple Vision C++ plugin is Aaron
Pereira's original work, linked at the top. Everything here is open source under
the MIT license, so you are welcome to read it, verify it, and change it.

# gcc.xctoolchain
A barebones Xcode xctoolchain that replaces Clang with GCC.

## **WARNING**
This warning is right up front and in-your-face. Yes, that's how much you need to pay attention here.

This was hacked together in under a day for a brief test environment, and I consider it in its current form a proof-of-concept. I used Homebrew to install GCC 8.1 under a root of `/usr/local/Cellar`, so that's where I had the symlinks point. Yes, everything other than the filter script and its links is a symlink into there. So it will be broken on your machine. Sorry about that. I recommend you install GCC 8.1 through `brew` and fix the symlinks to point there on your machine. Also, link all of the `filtered-` commands to `argument-filter`. That’s a required shim script.

Seriously, don't use this for anything you care about. If you really want to, by all means feel free to fork and improve (auto-symlinking and improved argument filters are the immediate stick-outs). I will pull back your changes if I see them useful.

While this software is provided with the hope that it will be useful, when I say not to use this for anything you care about, I mean it. Seriously, this thing comes with NO WARRANTY and NO SUPPORT, so your use of it is entirely AT YOUR OWN RISK.

## Usage Requirements
After configuring your GCC install location per the above, you'll now have to disable any unsupported options and warnings in your project file, including modules, header maps, any newish or clang-specific warning flags, and probably some other things I'm forgetting. Also, in Build Settings you must add two user-defined settings. Set `CC` equal to `filtered-gcc` and set `CPLUSPLUS` equal to `filtered-g++`.

## Implementation
I'll be honest here. I dug into my Toolchains folder, found an old Swift Development Snapshot one, ripped out the guts, changed the Info.plist and filled it with my GCC symlinks + filter script and its links. It wasn't pretty, but it sure darn worked. Seriously though, Apple, you need to document your `xctoolchain` format. Xcode is enough of an undocumented black box as it is.

The filter script exists to, well, filter out command-line arguments that Xcode insists on adding (i.e. can't be turned off in Build Settings). An unfortunate direct consequence of this is that one of these arguments is the location of the diagnostic listing. So, you won't be getting any inline diagnostics. This is almost certainly fixable through some script tinkering, but you know the drill by now (you did read the WARNING section, didn’t you?).

## Installation
If you've read all the above and still want to tinker/try it out, first clone the repository and fix the symlinks for your machine, as described above. Then rename the folder to “gcc.xctoolchain”, and move it to `[any Library dir]/Developer/Toolchains`. Navigate to Xcode Prefs > Components > Toolchains and select GCC. Xcode will probably pick it up immediately. If it doesn’t, just restart Xcode.

# nautilus-typeahead

**TL;DR:** This repository contains a patch file that is used to build a Ubuntu PPA package of nautilus with restored type-to-seek (typeahead) functionality.

## What's new

Version for Ubuntu 24.04 Noble is up (`1:46.0-0ubuntu2ppa1`).

## What does it do?

Long ago, whenever you typed a letter in an active nautilus window, the first file/folder starting with that letter would get highlighted. This is
the default behavior in a lot of file managers. At a certain point a decision was made to try something different, and as a result of that, typing a letter
in a nautilus window now starts a search instead. This had made many people very angry and has been widely regarded as a bad move.

This package reverts to the original, type-to-seek, behavior.

## How do I install it?

```
sudo add-apt-repository -y ppa:lubomir-brindza/nautilus-typeahead
sudo apt install nautilus
```
If you have any running nautilus windows, you first need to close them with `nautilus -q` or `killall nautilus`.

## Building it yourself

You can use this patch to build your own version and not rely on the PPA repository.

```
apt-get source nautilus  # retrieve sources w/ Ubuntu patches

cd nautilus-<version>
cp ~/<patch_folder>/nautilus-restore-typeahead.patch debian/patches/
echo nautilus-restore-typeahead.patch >> debian/patches/series
quilt push  # (try to) apply the patch

sudo apt-get build-dep nautilus  # install build dependencies
dch -i  # update changelog

debuild -us -uc -b
```
This will produce a .deb file in the parent directory, which you can then simply install by calling `dpkg -i nautilus-<version>.deb`

If you encounter problems building, please make sure you can build from source without the patch applied before opening an issue.

## I don't think I trust you, what are my alternatives?

Fair enough - you can give the `nemo` file manager a try.

You can also try upvoting the issue that asks for this functionality to be included by default, here: https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/142 

## Did you make this?

No. The patch file that makes this PPA build possible is maintained by the awesome folks of the Arch Linux community, 
to support their own AUR package: https://aur.archlinux.org/packages/nautilus-typeahead/

This repository was created at a point where I've had to make small edits to the original patch to make it apply cleanly, but currently there's no difference between the two except metadata (line numbers, etc.).

The current version of the typeahed patch for nautilus 42 (distributed with Ubuntu 22.04) and newer was authored by Xavier Claessens (see https://gitlab.gnome.org/xclaesse/nautilus/-/commits/type-ahead).


## FAQ

- I've updated my packages and the typeahead functionality stopped working, what do I do?

Whenever the Ubuntu team releases a new version of nautilus, it'll get install priority over the (older) version in the PPA repository. 
Typically I'll notice within a day or two and rebuild the package, but feel free to open an issue here on Github if I'm dragging my feet.

- I've installed nautilus from your PPA but typeahead does not work, what gives?

Patched nautilus versions from 43.0 onwards (Ubuntu 22.10 and newer) add a configuration toggle under **Preferences -> Search on type ahead**. You need to **disable** this toggle to override the default search behavior.


## Known limitations
- does not work in open/save file dialog

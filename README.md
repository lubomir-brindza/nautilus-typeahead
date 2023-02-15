# nautilus-typeahead

**TL;DR:** This is a pre-built Ubuntu PPA package restoring the type-to-seek (typeahead) functionality which was removed in upstream Nautilus, 
so you don't need to patch it in yourself.

## What's new

Nautilus has gone through a major rewrite and a new patch reintroducing the type-ahead functionality was written from scratch; it might take a little time to adjust; see the FAQ.

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

## I don't think I trust you, what are my alternatives?

Fair enough - you can give the `nemo` file manager a try.

You can also try upvoting the issue that asks for this functionality to be included by default, here: https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/142 

## Did you make this?

No. The patch file that makes this PPA build possible is maintained by the awesome folks of the Arch Linux community, 
to support their own AUR package: https://aur.archlinux.org/packages/nautilus-typeahead/

This repository was created at a point where I've had to make small edits to the original patch to make it apply cleanly
to nautilus sources, but currently there's no difference between the two.

## FAQ

- I've updated my packages and the typeahead functionality stopped working, what do I do?

Whenever the Ubuntu team releases a new version of nautilus, it'll get install priority over the (older) version in the PPA repository. 
Typically I'll notice within a day or two and rebuild the package, but feel free to open an issue here on Github if I'm dragging my feet.

- I've installed nautilus from your PPA but typeahead does not work, what gives?

Patched nautilus versions from 43.0 onwards (Ubuntu 22.10 and newer) add a configuration toggle under _Preferences -> Search on type ahead_. You need to uncheck this toggle to disable the default search behavior.

The little pop-up text box in the lower right is no more; if you've managed to forget what you entered (or made a typo), your selection will be cleared after 1 second of inactivity.

## Known limitations
- does not work in open/save file dialog
- does not sort directories before files - will jumscrp to a file named "aa" instead of directory "ab" on pressing "a"

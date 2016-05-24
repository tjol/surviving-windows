# Alt-Drag and CapsLock

Probably the two most basic UI fixes I need to keep my sanity.

## Alt-Drag

Stefan Sundin makes a free (GPL even) program called [AltDrag][altdrag] that
does exactly what it says on the tin. There is of course a portable version.

## ctrl:nocaps

There are a number of ways to get `ctrl:nocaps` behaviour (i.e. CapsLock acting
as an additional Ctrl key). The most robust way is to remap the keys using
registry settings. Without admin rights, [AutoHotkey][ahk] saves the day. This is
my AHK config file:

    ^+!Capslock::Capslock
    Capslock::Control

The first line maps Ctrl+Alt+Shift+CapsLock to CapsLock. This is necessary because
sometimes (very rarely) AutoHotkey misses an event and lets a CapsLock press through.
This allows me to disable CapsLock again when that happens.


[altdrag]: https://stefansundin.github.io/altdrag/
[ahk]: https://autohotkey.com/
# Changing the default browser by hand

Normally this is not something one has to worry about – unless of course
one wants to use [Google Chrome Portable][pchrome] or something like that.

Portable Chrome fails to set itself as default browser! It manages to configure
itself to open HTML files, but it doesn't use the "portable" wrapper executable
for this.

The "correct" way to change default browser is to use the *Default Programs*
application, which is buried somewhere in the depths of the start menu. Of course
this don't work for portable apps that the OS doesn't know about yet.
As [some badly written blog post][2] informs me, the default URL handlers are stored
in the registry at
`HKEY_CURRENT_USER\Software\Microsoft\Windows\Shell\Associations\UrlAssociations`; 
the `...\UserChoice\ProgId` keys refer to programs defined in the
`HKEY_CURRENT_USER\Software\Classes` hierarchy. If Chrome has already attempted (and
failed, probably) to register itself, there will be a a corresponding class named
`ChromeHTML.*` there.

Make sure the `<Class>\shell\open\command\` key refers to (e.g.)
`...\GoogleChromePortable.exe` and not `...\App\Chrome-bin\chrome.exe`!

When a suitable class is defined, it can be inserted into the `...\UrlAssociations` 
hierarchy at the appropriate places.

It is also probably necessary to change some file associations (*.html and whatnot)
but this is certainly easiest from the Windows shell.

[pchrome]: http://portableapps.com/apps/internet/google_chrome_portable
[2]: https://newoldthing.wordpress.com/2007/03/23/how-does-your-browsers-know-that-its-not-the-default-browser/
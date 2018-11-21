Sciter is an incredible C++ library that I know a long time ago (since its version 1), aimed to build HTML/CSS based desktop application, currently for Windows and Mac OS X platforms.

I think of it as a little unknown treasure because there is literally no community/blog/site/content about it, besides what you can learn in the author site, and also by asking questions in the forum where Andrew will always promptly answer.

Sciter basically renders standard HTML/CSS through the native GDI backend of the target OS. It has a SDK with Windows .dll and the C multi-platform API. It is the equivalent of using Webkit or Firefox Gecko for building HTML based desktops apps, though, since the tool was planned for desktop apps since its beginning, it has much better support for its sole purpose: desktop. For building a Windows application with Sciter, you normally will create a Win32 app with Visual Studio C++, link to the .lib and include the .h headers found in the SDK.
Octo Deskdex

OctoDeskdex is basically a Sciter based desktop app that shows up Github mascots from the Octodex site.
I’ve built with D instead of the standard C++ way because I wanted to port Sciter headers to D.
So I made a D-port library that basically contains the Sciter SDK headers ported to D and some basic classes for the Win32 infrastructure (message loop, HWND creation and manipulation).

Sciter library also provides an inspector (as Firebug and Chrome build-in inspector) so you can explore your app HTML structure and, most importantly, check log message while debugging it.

Just press F12 and another window containing it will appear. Then in the main window of the app, you can click any element while pressing CTRL+SHIT and its DOM element will be highlighted in the HTML tree of the inspector window, as you can see in the image below.
inspect
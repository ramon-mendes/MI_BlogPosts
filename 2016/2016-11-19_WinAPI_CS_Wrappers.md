If you are making any kind of C# **desktop app** for Windows, sooner or later you need to call **Win32 APIs**, and so resort to P/Invoke (which allows managed code to call unmanaged functions).

When working with SciterSharp for example, you really need to deal with windowing and so with native APIs that take a HWND parameter.

Instead of having to manually write P/Invoke declarations of those unmanaged functions, something time consuming and error-prone,
better to use a library which already contains those declaration, and in a much clever Object-Oriented approach.

[AArnott PInvoke](https://github.com/AArnott/pinvoke) has bindings for many Windows system DLLs.
For dealing with HWND APIs we need the P/Invoke declarations for user32.dll which are locate in [PInvoke.User32](https://www.nuget.org/packages/PInvoke.User32/) NuGeT.
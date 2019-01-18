**OmniCode v1.8** — [Download](/OmniCode), [Changelog](/OmniCode/Changelog)

This is a **major** update for OmniCode with maaany improvements, and is already tested for VS 2015 Update 3.
Among the new features, here are the highlights:

- TIScript auto-completion now lists all available HTML elements IDs for <code>self#</code> statements:<br><img src="/Content/BlogCDN/omnicode_listids.png" />

- Highlighting for strings that span multiple lines:<br><img src="/Content/BlogCDN/strings.png" />

- Added CTRL + E + G shortcut to, VS it opens your browser and searches any selected text in <img src="/Content/BlogCDN/icon_google.png" /> Google / <img src="/Content/BlogCDN/icon_github.png" /> GitHub, *pretty handy*

<br/>


**OmniView** — [more info](/Home/Post/OmniView), [source-code](https://github.com/ramon-mendes/OmniView)

I've made many improvements to this tool. One major one is that it nows recreates the child Sciter window whenever you edit your HTML (while same TIScript VM is preserved); previously, it was always the same HWND, so when you reloaded the page, event-handlers and popups could be kept 'alive'.


**OmniFiddler** — [more info](/Home/Post/OmniFiddler)

- Updated with fixes
- Added a proper app icon: <img src="/Content/BlogCDN/omnifiddler_logo.png" />
- Added a Recent File List to open recent shared Fiddles, so it allows you to edit then after closing the app:<br /><img src="/Content/BlogCDN/omnifiddler_rfl.png" />
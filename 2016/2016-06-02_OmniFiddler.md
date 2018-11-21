# THIS IS TOOL IS DISCONTINUED, CONSIDER USING [THE LIBRARY](/Home/Post/TheLibrary)

OmniFiddler is a .NET desktop app for easily editing and sharing Sciter content, that is, HTML/CSS/TIScript pages, inspired in the eponymous [JSFiddle](https://jsfiddle.net/).

<img src="/Content/BlogCDN/omnifiddler.png" />
<br />
<br />

### Download

Nowadays we are all Livin' On The Edge with CI build systems. So you can always grab the lastest bleeding edge version of this app:

<p>
    <a href="http://misoftware.rs/cdn/Apps/OmniFiddler/OmniFiddler-BleedingEdge.zip" class="btn btn-success">Download</a>
</p>

<p id="ci-status"></p>

<script>
    $.getJSON("/APIFiddler/CI_Version", function(data) {
        $('#ci-status').append("<code>Version: " + data.ver + "</code><br /><code>Build date: " + data.date + "</code>");
    });
</script>

### Hands on

- Download/extract/run the app
- At upper right, click the *Cog* button, then click *Register omnifiddler:// protocol* - this now allows you to open **omnifiddler://** URLs
- Get started by opening our **surprise** fiddle: [omnifiddler://code/NtyZ7](omnifiddler://code/NtyZ7) (drag'n'drop it inside OmniFiddler)
- Finally, click the **Run** button to view the result

### MVP

The goal for this first version was to get the necessary for a MVP where you can upload a 'Fiddle' and get a omnifiddler://ABC link. A URL which you can share and others can use to visualize your page using this same program.

To have this Minimum Viable Product up and running, I started with a [Bootstrap template](/Bootstrap), and it took me some hours over 5 days to join every piece of code necessary to have everything I'll describe bellow.

Well, to achieve a reasonable coding speed, requires having a set of tools and a productive loop where you sit, write code, and get the job done, without those obstacles that take days to solve. That may take a bit longer to learn, but it is all about a skill that you learn once, all about knowing the essentials of C#, Sciter HTML/CSS/TIScript, and how to extract the most with the right tools: Visual Studio, Omni, OmniCode, OmniView, ..

Anyway, it's a proof of, not only how Sciter is a good technology for making desktop applications in .NET and Web-based technology, but mainly that you can do it in a very productive manner.

### Features

- Panels/editors to write HTML/CSS/TIScript and Unittest/TIScript code
- Console + REPL prompt area (same as in OmniView) (press F3 to focus prompt)
- 'Run' button merges all the code, builds a Sciter page, and shows the output in the 'Result' pane
- 'Send and get URL' button to send this content to the cloud and get a omnifiddler://ABC link
- After you got the URL, the Send button becomes 'Update/resend' so you can update your work and keep the same URL
- You can register the 'omnifiddler://' protocol, so your standard browser knows that such URLs can be open with OmniFiddler
- Security: as you are loading unknown code from others in you own machine, you really want some degree of protection from 	
malicious code; so by default [Sandboxing is enable](http://sciter.com/forums/topic/security-sand-boxing/) which disables the System class and block access to file:// URLs; note that in the app, no remote code is executed until you press the 'Run' button (so you can inspect what you are about to execute); note that surely it is not a perfect sandboxing

### Datasheet

- Written in C# for .NET 4.6.1
- For now it runs on Windows only, but its fully portable to Linux and OSX, just needs the effort to do so
- Very assynchronous, so I am relying on the new built-in promise() support of TIScript
- Icon font from Fontello
- It is *free* and I will open-source it sooner or later

<!--- Next major thing I'll do is to create a GitHub repo for the 'lib_console' TIScript library, and a proper documentation. This library is shared by OmniFiddler, Omni, OmniView, OmniCode, and your own code (why not?), so it might be a common denominator API as the equivalent to the 'console' API we have in browsers.-->


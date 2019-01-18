Here is a complete search-text function and it's corresponding UI for Sciter engine, as we see in modern browsers. This implementation uses the [::mark(…)](http://sciter.com/mark-feature-is-comming/) feature to highlight the blocks of text, which makes things much easier, otherwise I would need to enclose the occurrence in ```<span>```'s tags, something with many side effects since you would be modifying the DOM. This makes me wonder, how do you implement such thing in modern browsers?

This is the exact implementation which will appear in the upcoming version Omni to let you search in the docs pages.

[Source at GitHub](https://github.com/ramon-mendes/lib_search.tis)

![](/Content/BlogCDN/search.gif)


**Keyboard shortcuts supported:**

- Ctrl + F: opens the #search-box and selects the input text
- F3: search next
- Shift + F3: search previous
- ESC: closes the #search-box

**Design notes:**

- I don't know why but, for ```position: fixed``` to work for an element, you need to set ```overflow: auto;``` for the ```<body>``` tag; that was essential to make #search-box stay fixed
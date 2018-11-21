CTRL+Space programming is how I consider the crazy way a programmer ends up coding, specifically when working with a statically typed language (like C++, C#, Java) and a good IDE.

Essentially it is all about autocompletion, about having the IDE telling what is available in the context where you are typing:

- telling you the available names, like variables, classes, methods, ...
- telling you the available APIs methods so you don't have to remember nor consult the docs
- letting you use code snippets
- autocompleting when there is only a single option available when you press CTRL+Space

So the benefits of autocompletion are many, so I don't think I need to explain the importance of it for beeing a productive programmer.

---


## TIScript autocompletion

Well, TIScript is a dynamic typed language, so you can't know the type of a variable with static analysis. That was a big challenge I faced when developing OmniCode and it's Intellisense support.

I had to resort to some smart ways to discover the type of variables and calls for known API methods so I can show you their signature and needed parameters.

There are 3 methods you should know that OmniCode uses to give you autocompletion where possible:

#### **1 - Naming conventions**

OmniCode infers the type of a given variable based on its name. That is, you programmer, by following a convention in naming your variables, is helping OmniCode to know the type of variables.

As in the following GIF, by naming the variable as `el_div`, it is inferred its type as **Element**, because of the `el_` prefix.

![](/Content/BlogCDN/ctrlspace1.gif)

Notice that identifiers with inferred type have a distinctive grayed color.

This way, when you type a dot, OmniCode can show the methods/properties available for `Element` class, or for whatever type which was inferred, as `Graphic` for variables named `gfx`.

All the rules for these naming conventions are available here: [conventions table](/OmniCode/Convention).

#### **2 - Dot stack analysis**

When you are typing the following code, OmniCode correctly shows the completion-list of methods/properties available:

![](/Content/BlogCDN/ctrlspace2.png)


The following analysis is made:
- Statement is splitted by the `.` separator
- `self#menu-cfg` is automatically inferred as `Element` type (it's a way in TISript that to get an `Element` reference by its ID)
- `$` is a valid method of the `Element` class and it returns an `Element`
- Knowing the returned type is `Element`, we know the names available, and as you already started typing `ht`, the list is filtered to show only names containing `ht`

All this is possible because OmniCode contains a database of all TIScript built-in API, that is, all methods/properties/constants of classes/types you find in [Sciter documentation](https://sciter.com/developers/sciter-docs/).

So with this analysis of statements, you have not just autocompletion lists, but all features of a good Intellisense experience:

**Signature help** - shows method prototype (and overrides) and highlights the current argument being typed

![](/Content/BlogCDN/ctrlspace3.png)

**QuickInfo** - show details about the name under cursor

![](/Content/BlogCDN/ctrlspace4.png)

#### **3 - Semantic analysis**

OmniCode keeps track, at real-time, of all user-defined symbols/names, so you get autocompletion for your own code:

![](/Content/BlogCDN/ctrlspace5.png)

That is, after you type any TIScript code, the file is parsed, and declarations of variables, consts, functions, classes and namespaces are extracted.

`import "xyz.tis"` statements will trigger scanning of the imported file.

This database of locations of every user-defined symbols makes possible the GoToDefinition functionality, that is, allows you to, for example, press F12 in a function call statement and navigate to where the function is defined.

#### **Bonus: self#ID completion**

Another clever autocompletion support is presented after you type `self#`.

It shows all ID available from your HTML markup:

![](/Content/BlogCDN/ctrlspace6.png)

The database of IDs is built from open HTML files. That is, make sure to open all HTML files that you want to have its DOM tree parsed and IDs extracted.

Pressing F12 in a `self#myElementID` expression opens the location of the tag/element in your HTML markup with the given ID, if it exists.

---

Next post I will show how do CTRL+Space programming in CSS, stay tunned.


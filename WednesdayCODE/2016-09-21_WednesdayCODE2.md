**[Trello](http://trello.com/)** is a kanban-like task management tool, the one which I personally use. In this **Wednesday CODE** installment we will be remaking its UI.

![](/Content/BlogCDN/WedCODE2.png)

I won't be making another TODO list system, the world is already full of then.

Instead, I will focus on what I think are the most clever UI details of this tool.
To me, it is distinctive because of the minimalist layout, and how you can nicely drag'n'drop the tasks between the columns.
If you want then, you might easily extend it to add a complete TODO list support.

The main goal is to achieve a functional UI with drag'n'drop where you can **rearrange** the columns, and also the tasks inside each column. So it is all about **frontend implementation** in Sciter. I will divide it in 2 posts:

1. This one deals with implementing every visual aspect required, that is, we will write all HTML and CSS code necessary, and any script required to dynamically construct HTML.
2. In the second part we make the UI interactive by adding the drag'n'drop support through scripting.

**[Download source-code / GitHub](https://github.com/ramon-mendes/WednesdayCODE/tree/master/Trello)**

**Test DEMO with this link: ([how to run it](/Home/Post/WednesdayCODE-Intro))**

<a href="omnifiddler://url/https://rawcdn.githack.com/ramon-mendes/WednesdayCODE/master/Trello/Trello.html" class="fiddler-link">Drag-me to OmniFiddler and <i class="icon-play-outline"></i>Run</a>

---

## HTML explained: templates

The file Trello.html contains all our HTML code, you should [check it at GitHub](https://github.com/ramon-mendes/WednesdayCODE/blob/master/Trello/Trello.html#L224).

Our Trello board has lists (columns) and cards/tasks (itens inside each column). Lists are arranged horizontally, cards vertically.

In the HTML approach I am using, I declare two HTML 'templates', `#tpl-list` and `#tpl-card`, one for each of these kind of item.
I use this HTML template as the basis to create every new list/card in order to populate the page with lists and cards:

```
<div #tpl-list style="display: none">
	<section>
		<div .list-header>
			<span />
			<i .ft.ft-dot-3 .dark-hover />
		</div>

		<ul />

		<div .list-newcard.dark-hover>Add a card...</div>
	</section>
</div>

<div #tpl-card style="display: none">
	<li>
		<div .content>
			<img .card-cover />
			<div .card-labels />
			<div .card-title />
			<div .badges />
		</div>
		<div .card-edit>
			<i .ft.ft-pencil.dark-hover />
		</div>
	</li>
</div>
```

The parent `div` is hidden so our template does not appears.
What really matters is the child `section`/`li` element which is actually our template, which we will `clone()` in script in order to create every new item.
I'll cover the code behind below.

## HTML explained: HTML/CSS subtleties

Sciter's HTML/CSS support has some subtleties which are not supported by standard browsers.
It is standard HTML, until you notice some subtleties. Notice how we declared the `<button>` element:

```
<button #btn-add-list.std-btn>
```

Instead of declaring the element ID with an **id="btn-add-list"** attribute, as in `<button id="btn-add-list" />`, we can simply say **#btn-add-list**, as you would write an ID selector in CSS.

You can do the same for the class attribute, so `<button class="btn btn-colored" />` can be written as `<button .btn.btn-colored />`. Pretty clever method for writing the ID or classes of an element by using the same syntax from CSS.

Sciter supports more HTML shortcuts, [see here](http://sciter.com/sciter-html-parsing-flavour/).

## CSS explained: 'flow' layout

Besides basic CSS code for making our Trello UI, for achieving our Trello drag'n'drop goal, we must set a proper flow to our layout.

For that, it is essential to understand the 'flow' CSS property. It is THE major method for construting layouts in Sciter. Essentially it is the equivalent of CSS3 Flexbox layouting, but it has different naming and behavior ([docs here](http://sciter.com/docs/flex-flow/flex-layout.htm)).

The 'flow' property dictates how children of the element will be positioned inside it. For our demo, we only use the following 'flow' modes:

- horizontal: position elements horizontally, from left-to-right (the reverse in RTL layouts)
- vertical: position elements vertically, from top-to-bottom
- stack: position elements one above the other

The 'vertical' value we use says to layout element vertically, one bellow the other, in rows, no matter if its inline or a block element.
The 'horizontal' value, of course, place elements horizontally, one beside the other. You use it, for example, to create toolbars.

One aspect of the 'horizontal' flow is that you can reverse the standard left-to-right direction to right-to-left:

```
div
{
	flow: horizontal;
	direction: rtl;
}
```
So 'vertical' and 'horizontal' are just the standard method to lay elements linearly (a linear layout).

---

What's the point with **`flow: stack`**?

The arrangement achieved with `flow: stack` is simply a stack of layers, as we have with visual layers in Photoshop.

It is an alternative and better approach than using `position: absolute` as it allows to set the x/y alignment of its childs by using
`horizontal-align` and `vertical-align`, which is much better than using CSS hacks.

## TIScript: Populating the page with JSON data

Data to fill our board will definitely come from a dynamic/database source, so we need to resort to scripting.

As a start, I hard-coded the JSON data in script.
The `data` variable holds and array with an object for every column of our board, and each of these object holds a `cards` field with an array of the cards inside it:

```
var data = [
	{
		board-name: "Basics",
		cards: [
			{ t: "Welcome to Trello!" },
			{ t: "This is a card." },
			{ t: "Click on a card to see what's behind it." },
			{ t: "You can attach pictures and files...", a: "taco.png" },
			{ t: "... any kind of hyperlink ..." },
			{ t: "... or checklists." }
		]
	},
	...
];
```

Then I simply put the code that populates our page from this JSON data.
It is simply dynamically creating the HTML that displays it to user:

```
function Setup()
{
	var el_main = $(main);
	for(var s in data)
	{
		var el_section = self#tpl-list[0].clone();
		el_main.insert(el_section, el_main.length - 2);
		el_section.$(.list-header span).text = s#board-name;

		for(var c in s.cards)
		{
			var el_card = self#tpl-card[0].clone();
			el_section.$(ul).append(el_card);
			el_card.$(.card-title).text = c.t;
			if(c.a)
			{
				el_card.$(.card-cover).@#src = c.a;
			}
		}
	}
}
Setup();
```


## TIScript: horizontall scrolling

The main area is supposed to overflow horizontally, that is, scrollbars must appear when content can't fit the window's width. In CSS we declare:
```
html { overflow: hidden; }
body { overflow-x: auto; }
```

And for beeing clever, we let the user use the mousel wheel to horizontally scroll it:
```
$(body).subscribe(function(evt) {
	this.scrollTo(this.scroll(#left)-10*evt.wheelDelta, 0);
}, Event.MOUSE, Event.MOUSE_WHEEL);
```
By default, Sciter only supports vertically scrolling the page with the wheel.


## Next time

Next time we’ll look at the drag'n'drop implementation:

- Lists drag'n'drop
- Cards drag'n'drop

The best way to learn is by playing. If you have some Sciter sample and would like it to be showcased here, be my guest!
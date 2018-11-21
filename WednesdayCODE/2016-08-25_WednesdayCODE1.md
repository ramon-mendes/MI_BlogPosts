Welcome to our first Wednesday CODE section, though it is thursday. Check our [Intro](/Home/Post/WednesdayCODE-Intro) post to know more about it.

For me, doing this examples will be just a lot of fun, so it is simply a pleasure to be sharing my knowledge with everyone.

Without further ado, out first installment is about CSS transitions. Our example will be the following.

<p><img src="/Content/BlogCDN/WedCODE1.gif" style="border: solid 1px silver" /></p>

In web-terms, we usually call those big headers as Hero (or sometimes Jumbotron).
In our example, when you move your mouse hover, it animates the background and the image displaying.

**[Download source-code / GitHub](https://github.com/MISoftware/WednesdayCODE/tree/master/TransitionsJoy)**

**Test DEMO with this link: ([how to run it](/Home/Post/WednesdayCODE-Intro))**

<a href="omnifiddler://url/https://rawcdn.githack.com/MISoftware/WednesdayCODE/master/TransitionsJoy/TransitionsJoy.html" class="fiddler-link">Drag-me to OmniFiddler</a>

---

Is Sciter, for creating such animations, which are pretty simple for instance, you can make them just using CSS, no scripting required.

In our example, the two animations are defined this way:

```CSS
section:hover
{
	background-position: -15px;
	background-color: #5ec8f7;
	transition:
		background-position(quart-out, 0.5s, quart-in)
		background-color(quad-out, 0.3s, quad-in, 0.5s);
}
```

```CSS
section:hover img
{
	foreground-position: 0 0;
	transition: foreground(quart-out, 0.5s, quart-in);
}
```

Essentially we are defining two transitions for when the user hovers the containing `<section>` that wraps our Hero area which embodies the following HTML:

```CSS
<section>
	Welcome to Wednesday CODE! <img src="http://misoftware.rs/Content/img/logo_mi.png" />
</section>
```

In order to declare these animations, you must understand the CSS syntax behind it. And actually, it's simply all about the **transition** CSS propery.

But where can I look for the docs describing the **transition** syntax? It is hidden in the SDK, at `\doc\content\css\css-transition-sciter.htm`. It states about the transition property:

> "The transition attribute accepts single value or list of values - functions describing animated transformation of CSS attributes between initial state and the state described by &nbsp;the rule where the transition is defined."

So, given this declaration:

```CSS
div:hover
{
    width: 200px;
    height: 200px;
    transition: width(linear,400ms)
                height(linear,400ms);
}
```

We are essentially saying that when we `:hover` the `<div>`, it will gain a new `width` and `height` value,
however, these changes in the element layout must be animated.

Note that the properties that you want to animate (in this case `width` and `height`) must be **animatable**.
That is, like in standard browser, some properties are **animatable**, others are not.

**Animatable** properties can change gradually from one value to another, normally these are all properties where the values is a size, number, percentage or color.
Unfornutelly I couldn't find a list of supported animatable properties, so you must normmally Test/Run to see if it does animates.

So, our element will animate its `width` and `height` when it gains the `:hover` state. But not only that. It will also animate when it looses the `:hover` state.
That is, a `transition:` declaration by default sets the animation to play when element start matching or stop matching the CSS selector where the `transition:` is declared. You can however specify if you want to play the animation in just one of the directions.

Then, we have the transition, which says how our property animates in two aspects:

- transition easing function
- duration of the transition

The duration is how long it takes to play the *in* or *out* transition.
The transition easing function is a math function saying how to ease the transition, you can understand it by looking at `\samples\animations-transitions-css\easing-functions.htm` in SDK. There you will get all available easing functions:

> linear, quad-in, quad-out, quad-in-out, cubic-in, cubic-out, cubic-in-out, quart-in, quart-out, quart-in-out,
> quint-in, quint-out, quint-in-out, sine-in, sine-out, sine-in-out, expo-in, expo-out, expo-in-out, circ-in, circ-out, circ-in-out,
> elastic-in, elastic-out, elastic-in-out, back-in, back-out, back-in-out, bounce-in, bounce-out, bounce-in-out

Transitions are a powerfull concept, and I tried here to make a brief technical introduction of the syntax, and also pointed where you can look for more info.
Yet, there are many more details to explore about transitions, but which are irrelevant for you to Get Started. That is, you are now able to add animations to Sciter pages!
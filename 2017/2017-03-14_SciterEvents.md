Events in Sciter happens mainly from user interaction, be it over mouse or keyboard input.

But they can also come from other sources. For example, you can trigger them programatically, or the engine itself might trigger events related to the UI, like when a CSS animation ends/starts, or when the DOM is changed.

Whatever events you are interested, you must use a method to handle then.

This post will describe the many coding approaches that you can use in **TIScript** to handle events.

#### **Event sinking/capturing + bubbling**

Before starting, it is fundamental for you to know what is event sinking/capturing + bubbling, which is how generated events gets prograpaged throughout the DOM hierarchy.

It is the same mechanism found in modern browsers Javascript support, so if you already know about it, Sciter follows the same model.

If you don't know this mechanism, do read this article: [http://javascript.info/tutorial/bubbling-and-capturing](http://javascript.info/tutorial/bubbling-and-capturing)

In the end you must know that in Sciter:
- event first has a sinking/capture phase, where it goes down the DOM hierarchy
- then the event has a bubbling phase, where it goes up the DOM hierarchy, that is, it *bubbles up*

Notice that Sciter documentation uses the term sinking, and not capturing, it is all about the same thing.

In standard browsers, if you want to handle events in the capture phase, you must to use `addEventListener()` with last argument set to `true`, so it is the only way to catch events at capturing phase.

I think that in Sciter, TIScript syntax allows you to more naturally choose wheter you want to handle an event in its sinking or bubble phase, as I will describe bellow.

#### **Non-bubbling events**

Also it's fundamental to know that some events don't have this sinking + bubble phase.

They simply go directly to the target DOM element. Events of this kind are called *Non-bubbling events*.

That is, only the target element can handle the event. For example, drawing events are Non-bubbling, only the target element can draw itself.

---

So let's get to know how in code you are supposed to declare event handlers.

There are 4 methods, but you usually only use the **first two** since they are wrapers around the more general and complex call to `Element.subscribe()`.

##### **1 - Method: `Element.on()`**

Sciter supports event handler assignment as in jQuery style:

```
myElement.on("event", "selector", function(evt) {...});
```

The `on()` method assigns the handler function to the particular event that may occur on the DOM element (`myElement`) or on one of its children.

You can filter for what children you want to handle the event using the `selector` string which is a CSS selector.

To subscribe to a event in EVENT_SINKING phase prepend the event name by ~ symbol. For example this handler

```
container.subscribe("~keydown", function() {...}); 
```
..will receive the keydown event before its children.

Read the docs of this method [here](https://sciter.com/docs/content/sciter/Element.htm#on-by-name).

##### **2 - Method: `element << event whatever` notation**

This is a syntatical-sugar way to declare a function and at the same time attach it as event handler of the element.

Syntax of this event subscription notations is
```
element << event eventname { ... } or
element + event eventname { ... }
```

So, for example, to handle the click event on the element with ID "myButton":

```
$(#myButton) << event click() {
	...
}
```

If you need to specify a CSS selector and also want the event parameters, the full form would be:

```
$(#myButton) << event click $(selector) (params) {
	...
}
```

##### **3 - Method: `Element.subscribe()`**

The subscribe() method is the low-level way to handle events, it ends up being a method to handle all kind of events, some which you are not able to handle with `Element.on()`.

Note that there are two signatures of Element.subscribe(). One is equivalent to Element.on(), and the other is the one we are interested:

```
subscribe( handler: function, eventGroup : int [, eventType: int] ) : <this element>
```

Internally, Sciter treat events by numbers, not strings. `Element.on` actually maps the string event name to the internal number.

Notice how we specify the event we want to handle with two integers parameters. These integer are enumerated in the Event class, see [here](https://sciter.com/docs/content/sciter/Event.htm) the values.

First is the event group, like Event.MOUSE. Second, you can optionally specify the exact event type you want to handle, like Event.MOUSE_DOWN.

The event type is an ORed integer that exactly identifies the event, its phase and if it was HANDLED when in bubbling phase. That is, it can be ORed with the event type and a flag identifing if on the sinking phase, or if was HANDLED. See **[Event flags](https://sciter.com/docs/content/sciter/Event.htm)** for usage samples.

To handle MOUSE_WHEEL events for example, you must use Element.subscribe because I won't find a event string name to use with `Element.on()`.

I use the following code in my Trello remake to make the mouse wheel to move the horizontal scrollbar instead of the vertical scrollbar.

```
$(body).subscribe(function(evt) {
	this.scrollTo(this.scroll(#left)-10*evt.wheelDelta, 0);
}, Event.MOUSE, Event.MOUSE_WHEEL);
```

##### **4 - Shortcut event handlers (legacy/deprecated)**

There are pre-defined functions names that you assign to the element that are called for handling events, see the list in the [Element class, Element events section](https://sciter.com/docs/content/sciter/Element.htm).

```
myElement.onClick = function() {
    ...
}
```

Problem is, you can only set a single function handler to the onClick property. That's why this method is deprecated, but still usable.

##### **Non-bubbling event handlers**

These are events that only the target element can handle, that is, they don't pass though the DOM parents, till the child/target element as sinking/bubbling events does.

*mouseenter/mouseleave* are example of non-bubbling events. I didn't found a list of *Non-bubbling event*, but do know that they exist.

Also, I consider *Non-bubbling event handlers* the paintXYZ properties that you can assign to an element, for example:

```
myElement.paintForeground = function(gfx) {
    ...
};
```

---

Ok, that was enough. I know it seems complex at first sight, but it gets trivial once you know the details.
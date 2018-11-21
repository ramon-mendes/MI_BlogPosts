![](/Content/BlogCDN/WedCODE3-Trello.png)

Continuing our series, we will now essentially write TIScipt code to make our Trello board interactive with **drag'n'drop support**! If you missed part 1, check [it here](/Home/Post/WednesdayCODE2).

**[Download source-code](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/Trello.zip)**

**Test DEMO with this link: ([how to run it](/Home/Post/WednesdayCODE-Intro))**

<a href="omnifiddler://url/https://rawcdn.githack.com/MISoftware/WednesdayCODE/master/Trello/Trello2.html" class="fiddler-link">Drag-me to OmniFiddler and <i class="icon-play-outline"></i>Run</a>

---

## Drag'n'drop in Sciter

Well, it's easier to make than to explain. I won't dive too deep in the implementation, instead, better to explain the logic behind it so you might be able to tweak and even reuse my drag'n'drop implementation.

I decided to code drag'n'drop from scratch. Sciter's SDK samples has a drag'n'drop library which you may want to resort as a more flexible and reusable approach. Check it at `sciter-sdk-3/samples/drag-n-drop-manager/`.

However, our Trello board drag'n'drop has so specific requirements that probably that library would not suffice or would make it a harder implementation.
 I recommend you to check `ddm.tis/ddm2.tis` and the samples as an option to your drag'n'drop scenario.

Check **Trello2.html** where is our final HTML code, [here at GitHub](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/Trello2.html).

And **drag-n-drop.tis** where I put the main script code,  [here at GitHub](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis).
 
## TIScript explained: element script prototype

Before I talk about drag'n'drop, you must understand Sciter's protoype mechanism that our demo uses to link script behavior to our HTML structure (also, you get a sense of development practices right out of the box).

Note that drag-n-drop.tis contains two behaviors: `DndListBehavior` handles drag'n'drop between lists (columns), and `DndCardBehavior` handles drag'n'drop between cards (tasks).

Well, behaviors are just a more organized way to set the code/logic-layer behind an element because you are declaring in CSS what element gets attached to what behavior, so you have all CSS cascading/priority rules applying.

Docs about behavios are [here](http://sciter.com/sciter-declarative-behavior-assignment-by-css-prototype-and-aspect-properties/).

In CSS there are 3 mechanism that you can use to relate/attach a behavior to an element:
- **aspect**: call a **TIScript** function with element set to 'this'
- **prototype**: instantiate and attachs a **TIScript** class to the element
- **behavior**: instantiate and attachs a **native** (C#, C++, Python, ..) behavior to the element

So yes, you can have a native behavior when you need the logic to be in the native side.

In our demo I am only using the prototype mechanism, see lines [81](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/Trello2.html#L81) and [140](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/Trello2.html#L140).


## TIScript explained: drag'n'drop algorithm

For creating any kind of drag'n'drop support in Sciter, you will essentially resort to a standard sequence
of handling user events and mouse capture. What is mouse capture?

> When an object captures the mouse, all mouse related events are treated as if the object with mouse capture perform the event, even if the mouse pointer is over another object.

Simply put: when you capture the mouse, the element capturing it will receive all mouse events, even if cursor is outside the element box/area or even the application window. This is very usefull for drag'n'drop.

The basic sequence for any custom drag'n'drop implementation in Sciter is:

- `Event.MOUSE_DRAG_REQUEST`
- `this.capture(#strict)` and `captured = true`
- `Event.MOUSE_MOVE` if `captured==true`
- `Event.MOUSE_UP` if `captured==true`
- `this.capture(false)` and `captured = false`

I'll explain each step and point the corresponding line of code:

- `Event.MOUSE_DRAG_REQUEST` [line 57](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis#L57):
logic to detect when to start a drag'n'drop operation;
this event happens when user clicks and holds mouse hover our element, and move it a little bit (which does not happens in a simple click event)

- `this.capture(#strict)` [line 69](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis#L69)
and `captured = true` [line 70](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis#L70):
to start our drag'n'drop, we capture the mouse making all mouse messages/events to be delivered to 'this' element;
also we need to set a global variable so we know that a drag'n'drop is in progress and we should handle MOUSE_MOVE events

- `Event.MOUSE_MOVE` if `captured==true` [line 25](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis#L25):
here we put all the logic of what happens while the object is beeing dragged

- `Event.MOUSE_UP` if `captured==true` [line 19](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis#L19)
here we put all the logic of what happens when user releases the mouse button / drops the object

- `captured = false` [line 21](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis#L21) and `this.capture(false)` [line 23](https://github.com/MISoftware/WednesdayCODE/blob/master/Trello/drag-n-drop.tis#L23):
with our drag'n'drop operation finished, we must manually release mouse capture (else mouse interaction behaves very oddly) and unset our global variable

## Next time

Our next installment is exciting. I'll cover Sciter C# native implementation. Stay tunned.
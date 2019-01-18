Today we will learn what are native behaviors and, as a DEMO, I've made a behavior for displaying charts which we can create natively with **OxyPlot** library.

**[Download source-code](https://github.com/ramon-mendes/WednesdayCODE/raw/master/Charts/Charts.zip)**

![](/Content/BlogCDN/WedCODE4-chart.png)


## Native behaviors explained

Sciter allows you to attach one or more native behaviors to a DOM element. A behavior is essentially a event-handler which allows you to participate in the event chain of that element
and perform any needed task. This way you are adding code-logic behind a DOM element in the native layer.

The behavior is a name you register in the engine, specifying which event-handler class corresponds to this name:

``` C#
// Registers the Chart behavior
host.RegisterBehaviorHandler("Chart", typeof(ChartBehavior));
```

In CSS you can then attach the given behavior name to elements:

```
<style>
chart
{
 behavior: Chart;// the name you registered
}
</style>
```

With this CSS declaration, any `<chart>` tag in our HTML will get the event-handler attached, or beeing more exact, any DOM element that matches the 'chart' CSS selector.

You can also manualy attach an event-handler for a single element entirely in C#:

```
private SciterWindow wnd;

public void SetupEvh()
{
    ChartBehavior evh = new ChartBehavior();

	SciterElement el = wnd.RootElement.SelectFirstById("main-chart");
	el.AttachEvh(evh);
}
```

This way you are able to attach the event-handler in an element-by-element basis natively.

## Coding the SciterEventHandler

The event-handler is a class you derive from SciterEventHandler:

``` C#
using System;
using SciterSharp;
					
public class ChartBehavior : SciterEventHandler
{
}
```

A event-handler receives every event targeting the element or **its children**, before it is dispatched to TIScript side.
You receive events for children because events **bubbles** from the root <html> element to the target element (event bubbling), so they need to pass through any parent element in middle of the DOM hierarchy.

Not all events are bubbling events. Only the target element receives a OnDraw request, for example.

You override the virtual methods of SciterEventHandler that correspond to the events you are interested. These are the methods you can override:

- Behavior related events: Subscription, Attached, Detached
- User input events: OnMouse, OnKey, OnScroll, OnGesture
- UI events: OnFocus, OnSize, OnDraw
- TIScript method call for the element: OnScriptCall
- Others: OnMethodCall, OnDataArrived, OnTimer

For **quickly overriding methods** in Visual Studio, type `override` and then press SPACE: Visual Studio will display an auto-suggestion list of the methods you can override.

A SciterEventHandler might be attached to many DOM elements. In case you need to keep track of those elements, or need to do any kind of initialization, you should override the Attached() method, which is called be the begine as soon as it's attached to an element, receiving as paramater the DOM element being attached:

``` C#
public class MyBehaviorEvh : SciterEventHandler
{
  protected override void Attached(SciterElement se)
  {
     // Initialize this behavior to the given 'se' element
     // Save 'se' variable
  }
}
```

When the element is removed from DOM, or you manually dettaches the behavior from it, the engine invokes the Detached() method, where you should do any needed deinitialization/cleanup.

## Charting solution with OxyPlot

A complete script-based charting solution for Sciter seems to not exist yet. But for any kind of problem, you have the option to search for a native solution, which you can then integrate in your Sciter UI.

OxyPlot has almost anything you need for chart creation. This lib is able to generate a PNG image of the chart that you create programatically, which we can then draw on screen.

For that, I use a native behavior where in its event-handler, I handle the OnDraw event and draw the PNG image using a **SciterGraphics** instance which you get from **DRAW_PARAMS**:

``` C#
class ChartBase : SciterEventHandler
{
	protected PlotModel _model;

	protected override bool OnDraw(SciterElement se, SciterXBehaviors.DRAW_PARAMS prms)
	{
		if(_model == null)
			return false;

		if(prms.cmd == SciterXBehaviors.DRAW_EVENTS.DRAW_CONTENT)
		{
			var pngExporter = new PngExporter()
			{
				Width = prms.area.Width,
				Height = prms.area.Height,
				Background = OxyColors.Transparent
			};

			using(var ms = new MemoryStream())
			{
				var bmp = pngExporter.ExportToBitmap(_model);

				using(var img = new SciterImage(bmp))
				{
					new SciterGraphics(prms.gfx).BlendImage(img, prms.area.left, prms.area.top);
				}
			}
				
			return true;
		}
		return false;
	}
}
```

Notice that this charting solution still needs some improvements:
- OxyPlot does supports chart interaction, but I need to implement the UI backend which links OxyPlot <-> Sciter event handling
- As it depends on System.Windows.Forms, it is Windows-only, although OxyPlot is a multi-platform library.
Solution it to port OSX by using OxyPlot.Xamarin.Forms and to Linux port by using OxyPlot.GtkSharp
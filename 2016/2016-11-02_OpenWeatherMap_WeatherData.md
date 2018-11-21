If you need weather forecast data, **OpenWeatherMap** and **Yahoo** have free APIs to get you covered.

Here I will show how grab this data in **C#** and, just for fun, how to plot the temperature forecast over a line graph using **OxyPlot**.

## Yahoo API request URL

Yahoo gives you a 7 days forecast in a daily fashion.

Simply access the following URL which contains YQL query to fetch data from the `weather.forecast` table and return it in JSON format (or XML if you want):

> [https://query.yahooapis.com/v1/public/yql?q=select * from weather.forecast where woeid in (select woeid from geo.places(1) where text='caxias do sul') and u='c'&format=json](<https://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text=%27caxias%20do%20sul%27)%20and%20u=%27c%27&format=json>)

This sample URL returns forecast to my city, Caxias do Sul / Brazil.

Notice the `u='c'` which makes it returns temperatures in Celsius.

## OpenWeatherMap API request URL

OpenWeatherMap free alternative offers a 5 day / 3 hour forecast.

First you need to [register](https://home.openweathermap.org/users/sign_up) and get an API key to query their REST API.

Then you need the ID of the city you want the forecast:
1. Go to [http://openweathermap.org/find](http://openweathermap.org/find)
2. Search your city
3. Click its name
4. In the resulting page URL, like [http://openweathermap.org/city/3466537](http://openweathermap.org/city/3466537), copy the numberic part, so 3466537 in this example

GET the following URL (replacing CITYID and APPID with the city ID and API key, respectively):

> [http://api.openweathermap.org/data/2.5/forecast?id={CITYID}&APPID={APPID}&units=metric](http://api.openweathermap.org/data/2.5/forecast?id={CITYID}&APPID={APPID}&units=metric)

Note that I want temperature values in Celsius degrees, which means to use the metric system, hence the `units=metric` parameter.

## Graph in C# / OxyPlot

The following graph is the live 5 day / 3 hour **temperature forecast** returned by **OpenWeatherMap** for my city:

![](/Weather/CxsForecastPlot)

Behind this image, I am using OxyPlot to generate a PNG image from the JSON data, and returning it through the `/Weather/CxsForecastPlot` URL.

Code behind the ASP.NET MVC action for this URL:

```C#
// GET: CxsForecastPlot
public ActionResult CxsForecastPlot()
{
	PlotModel model = new PlotModel()
	{
		PlotAreaBorderColor = OxyColor.Parse("#BBBDBE"),
		TextColor = OxyColor.Parse("#50606F"),
	};

	var temperatureAxis = new LinearAxis()
	{
		Minimum = 0,
		Maximum = 50,
		Title = "Temperature Cº",
		MajorGridlineThickness = 1,
		MajorGridlineStyle = LineStyle.Solid,
		MajorGridlineColor = OxyColor.Parse("#E9ECEF"),
		CropGridlines = true,
		TickStyle = TickStyle.None
	};
	model.Axes.Add(temperatureAxis);

	var dateAxis = new DateTimeAxis()
	{
		StringFormat = "MMM dd",
		TickStyle = TickStyle.None
	};
	model.Axes.Add(dateAxis);

	LineSeries serie = new LineSeries()
	{
		MarkerType = MarkerType.Circle,
		MarkerSize = 4,
		MarkerStrokeThickness = 1,
		MarkerStroke = OxyColors.Black,
		MarkerFill = OxyColors.White,
		StrokeThickness = 3,
		ItemsSource = GetForecastData().Select(e => new DataPoint(DateTimeAxis.ToDouble(e.dt), e.temperatureC)).ToList()
	};
	model.Series.Add(serie);

	using(var ms = new MemoryStream())
	{
		var export = new PngExporter() { Width = 900 };
		export.Export(model, ms);
		return new ImageResult(ms.ToArray(), "image/png");
	}
}

class WeatherData
{
	public DateTime dt;
	public double temperatureC;
	public double humidity;
}

private List<WeatherData> GetForecastData()
{
	List<WeatherData> result = new List<WeatherData>();

	// GET the data from REST API
	// HttpClient need Microsoft.Net.Http NuGeT
	using(var client = new HttpClient())
	{
		HttpResponseMessage response = client.GetAsync($"http://api.openweathermap.org/data/2.5/forecast?id={CITY_ID}&APPID={OWM_APPID}&units=metric").Result;
		response.EnsureSuccessStatusCode();

		string json = Encoding.UTF8.GetString(response.Content.ReadAsByteArrayAsync().Result);
		dynamic dynjson = JSON.DeserializeObject(json);
		foreach(var item in dynjson.list)
		{
			result.Add(new WeatherData
			{
				dt = Utils.FromUnixTime((long) item.dt),
				temperatureC = (double) item.main.temp,
				humidity = (double) item.main.humidity
			});
		}
	}

	return result;
}

public class ImageResult : ActionResult
{
	public ImageResult(byte[] image, string contentType)
	{
		if(image == null)
			throw new ArgumentNullException("image");
		if(contentType == null)
			throw new ArgumentNullException("contentType");

		this.Buffer = image;
		this.ContentType = contentType;
	}

	public byte[] Buffer { get; private set; }
	public string ContentType { get; private set; }

	public override void ExecuteResult(ControllerContext context)
	{
		if(context == null)
			throw new ArgumentNullException("context");

		HttpResponseBase response = context.HttpContext.Response;

		response.ContentType = this.ContentType;
		response.OutputStream.Write(Buffer, 0, Buffer.Length);
		response.End();
	}
}

public static class Utils
{
	public static DateTime FromUnixTime(this long unixTime)
	{
		var epoch = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);
		return epoch.AddSeconds(unixTime);
	}
}
```

Make sure to install **Newtonsoft.Json** and **OxyPlot.WindowsForms** NuGeTs. Each service has its own JSON format.

For easily navigating the returned JSON data, notice that I am using C# **dynamic** binding feature.
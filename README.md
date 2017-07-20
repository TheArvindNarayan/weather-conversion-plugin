# weather-conversion-plugin
## jQuery

Flat Weather is an easy to use jQuery plugin that supports switching between two different weather API sources (OpenWeatherMap.org or Yahoo)1 without changing any of your front-end code.

The code is well-documented and small, with its only external dependency being jQuery, which makes it easy to deploy and extend. The plugin comes with an attractive flat UI css stylesheet, and uses the weather icons2 font to provide attractive and scalable visuals. As you can see from the examples below, there are plenty of display options that will let you customize the widget for your particular application without forcing you read the source. Responsive css keeps it looking good across desktop and mobile.

Don't like any of the default styles? Build your own markup with our uniform data API, which bridges the gap between the Yahoo and OpenWeather APIs and presents a simple and consistent JSON object to interact with. 

Notes:
OpenWeatherMap has better location support, so we recommend using it by default.
Be sure to place the /fonts folder one level above the path to the flatWeatherPlugin.css file.

# setup


<head>
  <!-- include flatWeatherPlugin.css and copy the font folder to one folder above your /css folder -->
  <link href="path/to/css/flatWeatherPlugin.css" rel="stylesheet">
  <!-- include a copy of jquery (if you haven't already) -->
  <script src="path/to/js/jquery-2.1.1.min.js"></script>
  <!-- include flatWeatherPlugin -->
  <script src="path/to/js/jquery.flatWeatherPlugin.js"></script>  

  <script type="text/javascript">
  $(document).ready(function() {

	## Setup the plugin, see readme for more examples
	var example = $("#example").flatWeatherPlugin({
	  location: "Boston, MA", //city and region *required 
	  country: "USA",         //country *required 
	  //optional:
	  api: "openweathermap", //default: openweathermap (openweathermap or yahoo)
	  //apikey: "your-api-key",   //optional api key for openweather
	  view : "partial", //default: full (partial, full, simple, today or forecast)
	  displayCityNameOnly : true, //default: false (true/false) if you want to display only city name
	  forecast: 4, //default: 5 (0 -5) how many days you want forecast
	  render: true, //default: true (true/false) if you want plugin to generate markup
	  loadingAnimation: true //default: true (true/false) if you want plugin to show loading animation
	  //units : "metric" or "imperial" default: "auto"
	});

  });
  </script>
</head>
<body>
  <!-- have a target for where you want it shown -->
  <div id="example"></div>

</body>

## ex. all default options. full view, openweathermap weather
var example1 = $("#example-1").flatWeatherPlugin({
	location: "Waterloo, ON",
	country: "Canada",
});


## ex. simple view, openweathermap weather
var example2 = $("#example-2").flatWeatherPlugin({
	location: "Toronto, ON",
	country: "Canada",
	api: "yahoo",
	view : "simple"
});

## today only detailed view and display city name only, 
var example3 = $("#example-3").flatWeatherPlugin({
	location: "San Francisco, CA",
	country: "USA",
	api: "yahoo",
	displayCityNameOnly : true,
	view : "today",
});

## forecast only view, no loading animation from yahoo weather
## **force metric units**
var example4 = $("#example-4").flatWeatherPlugin({
	location: "New York, NY",
	country: "USA",
	api: "yahoo",
	view : "forecast",
	loadingAnimation : false,
	units : "metric", //force metric units 
});


## ex. Auto Refresh example
var example5 = $("#example-5").flatWeatherPlugin({
  //first intilize the plugin per normal
  location: "Malmo",
  country: "Sweden",
  view : "simple",
});

## then setup an interval to make repeat calls to fetchWeather 
var intervalID = window.setInterval(function() {
  example5.flatWeatherPlugin('fetchWeather').then(success, fail);
}, 2*60*1000); //call every two minutes

//then handle the success and fail states of the fetchWeather promise
function success(data) {
  example5.flatWeatherPlugin('render', data);
} 

function error(data) {
  example5.flatWeatherPlugin('error', data);
} 



## for custom output call with render: false
var custom_example = $("#example-6").flatWeatherPlugin({
	location: "London",
	country: "UK",
	api: "openweathermap",
	render : false //the plugin will not generate any markup
});

## then manually call 'fetchWeather' which returns a jquery promise
## hen complete, result contains the weather data
custom_example.flatWeatherPlugin('fetchWeather')
  .then(function(data){
    console.log(data);
    ## you can then do whatever you want with the data
   ## such as generate your own custom markup
    $("<h2/>", {"class" : "wi wi"+data.today.code})
      .text(" " + data.city)
      .appendTo(custom_example);
    $("<p/>").html(data.today.temp.now + "°C, " + data.today.desc )
      .appendTo(custom_example);
});

## Data object from fetchWeather looks like this:
{
  location : String, //as returned back from api
  today : {
  	temp : {
  		//temperatures are in units requested from api
  		now : Number, ex. 18 
  		min : Number, ex. 24
  		max : Number ex. 12
  	},
  	desc : String, ex. "Partly Cloudy"
  	code : Number, ex. "801" used by css font, see css.
  	wind : {
  		speed : 4, //either km/h or mph
  		deg : Number, //direction in degrees from North
  	},
  	pressure : Number, //barometric pressure
  	humidity : Number, //% humidity
  	sunrise : Time,
  	sunset : Time,
  	day :  String,  

  },
  forecast : [
  //array
  	{
  		Day: String, 
  		code:Number, 
  		desc: String, 
  		temp : {min:number, max:number}
  		}
  	]
}
*/

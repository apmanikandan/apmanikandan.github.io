---
title       : My Weather Application
subtitle    : App built using shiny and slidify
author      : Manikandan
job         : VO
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Why Weather Forecast?

Most people travel around the globe and it is imperative for them to be aware and prepared for the weather in the destination. This is an simple application 
which takes the city name as input and gets the weather information of the city from http://openweathermap.org

This weather application is developed with the aim for wider usage across the nations. The application uses a weather API ,which is simple, clear and free.  The data provider also offers higher levels of support.

Access current weather data for any location on Earth including over 200,000 cities! Current weather is frequently updated based on global models and data from more than 40,000 weather stations . Data is available in JSON, XML, or HTML format . Call current weather data for one location

--- .class #id 

## Slide 2
You can choose these search methods:
By city name
By city ID
By geographic coordinates

By city name
Calls by city name. API respond with a list of results that match a searching word.
Calls by city name with country code using API

Current weather data

Access current weather data for any location on Earth including over 200,000 cities!
Current weather is frequently updated based on global models and data from more than 40,000 weather stations
Data is available in JSON, XML, or HTML format

--- .class #id 

## 5 and 16 day forecast

Both 5 day forecasts and 16 day forecasts are available at any location or city
5 day forecasts include weather data every 3 hours and 16 day forecasts include daily weather
Forecasts are available in JSON, XML, or HTML format

Historical data

Through our API we provide both city historical weather data and raw data from weather stations
Historical data is available for 1 month previous in both free and developer accounts, for 1 year previous in Professional accounts, 
and is unlimited in  Enterprise accounts


Weather stations

Access recent data from weather stations
Search weather stations close to a geographic location
Get historical weather station data
More than 40,000 weather stations around the world are connected to OpenWeatherMap    
Join our network of private weather stations and get convenient data gathering and monitoring for your weather station

--- .class #id 

## Weather map layers

Weather maps include precipitation, clouds, pressure, temperature, wind, and more!
Connect our weather maps to your mobile applications and websites
Use as layers in Direct Tiles, WMS, OpenLayers, Leaflet, Google Maps, and Yandex maps

The application can be upgraded in such a way to add more features

Serch accuracy

You can use our geocoding system to find cities by name, country, zip-code (not included in standard package) or geographic coordinates. You can call by part of city name. To make the calling result more accurate you can put the city name and country divided by comma.

To set the accuracy level either use the 'accurate' or 'like' type parameter. 'accurate' returns exact match values. 'like' returns results by searching for that substring. type ['accurate', 'like'] 

--- .class #id 

## Units format

Internal, metric, and imperial units are available.


Multilingual support

You can use lang parameter to get the output in your language. We support the following languages that you can use with the corresponded lang values: English - en, Russian - ru, Italian - it, Spanish - es (or sp), Ukrainian - uk (or ua), German - de, Portuguese - pt, Romanian - ro, Polish - pl, Finnish - fi, Dutch - nl, French - fr, Bulgarian - bg, Swedish - sv (or se), Chinese Traditional - zh_tw, Chinese Simplified - zh (or zh_cn), Turkish - tr, Croatian - hr, Catalan - ca 

--- .class #id 

## Summary

Overall this application was developed in R & Shiny and uploaded in the shiny apps page. The application takes one input and gets the weather data using api call, which is embedded in the server.R code.

 InputData

Based on the value provided in the input text box the cityname is looked-up using the API.

Output
 
The Data will be received from the website using API'call. The XML is parsed for the values of clouds condition, temperature,humidity for the country as prediction.
Temperature is gicen in kelvin and humidity in %

--- .class #id 

## Appendix


```r
library(XML)
cityWeather <- function(City) {
        address=City
        url = paste( "http://api.openweathermap.org/data/2.5/weather?mode=xml&q=", URLencode(address), sep="" )
        xml = xmlTreeParse(url, useInternalNodes=TRUE) # take a look at the xml output:
        condition=xpathSApply(xml,"//current/clouds",xmlGetAttr,"name")
        temp_c=xpathSApply(xml,"//current/temperature",xmlGetAttr,"value")
        humidity=xpathSApply(xml,"//current/humidity",xmlGetAttr,"value")
        country=xpathSApply(xml,"//current/city/country",xmlValue)
        city=xpathSApply(xml,"//current/city",xmlGetAttr,"name")
        return(cat(paste("The weather prediction for", city, "in",country,"reports", condition, "
The temperature is", temp_c, "Kelvin and humidity is", humidity, "%.")))
}
shinyServer(
        function(input, output) {
                output$inputValue <- renderPrint({input$City})
                output$prediction <- renderPrint({cityWeather(input$City)})
        }
)
```

```
## Error in eval(expr, envir, enclos): could not find function "shinyServer"
```
Do feel free to use and provide your feedback.
--- .class #id 



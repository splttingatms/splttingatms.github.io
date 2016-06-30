---
title: Web Scraping with Scrapy
layout: post
---
This weekend we started looking at mountain bikes for Cathy, but we were having trouble finding one with a proper standover height. I needed a way to quickly compare frame geometries and ideally sort by the standover height. I *could* click on each bike model, or I could try and automate the process. I figured this was a great time to learn [Scrapy](http://scrapy.org/)! Scrapy is a Python framework for extracting data from websites, aka a web scraper. The goal is to use Scrapy to extract the frame sizes from a manufacturers website and then sort the sizes afterwards. Scrapy uses CSS selectors to extract specific data from web pages so we will need to reverse engineer the pages by looking at their HTML markup.

### Reverse Engineer
As an example, I'll focus on bikes from Fuji since their website was the easiest to scrape. Fuji has all of their bikes listed at <http://www.fujibikes.com/bikes>. The mountain bikes are conveniently organized in an unordered list element with an appropriate "mtn" ID `<ul id="mtn">`.

<figure>
	<img class="img-responsive" alt="Fuji Bikes" src="/assets/web-scraping-with-scrapy/fuji-screenshot.png">
	<figcaption>A list of Fuji Bikes</figcaption>
</figure>

What we want to do is iterate through every bike in the list and follow the link to model's page. This is where we will find the frame geometry. The geometry is displayed as a table under `<div class="bk_content_geometry">`.

<figure>
	<img class="img-responsive" alt="Fuji Bike Page" src="/assets/web-scraping-with-scrapy/fuji-bike-screenshot.png">
	<figcaption>Fuji Bike Page</figcaption>
</figure>

### Putting it together
Now that we know where the data is located, we can begin to put Scrapy to work. The way Scrapy works is you create a spider that defines what information to extract and then use Scrapy to run the spider. You can then save the results to a JSON file. It is easiest to show the final spider and explain it after the fact.

<script src="https://gist.github.com/splttingatms/9b95e23c9d2289dc1c8d171ef566eb35.js"></script>

I called the spider the *FujiBikeScraper*. A Scrapy spider needs to contain a `parse(self, response)` function. This is the entry point that Scrapy uses to execute the spider. The `response` variable is the page that loads from navigating to the `start_urls`. Since we know that the mountain bikes are located under the "mtn" ID, lets go ahead and extract each link by using the css selector `response.css('#mtn a::atr("href")').extract()`. 

Now we can iterate through each link and parse each individual bike page. The way you follow a link in Scrapy is to call `scrapy.Request(url, callback)`. The callback is the method to execute after the link is followed, in this case the bike page will be parsed in `parse_bike(self, response)` method.

Now for each bike page, we are going to extract the standover height, the name of the bike, the frame size, and the original link to the bike. We can extract all of this information using CSS selectors.

One thing that is a bit tricky is how we get the standover height. Remember that the geometry is listed as a table. Each row in the table has a name that corresponds to what part is being measured. Since we want the standover height, the loop is checking each name and returns the value if the name of the row is "STANDOVER".

Running the spider is a one line Scrapy command.

```
scrapy runspider fujiBikeScraper.py -o fujiBikes.json
```

And the output comes out as expected. You can now further process the JSON or leave it as is.

```json
[
{"link": "/rakan-29-11-", "name": "RAKAN 29 1.1 ", "standover": "766.4", "frame": "(S)15"},
{"link": "/tahoe-275-11-2", "name": "TAHOE 27.5 1.1 ", "standover": "732.5", "frame": "(S)15"},
{"link": "/rakan-29-13-", "name": "RAKAN 29 1.3 ", "standover": "766.4", "frame": "(S)15"},
{"link": "/rakan-29-15-", "name": "RAKAN 29 1.5 ", "standover": "766.4", "frame": "(S)15"},
{"link": "/tahoe-29-132", "name": "TAHOE 29 1.3", "standover": "791", "frame": "(S)15"},
...
]
```



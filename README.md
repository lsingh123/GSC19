# gsoc

Final submission for my Google Summer of Code 2019 project. I worked at the Internet Archive on the Wayback Machine Team. Thank you to my mentor, Mark Graham, for all his guidance this summer. I am grateful for the opportunity to work with the incredible people at the Archive and to learn about the awesome work they're doing. 

## World News

I spent the majority of my time on the World News Project. Final, operationalized code for the project can be found at the [GSC19worldnews repo](https://github.com/lsingh123/GSC2019worldnewsproject)

Old scripts, one-off scripts, and broken scripts live in [this repo](https://github.com/lsingh123/internet_archive_old)

I wrote about the project's goals in a blog post that will be published on the Internet Archive's blog. 

**TODO**

- push data to Wikidata (maybe using [Cradle](https://tools.wmflabs.org/wikidata-todo/cradle/#/))
- start archiving all sources 
- further metadata collection
- continue adding new links as found

**DONE**

- collected over 300,000 news sources from APIs, existing lists, and databases
- cleaned and deduplicated this list resulting in slightly over 100,000 URLs
- collected metadata about news source
- set up a local database to host the data: INSERT LINK
- created visualizations of the data: INSERT LINK

As part of this project, I set up a local Apache Jena Fuseki databse to store the data. See my [Fuseki starter pack](https://lsingh123.github.io/gsoc19/fuseki) for more information.

## IA Web Meter

As an "extra credit" project, I also did some very basic proof of concept work to create visualizations of the data produced by IA Web Meter, an API to access internal metrics of the Wayback Machine. This code is nowhere near operationalized or clean. It is a very rough proof of concept. Code can be found [here](https://github.com/lsingh123/ia_webmeter_viz)

## Mueller Report

I also helped manage work on the Mueller Report project briefly. The goal of this project was to add links to all the references in the Mueller Report. Here's a [blog post](https://blog.archive.org/2019/07/19/the-mueller-report-now-with-linked-footnotes-and-accessible/) detailing our efforts. 

I wrote a quick scraper to grab all the links and metadata from the NYT version of the Mueller report as a starting point. Here is the [code](https://github.com/lsingh123/mueller_report) for that. https://github.com/lsingh123/mueller_report



# Fuseki Starter Pack

## Setup

(1) The host machine must have JDK installed and set up. If JDK is already set up, then skip ahead to Step 3. To check if you have JDK, run:

```
java -version
```

(2) Install JDK:

```
sudo apt install default-jdk
```

(3) Download fuseki with:

```
wget "https://mirrors.koehn.com/apache/jena/binaries/apache-jena-fuseki-3.12.0.tar.gz"
```

(4) Unzip the tarball:

```
tar -xvzf apache-jena-fuseki-3.12.0.tar.gz
```

(5) To start the server, run: 

```
cd apache-jena-fuseki-3.12.0
 
./fuseki-server --update --mem /d 
```

To stop the server run: 

```
kill `ps -ef | grep 'java.*fuseki' | grep -v grep | awk '{ print $2 }'`
```

(6) The server will run on Port 3030. To expose port 3030 to the outside world:

- drop a fragment into /etc/ferm/input: 
```
proto tcp dport 3030 ACCEPT;
```

- run 
```
sudo service ferm reload
```

Additional resources:
[Jena Documentation Download Instructions](https://jena.apache.org/download/)

[JDK download instructions](https://docs.oracle.com/javase/9/install/installation-jdk-and-jre-linux-platforms.htm#JSJIG-GUID-19D58769-FD72-4353-A935-40FCD82A7A81)

[Wiki on Exposing a Port to the Outside World](https://docs.google.com/document/d/1k41bw2D32_1DqnpPBmu_E4RdOuewppOPn7CjKA6dFhU/edit#)

## Query Instructions

Datasets can be found here:
http://wwwb-db01.us.archive.org:3030/dataset.html

Please use the /wn database. The others are either empty or outdated. 

Please only adjust the highlighted portions of the query, otherwise it is likely to break. The http:// is NECESSARY before each url or your query will break. The /item after subject URIs within each graph is also necessary. 

If you try to download all the sources in the database, the query will take a LONG time, so be sure that you really need to do this before trying it.

## Querying

In the SPARQL Endpoint field, type `/wn/query`

### If you know the exact URL we have stored in our database: 

This query will return all metadata for the news source with URL `nytimes.com`. 

```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wnp: <http://worldnews/property/>
PREFIX wni: <http://worldnews/item/>
SELECT DISTINCT ?url ?country ?title ?language ?type ?description ?title_native ?region ?wikipedia_name ?wikipedia_page ?metasource ?paywall ?path
{ GRAPH <http://www.nytimes.com> {
    ?item wdt:P1896 ?url .
    OPTIONAL {
      ?item wdt:P17 ?country . }
    OPTIONAL {
      ?item wdt:P1448 ?title . }
    OPTIONAL {
      ?item wdt:P37 ?language . }
    OPTIONAL {
      ?item wdt:P31 ?type .}
    OPTIONAL {
      ?item wnp:metasource ?metasource .}
    OPTIONAL {
      ?item wdt:1704 ?title_native .}
    OPTIONAL {
      ?item wdt:131 ?region .}
    OPTIONAL {
      ?item wnp:wikipedia-name ?wikipedia_name .}
    OPTIONAL {
      ?item wnp:wikipedia-page ?wikipedia_page .}
    OPTIONAL {
      ?item wnp:paywalled ?paywall .}
    OPTIONAL {
      ?item wnp:haspath ?path .}
    OPTIONAL {
      ?item wnp:description ?description.}
  }
}
```

To add a different url, change the portion after http://. Please make sure to include http:// before the canonicalized form of the URL. If your query isn’t working, try this next one.

### If you don’t know the exact URL we have stored in our database:

If you’re looking for a URL and it’s not coming up, it may be in the database in a different form. Try using this query instead. Warning: This query is slower so only use it if the one before isn’t working:

```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wnp: <http://worldnews/property/>
PREFIX wni: <http://worldnews/item/>
SELECT ?url ?country ?title ?language ?type ?title_native ?region ?wikipedia_name ?wikipedia_page ?metasource ?paywall
{ GRAPH ?g {
    ?item wdt:P1896 ?url .
    OPTIONAL {
      ?item wdt:P17 ?country . }
    OPTIONAL {
      ?item wdt:P1448 ?title . }
    OPTIONAL {
      ?item wdt:P37 ?language . }
    OPTIONAL {
      ?item wdt:P31 ?type .}
    OPTIONAL {
      ?item wnp:metasource ?metasource .}
    OPTIONAL {
      ?item wdt:1704 ?title_native .}
    OPTIONAL {
      ?item wdt:131 ?region .}
    OPTIONAL {
      ?item wnp:wikipedia-name ?wikipedia_name .}
    OPTIONAL {
      ?item wnp:wikipedia-page ?wikipedia_page .}
    OPTIONAL {
      ?item wnp:paywalled ?paywall .}
  }
  FILTER regex (str(?g), "*cnn*")
}
```
The text inside asterixes is the value that you think should be contained in the url. (Running this query will show that there’s a lot of cleaning work left to be done)

### To find sources for a specific country:

This query will return urls of all the sources for the country with wikidata code Q16 (Canada).

```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wnp: <http://worldnews/property/>
PREFIX wni: <http://worldnews/item/>
SELECT ?url 
{ GRAPH ?g {
      ?item wdt:P17 wd:*Q16* .
      ?item wdt:1896 ?url . 
  }
}
```

### To count how many sources we have:

```
SELECT (COUNT(*) as ?count) 
WHERE 
  { GRAPH ?g
      { } 
  }
 ```
 
 ## Updating
 
In the SPARQL endpoint field, type: `/wn/update`

### To insert a new entry without overwriting existing metadata:

```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wnp: <http://worldnews/property/>
PREFIX wni: <http://worldnews/item/>
INSERT {
  GRAPH <http://www.cnn.com> {<http://www.cnn.com/item> wdt:P17 wd:Q30 }  }
WHERE {FILTER (NOT EXISTS {GRAPH <http://www.cnn.com> {?item wdt:P17 ?country}})} ;
```

You can also use this query to add additional metadata to an existing source. 

### To automate adding sources:

Look at the scripts in [this](https://github.com/lsingh123/GSC2019worldnewsproject) repo, specifically `feed_fuseki.py`

## Additional Resources

[RDF Intro](https://jena.apache.org/tutorials/rdf_api.html)

[SPARQL tutorial](https://jena.apache.org/tutorials/sparql.html)

[Wikidata Query Service](https://query.wikidata.org/) (really good resource to practice SPARQL queries)






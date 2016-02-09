# Tutorial Reflections
_Feb 8, 2016_

Zotero was a pain. I continue to struggle with python. Well it's not really a struggle so much as I get confused with how and where I should be initiating python scripts. It is a shame because I really like scripts and I like how they provide a sort of step-by-step overview how I got from point A to point B. This process would/could/will have major implications on the conclusion drawn from research. Making python scripts I might use in my work available to the public also means that others could review my steps and see if there are any variables I missed or areas that need altering to improve the operations of the script. 

I significant source of my confusion with this are the tutorials from [Programming Historian](http://programminghistorian.org/lessons), most which are written for Mac or Linux users, thereby requiring me to do a sort of _translation_ of the steps for Windows. At times this can be quite difficult because I'm no veteran of the coding/computing world. All I really know how to do is play video games and build an average-looking website.

However, the [SPARQL](http://programminghistorian.org/lessons/graph-databases-and-SPARQL) tutorial on PH was excellent. There was a lot of information ~~took a couple rounds of reading to get through~~ and it was laid out in a, comparitively, really accessible way. The construction of this tutorial, in my opinion, provided more oppurtunities for experimentation, however, that may just be the nature of SPARQL and the British Museum's online collection. When I first attempted to experiment with obtaining information about donors of to the British Museum I kept getting errors, I realized that as a tool SPARQL is useful only if you already know how the database is built. You need to have some idea of the results you are looking for in order to create a proper SPARQL query.


```# Programming Historian SPARQL Querry

SELECT ?object
WHERE {

  # Search for all values of ?object that have a given "object type"
  ?object bmo:PX_object_type ?object_type .

  # That object type should have the label "print"
  ?object_type skos:prefLabel "print" .
}```

During our class discussion, the idea of constructing a query _backwards_ was brought up. Beginning with the object page in the collection and identifying the specifc query that you want to search and then editing your SPARQL to look for that field.

```# My Edited SPARQL Query

# To run the prefix ecrm, you must include CIDOC-CRM Ontology
PREFIX ecrm: <http://erlangen-crm.org/current/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT ?object
WHERE {
# Searching for all objects that have a common "person or institution"
  ?object ecrm:P51_has_former_or_current_owner ?person_institution .
  ?object_type skos:prefLabel "print" .
}```
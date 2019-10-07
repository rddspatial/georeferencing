documentation:
@developer: Rahul Deb Das
06.10.2019
-------------------------------------------------------------------------------------------

This project intends to retrieve geographic placenames from unstructured text, for example, tweets, 
facebook posts, people's review on travel blogs, to name a few. Once the placenames are
detected they can be geocoded to plot them on a map or to use them in other location-based service(s)
or decision making process.
For example, the text 'There is a heavy traffic near Bapuji marg in Mumbai' contains two placenames,
Bapuji marg and Mumbai and the model ideally detects them and assigns coordinates to those placenames
in an automated manner.

The main driver class is GIR_Driver.java and located in GeographicInformationRetrieval_RD --> GIR_Driver.java

You can add (queriedd) texts in the driver class either by manual addition (hardcoded) or from a file/database.
Each queried text is assumed to contain some placenames. The model detects the placenames and
maps them to a unique spatial footprint (coordinate) on the earth. The coordinates are based 
on WGS84 coordinate system. Currently there are two example texts provided (text1, text2) just for 
demonstration purpose. But the code can be easily customised for batch processing by reading the texts from 
a file or database in an efficient manner and get the placenames retrieved with their associated information
(coordinate, placetype, state, country). 

The main method primarily triggers getToponymList method in ToponymRetriever class which in turn 
calls other methods from other classes to do the necessary jobs (for details see the description of the 
workflow in the paper). The constructor of ToponymRetriever class is also the entry point and initializer
of three toponym retriever (OpenNLP, Stanford NLP and Rulebase). 

OpenNLP is retrained on a training data (./res/OpenNLPTraining_NER_loc.txt). See the training .txt file to
see how to annotate (tag) the location mentions which can be read by Apache OpenNLP NER.
Stanford NLP is used as it is provided and trained on formal data.
Rule base is based on spatial rules (see the paper for details) which leverages spatial prepositions,
Noun POS and a hand-crafted lexicon of vernacular placenames. The lexicon is provided in ./res/post_vernacular.txt

To improve the performance of the model or to adapt it to other geographical area, add new and more training 
data in OpenNLPTraining_NER_loc.txt. Vernacular placenames are peculiar to a specific geographical region, although
our vernacular lexicon contains lots of generic object types in English terms that are globally used in almost all 
the geographical regions (e.g., park, college, street, road, hospital, market, building, bridge etc), 
which generally appear after a placename. Thus the existing model will work in different geography with varied
accuracy,  with the best performance in Indian cities, specifically in Mumbai as the training data and lexicon 
is more focussed on Mumbai. However, to adapt this geoparser to another region or to improve the performance, 
the existing lexicon can be updated with the vernacular names or more object types critical 
to the given geography. In the given lexicon some of the vernacular terms critical 
to the Indian cities are 'marg' (road), 'sadak' (road), 'chowpatty' (where fishermen live), 
'chowk' (junction), 'bhavan' (building) etc. 

Once the placenames are detected, OpenStreetMap (OSM) is used to geocode the places. 
To do this Nominatim API was used which is implemented in GeoCoderOSMExtended --> OpenStreetMapUtils.java 
(To use OSM Nominatim API, no username or secret key is required. But for geonames API you need a
username / secret key).
For placename (toponym) disambiguation, a spatial_context was used which is Maharashtra in this case, but
depending on the study area, the spatial_context can be changed (see the driver class).
A provision is also made to incorporate geonames for placename lookup and geocoding purpose but 
not used in this project. To use geonames API, use your own username (registered with geonames).

This project is compatible with java 1.8, opennlp-tools-1.8.1 and Stanford-corenlp-3.8.0.
To develop the dependency please check the following jar libraries and .bin files in ./res project folder.

1. geonames-1.1.13.jar
2. hamcrest-core-1.1.jar
3. jdom-1.0.jar
4. json-simple-1.1.1.jar
5. junit-4.10.jar
6. log4j-1.2.17.jar
7. opennlp-tools-1.8.1.jar
8. sl4j-simple.jar
9. stanford-corenlp-3.8.0-models.jar
10. stanford-corenlp-3.8.0.jar 
11. en-pos-maxent.bin
12. en-token.bin
13. ner-location-custom-model.bin

How to run this code?

Download the zip folder and import it in your Eclipse environment. Check all the dependencies. 
For space constraint we have removed stanford library from the resources. 
Please download stanford-corenlp-3.8.0.jar 
and stanford-corenlp-3.8.0-models.jar from https://stanfordnlp.github.io/CoreNLP/. 
Build the dependencies by importing all the above listed jar files in your project resource. 
Go to the Driver class located in GeographicInformationRetrieval_RD package
and run the code. Enjoy!!!

For any question please contact: spatialinformatica@gmail.com

documentation:
@developer: Rahul Deb Das
06.10.2019
-------------------------------------------------------------------------------------------

This project intends to retrieve geographic placenames from unstructured text and geocode them.

The main driver class is GIR_Driver.java and located in GeographicInformationRetrieval_RD --> GIR_Driver.java

You can add texts in the driver class either by manual addition or by reading a file.
In this version the driver class allows hardcoded addition of texts for demonstration only. 
But the code can easily be customised for batch processing by reading the texts from 
a file or database in an efficient manner by looping over a file/database and extract each text from a 
row/record and pass it to the code block in main method and get the placenames and its associated information
(coordinate, placetype, state, country) returned. 

The main method mainly primarily triggers getToponymList method in ToponymRetriever class which in turn 
calls other methods from other classes to do the necessary jobs (for details see the description of the 
workflow in the paper). The constructor of ToponymRetriever class is also the entry point and initializer
of three toponym retriever (OpenNLP, Stanford NLP and Rulebase). 

OpenNLP is retrained on a training data (./res/OpenNLPTraining_NER_loc.txt). See the training .txt file to
see how to annotate (tag) the location mentions which can be read by Apache OpenNLP NER.
Stanford NLP is used as it is provided and trained on formal data.
Rule base is based on spatial rules (see the paper for details) which leverages spatial prepositions,
Noun POS and a hand-crafted lexicon of vernacular placenames. The lexicon is provided in ./res/post_vernacular.txt).

To improve the performance of the model or to adapt it to other geographical area, add new and more training 
data in OpenNLPTraining_NER_loc.txt. Vernacular placenames are peculiar to a specific geographical region, although
our vernacular lexicon contains lots of generic object types in English terms that are globally used in almost all the
geographical region (e.g., park, college, street, road, hospital, market, building, bridge etc), which generally
appear after a placename. But to adapt this geoparser to another region, a new or modified vernacular lexicon can be added
or updated the existing one.

Once the placenames are detected, OpenStreetMap (OSM) was used to geocode the places. 
To do this Nominatim API was used which is implemented in GeoCoderOSMExtended --> OpenStreetMapUtils.java (no username or secret key is required) 
For placenmae (toponym) disambiguation, a spatial_context was used which is Maharashtra in this case, but
depending on the study area, the spatial_context can be changed (see the driver class).
A provision is also made to incorporate geonames for placename retrieval and geocoding purpose but 
not used in this project. To use geonames API, use your username (registered with geonames).

This project is compatible with java 1.8, opennlp-tools-1.8.1 and Stanford-corenlp-3.8.0.
to develop the dependency please check the following jar libraries and .bin files in ./res

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

For any question, contact: spatialinformatica@gmail.com

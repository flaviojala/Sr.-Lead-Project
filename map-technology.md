# Introduction

The second half of the 20th century was a turning point for cartography. Computers became a tool of choice. Aerial photography, satellite imagery, and remote sensing changed the way spatial data is gathered. Geographic Information Systems (GIS) were born. Eventually, GIS maps started moving from the desktop to the web, and big GIS vendors started making the first frameworks for online maps.

But GIS mapping is not easy. It requires many server-side technologies, geospatial standards, and protocols, along with their implementations. It requires understanding geospatial data and map projections, knowledge of how to gather the data, how to display the data, which colors to use, how to generalize the data to specific scales, how to place labels on the map, how to set up a server that will serve the maps, how to use a spatial database, and so on. GIS is full of abbreviations, such as WMS, WFS, EPSG, CRS, SLD, GML, TMS, just to name a few, and to know and understand them, you need to read books, academic papers and articles.

The first web maps typically showed only a single, very small map image. At that time, panning was implemented by moving one step, usually by half of the map size, in one of eight possible compass directions - N, NW, W, SW, S, SE, E, NE. After the user clicked the pan or zoom button, a whole new image would need to be rendered on the map server, loaded over the network, and then processed by the browser. Because of the constraints of the technology, maps only occupied a very small part of the whole web page. To get better interaction, early maps required plugins like Flash or propriety plugins based on Java, or even ActiveX, which worked only in Internet Explorer.

Google turned the mapping world upside down when it introduced Google Maps in 2005. Among its innovations, Google introduced continuous panning by dragging. Their solution was to display a map sliced into many smaller square images called “tiles”. These tiles were rendered and served from a “map tile server,” and are usually 256 x 256 pixels. Zooming and panning now only required loading new map tiles instead of reloading the entire web page. The result was a bigger visible map that covered more than half of the browser window, and offered a smooth experience for exploring the map. Because of the ability to “slip” the map around with the smooth zooming and panning functions, these new maps were called “slippy maps”. Google also allowed scripting, so users could put Google’s maps on their own websites and add their own data to the map. This resulted in another new term being coined: “Map mash-ups.”

Suddenly, to add a nice-looking map to your website, you no longer needed to be a cartographer or GIS expert and online maps became popular.

<img src="https://learn.microsoft.com/en-us/bingmaps/articles/media/5cff54de-5133-4369-8680-52d2723eb756.jpg"
     alt="bing map tile system exemple"
     style="float: left; margin-right: 10px;" />
Ref: bing map tile system exemple

## Map Data Provider

As mentioned before, early online maps were based on sets of GIS data and their spatial geodatabases. Not many people had access to that data, not to mention its price tag. Google and the OpenStreetMap (OSM) project datasets changed that. Google’s database is private and comes with restrictions, while OSM was inspired by the concept of Wikipedia, as a collaborative project to create a free map of the world. OpenStreetMap is built by a community of volunteer mappers, who contribute to and maintain the spatial data.

[![osm-data-flow.jpg](https://i.postimg.cc/L5P4mWjB/osm-data-flow.jpg)](https://postimg.cc/kRnPcf1B)

### About OpenStreetMaps

OpenStreetMap (OSM) is a digital map database of the world built through crowdsourced volunteered geographic information (VGI). OSM is supported by the nonprofit OpenStreetMap Foundation(link is external). The data from OSM is freely available for visualization, query, download, and modification under open licenses(link is external).

OSM works in a style similar to Wikipedia, in which virtually all features are open to editing by any member of the user community. OSM was conceived in 2004 and has grown to over two million registered users(link is external) since that time. Although only a fraction of these are frequent map editors, the map has matured enough in some locations to the point where its detail and precision rival "authoritative" datasets from governments and commercial entities. This is particularly true in Western Europe and some parts of the US. The image below of the Penn State campus provides an idea of the intricate features that can be submitted to OSM.

OSM originally gained popularity in places where government data was not freely available, but a thriving GIS community existed. For example, in the mid-2000s, the UK Ordnance Survey data was available only for purchase, and OSM grew rapidly as an attractive free alternative. In places where governments were willing to freely share their data, bulk upload negotiations were sometimes arranged. For example, the US has fairly thorough road coverage due to a US Census TIGER street data bulk upload.

OSM volunteer efforts constitute a social event and hobby for many, who gather for group data collection events known as "mapping parties." These activities organized armies of volunteers to walk, bike, and drive through sectors of a city with GPS units and notepads, returning later to a central lab to enter the data (Perkins and Dodge 2008). Although this is still useful in cases, nowadays, many OSM beginners can get pretty far just through tracing aerial photographs in simple browser-based editors. In addition to a physical exploration of the city, mapping parties now offer training, awareness, and renewed enthusiasm of OSM (Hristova et al. 2013).

#### Benefits and weaknesses of OSM

OSM is not the only crowdsourced VGI project, but it is one of the most well known. As such, it provides a useful exemplification of the pros and cons of crowdsourcing and VGI.

##### Some of the main benefits of OSM include

There is no cost to use the data. In some locales, OSM may be the only freely available source of high-resolution GIS vector data. In other places, it may be the only source of data.
The source data is available for download and use in derived cartographic products. Here OSM differs from the crowdsourcing mechanism that Google Maps deployed, called Map Maker. Cartographers cannot download and re-use Google Map Maker data; whereas OSM is available to anyone with enough technical know-how to get the data.
Because OSM allows people to add any type of feature, it may include a richer and more socially valuable set of features than commercial or government maps. Possibilities include trees, wheelchair ramps, food banks, spigots for potable water, and so forth.
OSM data is flexible and can quickly be updated in the event that a new business opens, a bridge gets washed away, etc. In contrast, commercial and government maps tend to be updated on fixed cycles.

##### Some of the main challenges of OSM include

There is no systematic quality check performed on the data. You use the data at your own risk and should exercise care when using OSM information for mission-critical functions.
The detail, precision, and accuracy of OSM coverage varies across space, without a simple means of detecting the variation. In the global South, some cities are missing basic street data, let alone other useful features such as parks, schools, and civic buildings. The Missing Maps project, heavily promoted by the American Red Cross, is one organized effort attempting to address this situation.
OSM is subject to contributor biases. Each contributor must make decisions about the types of features he or she will place on the map and the places he or she will map. Looking at the map of any given place on openstreetmap.org, we have little idea of who created the map and why. The browser-based tool Crowd Lens for OpenStreetMap attempts to offer a window into the crowd that created OSM in a particular place. It shows that some places have garnered much more attention than others.
The OSM community decides the types of features that are worthy of the community tagging system. This is accomplished through a semi-formal proposal and voting process which tends to reflect the interests of the contributors. In 2013, Stephens lamented that there were multiple tags for marking sexual entertainment venues in OSM, whereas proposals for tags denoting hospice services and daytime childcare had floundered. She attributed this directly to the fact that a significant majority of OSM contributors are male. Recent speakers at OSM conferences have raised attention to the consequences of gender and other imbalances in OSM, and have provided suggestions of how to make the OSM community more diverse.
Unless you're just viewing the default map tiles, getting a focused set of data out of OSM often takes a lot more technical skill than getting the data in. Data ingress and retrieval techniques are covered in the lesson walkthrough and assignment, where you can judge this point for yourself.

## Choosing a Web Mapping Framework

| Technology | Google Maps | Mapbox GL JS | OpenLayers | Leaflet | Bing Maps | Map Quest | HERE Maps | ArcGIS | MapTiler Cloud |
| ---------- | ----------- | ------------ | ---------- | ------- | --------- | --------- | --------- | ------ | -------------- |
| Map data Provider | own provider | own provider | OSM | OSM | own provider | own provider | own provider | own provider | own provider |
| Open Source? | :no_entry: | :no_entry: | :white_check_mark: | :white_check_mark: | :no_entry: | :no_entry: | :no_entry: | :no_entry: | :no_entry: |
| Integration with React? | :white_check_mark: | :white_check_mark: | :no_entry: | :white_check_mark: | :no_entry: | :no_entry: | :no_entry: | :no_entry: | :no_entry: |
| Official Integration with React? | :white_check_mark: | :white_check_mark: | :no_entry: | :white_check_mark: | :no_entry: | :no_entry: | :no_entry: | :no_entry: | :no_entry: |
| Frendly Documentation? | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Direction/Routes | :white_check_mark: | :white_check_mark: | :white_check_mark:* | :white_check_mark:* | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Distance Matrix | :white_check_mark: | :white_check_mark: | :white_check_mark:* | :white_check_mark:* | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Geocoding | :white_check_mark: | :white_check_mark: | :white_check_mark:* | :white_check_mark:* | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |

OSM - OpenStreetMaps.
*with plugins.

for our project, we wanted a technology that was open source, compatible and easy to implement with React (the library chosen for the front-end), among the various services surveyed, according to the table above, the only ones eligible for the project would be OpenLayers and Leaflet both using geographic data provided by OpenStreetMaps.

### About OpenLayers

OpenLayers was developed by MetaCarta as an open source equivalent to Google Maps, and the first version was published in June 2006. OpenLayers is an onling mapping tool that implements a JavaScript API for building rich web-based geographic applications, with an API similar to the Google Maps API. OpenLayers gained a lot of traction very fast, and development in the beginning was rapid. OpenLayers 2 was released only two months after version 1, in August 2006. The library was constantly under development, and new versions with new features were constantly being added. The downside of this rapid progress was that the version 2 library became very big and clunky, eventually reaching 1MB in size and containing over 100,000 lines of code! While it came with a lot of features, not all were needed by regular users.

This was the major reason for a comprehensive rewrite of its library. The goal was to target the latest HTML5 and CSS3 features, with the same functionality from OpenLayers 2, such as support for projections, standard protocols, and editing functionality. The main focus was on performance improvements, lighter builds, prettier visual components, and a better API. This much-improved OpenLayers 3 was published in August 2014.

#### Pros

- Free and open source.
- Feature-packed library for your mapping needs.
- Plenty of examples.
- Support for a range of data types and GIS standards.
- Built-in support for map projections and editing features.

#### Cons

- Version 3 is still in heavy development, and the API is still changing with every point release.
- Complicated API syntax.
- Version 3 documentation is currently not as thorough as it could be.

### About Leaflet

It is safe to say that Leaflet was born as a reaction to OpenLayers’ bloat, clutter and complexity. Vladimir Agafonkin was asked to build a wrapper around OpenLayers, but he instead created a simple and lightweight OpenLayers alternative, and in May 2011 Leaflet was born. Vladimir focused on simplicity, performance and usability for this online map tool. The core library has only basic functionality, which is enough for most real-life use cases. Still, Leaflet can be extended with a huge amount of plugins that are easy to develop and add on top of the core library. Additionally, Leaflet was developed from scratch with mobile support in mind.

Leaflet is easy to use and has a well-documented API, along with simple source code that is available on GitHub. As a result of its focus on performance, usability, simplicity, small size, and mobile support, it is significantly less complicated than OpenLayers.

Leaflet’s future is looking interesting, too. According to Vladimir, he plans for the next major release to be even simpler, improving performances further, and upgrading the plugin infrastructure.

#### Pros

- Free and open source.
- Small and fast.
- Simple and easy API syntax.
- Mobile friendly.
- Good for getting an online map up quickly and easily.
- Plenty of examples with very good documentation.

#### Cons

- Lack of advanced functionality

## Conclusion

The disadvantage presented for the leaflet is easily circumvented by adding a plugin to the application. In addition, there is an official library for using a leaflet with react, which facilitates its integration with the project.

For all the arguments presented, the Leaflet framework was the chosen map technology to compose the project.


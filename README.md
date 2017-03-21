**Final Project: Web Map - Soccer in the Pacific Northwest**

    Winter 2017 | Geography 371 | Geovisualization: Web Mapping | Oregon State University

    Author: Riley Johnson| Instructor: Bo Zhao | TA: Andy Wilson | Location: 210 Wilkinson

This is a web map about soccer in the Pacific Northwest. Soccer is quickly gaining popularity in the United States but here in the Pacific Northwest, the game has had a substantial following for over 40 years. This web map serves to look at the history of the three major teams, Seattle Sounders FC, Vancouver Whitecaps FC, and the Portland Timbers by tracing where and when each team played. However, this web map also has another purpose. While it is important to acknowledge the impact that the three big teams have had in establishing the game in the Pacific Northwest, soccer has come a long way since then. The success of the United States Women's National Team has given rise to creation of professional women's soccer leagues, which have some of its strongest teams in the Pacific Northwest. The key part of this web map however, is the its intention to introduce localism. People love to cherish and support the three big teams because of their success and name recognition. While it is valuable to support these teams, recognizing that there are many smaller teams is just as important. This web map ties to show that while we can support the big three, looking closer reveals a multitude of other teams, both professional and amateur.

The web map explores the history of the Seattle Sounders FC, Vancouver Whitecaps FC, Portland Timbers, the rivalries that make the Pacific Northwest unique, women's soccer, and all the current professional and amateur men's and women's teams that populate the Pacific Northwest.

1. Homepage

From the homepage there are three main sections that make up the bulk of this web map.

Profiles

The profiles section is the key component of the web map. There are six profiles, each with specific content. The first three explore the three main teams of the Pacific Northwest and the last three explore other areas in the Pacific Northwest that are either unique and influential to the impact of the game.

Seattle Sounders FC: explores the history of the team and where they have played throughout their history;

Vancouver Whitecaps FC: explores the history of the team and where they have played throughout their history;

Portland Timbers FC: explores the history of the team and where they have played throughout their history;

Rivalries of the Pacific Northwest: explore what makes the Pacific Northwest unique and influential in soccer culture;

National Women's Soccer League: explores the impact of women's soccer in the pacific northwest and the country;

Soccer Clubs in the Pacific Northwest: explores all professional and amateur men's and women's teams in the Pacific Northwest.

Each is accessible through a series of modals that when clicked open to reveal information about each area as well as an interactive map relating to each.

Example:

example

example

About

This section explores the purpose of this web map and introduces what is included

Script

This is a sample of what each web map looks like with each modal. Each modal needs to be treated as a separate map. The means that if a new modal is added, the location and map name will need to change. For example the script below is for the first modal only and is named map. The next modal, profile2 will need to be named map2 for the tile layer to render in the modal.

//This function allows for the tilelayers to completly render when opening up the modal
(function() {
        var ev = new $.Event('style'),
            orig = $.fn.css;
        $.fn.css = function() {
            $(this).trigger(ev);
            return orig.apply(this, arguments);
        }
    })();

    var map = L.map('map', {center: [47.54550104198087, -122.30016410000002], zoom: 11});
    L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token=pk.eyJ1IjoiamFrb2J6aGFvIiwiYSI6ImNpcms2YWsyMzAwMmtmbG5icTFxZ3ZkdncifQ.P9MBej1xacybKcDN_jehvw', {
        maxZoom: 18,
        attribution: '<a target="https://www.mapbox.com/maps/">Mapbox</a>',
        id: 'mapbox.streets',
        detectRetina: true
    }).addTo(map);
    //L.geoJson.ajax('assets/SoundersHistory.geojson').addTo(map);

    $('#portfolioModal1').bind('style', function(e) {
        map.invalidateSize();
        $("body").css({'padding-right': "0px"});
        // $(".Text").css('margin-top', vertAlign);
    });

The tile layer I am using comes from Map Box. The streets color helps the main features/theme of a web map stand out while mirroring the colors that of Pacific Northwest. Moreover, the color allows for a more detailed look at each points location. While Map Box is being used as the base map, several other work also well, such as OpenStreetMap, Google Maps, Bing Layers, or any other base map tile layer one may find.

Geojson Layers

Added to each map layer is a geojson layer. Each geojson layer for this web map was created by collecting data from Google and creating a kml file in Google My Maps. These kml's were then ported into Qgis to create the geojsons. The following script places the geojson point layer onto the map.

//This allows for each geojson to be loaded onto the map
var SoundersHistory = L.geoJson.ajax('assets/SoundersHistory.geojson', {
        onEachFeature: function (feature, layer) {
            layer.bindTooltip(feature.properties.Name + "<br />" + feature.properties.Description);
        }, pointToLayer: function (feature, latlng) {
            var marker = null;
            if (feature.properties.Team == "Y"){
                marker = L.marker(latlng,{icon: soccer2Icon});
                return marker
            }
            //return marker;
        }
    });

Multiple geojons can be added to the same tile layer. Simply repeat the above script and change the geojson file. The image below shows two different geojson layers.

example3
2. Custom Point Markers

The geojson layers when added to the basemap have blue default icon these can be changed by creating custom icons

You can make your own icons by using an existing image or creating one using graphics software (i.e. Illustrator, Photoshop). If your graphic is saved as an image (the most space efficient images for the web are usually in png or jpg format), and upload it to your server for use as an icon.

// Create Custom Icons Here
 var soccerIcon = L.icon({
        iconUrl: 'img/portfolio/soccerball.png',
        shadowUrl: 'img/portfolio/soccerball_sd.png',
        iconSize:     [20, 20],
        shadowSize:   [27, 25],
        iconAnchor:   [16, 16],
        shadowAnchor: [16, 16],
        popupAnchor:  [-8, -18]
    });

This code creates the soccer ball icons that can be seen on the maps. Each time an a new icon is created this script must be added.
3. Map Elements

Each map contains a series of elements that help make the more readable and easier to understand.
4.1 Add a Legend

//This map script creates a legend that is added to the map. 
legend.onAdd = function () {

        // Create Div Element and Populate it with HTML
        var div = L.DomUtil.create('div', 'legend');
        div.innerHTML += '<b>Competition</b><br />';
        div.innerHTML += '<img src="img/portfolio/soccerball.png" height="18px" width="18px"><p>Cascadia</p>';
        div.innerHTML += '<img src="img/portfolio/redball.png" height="18px" width="18px"><p>Canadian</p>';
        // Return the Legend div containing the HTML content
        return div;
    };
    // Add Legend to Map
    legend.addTo(map4);

Within the Freelancer CSS is the code to style the legend and make is appear on the map.

.legend {
  line-height: 16px;
  height: auto;
  width: auto;
  color: #333333;
  font-family: 'Open Sans', Helvetica, sans-serif;
  padding: 6px 8px;
  background: white;
  background: rgba(255,255,255,0.5);
  box-shadow: 0 0 15px rgba(0,0,0,0.2);
  border-radius: 5px;
  display: inline-block;
}
.legend i {
  width: 16px;
  height: 16px;
  margin-left: 8px;
  opacity: 0.9;
}
.legend p {
  font-size: 12px;
  line-height: 16px;
  margin: 0;
  text-align: left;
}
.legend img {
  width: 16px;
  height: 16px;
  float: left;
  margin-left: -7px
}

A scale bar is also visible of the map. This can be added by using the following code

//To change which map the scale bar appears on, change the name on the addTo. This would currently display a scal bar on both maps one and two
L.control.scale({position: 'bottomleft'}).addTo(map);
L.control.scale({position: 'bottomleft'}).addTo(map2);

4. Styling the Interface

This web map uses a bootstrap template. The bootstrap template creates the interface therefore there is no real need to change anything. However, for this web map I changed the background colors of each section to reflect the colors of the three main teams. This can be done by going into freelancer.css and changing the color code.
5. Takeaway and Learning

The takeaway of this web map is that there is more than just the big three soccer teams in the Pacific Northwest. The intended audience of this web map is to educate people in the Pacific Northwest who have little to no knowledge about soccer and want to learn about the game and its impact in the Pacific Northwest. While the intended audience is people with little knowledge about soccer, people who already support the big three teams can also learn something. Some people may not know much about the history of team they support. Moreover, I hope it encourages people to explore and support the smaller teams in their area and help the sport continue to grow. Additionally, this web map might also encourage fans to think about playing for a local team and get more involved.

What I leaned in this project is that data about soccer teams is almost impossible to find. Every geojson file was hand created. However, while existing data may be difficult to come by sometimes going out and collecting your own is more beneficial. I know I learned a lot about just how big soccer and how many teams there are in the Pacific Northwest. Furthermore, by creating your own data you can be more selective in what you want to show on your map, eliminating excess data and making it more simple.

The things that I learned on this project will be very valuable to me in my professional or academic career. Things like creating, geojson's and debugging code. Prior to this class and project I had no experience with coding of any kind. I learned that just because the code from one place works does not mean it will work when moving it to another html. Moving forward, I feel confident that I can create more and better web maps than this one in the future. I feel that I can analyze code and find solutions to fix problems and errors. This has been a tremendous learning experience for me.

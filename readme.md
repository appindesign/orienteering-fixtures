# Unified Schedule of Fixtures for an Association(s) within the British Orienteering Federation
Shared here is the code used by the Northern Ireland Orienteering Association to create a unified schedule of events for all the clubs in the association. The result of the code may be seen [here](https://www.niorienteering.org.uk/fixtures/). The fixture list uses the British Orienteering Federation database as its source - specifically the event date, venue, event specific website and club fields.

The code could be modified (without any coding experience) by other associations to create a unified schedule of their fixtures (or indeed all BOF fixtures or multiple associations). You are welcome to use it. If you wish to customise the code, the documentation in this "readme" signals where customisation may be required by the word *customisation* in italics.

*I am happy to answer questions about the code though I am only an amateur programmer and none of the three languages (CSS, html and JavaScript) and one protocol (JSON) are things I am terribly familiar with so I may not be able to answer your questions. My contact address is Geoffrey Collins (email "webmaster" then "@" then "lvo.org.uk").*

*Credits* Many thanks to Jon Ockenden for unlocking mysteries in the API, Jon Hall at Zygo Consulting for making changes to the BOF API on behalf of BOF. These enabled the schedule. Also to those at BOF who facilitated this - Craig Anthony and Ric Gamble. Finally, to the LVO committee for coming up with the idea and Stephen Gilmore for suggesting recent enhancements.

## Part 1 CSS
*Customisation* The CSS is generic and may be left unchanged. If only affects the styling of the page.

The CSS defines the
- font face
- table format (borders, column header background, alignment and padding, text colour and alignment)
- the background and text colour for each level of event
- the level 1 heading style
- hyperlink style
- the font size to use on widescreen v narrow screen (e.g. desktop v mobile)

## Part 2a HTML
This part constructs a list of links to fixtures other than NIOA fixtures. 

*Customisation* All these links could be of interest to all BOF associations though with NIOA sitting close to the Irish Orieteering Association (IOA), a link to the IOA comes first.

## Part 2b JavaScript to fetch the data
*Customisation* The request url needs customised for the specific association. For example the url for NIOA fixtures is https://www.britishorienteering.org.uk/fixturesjson.php?assoc=NIOA. If you wished, you could include more than one association e.g. https://www.britishorienteering.org.uk/fixturesjson.php?assoc=NIOA,SOA would include the NI and Scottish Orienteering Association fixtures and https://www.britishorienteering.org.uk/fixturesjson.php would include all associations.

To fetch the data from the BOF website
- The request is prepared, 
- the request url is specified (I have commented out a couple of URLs I had been experimenting with) and 
- the script is instructured to interpret the returned data as JSON format.
- the execution instruction is issued.

*Important note*
I use https://www.britishorienteering.org.uk/fixturesjson.php to fetch the data. This is the API I had used in my original iteration of the schedule - but there is a newer feed available. I have not used the newer feed as I was just making a small "tweak" to my first iteration when I learned of the newer API. The newer API is
- https://www.britishorienteering.org.uk/openactivefeed.php
- https://openactive.io/realtime-paged-data-exchange/
- https://data.britishorienteering.org.uk/

## Part 2c JavaScript to prepare and present the fixtures table
*Customisation* The club hyperlinks are the only section of this code needing customised. They require the url of the club and a png or jpg image of the club log (the image files are assumed to be stored in the same folder as the code).

A single function "displayFixtures" prepares and presents the fixtures table. It takes as an input parameter the data returned by part 2b.

First the fixture table header is constructed. The header contains the date, venue, event hyperlink and club hyperlink. The relative width of each column is defined. The widths do not add up to 100% and, in particular, the width for "Club" is set to 1%. My memory of this is that this works in that it allows the club hyperlink (which is an image hyperlink) to take up the space it requires.

Next the JavaScript loops through the data returned by BOF, each loop constructs a row of the fixture table. Each loop consists of preparing data for a row of the fixture table and constructing the row itself. The preparation required is,
- prepare day number, name, month and year of the event from the fixture date (I'm sure there's a more elegant approach than the one I've used).
- prepare the club hyperlinks. These are image hyperlinks and expect a club logo image file to be placed in the same folder as the code.
- prepare the fixture url. This first tests if the event_specific_website field has been returned from BOF (i.e. has the club entered the event-specific website into the BOF database). If so it uses that url. If not it defaults to the BOF page for the fixture, be it an event or an activity.
- prepare the colour banding depending on the fixture level.
- prepare the event link class, depending on major/national level.
At this point all the data is prepared and an html row is constructed and added to the table.

With all the data looped through, the table can be presented.

## Part 3 html for Explanatory Text
The only thing left to do is add some explanatory text. The text explains the colour coding used for different levels of fixtures.



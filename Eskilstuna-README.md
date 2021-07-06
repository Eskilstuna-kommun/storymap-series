
# Eskilstuna-oversiktsplan
Eskilstuna Översiktsplan is created using Esri StoryMaps.

**WARNING:** Never use the editor in the "original" application found on [https://eskilstuna.maps.arcgis.com/](https://eskilstuna.maps.arcgis.com/). Instead, always use the editor in the custom applications found in this project. The "original" applications have more limits and are missing custom functionality which if you save the StoryMap using the "original" application will cause the StoryMap to look strange/broken and with missing HTML elements.

# Esri StoryMaps
Eskilstuna Översiktsplan is created using Esri StoryMaps (https://storymaps.arcgis.com/en/). The data for the StoryMaps are stored at ArcGis. The application consists of two applications, StoryMap Series and StoryMap Cascade.

# Build and deploy to Eskilstunas server
1. Build StoryMap Cascade by running the following command from the cascade folder:
  
   npm i
   npm run build

2. Build StoryMap Series by running the following command from the series folder:

npm i
npm run build

3. Create a new folder for the release and name it to OP-2019-04-25 (but with the actual date)
4. Copy the content from Series deploy folder to your release folder.
5. Createa a new folder in the release folder and name it to storymap-cascade
6. Copy the content from the Cascade deploy folder to the newly created storymap-cascade folder
7. Update oAuthAppId in [RELASE-FOLDER]/index.html and [RELASE-FOLDER]/storymap-cascade/index.html. The oAuthAppId should be WDA7shYMaYNEYxsZ
8. Go to https://ag01.eskilstuna.se/Citrix/EskilstunaWeb and login
9. Login to the remote desktop you will find there
10. Lookup your release folder and copy it
11. On the remote desktop, connect to remote desktop (Anslut till fjärrskrivbord): eka11v52.eskilstuna.se
12. Paste your release folder to D:\Temp\OP
13. Copy your folder from D:\Temp\OP and paste it to D:\inetpub\wwwroot
14. Open "Internet Information Services (IIS) Manager"
15. Select till Eskilstuna ÖP site (currently named OP2).
16. Select "Basic settings" in the right panel
17. Change "Physical path" to your new release folder
18. Restart website (right panel)
19. Open Chrome (still on the the remote desktop) and go to https://storymaps.eskilstuna.se and make sure it works

# Original source code
This project consists of modified versions of Esri StoryMap Cascade and Esri StoryMap Series.

## Esri StoryMap Cascade
Original source: https://github.com/Esri/storymap-cascade/tree/V1.10.0

## Esri StoryMap Series
Original source: https://github.com/Esri/storymap-series/tree/V1.14.0

# ArcGIS API
The applications use ArcGIS API for Javascript 3.27 for the web maps. Documentation of the API is found at https://developers.arcgis.com/javascript/3/.

# Customizations
The orginal StoryMaps have been customized for Eskilstuna Översiktsplan.

## Esri StoryMap Cascade
Custom styling and javascripts have been added. `index.html` has also been modified.

### CSS
Custom CSS has been added to `Eskilstuna.less`.

### Mobile menu
A mobile menu has been added be able to navigate between Cascade apps belonging to the same Series app.

### Javascript
Custom javascript has been added to `custom-scripts.js`, and the original React source code has been modified as well. 
`custom-scripts.js` includes code for:
- handling internal bookmarks
- handling links to other Series tabs (with or without bookmark)
- print handler (for redirecting to print version when using the browsers print function)
- make illustrations (next to text blocks) "sticky"
- toggling accordions

#### Text to html
It's not possible to write HTML in the original StoryMap Cascade app. To fix this the text is parsed and outputted as HTML when the story loads.

#### Accordions
A custom accordions element has been added to make it possible to toggle text. An accordion should be written like this in the builder:

	<div class="accordion">En rubrik<div class="accordion-content">Mer text som inte visas från början.</div></div>

#### Text with an image (illustration) to the right
To be able to have a text block to left and an image (illustration) to the right of it we have created custom CSS classes.


#### Internal links
Custom javascript code (orginal source: https://medium.com/story-maps-developers-corner/adding-links-to-sections-in-cascade-930a091365c0) has been added to allow for internal links. You can now link to a section in the same Cascade StoryMap by creating a link to:

	inline-bookmark-<bookmarkName>

where bookmarkName is the exact name as you have it setup in your story settings. You can also write:
	
	inlink-index-4

to link to section number 4 (index starts at 0).

#### Links to other Cascade StoryMap in a Series
Custom javascript code has been added to enabled linking between multiple Cascade StoryMaps when embedded in a Series StoryMap. This is done by replacing the normal links with posting messages from the iframe (Cascade) to the Parent (series). The link should just point to the Cascade URL:
     
     [URL to the Cascade StoryMap]/index.html?appid=[Id of the Cascade app]


### SeriesId
The configuration in `index.html` now includes `seriesId` which is the id of the StoryMap series the Cascade StoryMap "belongs to". When the Cascade StoryMap is initialised, the data for the Series StoryMap is also requested, in order to create the mobile menu and the links to the other Series pages/tabs in the Cascade footer.

### WebMap
Some modifications has been done to the webmap (WebMap.jsx):
- legend has been added
- search input field has been added
- max and min scale has been set

### Editor
Some modifications has been done to the editor for removing limits, such as max number of characters for different input fields, allowing relative paths to images, allowing HTML tags in different input fields.

**WARNING:** Never use the editor in the "original" application found on [https://eskilstuna.maps.arcgis.com/](https://eskilstuna.maps.arcgis.com/). Instead, always use the editor in the custom applications found in this project. The "original" applications have more limits and are missing custom functionality which if you save the StoryMap using the "original" application will cause the StoryMap to look strange/broken and with missing HTML elements.

### grunt
The Grunt task of building the deployment has been updated to use [https://github.com/nDmitry/grunt-postcss](https://github.com/nDmitry/grunt-postcss) and AutoPrefixer to make the CSS compatible with older devices.

## Esri StoryMap Series
Custom styling and javascript has been added and some HTML code has been modified/added/removed.

### CSS
Custom CSS has been added to `index.html`.

### JavaScript
Custom javascript has been added to `custom-scripts.js` for handling:
- links from one Cascade page to another using bookmarks (anchor links)

#### Redirect mobile and tablet users
The original source code has been modified to redirect mobile and tablet users to the first Cascade tab since iOS devices can't handle embedded Cascade apps in a Series app.

#### Links to other StoryMap in a series
Custom javascript code has been added to enabled linking between multiple Cascade StoryMaps when embedded in a Series StoryMap. This is done by replacing the normal links in the Cascade StoryMap with posting messages from the iframe (cascade) to the parent (series).
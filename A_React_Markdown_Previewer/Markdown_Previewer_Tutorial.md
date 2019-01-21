## Prepare
```
npm install marked --save
```

## Step1 - Create-React-App
Follow this tutorial to create a react app from facebook.
<br/>
<br/>
<br/>


## Step 2 — Create a Markdown React Container Component
There are many ways to implement Markdown feature in React.<br/>

The one we choose here is [marked.js](https://www.npmjs.com/package/marked). Because this package has most weekly downloads. <br/>
You can install this package by:  `npm install marked --save` <br/>
You can find more documents about how to use marked.js [here](https://marked.js.org/#/README.md#README.md) <br/>
Or google "use marked.js with React" <br/>

Create a new Markdown.jsx file, and write code as below to try out marked.js first. <br/>
``` javascript
import React from 'react'
import marked,{ Renderer } from 'marked'

class Markdown extends React.Component {
  constructor(props){
    super(props)
    const text ="# Header1"
    this.state = {
      markdown: text
    }
  }
  rawMarkup(markdown) {
  	var rawMarkup = marked(markdown, {sanitize: true})
  	return {
  		__html: rawMarkup
  	}
  }
  render() {
    const markdown = this.state.markdown
    return(
      <div className="MarkdownPreviewWrapper">
        <div className="MarkdownPreview">
          <span dangerouslySetInnerHTML={this.rawMarkup(markdown)} />
        </div>
      </div>
    )
  }
}
export default Markdown
```
You should be able see Header1 is processed as "Header1" by marked.js
<img src="https://i.ibb.co/09GZw1f/MD-Step2.png" width="50%">
<br/>
<br/>
<br/>





## Step 3 — TextArea on the left while Previewer on the right
Since we want to show a TextArea on the left, and have a live Previewer on the right.  <br/>
Let's use bootstrap to separate component Left as 'TextArea' and component Right as 'Previewer' <br/>

We add a simple `handleChange(e) {}` function here. <br/>
Whenever there is a change in TextArea input, we sync the input to this.state.markdown <br/>
As right Previewer uses this.state.markdown as function input, so we always have the same and synced data.


``` javascript
import React from 'react'
import marked,{ Renderer } from 'marked'

class MarkdownTut extends React.Component {
  constructor(props){
    super(props)
    const text ="# Header1"
    this.state = {
      markdown: text
    }
  }
  rawMarkup(markdown) {
  	var rawMarkup = marked(markdown, {sanitize: true})
  	return {
  		__html: rawMarkup
  	}
  }
  handleChange(e) {
    this.setState({
      markdown: e.target.value
    })
  }
  render() {
    const markdown = this.state.markdown
    return(
      <div className="MarkdownPreviewWrapper row">
        <div className="col-xs-12 col-sm-6 MarkdownTextInput">
          <textarea id="editor"
                    className="editor"
                    value={markdown}
                    onChange={this.handleChange.bind(this)} />
        </div>
        <div className="col-xs-12 col-sm-6 MarkdownPreview">
          <span dangerouslySetInnerHTML={this.rawMarkup(markdown)} />
        </div>
      </div>
    )
  }
}
export default MarkdownTut
```
Run your code and should have layout like:
<img src="https://i.ibb.co/r5053XW/MD-Step3.png" width="100%">
<br />
<br />
<br />



## Step 4 — Add Css file and make TextArea look good.
Add a new style.css file next to Markdown.jsx file. <br/>
Add below css code to style TextArea a little bit.
``` css
.MarkdownPreviewWrapper .MarkdownTextInput .editor{
  /* background-color: yellow; */
    padding: 10px 5px;
  	resize: vertical;
  	overflow: auto;
  	width: 100%;
  	height: 564px;
  	margin: 0 auto;
  	display: block;
  	margin-bottom: 15px;
}
```
Then import this style.css file to Markdown.jsx <br/>
Simply add this line into Markdown.jsx <br/>
`import './style.css'`

Top of Markdown.jsx should be like:
``` javascript
import React from 'react'
import marked,{ Renderer } from 'marked'

import './style.css'

class MarkdownTut extends React.Component {
  ...
}
export default MarkdownTut
```

<img src="https://i.ibb.co/Hq770Zm/MD-Step4.png" width="100%">
<br />
<br />
<br />


## Step 5 — import InitialMarkdown from other file
To mimic real case, InitialMarkdown markdown is called from backend API. <br/>
For now, we just import InitialMarkdown markdown from local file.<br/>

Create a initialMarkdown.js file, and write code inside:
``` javascript
export const text =
`
\`\`\`
function addTwoNumbers(a, b) {
  return a + b
}
const name = "Ben"
const age = 27
cosnt number = Math.random() * 10
\`\`\`
`
```
Then import this initialMarkdown.js file to Markdown.jsx <br/>
Simply add this line into Markdown.jsx <br/>
`import { text } from './initialMarkdown.js'`

Top of Markdown.jsx should be like:
``` javascript
import React from 'react'
import marked,{ Renderer } from 'marked'

import './style.css'
import { text } from './initialMarkdown.js'

class MarkdownTut extends React.Component {
  ...
}
export default MarkdownTut
```
<img src="https://i.ibb.co/5G8X7LR/MD-Step5.png" width="100%">
<br />
<br />
<br />


## Step 6 — Change Icons
``` python
#Import Library
import folium
from folium.plugins import MarkerCluster
import pandas as pd

#Load Data
data = pd.read_csv("Volcanoes_USA.txt")
lat = data['LAT']
lon = data['LON']
elevation = data['ELEV']

#Function to change colors
def color_change(elev):
    if(elev < 1000):
        return('green')
    elif(1000 <= elev <3000):
        return('orange')
    else:
        return('red')

#Create base map
map = folium.Map(location=[37.296933,-121.9574983], zoom_start = 5, tiles = "Mapbox bright")

#Plot Markers
for lat, lon, elevation in zip(lat, lon, elevation):
    folium.CircleMarker(location=[lat, lon], radius = 9, popup=str(elevation)+" m", fill_color=color_change(elevation), color="gray", fill_opacity = 0.9).add_to(map)

#Save the map
map.save("map1.html")
```
<img src="https://i.ibb.co/F8DNYmq/Step6.png" width="100%">
<br />
<br />



## Step 7 — Cluster all Markers
``` python
#Import Library
import folium
from folium.plugins import MarkerCluster
import pandas as pd

#Load Data
data = pd.read_csv("Volcanoes_USA.txt")
lat = data['LAT']
lon = data['LON']
elevation = data['ELEV']

#Function to change colors
def color_change(elev):
    if(elev < 1000):
        return('green')
    elif(1000 <= elev <3000):
        return('orange')
    else:
        return('red')

#Create base map
map = folium.Map(location=[37.296933,-121.9574983], zoom_start = 5, tiles = "CartoDB dark_matter")

#Create Cluster
marker_cluster = MarkerCluster().add_to(map)

#Plot Markers and add to 'marker_cluster'
for lat, lon, elevation in zip(lat, lon, elevation):
    folium.CircleMarker(location=[lat, lon], radius = 9, popup=str(elevation)+" m", fill_color=color_change(elevation), color="gray", fill_opacity = 0.9).add_to(marker_cluster)

#Save the map
map.save("map1.html")
```
<img src="https://i.ibb.co/t2ddMv8/Step7.png" width="100%">

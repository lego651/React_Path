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


## Step 5 — import InitialMarkdown from local file
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


## Step 6 — Customzie code snippet highlight syntax
We want to customize marked.js a little bit, especially for its code highlights. <br/>
This can be done by using [highlight.js](https://highlightjs.org/) <br/>
We need to install highlight.js package first:  `npm install --save highlight.js` <br/>
Firstly, update Markdown.jsx as below:
``` javascript
import React from 'react'
import marked,{ Renderer } from 'marked'
import hljs from 'highlight.js';

import './style.css'
import { text } from './initialMarkdown.js'

class MarkdownTut extends React.Component {
  constructor(props){
    super(props)
    // const text ="# Header1"
    this.state = {
      markdown: text
    }
  }
  rawMarkup(markdown) {
  	marked.setOptions({
  		renderer: new marked.Renderer(),
  		gfm: true,
  		tables: true,
  		breaks: true,
  		pedantic: false,
  		sanitize: true,
  		smartLists: true,
  		smartypants: false,
  		highlight: function (code) {
  			return hljs.highlightAuto(code).value
  		}
  	})

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

Secondly, to use highlight.js, we need to add a few lines into index.html
``` html
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/8.4/styles/default.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/8.4/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

Thirdly, Add below css code to style.css
``` css
/*
Atom One Dark by Daniel Gamage
Original One Dark Syntax theme from https://github.com/atom/one-dark-syntax
base:    #282c34
mono-1:  #abb2bf
mono-2:  #818896
mono-3:  #5c6370
hue-1:   #56b6c2
hue-2:   #61aeee
hue-3:   #c678dd
hue-4:   #98c379
hue-5:   #e06c75
hue-5-2: #be5046
hue-6:   #d19a66
hue-6-2: #e6c07b
*/


.hljs {
  display: block;
  overflow-x: auto;
  padding: 0.5em;
  color: #abb2bf;
  background: #282c34;
  /* background:  #3a3a3a; */
}

.hljs-comment,
.hljs-quote {
  color: #5c6370;
  font-style: italic;
}

.hljs-doctag,
.hljs-keyword,
.hljs-formula {
  color: #c678dd;
  /* color: red; */
}

.hljs-section,
.hljs-name,
.hljs-selector-tag,
.hljs-deletion,
.hljs-subst {
  color: #e06c75;
}

.hljs-literal {
  color: #56b6c2;
}

.hljs-string,
.hljs-regexp,
.hljs-addition,
.hljs-attribute,
.hljs-meta-string {
  color: #98c379;
}

.hljs-built_in,
.hljs-class .hljs-title {
  color: #e6c07b;
}

.hljs-attr,
.hljs-variable,
.hljs-template-variable,
.hljs-type,
.hljs-selector-class,
.hljs-selector-attr,
.hljs-selector-pseudo,
.hljs-number {
  color: #d19a66;
}

.hljs-symbol,
.hljs-bullet,
.hljs-link,
.hljs-meta,
.hljs-selector-id,
.hljs-title {
  color: #61aeee;
}

.hljs-emphasis {
  font-style: italic;
}

.hljs-strong {
  font-weight: bold;
}

.hljs-link {
  text-decoration: underline;
}

/* code {
  background-color: #ffe5e5;
  color: #ff3232;
  padding: .25vw .5vw;
  border-radius: 5px;
} */

pre {
  display: inline-block;
  background-color: #3a3a3a;
  color: white;
  padding: .25vw .5vw;
}
code {
  background-color: #3a3a3a !important;
}
```
<img src="https://i.ibb.co/wCQyWGN/MD-Step6.png" width="100%">

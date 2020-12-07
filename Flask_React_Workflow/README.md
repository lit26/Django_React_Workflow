# Flask_React_Workflow

This is a full stack web develop setup which use Flask for the backend, React for the frontend and integrate React into Flask using Webpack & Babel


## Config the frontend app, direct to the `frontend` folder

```
frontend folder structure
- static
	- dist
- templates
	- index.html
- src
	- components
	- App.js
	- index.js
```
Using terminal (shortcut)
```bash
mkdir static
mkdir templates
mkdir src
mkdir src/components
touch babel.config.js
touch webpack.config.js
touch templates/index.html
touch urls.py
touch src/App.js
touch src/index.js
touch app.py
```

Install npm packages
```bash
npm init -y
npm i webpack webpack-cli --save-dev
npm i @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev
npm i file-loader style-loader css-loader --save-dev
npm i react react-dom --save-dev
npm install @babel/plugin-proposal-class-properties
npm install react-router-dom 
```

### Config some settings

In `package.json`
```jsx
  "scripts": {
    "dev":"webpack --mode development --watch",
    "build": "webpack --mode production"
  },
```

In `babel.config.js`
```jsx
module.exports = {
    presets: ["@babel/preset-env", "@babel/preset-react"],
};
```

In `webpack.config.js`
```jsx
module.exports = {
    entry: {
        main: "./src/index.js",
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: "babel-loader",
            },
            {
                test: /\.(svg|png|jpg|jpeg|gif)$/,
                loader: "file-loader",

                options: {
                    name: "[name].[ext]",
                    outputPath: "../../static/dist",
                },
            },
            {
                test: /\.css$/i,
                use: ["style-loader", "css-loader"],
            },
        ],
    },
    output: {
        path: __dirname + "/static/dist",
        filename: "[name].bundle.js",
    },
};
```

### Config urls and views
In `templates/frontend/index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <title>React-Flask Integration</title>

        <meta charset="utf-8" />
        <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
    </head>

    <body>
        <div id="root">
        </div>
        <script
            type="text/javascript"
            src="{{ url_for('static', filename='dist/main.bundle.js') }}"
        ></script>
    </body>
</html>
```
In `src/index.js`
```
import React from "react";
import { render } from "react-dom";
import App from './App';

render(
    <App />,
  document.getElementById('root')
);
```

In `src/App.js`
```jsx
import React from 'react'

function App() {
    return (
        <div>
            <h1>Hello Frontend.</h1>
        </div>
    )
}

export default App
```

## Set the urls in Flask
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(debug=True)
```

## Develop the project
For the frontend
```bash
npm run dev
```
For the Flask
```bash
python3 app.py
```



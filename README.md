# Django_React_Workflow

This is a full stack web develop setup which use Django for the backend and React for the frontend

## Set up the projects

Set up the django project, frontend app and backend app. 
```
django-admin startproject Django_React_Workflow
cd Django_React_Workflow
django-admin startapp backend
django-admin startapp frontend
```

In `Django_React_Workflow/settings.py`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'backend.apps.BackendConfig',
    'frontend.apps.FrontendConfig'
]
```

Setting urls, in `Django_React_Workflow/urls.py`
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('backend/', include('backend.urls')),
    path('', include('frontend.urls'))
]
```

## Config the backend app, direct to the `backend` folder

In `views.py`
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def main_view(request):
    return HttpResponse('<h1>Hello Backend.</h1>')
```

In `urls.py`
```python
from django.urls import path
from .views import main_view

urlpatterns = [
    path('', main_view)
]
```

## Config the frontend app, direct to the `frontend` folder

```
frontend
- static
	- frontend
	- css
- templates
  	- frontend
- src
	- components
	- App.js
	- index.js
```
Using terminal
```bash
mkdir static
mkdir templates
mkdir src
mkdir static/css
mkdir static/frontend
mkdir templates/frontend
mkdir src/components
touch babel.config.json
touch webpack.config.js
touch templates/frontend/index.html
touch urls.py
touch src/App.js
touch src/index.js
```

Install npm packages
```bash
npm init -y
npm i webpack webpack-cli --save-dev
npm i @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev
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

In `babel.config.json`
```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "node": "10"
        }
      }
    ],
    "@babel/preset-react"
  ],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

In `webpack.config.js`
```jsx
const path = require("path");
const webpack = require("webpack");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "./static/frontend"),
    filename: "[name].js",
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },
  optimization: {
    minimize: true,
  },
  plugins: [
    new webpack.DefinePlugin({
      "process.env": {
        // This has effect on the react lib size
        NODE_ENV: JSON.stringify("production"),
      },
    }),
  ],
};
```

### Config urls and views
In `templates/frontend/index.html`
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    {% load static %}
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <link rel="stylesheet" type="text/css" href="{% static "css/index.css" %}">
    <title>Front End</title>
  </head>
  <body>
    <div id="root"></div>
    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.

      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.

      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
    <script src="{% static "frontend/main.js" %}"></script>
  </body>
</html>
```
In `urls.py`
```python
from django.urls import path
from .views import index

urlpatterns = [
    path('', index)
]
```
In `views.py`
```python
from django.shortcuts import render

# Create your views here.
def index(request, *args, **kwargs):
    return render(request, 'frontend/index.html')
```
In `src/index.js`, 
```jsx
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

## Run
To run the React
```bash
npm run dev
```
To run the Django backend
```bash
python3 manage.py makemigration
python3 manage.py migrate
python3 manage.py runserver
```



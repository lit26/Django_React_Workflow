# Django_Typescript_React_Workflow

This is a full stack web develop setup which use Django for the backend, Typescript for the frontend and integrate Typescript into Django using Webpack & Babel

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
frontend folder structure
- static
	- frontend
	- css
- templates
  	- frontend
		- index.html
- src
	- components
	- App.tsx
	- index.tsx
```
Using terminal (shortcut)
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
touch tsconfig.json
```

Install npm packages
```bash
npm init -y
```

### Config some settings

In `package.json`
```jsx
{
  "name": "frontend",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.config.js",
  "scripts": {
    "dev":"webpack --mode development --watch",
    "build": "webpack --mode production",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@testing-library/jest-dom": "^5.14.1",
    "@testing-library/react": "^12.0.0",
    "@testing-library/user-event": "^13.2.1",
    "@types/jest": "^27.0.1",
    "@types/node": "^16.7.13",
    "@types/react": "^17.0.20",
    "@types/react-dom": "^17.0.9",
    "axios": "^0.24.0",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "5.0.0",
    "typescript": "^4.4.2",
    "web-vitals": "^2.1.0"
  },
  "devDependencies": {
    "webpack": "^5.68.0",
    "webpack-cli": "^4.9.2",
    "@babel/core": "^7.13.8",
    "@babel/preset-env": "^7.13.9",
    "@babel/preset-react": "^7.12.13",
    "@babel/preset-typescript": "^7.13.0",
    "babel-loader": "^8.2.2",
    "css-loader": "^5.1.1",
    "css-modules-typescript-loader": "^4.0.1",
    "file-loader": "^6.2.0",
    "html-webpack-plugin": "^5.2.0",
    "path": "^0.12.7",
    "style-loader": "^2.0.0",
    "ts-loader": "^8.0.17",
    "webpack-bundle-tracker": "^1.4.0",
    "webpack-dev-server": "^3.11.2"
  }
}

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
const BundleTracker = require("webpack-bundle-tracker");

module.exports = {
  entry: "./src/index.tsx",
  output: {
    path: path.resolve(__dirname, "./static/frontend"),
    filename: "[name].js",
  },
  mode: process.env.NODE_ENV || "development",
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  devServer: { contentBase: path.join(__dirname, "src") },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ["babel-loader"],
      },
      {
        test: /\.(ts|tsx)$/,
        exclude: /node_modules/,
        use: ["ts-loader"],
      },
      {
        test: /\.(css|scss)$/,
        use: ["style-loader", "css-loader", "css-modules-typescript-loader"],
      },
      {
        test: /\.(jpg|jpeg|png|gif|mp3|svg)$/,
        use: ["file-loader"],
      },
    ],
  },
  plugins: [new BundleTracker({ filename: "./webpack-stats.json" })],
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
In `src/index.tsx`, 
```jsx
import React from "react";
import { render } from "react-dom";
import App from './App';

render(
    <App />,
  document.getElementById('root')
);
```
In `src/App.tsx`
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

## Develop the project
For the frontend
```bash
npm run dev
```
For the Django backend
```bash
python3 manage.py makemigration
python3 manage.py migrate
python3 manage.py runserver
```



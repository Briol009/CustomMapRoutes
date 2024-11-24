# Personalized Custom Routing Creation Tool

By Laure Briol with the assistance of Generative AI

## Project Overview

My final project for Advanced Geocomputing (GEOG 5543) aimed to create a personalized routing service. This routing service enabled a user to visit all locations, while being able to avoid designated areas on a map and while minimizing their travel time. The project's initial scope focused on how to improve a shopping trip/experience. Using the tool, a user can go to multiple store locations, without backtracking on their drive. A user can choose multiple store location's and then be given the optimal route to reach each destination. There are some areas a user may want to avoid and the personal route creation tool takes these preferences into account, in order to improve the overall shopping experience. Therefore, the route creation tool implemented a feature, which allowed for a user to draw polygon areas that the routing service will avoid, when planning the route.

## Implementation Approach

It was a goal to try out alternative services other than the expensive Google API’s, when implementing this project. To do this, there is a website called https://openrouteservice.org/, which has an open source API that is able to provide routing services for free. An added benefit of this service is that a user can state which locations they want to avoid and the API will automatically avoid them when creating the best route.

My goals for this project were as follows:

* Open an interactive map
ipyleaflet was used to create a map on the bottom of the screen
Be able to add points of my home location and places a user wants to visit/shop at
    * A ‘click’ function was added to the map, allowing a user to create marker points
* Be able to add polygons of areas a user wants to avoid on the map
    * A ‘draw’ function was added to the the map, allowing a user to create polygons of areas they want to avoid on the map
* Be able to save and load locations & areas that are used regularly
    * A function to save the current map’s points and polygons to a file was added. Additionally, the ability to load a file to add in points and polygons to an existing map
* Be able to optionally route back to the original starting point
    * A checkbox was added, which tells the routing optimizer to go back to the original location
* Automatically find the best route to visit all locations (and optionally return home)
    * This is what is called the [Travelling Salesman Problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem). [More Info here](https://developers.google.com/optimization/routing/tsp). To be able to find a route between all the points, the program needs to know the time it takes to travel from each individual point to every single other point. This is called a time matrix. With that information, the program can create an optimal route for the day of travel!
* Display directions
    * A box to print out the directions for each leg of the optimal route was added. A display of the total travel time and total distance traveled was also included.

## Project Setup

### API Key
This project is run entirely in just one Jupyter Notebook file. A user only needs to do one thing to prep the file, which is get a free API key from openrouteservice. Go to `https://openrouteservice.org/dev/#/login` and then click ‘sign up’. Fill in an email, name, and password prompts. Once the email is verified, the user can log in and that should lead to the Dev dashboard `https://openrouteservice.org/dev/#/home`. If it doesn’t already give a free API key, the user can “Request a token” that is “Standard” and give it a name.

The openrouteservice website then gives a free API key that the user can copy and paste into the top of the second code cell in the Jupyter Notebook.

### Required Libraries

This project is set up to create one single interactive map through a large Jupyter code chunk. First, there are the Import statements. These are needed packages for the project:
* ipywidgets -- This is used for creating different interactive ‘widgets’ and buttons at the bottom of the code cell.
* ipyleaflet -- This is used to create maps with various features at the bottom of the code cell.
* IPython.display -- This is used to make it so the bottom of the code cell clears when new information is placed, rather than creating a very long print statement that is unmanageable and unreadable.
* openrouteservice -- This is the python package for the openrouteservice API. This allows the program to make API calls very easily, without having to create a full API website/URL link for getting data.
* time -- This is used to pause the code so it does not run too fast and over-load the API and hit rate limits.
* ortools -- (Operations Research Tools) This is a Google-made package designed to perform optimization. This is used to find the best route given the time it takes to travel between all the locations.
* shapely -- This is used to handle GIS shapes and geometries.
* json -- This is used to save map data to a file and re-open map data later on.

## Code Sections

The project is an interactive map, meaning it cannot split into multiple pieces. Instead, the code is organized into these sections:

### Section 1: Global Variables:

* This section contains all the code that is repetitively called in the program.
* This includes setting up the API key, colors for the map markers, lists to store the locations of the stops, and setting up an empty default map. 

### Section 2: Avoid area polygons

* This section contains code designed for prepping the 'avoiding areas polygon' feature.
* This section adds in the map control button to draw a polygon. When the polygon is drawn, the polygon is added to a list of locations and is displayed on the map. 

### Section 3: Adding locations

* This section contains code designed for adding new locations onto the map.
* This section adds in the button to enable adding locations and also adds the button to turn on ‘add new locations’.
* Also included in this section is the checkbox option to ‘return to start’.

### Section 4: Saving/loading map data

* This section contains code designed for saving data from the map and then loading said data onto the map.
* This section adds the buttons and text box to save data and load data. Additionally, this section implements the feature to save the data to a json file when the save button is clicked.

### Section 5: Optimizing route

* This is the largest section of the code. This section creates an optimized route for the person to follow.
* This section is run when the user clicks the ‘run optimization’ button.
* The function starts by running a function called get_time_matrix_with_avoid to create a ‘time matrix’, calculating the time it takes to go between all point pair permutations.
* OR tools is used to optimize the route between the two locations. This required the data to be re-adjusted to its own special format, which ChatGPT shared how to do. Then, ‘parameters’/settings were enabled for doing the optimization algorithms.
* Once the OR tools finds the best route between all the locations, the program then updates the colors of markers to better indicate the order in which a person is traveling.
* The openrouteservice API is also called to give the proper step by step directions for each ‘leg’ of the journey between two points.
    * The program keeps track of the total amount of time traveled, based on the step by step travel times and travel distances.
    * The API also shares the polyline for each leg, so that is displayed on the map as well.
* The program finally prints out the step by step directions and total travel times at the bottom of the screen.

### Output

The output is a final interactive map that can be used to create a custom route. There are buttons and a text input box at the top for saving & loading existing map json features. Below that, there is a button to toggle, which enables the user to add new points to the map and a checkbox to enable a user to return to the start point. The map itself is then displayed and has the option to draw a polygon area for avoiding features (when the 'Add Locations' is toggled to off). An 'Optimize Routes' button is displayed at the bottom of the screen. When the optimize routes button is clicked, the program will find the best route and display it on the map, as well as print step-by-step directions below the map display.


## Run this project

Open the Binder link below, wait a minute or two, and it will launch an interactive Jupyter Notebook containing my code. Input your own OpenRouteService API Key, then run all the cells (click Run at the top--> Run All Cells) and the program will run an interactive map application at the bottom!

https://mybinder.org/v2/gh/briollaure/CustomMapRoutes/main?labpath=CustomRouting.ipynb

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/briollaure/CustomMapRoutes/main?labpath=CustomRouting.ipynb)

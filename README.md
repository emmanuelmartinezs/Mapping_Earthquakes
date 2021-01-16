# Mapping Earthquakes 
## Overview of Project
> Roza has a partially completed dashboard that she needs to finish. She has a completed panel for demographic information and now needs to visualize the bacterial data for each volunteer. Specifically, her volunteers should be able to identify the top 10 bacterial species in their belly buttons. That way, if Improbable Beef identifies a species as a candidate to manufacture synthetic beef, Roza's volunteers will be able to identify whether that species is found in their navel.

1. ***Deliverable 1***: Create a Horizontal Bar Chart
2. ***Deliverable 2***: Create a Bubble Chart
3. ***Deliverable 3***: Create a Gauge Chart
4. ***Deliverable 4***: Customize the Dashboard
5. ***Deliverable 5***: A written report on the Mapping Earthquakes analysis [`README.md`](https://github.com/emmanuelmartinezs/Mapping_Earthquakes/). 

## Resources and Before Start Notes:

* Data Source: `BellyButton_bar_chart_starter_code.js`, `BellyButton_bubble_chart_starter_code.js`, `BellyButton_bubble_chart_starter_code.js` and `index.html`
* Data Tools: JavaScript, JSON, GeoJSON and IO (Web Server)
* Software: ES6+, ECMAScript and Visual Studio Code 1.50.0

For more information, visit [`The Mapbox API`](https://www.mapbox.com/pricing/?utm_medium=blog&utm_source=mapbox-blog&utm_campaign=blog%7Cmapbox-blog%7Cpricing%7Cnew-pricing-46b7c26166e7-19-05&utm_term=pricing&utm_content=new-pricing-46b7c26166e7/). 


## Tech-Overview
In this module, you will use the Leaflet.js Application Programming Interface (API) to populate a geographical map with GeoJSON earthquake data from a URL. Each earthquake will be visually represented by a circle and color, where a higher magnitude will have a larger diameter and will be darker in color. In addition, each earthquake will have a popup marker that, when clicked, will show the magnitude of the earthquake and the location of the earthquake.

## Basic Project Plan

**Purpose**
The purpose of this project is to visually show the differences between the magnitudes of earthquakes all over the world for the last seven days.

**Tasks**
To complete this project, use a URL for GeoJSON earthquake data from the USGS website and retrieve geographical coordinates and the magnitudes of earthquakes for the last seven days. Then add the data to a map.

**Approach**
Your approach will be to use the JavaScript and the D3.js library to retrieve the coordinates and magnitudes of the earthquakes from the GeoJSON data. You'll use the Leaflet library to plot the data on a Mapbox map through an API request and create interactivity for the earthquake data.

Now that you have an overview of the project plan, let's set up a Mapbox account and get the API token you'll need to create geographical maps.

# The Leaflet JavaScript Library
On the [`Leaflet`](https://leafletjs.com/index.html) homepage, scroll midpage and click the [`Leaflet Quick Start Guide`](https://leafletjs.com/examples/quick-start/) link:

![name-of-you-image](https://github.com/emmanuelmartinezs/Mapping_Earthquakes/blob/main/Resources/Images/s1.png?raw=true)

The Leaflet Quick Start Guide provides steps for setting up a Leaflet map. To begin, scroll midpage to the "Preparing your page" section:

![name-of-you-image](https://github.com/emmanuelmartinezs/Mapping_Earthquakes/blob/main/Resources/Images/s2.png?raw=true)

The "Preparing your page" section includes links and HTML code that we'll add to our [`Mapping Earthquakes - index.html`](https://emmanuelmartinezs.github.io/Mapping_Earthquakes/) page. 


## Use the Leaflet Documentation
The [`Leaflet Quick Start Guide`](https://leafletjs.com/examples/quick-start/) provides the `tileLayer()` code:

![name-of-you-image](https://github.com/emmanuelmartinezs/Mapping_Earthquakes/blob/main/Resources/Images/s3.png?raw=true)


We can copy this tile layer code and assign it to the streets variable, since the tile layer will create a street-level map. Add the following code block to your `logic.js` file:

````java
// We create the tile layer that will be the background of our map.
let streets = L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
    maxZoom: 18,
    id: 'mapbox.streets',
    accessToken: API_KEY
});
// Then we add our 'graymap' tile layer to the map.
streets.addTo(map);
````

> Let's move on!

# Deliverable 1:  
## Create a Horizontal Bar Chart
### Deliverable Requirements:
Using your knowledge of JavaScript, Plotly, and D3.js, create a horizontal bar chart to display the top 10 bacterial species (OTUs) when an individual’s ID is selected from the dropdown menu on the webpage. The horizontal bar chart will display the `sample_values` as the values, the `otu_ids` as the labels, and the `otu_labels` as the hover text for the bars on the chart.

Your bar chart should look like the following image:

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/s2.png?raw=true)



1. Code is written to create the arrays when a sample is selected from the dropdown menu.
2. Code is written to create the trace object in the `buildCharts()` function, and it contains the following:
    - The y values are the `otu_ids` in descending order.
    - The x values are the `sample_values` in descending order
    - The hover text is the `otu_labels` in descending order.
3. Code is written to create the layout array in the `buildCharts()` function that creates a title for the chart.
4. When the dashboard is first opened in a browser, **ID 940**’s data should be displayed in the dashboard, and the bar chart has the following:
    - The top 10 `sample_values` are sorted in descending order
    - The top 10 `sample_values` as values
    - The `otu_ids` as the labels

 
### Results with detail analysis:

1. **Code is written to create the arrays when a sample is selected from the dropdown menu.**


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
// COLUMBIA ENGINEERING 
// By Emmanuel Martinez
// Module 12


// DELIVERABLE 1: Create a Horizontal Bar Chart

function init() {
  // Grab a reference to the dropdown select element
  var selector = d3.select("#selDataset");

  // Use the list of sample names to populate the select options
  d3.json("JS/data/samples.json").then((data) => {
    var sampleNames = data.names;

    sampleNames.forEach((sample) => {
      selector
        .append("option")
        .text(sample)
        .property("value", sample);
    });

    // Use the first sample from the list to build the initial plots
    var firstSample = sampleNames[0];
    buildCharts(firstSample);
    buildMetadata(firstSample);
  });
}

// Initialize the dashboard
init();

function optionChanged(newSample) {
  // Fetch new data each time a new sample is selected
  buildMetadata(newSample);
  buildCharts(newSample);
  
}

// Demographics Panel 
function buildMetadata(sample) {
  d3.json("JS/data/samples.json").then((data) => {
    var metadata = data.metadata;
    // Filter the data for the object with the desired sample number
    var resultArray = metadata.filter(sampleObj => sampleObj.id == sample);
    var result = resultArray[0];
    // Use d3 to select the panel with id of `#sample-metadata`
    var PANEL = d3.select("#sample-metadata");

    // Use `.html("") to clear any existing metadata
    PANEL.html("");

    // Use `Object.entries` to add each key and value pair to the panel
    // Hint: Inside the loop, you will need to use d3 to append new
    // tags for each key-value in the metadata.
    Object.entries(result).forEach(([key, value]) => {
      PANEL.append("h6").text(`${key.toUpperCase()}: ${value}`);
    });

  });
}

````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/1.1.JPG?raw=true)



2. **Code is written to create the trace object in the `buildCharts()` function, and it contains the following:**
    - The y values are the `otu_ids` in descending order.
    - The x values are the `sample_values` in descending order
    - The hover text is the `otu_labels` in descending order.


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
// 1. Create the buildCharts function.
function buildCharts(sample) {
  // 2. Use d3.json to load and retrieve the samples.json file 
  d3.json("JS/data/samples.json").then((data) => {
    // 3. Create a variable that holds the samples array. 
    var samples = data.samples;
    // 4. Create a variable that filters the samples for the object with the desired sample number.
    var resultArray = samples.filter(sampleObj => sampleObj.id == sample);
    //  5. Create a variable that holds the first sample in the array.
    var result = resultArray[0];

    // 6. Create variables that hold the otu_ids, otu_labels, and sample_values.
    var  ids = result.otu_ids;
    var labels = result.otu_labels.slice(0, 10).reverse();
    var values = result.sample_values.slice(0,10).reverse();

    var bubbleLabels = result.otu_labels;
    var bubbleValues = result.sample_values;

    // 7. Create the yticks for the bar chart.
    // Hint: Get the the top 10 otu_ids and map them in descending order  
    //  so the otu_ids with the most bacteria are last. 

    var yticks = ids.map(sampleObj => "OTU " + sampleObj).slice(0,10).reverse();

    console.log(yticks)

    // 8. Create the trace for the bar chart. 
    var barData = [{
      x: values,
      y: yticks,
      type: "bar",
      orientation: "h",
      text: labels 
    }];
    // 9. Create the layout for the bar chart. 
    var barLayout = {
     title: "Top 10 Bacteria Cultures Found"
    };
    // 10. Use Plotly to plot the data with the layout. 
    Plotly.newPlot("bar", barData, barLayout);

    // 1. Create the trace for the bubble chart.
    var bubbleData = [{
      x: ids,
      y: bubbleValues,
      text: bubbleLabels,
      mode: "markers",
       marker: {
         size: bubbleValues,
         color: bubbleValues,
         colorscale: "Portland" 
       }
    }];
````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/1.2.JPG?raw=true)



3. **Code is written to create the layout array in the `buildCharts()` function that creates a title for the chart.**


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
    // 9. Create the layout for the bar chart. 
    var barLayout = {
     title: "Top 10 Bacteria Cultures Found"
    };
    // 10. Use Plotly to plot the data with the layout. 
    Plotly.newPlot("bar", barData, barLayout);
````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/1.3.JPG?raw=true)



4. **When the dashboard is first opened in a browser, ID 940’s data should be displayed in the dashboard, and the bar chart has the following:**
    - The top 10 `sample_values` are sorted in descending order
    - The top 10 `sample_values` as values
    - The `otu_ids` as the labels


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
// COLUMBIA ENGINEERING 
// By Emmanuel Martinez
// Module 12


// DELIVERABLE 1: Create a Horizontal Bar Chart

function init() {
  // Grab a reference to the dropdown select element
  var selector = d3.select("#selDataset");

  // Use the list of sample names to populate the select options
  d3.json("JS/data/samples.json").then((data) => {
    var sampleNames = data.names;

    sampleNames.forEach((sample) => {
      selector
        .append("option")
        .text(sample)
        .property("value", sample);
    });

    // Use the first sample from the list to build the initial plots
    var firstSample = sampleNames[0];
    buildCharts(firstSample);
    buildMetadata(firstSample);
  });
}

// Initialize the dashboard
init();

function optionChanged(newSample) {
  // Fetch new data each time a new sample is selected
  buildMetadata(newSample);
  buildCharts(newSample);
  
}

// Demographics Panel 
function buildMetadata(sample) {
  d3.json("JS/data/samples.json").then((data) => {
    var metadata = data.metadata;
    // Filter the data for the object with the desired sample number
    var resultArray = metadata.filter(sampleObj => sampleObj.id == sample);
    var result = resultArray[0];
    // Use d3 to select the panel with id of `#sample-metadata`
    var PANEL = d3.select("#sample-metadata");

    // Use `.html("") to clear any existing metadata
    PANEL.html("");

    // Use `Object.entries` to add each key and value pair to the panel
    // Hint: Inside the loop, you will need to use d3 to append new
    // tags for each key-value in the metadata.
    Object.entries(result).forEach(([key, value]) => {
      PANEL.append("h6").text(`${key.toUpperCase()}: ${value}`);
    });

  });
}

// 1. Create the buildCharts function.
function buildCharts(sample) {
  // 2. Use d3.json to load and retrieve the samples.json file 
  d3.json("JS/data/samples.json").then((data) => {
    // 3. Create a variable that holds the samples array. 
    var samples = data.samples;
    // 4. Create a variable that filters the samples for the object with the desired sample number.
    var resultArray = samples.filter(sampleObj => sampleObj.id == sample);
    //  5. Create a variable that holds the first sample in the array.
    var result = resultArray[0];

    // 6. Create variables that hold the otu_ids, otu_labels, and sample_values.
    var  ids = result.otu_ids;
    var labels = result.otu_labels.slice(0, 10).reverse();
    var values = result.sample_values.slice(0,10).reverse();

    var bubbleLabels = result.otu_labels;
    var bubbleValues = result.sample_values;

    // 7. Create the yticks for the bar chart.
    // Hint: Get the the top 10 otu_ids and map them in descending order  
    //  so the otu_ids with the most bacteria are last. 

    var yticks = ids.map(sampleObj => "OTU " + sampleObj).slice(0,10).reverse();

    console.log(yticks)

    // 8. Create the trace for the bar chart. 
    var barData = [{
      x: values,
      y: yticks,
      type: "bar",
      orientation: "h",
      text: labels 
    }];
    // 9. Create the layout for the bar chart. 
    var barLayout = {
     title: "Top 10 Bacteria Cultures Found"
    };
    // 10. Use Plotly to plot the data with the layout. 
    Plotly.newPlot("bar", barData, barLayout);
````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/1.4.JPG?raw=true)




# Deliverable 2:  
## Create a Bubble Chart
### Deliverable Requirements:
Using your knowledge of JavaScript, Plotly, and D3.js, create a bubble chart that will display the following when an individual’s ID is selected from the dropdown menu webpage:

- The `otu_ids` as the x-axis values.
- The `sample_values` as the y-axis values.
- The `sample_values` as the marker size.
- The `otu_ids` as the marker colors.
- The `otu_labels` as the hover-text values.

Your bubble chart should look like the following image:

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/s3.png?raw=true)



1. The code for the trace object in the `buildCharts()`; function does the following:
    - Sets the `otu_ids` as the x-axis values
    - Sets the `sample_values` as the y-axis values
    - Sets the `otu_labels` as the hover-text values
    - Sets the `sample_values` as the marker size
    - Sets the `otu_ids` as the marker colors
2. The code for the layout in the `buildCharts()`; function does the following:
    - Creates a title
    - Creates a label for the x-axis
    - The text for a bubble is shown when hovered over
3. When the dashboard is first opened in a browser, ID 940’s data should be displayed in the dashboard. All three charts should also be working according to their requirements when a sample is selected from the dropdown menu

 
### Results with detail analysis:

1. **The code for the trace object in the `buildCharts()`; function does the following:**
    - Sets the `otu_ids` as the x-axis values
    - Sets the `sample_values` as the y-axis values
    - Sets the `otu_labels` as the hover-text values
    - Sets the `sample_values` as the marker size
    - Sets the `otu_ids` as the marker colors


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
// 1. Create the trace for the bubble chart.
    var bubbleData = [{
      x: ids,
      y: bubbleValues,
      text: bubbleLabels,
      mode: "markers",
       marker: {
         size: bubbleValues,
         color: bubbleValues,
         colorscale: "Portland" 
       }
    }];

````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/2.1.JPG?raw=true)



2. **The code for the layout in the `buildCharts()`; function does the following:**
    - Creates a title
    - Creates a label for the x-axis
    - The text for a bubble is shown when hovered over.


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
// 2. Create the layout for the bubble chart.
    var bubbleLayout = {
        title: "Bacteria Cultures Per Sample",
        xaxis: {title: "OTU ID"},
        automargin: true,
        hovermode: "closest"
    };
````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/2.2.JPG?raw=true)



3. **When the dashboard is first opened in a browser, ID 940’s data should be displayed in the dashboard. All three charts should also be working according to their requirements when a sample is selected from the dropdown menu.**


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
    // 3. Use Plotly to plot the data with the layout.
    Plotly.newPlot("bubble", bubbleData, bubbleLayout)
````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/2.3.JPG?raw=true)




# Deliverable 3:  
## Create a Gauge Chart
### Deliverable Requirements:
Using your knowledge of JavaScript, Plotly, and D3.js, create a gauge chart that displays the weekly washing frequency's value, and display the value as a measure from 0-10 on the progress bar in the gauge chart when an individual ID is selected from the dropdown menu.

Your gauge chart should look similar to the following image:

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/s4.png?raw=true)



1. The code to build the gauge chart does the following:
    - Creates a title for the chart.
    - Creates the ranges for the gauge in increments of two, with a different color for each increment.
    - Adds the washing frequency value on the gauge chart.
    - The indicator shows the level for the washing frequency on the gauge.
    - The gauge is added to the dashboard.
    - The gauge fits in the margin of the `<div>` element.
2. When the webpage loads, the bar and bubble chart are working according to the requirements in Deliverable 1 and 2, respectively, and the gauge chart is working according to the requirements listed for this Deliverable

 
### Results with detail analysis:

1. **The code to build the gauge chart does the following:**
    - Creates a title for the chart.
    - Creates the ranges for the gauge in increments of two, with a different color for each increment.
    - Adds the washing frequency value on the gauge chart.
    - The indicator shows the level for the washing frequency on the gauge.
    - The gauge is added to the dashboard.
    - The gauge fits in the margin of the `<div>` element.


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````java
// 1. Create a variable that filters the metadata array for the object with the desired sample number.
    var metadata = data.metadata;
    var gaugeArray = metadata.filter(metaObj => metaObj.id == sample);  

    // 2. Create a variable that holds the first sample in the metadata array.
        var gaugeResult = gaugeArray[0];

    // 3. Create a variable that holds the washing frequency.  
    var wfreqs = gaugeResult.wfreq;
    console.log(wfreqs)

    // 4. Create the trace for the gauge chart.
    var gaugeData = [{
      value: wfreqs,
      type: "indicator",
      mode: "gauge+number",
      title: {text: "<b> Belly Button Washing Frequency </b> <br></br> Scrubs Per Week"},
      gauge: {
        axis: {range: [null,10], dtick: "2"},

        bar: {color: "black"},
        steps:[
          {range: [0, 2], color: "red"},
          {range: [2, 4], color: "orange"},
          {range: [4, 6], color: "yellow"},
          {range: [6, 8], color: "lightgreen"},
          {range: [8, 10], color: "green"}
        ],
        dtick: 2
      }
    }];
    
    // 5. Create the layout for the gauge chart.
    var gaugeLayout = { 
     automargin: true
    };

    // 6. Use Plotly to plot the gauge data and layout.
    Plotly.newPlot("gauge", gaugeData, gaugeLayout)
  });
}

````
> HTML Code
````html
      <div class="col-md-5">
        <div id="gauge"></div>
      </div>

````



![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/3.1.JPG?raw=true)

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/3.1.1.JPG?raw=true)



2. **When the webpage loads, the bar and bubble chart are working according to the requirements in Deliverable 1 and 2, respectively, and the gauge chart is working according to the requirements listed for this Deliverable**


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Bellybutton Biodiversity</title>
    <link
      rel="stylesheet"
      href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    />
  </head>

<body>

  <div class="container">
    <div class="row">
      <div class="col-md-12 jumbotron text-center">
        <h1>Belly Button Biodiversity Dashboard</h1>
        <p>Use the interactive charts below to explore the dataset</p>
      </div>
    </div>
    <div class="row">
      <div class="col-md-2">
        <div class="well">
          <h5>Test Subject ID No.:</h5>
          <select id="selDataset" onchange="optionChanged(this.value)"></select>
        </div>
        <div class="panel panel-primary">
          <div class="panel-heading">
            <h3 class="panel-title">Demographic Info</h3>
          </div>
          <div id="sample-metadata" class="panel-body"></div>
        </div>
      </div>
      <div class="col-md-5">
        <div id="bar"></div>
      </div>
      <div class="col-md-5">
        <div id="gauge"></div>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <div id="bubble"></div>
      </div>
    </div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/5.5.0/d3.js"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="./JS/charts.js"></script>


</body>

</html>
````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/3.2.JPG?raw=true)





# Deliverable 4:  
## Customize the Dashboard
### Deliverable Requirements:
Use your knowledge of HTML and Bootstrap to customize the webpage for your dashboard.

1. Customize your dashboard with three of the following:
    - Add an image to the jumbotron.
    - Add background color or a variety of compatible colors to the webpage.
    - Use a custom font with contrast for the colors.
    - Add more information about the project as a paragraph on the page.
    - Add information about what each graph visualizes, either under or next to each graph.
    - Make the webpage mobile-responsive.
    - Change the layout of the page.
    - Add a navigation bar that allows you to select the bar or bubble chart on the page.
2. When the dashboard is first opened in a browser, ID 940’s data should be displayed in the dashboard, and the three charts should be working according to their requirements.
3. When a sample is selected, the dashboard should display the data in the panel and all three charts according to their requirements.

 
### Results with detail analysis:

1. **Customize your dashboard with three of the following:**
    - Add an image to the jumbotron.
    - Add background color or a variety of compatible colors to the webpage.
    - Use a custom font with contrast for the colors.
    - Add more information about the project as a paragraph on the page.
    - Add information about what each graph visualizes, either under or next to each graph.
    - Make the webpage mobile-responsive.
    - Change the layout of the page.
    - Add a navigation bar that allows you to select the bar or bubble chart on the page.


> Image with `JavaScript` & `HTML` Code below.

**Code and Image**


````css
body {

    color: #000000;
    background-color: rgb(255, 255, 255);
  }
 
 .tag {
     font-family: inherit; 
     font-size: 1rem;
    }  

.jumbotron {
    background-image: url("../images/bacteria.jpg");
    background-size: 100% 100%;
    text-align: center;
  }

````

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/4.1.JPG?raw=true)



2. **When the dashboard is first opened in a browser, ID 940’s data should be displayed in the dashboard, and the three charts should be working according to their requirements.**



> Image with `JavaScript` & `HTML` Code below.


![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/4.2.JPG?raw=true)



3. **When a sample is selected, the dashboard should display the data in the panel and all three charts according to their requirements.**

> Page Fully Responsive

![name-of-you-image](https://github.com/emmanuelmartinezs/Plotly/blob/main/Resources/Images/4.3.JPG?raw=true)


> Please visit the Site below to intercat with the Data!

[`Belly Button Biodiversity Dashboard`](https://emmanuelmartinezs.github.io/Plotly). 



##### JavaScript, Bootstrap, and Mapping Earthquakes Analysis Completed by Emmanuel Martinez

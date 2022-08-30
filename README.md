# PlotlyPages
### Overview
Improbable Beef is searching for a bacterial species to use in order to manufacture synthetic beef. They've enlisted volunteers who have provided samples of the bacteria found in their belly buttons. Should the company identify a worthy bacterial candidate, volunteers with that bacteria could be well compensated. The challenge was to create an interactive website for the volunteers to view their specific results, including the top 10 bacterial species in their belly buttons. This way, they can easily check if their belly button contains a potentially lucrative bacteria.

#### Results

##### Interactivity & Deliverable 1
Volunteers are assigned test subject numbers so that their information can remain anonymous. To learn about their results, volunteers can select their test number from a drop down menu. The Demographic Info panel will update to that test subject, as will the horizontal bar chart, which was the goal of Deliverable 1.

Using JavaScript, here's the code:

```
// Deliverable 1
// 1. Create the buildCharts function.
// Create the buildChart function.
function buildCharts(sample) {
  // Use d3.json to load the samples.json file 
  d3.json("samples.json").then((data) => {
    console.log(data);

    // Create a variable that holds the samples array. 
    var samples = data.samples;
    console.log(samples);

    // Create a variable that filters the samples for the object with the desired sample number.
    var resultArray = samples.filter(sampleObj => sampleObj.id == sample);
// 1. Create a variable that filters the metadata array for the object with the desired sample number.
    var metadataArray = data.metadata.filter(sampleObj => sampleObj.id == sample);

    // Create a variable that holds the first sample in the array.
    var result = resultArray[0];

    // 2. Create a variable that holds the first sample in the metadata array.
    var metadata = metadataArray[0];

    // Create variables that hold the otu_ids, otu_labels, and sample_values.
    var otu_ids = result.otu_ids;
    var otu_labels = result.otu_labels;
    var sample_values = result.sample_values;

    // 3. Create a variable that holds the washing frequency.
    var frequency = parseFloat(metadata.wfreq);


    // Create the yticks for the bar chart.
    // Hint: Get the the top 10 otu_ids and map them in descending order 
    // so the otu_ids with the most bacteria are last. 
    var yticks = otu_ids.slice(0, 10).map(otuID => `OTU ${otuID}`).reverse();
    // Create the trace for the bar chart. 
    var barData = [
      {
        y: yticks,
        x: sample_values.slice(0, 10).reverse(),
        text: otu_labels.slice(0, 10).reverse(),
        type: "bar",
        orientation: "h",
      }
    ];
    // Create the layout for the bar chart. 
    var barLayout = {
      title: "Top 10 Bacteria Cultures Found",
      margin: { t: 30, l: 150 }
    };
    // Use Plotly to plot the data with the layout. 
    Plotly.newPlot("bar", barData, barLayout);
    
```

And the result:

![image](https://user-images.githubusercontent.com/107162310/187500688-574e3ca3-4676-4f78-9826-6914870aafe9.png)

##### Deliverable 2
The goal in Deliverable 2 was to create a bubble chart that'll update to the selected test subject and display the bacteria cultures in their sample. (This is all part of the function started in Deliverable 1). Here's the code:

```
// Create the trace for the bubble chart.
    var bubbleData = [
      {
        x: otu_ids,
        y: sample_values,
        text: otu_labels,
        mode: "markers",
        marker: {
          size: sample_values,
          color: otu_ids,
          colorscale: "Earth"
        }
      }
    ];

    // Create the layout for the bubble chart.
    var bubbleLayout = {
      title: "Bacteria Cultures Per Sample",
      margin: { t: 0 },
      hovermode: "closest",
      xaxis: { title: "OTU ID" },
      margin: { t: 30}
    };
    // Use Plotly to plot the data with the layout.
    Plotly.newPlot("bubble", bubbleData, bubbleLayout);
    
 ```
 
 And the result:

![image](https://user-images.githubusercontent.com/107162310/187501134-275d16d2-1655-4fbc-bd5a-c1b049150c69.png)

##### Deliverable 3
The goal in Deliverable 3 was to create a gauge chart that showed the frequency at which the test subject washed their belly button. Here's the JavaScript code:

```
    // 4. Create the trace for the gauge chart.
    var gaugeData = [
      {
        domain: { x: [0, 1], y: [0, 1] },
        value: frequency,
        title: { text: "<b>Belly Button Washing Frequency</b> <br> Scrubs per Week"},
        type: "indicator",
        mode: "gauge+number",
        gauge: {
          axis: { range: [null, 10] },
          bar: { color: "black"},
          steps: [
            { range: [0, 2], color: "maroon" },
            { range: [2, 4], color: "orangered" },
            { range: [4, 6], color: "yellow" },
            { range: [6, 8], color: "lawngreen" },
            { range: [8, 10], color: "darkgreen" }
          ],
        }
      }
    ];
    
    // 5. Create the layout for the gauge chart.
    var gaugeLayout = { 
      width: 500, 
      height: 425, 
      margin: { t: 0, b: 0 } };

    // 6. Use Plotly to plot the gauge data and layout.
    Plotly.newPlot("gauge", gaugeData, gaugeLayout);
  });
}

```

And the chart:

![image](https://user-images.githubusercontent.com/107162310/187505919-3f7ec8d5-8368-4b3b-85f6-d56712757307.png)

##### Deliverable 4
The goal here was to customize the dashboard with at least three changes. I chose:

- Add an image to the jumbotron
- Add background color or a variety of compatible colors to the webpage
- Use a custom font with contrast for the colors
- Add more information about the project as a paragraph on the page

Here's the final HTML code:

```
 <!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Bellybutton Biodiversity</title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
</head>

<body style="background-color:cadetblue;">
  <div class="container">
    <div class="row">
      <div class="col-md-12 jumbotron text-center" style="background-image: url(background.jpg); -webkit-background-size: cover; -moz-background-size: cover; -o-background-size: cover; background-size: cover;">
        <h1 style="color:white; font-family: Georgia, 'Times New Roman', Times, serif"><b>Belly Button Biodiversity Dashboard</b></h1>
        <h2 style="color: blueviolet; font-family: Georgia, 'Times New Roman', Times, serif"><b>Use the interactive charts below to explore the dataset</b></h2>
      </div>
    </div>
    <div class="row">
      <div class="col-md-2">
        <div class="well">
          <h5>Test Subject ID No.:</h5>
          <!-- <select id="selDataset"></select> -->
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
    <p><h3 style="color:black; text-align:center; font-family: Georgia, 'Times New Roman', Times, serif">Select a Test Subject ID number from the drop-down menu. The Demographic Info panel will then update to that test subject. Use the bar chart, bubble chart, and gauge chart to learn more about the subject. Each of these charts will update when a test subject is selected.</h3></p>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/5.5.0/d3.js"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="charts.js"></script>
  <!-- <script src="plots.js"></script> -->

</body>

</html>

```

And the resulting webpage:

![image](https://user-images.githubusercontent.com/107162310/187513620-7d9c151e-8391-48c9-b08e-72e072ebe568.png)

Visit the page (https://ajp71088.github.io/PlotlyPages "here").

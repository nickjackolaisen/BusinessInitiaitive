---
title: "The Van Westendorp's Price Sensitivity Meter (PSM)"
layout: post
date: 2023-11-29
author: jack_nicholaisen
summary: "Learn how to determine the optimal price for your product or service using Van Westendorp's Price Sensitivity Meter. #pricingstrategy #marketresearch" 
permalink: /tools/calculator/price-sensitivity-meter/
tags: price sensitivity meter questions, price sensitivity meter calculator, van westendorp calculator, van westendorp price sensitivity
---

As a marketer, understanding how your customers perceive pricing is vital. 

The Van Westendorp's Price Sensitivity Meter (PSM) is a tool that can help you determine the optimal price for your product or service. 

## What is the Van Westendorp's Price Sensitivity Meter or PSM?

The Van Westendorp's PSM is a survey-based method used to determine the optimal price for a product or service. 

It is based on the idea that consumers have a certain range of acceptable prices for a product or service, and that this range can be determined through survey questions.

The PSM uses four different questions to determine the acceptable price range:

1.  At what price would you consider the product to be so expensive that you would not consider buying it?

2.  At what price would you consider the product to be priced so low that you would feel the quality couldn't be very good?

3.  At what price would you consider the product starting to get expensive, so that it is not out of the question, but you would have to give some thought to buying it?

4.  At what price would you consider the product to be a bargain, a great buy for the money?

The survey results are then used to create a graph that shows the acceptable price range for the product or service.

### How to Use the Van Westendorp's PSM

To use the PSM, you will need to create a survey that includes the four questions listed above. 

You can use a survey tool such as [SurveyMonkey](https://www.surveymonkey.com/) or [Google Forms](https://www.google.com/forms/about/) to create your survey.

Once you have collected enough responses, you can use this PSM calculator to analyze the results and create the graph that shows the acceptable price range. 


<h3>Van Westendorp's Price Sensitivity Meter</h3>
<style>
    /* Add your CSS styles here */
    #questionnaire {
      margin-bottom: 20px;
    }
    canvas {
      border: 1px solid #ccc;
    }
</style>
<body>
  <div id="questionnaire">
    <label for="numColumns">Number of Columns:</label>
    <input type="number" id="numColumns" min="1" value="3">
    <button onclick="addColumns(1)">+1</button>
    <button onclick="addColumns(5)">+5</button>
    <button onclick="addColumns(10)">+10</button>
    <button onclick="removeColumns(1)">-1</button>
    <button onclick="removeColumns(5)">-5</button>
    <button onclick="removeColumns(10)">-10</button>
    <hr>
    <table id="inputTable">
      <!-- Table rows for user inputs will be dynamically generated here -->
    </table>
    <button onclick="calculateIntersections()">Calculate Intersections</button>
  </div>
  <canvas id="graph" width="600" height="400"></canvas>
  <div id="intersectionPoints"></div>

  <script>
    let numColumns = document.getElementById('numColumns');
    let inputTable = document.getElementById('inputTable');
    let graphCanvas = document.getElementById('graph');
    let intersectionPointsDiv = document.getElementById('intersectionPoints');
    let ctx = graphCanvas.getContext('2d');
    let data = [];

    // Function to add columns for user inputs
    function addColumns(columnsToAdd) {
      let columns = parseInt(numColumns.value) + columnsToAdd;
      numColumns.value = columns;

      inputTable.innerHTML = ''; // Clear existing table

      for (let i = 1; i <= columns; i++) {
        let row = document.createElement('tr');
        let label = document.createElement('td');
        label.textContent = i;
        row.appendChild(label);

        for (let j = 0; j < 4; j++) {
          let cell = document.createElement('td');
          let input = document.createElement('input');
          input.type = 'number';
          cell.appendChild(input);
          row.appendChild(cell);
        }

        inputTable.appendChild(row);
      }
    }

    // Function to remove columns for user inputs
    function removeColumns(columnsToRemove) {
      let columns = parseInt(numColumns.value) - columnsToRemove;
      if (columns < 1) {
        columns = 1;
      }
      numColumns.value = columns;
      addColumns(0);
    }

    // Function to calculate intersection points
    function calculateIntersections() {
      data = []; // Clear previous data

      for (let i = 1; i <= parseInt(numColumns.value); i++) {
        let tooExpensive = parseFloat(inputTable.rows[i - 1].cells[1].children[0].value);
        let tooCheap = parseFloat(inputTable.rows[i - 1].cells[2].children[0].value);
        let expensive = parseFloat(inputTable.rows[i - 1].cells[3].children[0].value);
        let cheap = parseFloat(inputTable.rows[i - 1].cells[4].children[0].value);

        // Inverting cumulative frequencies for too cheap and cheap/good value
        let invertedCheap = 100 - cheap;
        let invertedTooCheap = 100 - tooCheap;

        // Calculating intersection points
        let PMC = (invertedTooCheap - expensive) / (invertedTooCheap - tooExpensive) * (cheap - tooCheap) + cheap;
        let PME = (cheap - invertedCheap) / (tooCheap - invertedCheap) * (tooExpensive - expensive) + tooExpensive;
        let IPP = (expensive - tooExpensive) / (cheap - tooCheap) * (invertedCheap - invertedTooCheap) + invertedCheap;
        let OPP = (tooExpensive - tooCheap) / (invertedCheap - invertedTooCheap) * (cheap - tooCheap) + cheap;

        data.push({ column: i, PMC, PME, IPP, OPP });
      }

      drawGraph();
      displayIntersectionPoints();
    }

    // Function to draw the graph
    function drawGraph() {
      ctx.clearRect(0, 0, graphCanvas.width, graphCanvas.height);

      let maxX = Math.max(...data.map(item => item.PMC, item.PME, item.IPP, item.OPP));
      let maxY = Math.max(...data.map(item => item.column));

      let xScale = graphCanvas.width / maxX;
      let yScale = graphCanvas.height / maxY;

      // Draw lines for PMC, PME, IPP, and OPP
      ctx.beginPath();
      ctx.moveTo(0, data[0].PMC * yScale);
      ctx.lineTo(graphCanvas.width, data[data.length - 1].PMC * yScale);
      ctx.strokeStyle = 'blue';
      ctx.stroke();

      ctx.beginPath();
      ctx.moveTo(0, data[0].PME * yScale);
      ctx.lineTo(graphCanvas.width, data[data.length - 1].PME * yScale);
      ctx.strokeStyle = 'red';
      ctx.stroke();

      ctx.beginPath();
      ctx.moveTo(0, data[0].IPP * yScale);
      ctx.lineTo(graphCanvas.width, data[data.length - 1].IPP * yScale);
      ctx.strokeStyle = 'green';
      ctx.stroke();

      ctx.beginPath();
      ctx.moveTo(0, data[0].OPP * yScale);
      ctx.lineTo(graphCanvas.width, data[data.length - 1].OPP * yScale);
      ctx.strokeStyle = 'orange';
      ctx.stroke();

      // Draw intersection points
      ctx.fillStyle = 'black';
      data.forEach(item => {
        ctx.beginPath();
        ctx.arc(item.PMC * xScale, item.column * yScale, 5, 0, Math.PI * 2);
        ctx.fill();

        ctx.beginPath();
        ctx.arc(item.PME * xScale, item.column * yScale, 5, 0, Math.PI * 2);
        ctx.fill();

        ctx.beginPath();
        ctx.arc(item.IPP * xScale, item.column * yScale, 5, 0, Math.PI * 2);
        ctx.fill();

        ctx.beginPath();
        ctx.arc(item.OPP * xScale, item.column * yScale, 5, 0, Math.PI * 2);
        ctx.fill();
      });
    }

    // Function to display intersection points
    function displayIntersectionPoints() {
      intersectionPointsDiv.innerHTML = '<h3>Intersection Points:</h3>';
      data.forEach(item => {
        intersectionPointsDiv.innerHTML += `<p>Column ${item.column} - PMC: ${item.PMC.toFixed(2)}, PME: ${item.PME.toFixed(2)}, IPP: ${item.IPP.toFixed(2)}, OPP: ${item.OPP.toFixed(2)}</p>`;
      });
    }

  </script>
</body>

## Interpreting the PSM Results

The PSM graph will show four lines that represent the four questions asked in the survey. 

The intersection of these lines represents the optimal price range for the product or service.

The graph can be used to determine the following:

-   The upper price limit (PME): This is the point where consumers start to feel that the product is too expensive and would not consider buying it.

-   The lower price limit (PMC): This is the point where consumers start to feel that the product is of low quality and would not consider buying it.

-   The point of indifference (IPP): This is the point where consumers feel that the product is priced fairly and would consider buying it.

-   The optimal price point (OPP): This is the point where an equal number of customers describe the price as too expensice or too cheap. Optimal in this sense refers to the fact that there is an equal tradeoff in extreme sensitivities to the price at both ends of the price spectrum.

You can use this information to inform your pricing strategy and stay competitive in the market.



Try out the PSM calculator now and see what insights you can gain for your product or service!






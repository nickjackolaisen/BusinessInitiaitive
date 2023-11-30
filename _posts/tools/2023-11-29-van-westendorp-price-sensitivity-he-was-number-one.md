---
title: "The Van Westendorp's Price Sensitivity Meter (PSM)"
layout: post
date: 2023-11-29
author: jack_nicholaisen
summary: "Learn how to determine the optimal price for your product or service using Van Westendorp's Price Sensitivity Meter. #pricingstrategy #marketresearch" 
permalink: /tools/guide/price-sensitivity-meter/
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



<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<body>
  <div id="questionnaire">
    <h2>Customer Questionnaire</h2>
    <button onclick="addColumns(1)">Add Column (+1)</button>
    <button onclick="addColumns(5)">Add Columns (+5)</button>
    <button onclick="removeColumns(1)">Remove Column (-1)</button>
    <button onclick="removeColumns(5)">Remove Columns (-5)</button>
    <table id="surveyTable">
      <!-- The table content will be generated dynamically -->
    </table>
    <button onclick="calculatePSM()">Calculate</button>
  </div>

  <div id="psmGraph">
    <h2>PSM Graph</h2>
    <div id="chart"></div>
  </div>

  <script>
    let numColumns = 1; // Initial number of columns
    generateSurveyTable();

    function generateSurveyTable() {
      const table = document.getElementById("surveyTable");
      table.innerHTML = ''; // Clear previous content
      
      // Create rows and columns dynamically based on the number of columns
      for (let i = 0; i < 4; i++) {
        const row = table.insertRow();
        for (let j = 0; j <= numColumns; j++) {
          const cell = row.insertCell();
          if (j === 0) {
            cell.innerHTML = `Row ${i + 1}`;
          } else {
            const input = document.createElement("input");
            input.type = "number";
            input.placeholder = "Enter value";
            cell.appendChild(input);
          }
        }
      }
    }

    function addColumns(num) {
      numColumns += num;
      generateSurveyTable();
    }

    function removeColumns(num) {
      numColumns -= num;
      if (numColumns < 1) numColumns = 1; // Ensure there's at least 1 column
      generateSurveyTable();
    }

    function calculatePSM() {
      const data = [];
      const prices = [];
      const percentages = [];
      
      // Retrieve values from the input fields
      for (let j = 1; j <= numColumns; j++) {
        const columnValues = [];
        for (let i = 0; i < 4; i++) {
          const input = document.querySelector(`#surveyTable tr:nth-child(${i + 1}) td:nth-child(${j + 1}) input`);
          if (input) {
            columnValues.push(parseFloat(input.value) || 0);
          }
        }
        data.push(columnValues);
      }
      
      // Calculate cumulative frequencies
      for (let i = 0; i < data[0].length; i++) {
        let tooExpensive = 0,
            tooCheap = 0,
            expensiveHighSide = 0,
            cheapGoodValue = 0;
        
        for (let j = 0; j < data.length; j++) {
          if (data[j][i]) {
            tooExpensive += data[j][0] < data[j][i] ? 1 : 0;
            tooCheap += data[j][1] > data[j][i] ? 1 : 0;
            expensiveHighSide += data[j][2] < data[j][i] ? 1 : 0;
            cheapGoodValue += data[j][3] > data[j][i] ? 1 : 0;
          }
        }
        
        prices.push(data[0][i]);
        const totalResponses = data.length * 100; // Total number of responses
        percentages.push({
          x: data[0][i],
          y: (tooExpensive + tooCheap) / totalResponses * 100, // Inverted cumulative frequencies
        });
      }
      
      // Plotting the graph using Plotly
      const trace = {
        x: prices,
        y: percentages.map((p) => p.y),
        type: 'scatter',
        mode: 'lines+markers',
      };
      
      const layout = {
        title: 'Price Sensitivity Meter (PSM)',
        xaxis: {
          title: 'Price',
        },
        yaxis: {
          title: 'Percentage of Consumers',
        },
      };
      
      Plotly.newPlot('chart', [trace], layout);
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






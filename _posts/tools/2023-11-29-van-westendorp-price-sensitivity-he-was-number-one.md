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

<script>
    function updateColumns(action, column) {
      const columnElement = document.getElementById(column);
      let value = parseInt(columnElement.textContent);
      if (action === 'add') {
        value += 1;
      } else if (action === 'subtract') {
        value -= 1;
      } else if (action === 'add5') {
        value += 5;
      } else if (action === 'subtract5') {
        value -= 5;
      } else if (action === 'add10') {
        value += 10;
      } else if (action === 'subtract10') {
        value -= 10;
      }
      columnElement.textContent = value < 1 ? 1 : value; // Ensure value doesn't go below 1
    }
    function calculatePSM() {
      const tooExpensive = parseFloat(document.getElementById('row1').value);
      const tooCheap = parseFloat(document.getElementById('row2').value);
      const expensiveHigh = parseFloat(document.getElementById('row3').value);
      const cheapValue = parseFloat(document.getElementById('row4').value);
      // Calculate cumulative frequencies for "too cheap" and "cheap/good value"
      const cumulativeTooCheap = 100 - tooCheap;
      const cumulativeCheapValue = 100 - cheapValue;
      // Calculate intersection points
      const pmc = tooExpensive / (1 - tooCheap / 100);
      const pme = tooCheap / (1 - expensiveHigh / 100);
      const ipp = expensiveHigh / (1 - cheapValue / 100);
      const opp = (tooExpensive + tooCheap) / 2;
      // Display intersection points on the graph
      const graph = document.getElementById('graph');
      graph.innerHTML = `
        <div class="intersection" style="left: ${pmc}%; top: ${cumulativeTooCheap}%;"></div>
        <div class="intersection" style="left: ${pme}%; top: ${cumulativeCheapValue}%;"></div>
        <div class="intersection" style="left: ${ipp}%; top: ${expensiveHigh}%;"></div>
        <div class="intersection" style="left: ${opp}%; top: ${(100 - (tooExpensive + tooCheap) / 2)}%;"></div>
      `;
      // Display intersection point values
      const intersectionValues = document.getElementById('intersectionValues');
      intersectionValues.innerHTML = `
        <p>Point of Marginal Cheapness (PMC): $${pmc.toFixed(2)}</p>
        <p>Point of Marginal Expensiveness (PME): $${pme.toFixed(2)}</p>
        <p>Indifference Price Point (IPP): $${ipp.toFixed(2)}</p>
        <p>Optimal Price Point (OPP): $${opp.toFixed(2)}</p>
      `;
    }
  </script>
<style>
    .intersection {
      position: absolute;
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background-color: red;
      border: 2px solid white;
      transform: translate(-50%, -50%);
      position: absolute;
    }
</style>

<body>
  <h3>Price Sensitivity Meter (PSM)</h3>

  <div>
    <h2>Column Labels:</h2>
    <button onclick="updateColumns('subtract', 'column1')">-1</button>
    <button onclick="updateColumns('subtract5', 'column1')">-5</button>
    <button onclick="updateColumns('subtract10', 'column1')">-10</button>
    <span id="column1">1</span>
    <button onclick="updateColumns('add', 'column1')">+1</button>
    <button onclick="updateColumns('add5', 'column1')">+5</button>
    <button onclick="updateColumns('add10', 'column1')">+10</button>
  </div>

  <table border="1">
    <tr>
      <th></th>
      <th>1</th>
    </tr>
    <tr>
      <td>Too Expensive</td>
      <td><input type="number" id="row1" /></td>
    </tr>
    <tr>
      <td>Too Cheap</td>
      <td><input type="number" id="row2" /></td>
    </tr>
    <tr>
      <td>Expensive/High Side</td>
      <td><input type="number" id="row3" /></td>
    </tr>
    <tr>
      <td>Cheap/Good Value</td>
      <td><input type="number" id="row4" /></td>
    </tr>
  </table>

  <button onclick="calculatePSM()">Calculate PSM</button>

  <div style="position: relative; height: 300px; width: 400px; border: 1px solid black; margin-top: 20px;">
    <div id="graph" style="position: absolute; height: 100%; width: 100%;"></div>
  </div>

  <div id="intersectionValues">
    <!-- Intersection point values will be displayed here -->
  </div>

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






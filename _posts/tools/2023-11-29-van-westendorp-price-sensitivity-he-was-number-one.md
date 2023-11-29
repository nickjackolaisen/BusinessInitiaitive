---
title: "The Van Westendorp's Price Sensitivity Meter (PSM)"
layout: post
date: 2023-11-29
author: jack_nicholaisen
summary: "Learn how to determine the optimal price for your product or service using Van Westendorp's Price Sensitivity Meter. #pricingstrategy #marketresearch" 
image: /images/posts-headers/calculator/tmv-present-value-calculator-header.png
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

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.7.0/chart.min.js"></script>

<body>
  <h3>Price Sensitivity Meter</h3>
  <canvas id="myChart" width="400" height="400"></canvas>
  
  <div id="intersectionPoints">
    <b>Intersection Points:</b>
    <ul>
      <li id="pmcDesc">PMC (Point of Marginal Cheapness): <span id="pmc"></span></li>
      <li id="pmeDesc">PME (Point of Marginal Expensiveness): <span id="pme"></span></li>
      <li id="ippDesc">IPP (Indifference Price Point): <span id="ipp"></span></li>
      <li id="oppDesc">OPP (Optimal Price Point): <span id="opp"></span></li>
    </ul>
  </div>
  
  <div id="questionnaire">
    <h3>Questionnaire</h3>
    <label>Too expensive:</label>
    <input type="number" id="tooExpensive"><br>
    <label>Too cheap:</label>
    <input type="number" id="tooCheap"><br>
    <label>Expensive/High Side:</label>
    <input type="number" id="expensiveHigh"><br>
    <label>Cheap/Good Value:</label>
    <input type="number" id="cheapGoodValue"><br>
    <button onclick="calculateIntersections()">Calculate</button>
  </div>

  <script>
    function calculateIntersections() {
      const tooExpensive = parseFloat(document.getElementById('tooExpensive').value);
      const tooCheap = parseFloat(document.getElementById('tooCheap').value);
      const expensiveHigh = parseFloat(document.getElementById('expensiveHigh').value);
      const cheapGoodValue = parseFloat(document.getElementById('cheapGoodValue').value);

      const intersectionPoints = {
        pmc: tooCheap - (expensiveHigh - tooCheap),
        pme: tooExpensive + (tooExpensive - cheapGoodValue),
        ipp: (expensiveHigh + tooCheap) / 2,
        opp: (tooExpensive + tooCheap) / 2
      };

      document.getElementById('pmc').innerText = `$${intersectionPoints.pmc.toFixed(2)}`;
      document.getElementById('pme').innerText = `$${intersectionPoints.pme.toFixed(2)}`;
      document.getElementById('ipp').innerText = `$${intersectionPoints.ipp.toFixed(2)}`;
      document.getElementById('opp').innerText = `$${intersectionPoints.opp.toFixed(2)}`;

      document.getElementById('pmcDesc').title = 'Point of Marginal Cheapness (PMC)';
      document.getElementById('pmcDesc').setAttribute('data-tooltip', 'This is the lower bound of an acceptable price range. It represents the point where the product is perceived as potentially too cheap, impacting perceived quality.');

      document.getElementById('pmeDesc').title = 'Point of Marginal Expensiveness (PME)';
      document.getElementById('pmeDesc').setAttribute('data-tooltip', 'This is the upper bound of an acceptable price range. It signifies the point where the product is perceived as potentially too expensive, affecting its value.');

      document.getElementById('ippDesc').title = 'Indifference Price Point (IPP)';
      document.getElementById('ippDesc').setAttribute('data-tooltip', 'This is the price point where an equal number of respondents view the product as cheap and expensive, indicating a balance between perceived value and cost.');

      document.getElementById('oppDesc').title = 'Optimal Price Point (OPP)';
      document.getElementById('oppDesc').setAttribute('data-tooltip', 'This represents the point where an equal number of respondents view the product as exceeding their upper or lower price limits, indicating a balanced trade-off in price perception.');

      const ctx = document.getElementById('myChart').getContext('2d');
      const myChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: ['Too Expensive', 'Too Cheap', 'Expensive/High', 'Cheap/Good Value'],
          datasets: [{
            label: 'Price Sensitivity Meter',
            data: [tooExpensive, tooCheap, expensiveHigh, cheapGoodValue],
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
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






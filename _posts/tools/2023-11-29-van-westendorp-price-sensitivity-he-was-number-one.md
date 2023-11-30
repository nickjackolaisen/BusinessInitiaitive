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


  <h3>Price Sensitivity Meter (PSM)</h3>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<body>
  <div>
    <label for="addColumn">Add/Subtract Columns:</label>
    <button onclick="addColumn(1)">+1</button>
    <button onclick="addColumn(5)">+5</button>
    <button onclick="addColumn(-1)">-1</button>
    <button onclick="addColumn(-5)">-5</button>
  </div>

  <div id="questionnaire">
    <!-- The questionnaire will be dynamically generated here -->
  </div>

  <div id="chart"></div>

  <script>
    let numColumns = 1; // Initial number of columns

    // Function to add or subtract columns based on the user's choice
    function addColumn(value) {
      numColumns += value;
      renderQuestionnaire();
    }

    // Function to render the questionnaire based on the number of columns
    function renderQuestionnaire() {
      let questionnaireHTML = '<table>';
      for (let i = 1; i <= 4; i++) {
        questionnaireHTML += `<tr><td>Row ${i}</td>`;
        for (let j = 1; j <= numColumns; j++) {
          questionnaireHTML += `<td><input type="number" placeholder="${i === 1 ? 'Too Expensive' : (i === 2 ? 'Too Cheap' : (i === 3 ? 'Expensive/High Side' : 'Cheap/Good Value'))}"></td>`;
        }
        questionnaireHTML += '</tr>';
      }
      questionnaireHTML += '</table>';

      document.getElementById('questionnaire').innerHTML = questionnaireHTML;
      updateChart();
    }

    // Function to update the PSM chart based on user inputs
    function updateChart() {
      const data = [];
      for (let j = 1; j <= numColumns; j++) {
        const xValues = [];
        const yValues = [];

        for (let i = 1; i <= 4; i++) {
          const inputValue = parseFloat(document.querySelector(`#questionnaire input:nth-of-type(${(i - 1) * numColumns + j})`).value);
          if (!isNaN(inputValue)) {
            xValues.push(i);
            yValues.push(inputValue);
          }
        }

        data.push({
          x: xValues,
          y: yValues,
          mode: 'lines+markers',
          name: `Column ${j}`
        });
      }

      const layout = {
        title: 'Price Sensitivity Meter (PSM)',
        xaxis: {
          title: 'Price Perception'
        },
        yaxis: {
          title: 'Percentage of Consumers Willing to Pay',
          tickformat: ',.0%'
        }
      };

      const intersections = findIntersections(data);
      const intersectionPoints = intersections.map((point, index) => {
        return {
          x: point[0],
          y: point[1],
          text: `Intersection ${index + 1}: (${point[0]}, ${point[1].toFixed(2)})`,
          showarrow: true,
          arrowhead: 2,
          ax: 0,
          ay: -40
        };
      });

      Plotly.newPlot('chart', data, layout).then(() => {
        Plotly.relayout('chart', { shapes: intersections.map((_, index) => ({
          type: 'circle',
          xref: 'x',
          yref: 'y',
          x0: intersections[index][0],
          y0: intersections[index][1],
          x1: intersections[index][0] + 0.1,
          y1: intersections[index][1] + 0.1,
          line: { color: 'red' },
          opacity: 0.7
        })) });
        Plotly.relayout('chart', { annotations: intersectionPoints });
      });
    }

    // Function to find intersections of lines
    function findIntersections(data) {
      const intersections = [];
      for (let i = 0; i < data.length - 1; i++) {
        for (let j = i + 1; j < data.length; j++) {
          const intersection = [];
          for (let k = 0; k < data[i].x.length; k++) {
            const x1 = data[i].x[k];
            const y1 = data[i].y[k];
            const x2 = data[j].x[k];
            const y2 = data[j].y[k];

            if ((y1 < y2 && y1 >= 0 && y2 >= 0) || (y1 > y2 && y1 <= 0 && y2 <= 0)) {
              const x = (x1 * y2 - x2 * y1) / (y2 - y1);
              const y = (y2 * x1 - y1 * x2) / (x1 - x2);
              intersection.push([x, y]);
            }
          }
          intersections.push(...intersection);
        }
      }
      return intersections;
    }

    // Initially render the questionnaire with 1 column
    renderQuestionnaire();
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






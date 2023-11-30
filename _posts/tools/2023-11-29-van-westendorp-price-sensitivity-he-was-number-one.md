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



<body>
    <div>
        <h2>Van Westendorp's Price Sensitivity Meter</h2>
        <label for="addColumn">Add/Subtract Columns by:</label>
        <input type="number" id="addColumn" value="1" min="1" step="1">
        <button onclick="addColumns()">Apply</button>
        <button onclick="calculate()">Calculate</button>
    </div>
    <div>
        <table id="surveyTable">
            <tr>
                <th></th>
            </tr>
            <tr>
                <th>Too Expensive</th>
            </tr>
            <tr>
                <th>Too Cheap</th>
            </tr>
            <tr>
                <th>Expensive/High Side</th>
            </tr>
            <tr>
                <th>Cheap/Good Value</th>
            </tr>
        </table>
    </div>
    <div id="graph"></div>
    <div id="results"></div>
    <script>
        let columns = 1;
        function addColumns() {
            const addValue = parseInt(document.getElementById("addColumn").value);
            if (addValue > 0) {
                columns += addValue;
                renderSurveyTable();
            } else {
                alert("Please enter a valid number to add columns.");
            }
        }
        function renderSurveyTable() {
            const table = document.getElementById("surveyTable");
            table.innerHTML = "<tr><th></th></tr><tr><th>Too Expensive</th></tr><tr><th>Too Cheap</th></tr><tr><th>Expensive/High Side</th></tr><tr><th>Cheap/Good Value</th></tr>";
            for (let i = 1; i <= columns; i++) {
                const columnNumber = i;
                const th = document.createElement("th");
                th.textContent = columnNumber.toString();
                const trs = table.getElementsByTagName("tr");
                for (let j = 1; j < trs.length; j++) {
                    const td = document.createElement("td");
                    td.innerHTML = `<input type="number" id="q${j}_c${i}" step="5">`;
                    trs[j].appendChild(td);
                }
            }
        }
        function calculate() {
            const data = [];
            for (let i = 1; i <= columns; i++) {
                const columnData = [];
                for (let j = 1; j <= 4; j++) {
                    const value = parseInt(document.getElementById(`q${j}_c${i}`).value);
                    columnData.push(value);
                }
                data.push(columnData);
            }
            const prices = data.map(columnData => columnData[0]);
            const tooExpensive = data.map(columnData => columnData[0]);
            const tooCheap = data.map(columnData => columnData[1]);
            const expensiveHighSide = data.map(columnData => columnData[2]);
            const cheapGoodValue = data.map(columnData => columnData[3]);
            const tooExpensivePercent = tooExpensive.map((value, index) => ((columns - index) / columns) * 100);
            const tooCheapPercent = tooCheap.map((value, index) => ((columns - index) / columns) * 100);
            const expensiveHighSidePercent = expensiveHighSide.map((value, index) => (index / columns) * 100);
            const cheapGoodValuePercent = cheapGoodValue.map((value, index) => (index / columns) * 100);
            const trace1 = {
                x: prices,
                y: tooExpensivePercent,
                mode: 'lines+markers',
                name: 'Too Expensive',
            };
            const trace2 = {
                x: prices,
                y: tooCheapPercent,
                mode: 'lines+markers',
                name: 'Too Cheap',
            };
            const trace3 = {
                x: prices,
                y: expensiveHighSidePercent,
                mode: 'lines+markers',
                name: 'Expensive/High Side',
            };
            const trace4 = {
                x: prices,
                y: cheapGoodValuePercent,
                mode: 'lines+markers',
                name: 'Cheap/Good Value',
            };
            const layout = {
                title: 'Price Sensitivity Meter',
                xaxis: {
                    title: 'Price Range',
                },
                yaxis: {
                    title: 'Percentage of Consumers',
                },
            };
            const graphData = [trace1, trace2, trace3, trace4];
            Plotly.newPlot('graph', graphData, layout);
            calculateIntersections(prices, tooExpensivePercent, tooCheapPercent, expensiveHighSidePercent, cheapGoodValuePercent);
        }
        function calculateIntersections(prices, tooExpensivePercent, tooCheapPercent, expensiveHighSidePercent, cheapGoodValuePercent) {
            const intersections = [];
            for (let i = 0; i < prices.length - 1; i++) {
                if ((tooCheapPercent[i] <= tooExpensivePercent[i] && tooCheapPercent[i + 1] >= tooExpensivePercent[i + 1]) ||
                    (tooCheapPercent[i] >= tooExpensivePercent[i] && tooCheapPercent[i + 1] <= tooExpensivePercent[i + 1])) {
                    const intersection = calculateIntersection(prices[i], prices[i + 1], tooCheapPercent[i], tooCheapPercent[i + 1], tooExpensivePercent[i], tooExpensivePercent[i + 1]);
                    intersections.push({ price: intersection, label: "PMC" });
                }
                if ((cheapGoodValuePercent[i] <= expensiveHighSidePercent[i] && cheapGoodValuePercent[i + 1] >= expensiveHighSidePercent[i + 1]) ||
                    (cheapGoodValuePercent[i] >= expensiveHighSidePercent[i] && cheapGoodValuePercent[i + 1] <= expensiveHighSidePercent[i + 1])) {
                    const intersection = calculateIntersection(prices[i], prices[i + 1], cheapGoodValuePercent[i], cheapGoodValuePercent[i + 1], expensiveHighSidePercent[i], expensiveHighSidePercent[i + 1]);
                    intersections.push({ price: intersection, label: "PME" });
                }
                if ((tooExpensivePercent[i] <= expensiveHighSidePercent[i] && tooExpensivePercent[i + 1] >= expensiveHighSidePercent[i + 1]) ||
                    (tooExpensivePercent[i] >= expensiveHighSidePercent[i] && tooExpensivePercent[i + 1] <= expensiveHighSidePercent[i + 1])) {
                    const intersection = calculateIntersection(prices[i], prices[i + 1], tooExpensivePercent[i], tooExpensivePercent[i + 1], expensiveHighSidePercent[i], expensiveHighSidePercent[i + 1]);
                    intersections.push({ price: intersection, label: "IPP" });
                }
                if ((tooCheapPercent[i] <= cheapGoodValuePercent[i] && tooCheapPercent[i + 1] >= cheapGoodValuePercent[i + 1]) ||
                    (tooCheapPercent[i] >= cheapGoodValuePercent[i] && tooCheapPercent[i + 1] <= cheapGoodValuePercent[i + 1])) {
                    const intersection = calculateIntersection(prices[i], prices[i + 1], tooCheapPercent[i], tooCheapPercent[i + 1], cheapGoodValuePercent[i], cheapGoodValuePercent[i + 1]);
                    intersections.push({ price: intersection, label: "OPP" });
                }
            }
            intersections.sort((a, b) => a.price - b.price);
            displayIntersectionResults(intersections);
        }
        function calculateIntersection(x1, x2, y1, y2, z1, z2) {
            return x1 + ((x2 - x1) * (z1 - y1)) / (y2 - y1 - z2 + z1);
        }
        function displayIntersectionResults(intersections) {
            const resultsDiv = document.getElementById("results");
            resultsDiv.innerHTML = "<h3>Intersection Points:</h3>";
            intersections.forEach((point, index) => {
                resultsDiv.innerHTML += `<p>${index + 1}. ${point.label}: $${point.price.toFixed(2)}</p>`;
            });
        }
        renderSurveyTable();
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






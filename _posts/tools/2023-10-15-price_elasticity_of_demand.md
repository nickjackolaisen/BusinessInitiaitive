---
title: "How to Calculate the Price Elasticity of Demand"
layout: post
date: 2023-10-15
author: jack_nicholaisen
summary: "Price Elasticity of Demand is the measure of the responsiveness of the quantity demanded to a change in price." 
permalink: /tools/calculator/price-elasticity-of-demand/
---

Price elasticity is calculated using the following formula: 

PED = (Q2 - Q1) / ((Q2 + Q1) / 2) / ((P2 - P1) / ((P2 + P1) / 2))

Q1 is the initial quantity demanded, 

Q2 is the new quantity demanded, 

P1 is the initial price, 

and P2 is the new price.

<body>
    <div class="calculator" style="text-align:center">
        <h2>Price Elasticity of Demand Calculator</h2>
        <div class="input-group">
            <label for="initialPrice">Initial Price:</label>
            <input type="number" id="initialPrice" step="0.01" required>
        </div>
        <div class="input-group">
            <label for="finalPrice">Final Price:</label>
            <input type="number" id="finalPrice" step="0.01" required>
        </div>
        <div class="input-group">
            <label for="initialQuantity">Initial Quantity Demanded:</label>
            <input type="number" id="initialQuantity" step="1" required>
        </div>
        <div class="input-group">
            <label for="finalQuantity">Final Quantity Demanded:</label>
            <input type="number" id="finalQuantity" step="1" required>
        </div>
        <button onclick="calculateElasticity()">Calculate</button>
        <div class="result" id="result"></div>
    </div>

    <script>
        function calculateElasticity() {
            var initialPrice = document.getElementById("initialPrice").value;
            var finalPrice = document.getElementById("finalPrice").value;
            var initialQuantity = document.getElementById("initialQuantity").value;
            var finalQuantity = document.getElementById("finalQuantity").value;

            var elasticity = ((finalQuantity - initialQuantity) / ((finalQuantity + initialQuantity) / 2)) /
                ((finalPrice - initialPrice) / ((finalPrice + initialPrice) / 2));

            document.getElementById("result").textContent = "Price Elasticity of Demand: " + elasticity.toFixed(2);
        }
    </script>
</body>
<style>
        body {
            font-family: Arial, sans-serif;
            margin: 50px;
        }

        .calculator {
            width: 300px;
            margin: 0 auto;
        }

        .input-group {
            margin-bottom: 10px;
        }

        input[type="number"] {
            width: 100%;
            padding: 8px;
            box-sizing: border-box;
        }

        .result {
            font-weight: bold;
        }
</style>




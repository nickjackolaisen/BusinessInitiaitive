---
title: "How to Calculate Customer Acquisiton Costs (CAC)"
layout: post
date: 2023-10-15
author: jack_nicholaisen
summary: "This represents the cost of acquiring a new customer." 
permalink: /tools/calculator/customer-acquisition-costs/
---


CAC is an important metric because it helps businesses understand how much they need to spend on marketing and sales to acquire new customers, and whether these costs are sustainable over the long term.


<body style="text-align:center">
    <div class="calculator">
        <h2>Customer Acquisition Cost (CAC) Calculator</h2>
        <div class="input-group">
            <label for="expenses">Total Marketing and Sales Expenses ($):</label>
            <input type="number" id="expenses" step="0.01" required>
        </div>
        <div class="input-group">
            <label for="customers">Number of New Customers Acquired:</label>
            <input type="number" id="customers" step="1" required>
        </div>
        <button onclick="calculateCAC()">Calculate</button>
        <div class="result" id="result"></div>
    </div>

    <script>
        function calculateCAC() {
            var expenses = document.getElementById("expenses").value;
            var customers = document.getElementById("customers").value;

            var cac = expenses / customers;

            document.getElementById("result").textContent = "Customer Acquisition Cost (CAC): $" + cac.toFixed(2);
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


In this calculator, just input the total marketing and sales expenses (in dollars) and the number of new customers acquired. Then click the "Calculate" button to display the Customer Acquisition Cost (CAC) in dollars.

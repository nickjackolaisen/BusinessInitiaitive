---
title: "How to Calculate Customer Lifetime Value (LTV)"
layout: post
date: 2023-10-15
author: jack_nicholaisen
summary: "Your company's LTV measures the total amount a customer is expected to spend on your products or services over their lifetime." 
permalink: /tools/calculator/customer-lifetime-value/
---

LTV is an important metric because it helps businesses understand the overall value of their customer base and make decisions about how much to invest in retaining customers.

<body>
    <div class="calculator" style="text-align:center">
        <h2>Customer Lifetime Value (LTV) Calculator</h2>
        <div class="input-group">
            <label for="avgPurchaseValue">Average Purchase Value ($):</label>
            <input type="number" id="avgPurchaseValue" step="0.01" required>
        </div>
        <div class="input-group">
            <label for="avgPurchaseFrequency">Average Purchase Frequency per Year:</label>
            <input type="number" id="avgPurchaseFrequency" step="0.1" required>
        </div>
        <div class="input-group">
            <label for="avgCustomerLifespan">Average Customer Lifespan (Years):</label>
            <input type="number" id="avgCustomerLifespan" step="0.1" required>
        </div>
        <div class="input-group">
            <label for="avgGrossMargin">Average Gross Margin (%):</label>
            <input type="number" id="avgGrossMargin" step="0.1" required>
        </div>
        <button onclick="calculateLTV()">Calculate</button>
        <div class="result" id="result"></div>
    </div>

    <script>
        function calculateLTV() {
            var avgPurchaseValue = document.getElementById("avgPurchaseValue").value;
            var avgPurchaseFrequency = document.getElementById("avgPurchaseFrequency").value;
            var avgCustomerLifespan = document.getElementById("avgCustomerLifespan").value;
            var avgGrossMargin = document.getElementById("avgGrossMargin").value;

            var ltv = (avgPurchaseValue * avgPurchaseFrequency * avgCustomerLifespan * (avgGrossMargin / 100));

            document.getElementById("result").textContent = "Customer Lifetime Value (LTV): $" + ltv.toFixed(2);
        }
    </script>
</body>

<style>
        body {
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




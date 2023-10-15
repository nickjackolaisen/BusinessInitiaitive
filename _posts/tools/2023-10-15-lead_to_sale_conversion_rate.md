---
title: "How to Calculate Your Lead-to-Sale Conversion Rate"
layout: post
date: 2023-10-15
author: jack_nicholaisen
summary: "Use this calculator to find out where your business can improve it's sales procedures." 
permalink: /tools/calculator/lead-to-sale-conversion-rate/
---

This represents the percentage of leads that result in a sale. 

It is an important metric because it helps businesses understand how effective their sales processes are and where they can make improvements.

<body>
    <div class="calculator">
        <h2>Lead-to-Sale Conversion Rate Calculator</h2>
        <div class="input-group">
            <label for="leads">Number of Leads:</label>
            <input type="number" id="leads" step="1" required>
        </div>
        <div class="input-group">
            <label for="sales">Number of Sales:</label>
            <input type="number" id="sales" step="1" required>
        </div>
        <button onclick="calculateConversionRate()">Calculate</button>
        <div class="result" id="result"></div>
    </div>

    <script>
        function calculateConversionRate() {
            var leads = document.getElementById("leads").value;
            var sales = document.getElementById("sales").value;

            var conversionRate = (sales / leads) * 100;

            document.getElementById("result").textContent = "Lead-to-Sale Conversion Rate: " + conversionRate.toFixed(2) + "%";
        }
    </script>
</body>


<style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
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

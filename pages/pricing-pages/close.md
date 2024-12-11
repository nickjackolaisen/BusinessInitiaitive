---
title: "Close a Business"
layout: page
permalink: /close/
---

## Close a Business

<div class="form-container" style="margin: 0 auto; max-width: 400px; padding: 20px; background-color: #ffffff; border-radius: 10px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
    <form id="business-form" style="display: flex; flex-direction: column; align-items: center;">
        <select id="state" name="state" style="width: 100%; padding: 12px; margin-bottom: 20px; border: 1px solid #ced4da; border-radius: 5px; box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);">
            <option value="" disabled selected>Select State</option>
            <option value="new-york">New York</option>
            <!-- Add more states as needed -->
        </select>
        <select id="entity" name="entity" style="width: 100%; padding: 12px; margin-bottom: 20px; border: 1px solid #ced4da; border-radius: 5px; box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);">
            <option value="" disabled selected>Select Entity</option>
            <!-- Options will be populated dynamically -->
        </select>
        <button type="submit" style="padding: 10px 20px; background-color: #007BFF; color: white; border: none; border-radius: 5px; cursor: pointer; box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">Show Options</button>
    </form>
</div>

<div id="pricing-cards" style="display: none;">
    <!-- Pricing cards will be dynamically inserted here based on form selection -->
</div>

<script>
    document.getElementById('state').addEventListener('change', function() {
        const state = this.value;
        const entitySelect = document.getElementById('entity');
        entitySelect.innerHTML = '<option value="" disabled selected>Select Entity</option>'; // Reset options

        fetch(`/data/products/${state}.json`)
            .then(response => response.json())
            .then(data => {
                for (const [entity, details] of Object.entries(data)) {
                    if (details.category.includes("Closure")) {
                        const option = document.createElement('option');
                        option.value = entity;
                        option.textContent = entity.charAt(0).toUpperCase() + entity.slice(1);
                        entitySelect.appendChild(option);
                    }
                }
            })
            .catch(error => {
                console.error('Error fetching entities:', error);
            });
    });

    document.getElementById('business-form').addEventListener('submit', function(event) {
        event.preventDefault();
        
        const state = document.getElementById('state').value;
        const entity = document.getElementById('entity').value;
        const pricingCardsContainer = document.getElementById('pricing-cards');

        fetch(`/data/products/${state}.json`)
            .then(response => response.json())
            .then(data => {
                const pricingInfo = data[entity];
                if (pricingInfo && pricingInfo.category.includes("Closure")) {
                    pricingCardsContainer.innerHTML = `
                        <div class="pricing-card" style="padding: 20px; background-color: #fff; border-radius: 10px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); margin-top: 20px;">
                            <h3>${entity.toUpperCase()} Pricing</h3>
                            <p>Price: $${pricingInfo.price}</p>
                            <p>Description: ${pricingInfo.description}</p>
                        </div>
                    `;
                    pricingCardsContainer.style.display = 'block';
                } else {
                    pricingCardsContainer.innerHTML = '<p>No pricing information available for the selected entity.</p>';
                    pricingCardsContainer.style.display = 'block';
                }
            })
            .catch(error => {
                console.error('Error fetching pricing data:', error);
                pricingCardsContainer.innerHTML = '<p>Error loading pricing information. Please try again later.</p>';
                pricingCardsContainer.style.display = 'block';
            });
    });
</script>
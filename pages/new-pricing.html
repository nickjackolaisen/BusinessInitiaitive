---
layout: page
title: New Pricing
permalink: /new-pricing/
tags: services, offerings, registration, filing, call, meeting, schedule, message, registration, business, llc, sole-proprietorship, partnership, corporation
---

<link rel="stylesheet" href="{{ '/assets/css/pricing.css' | relative_url }}">

<!-- Dropdown Menus -->
<div class="dropdown-container">
    <select id="state-select">
        <option value="">Select State</option>
        <option value="new-jersey">New Jersey</option>
        <option value="new-york">New York</option>
        <!-- Add more states as needed -->
    </select>

    <select id="entity-select">
        <option value="">Select Entity</option>
        <option value="corporation">Corporation</option>
        <option value="llc">LLC</option>
        <!-- Add more entities as needed -->
    </select>

    <select id="sort-select">
        <option value="">Sort by Price</option>
        <option value="asc">Ascending</option>
        <option value="desc">Descending</option>
    </select>
</div>

<div class="pricing-container" id="pricing-container">
    <!-- Pricing cards will be dynamically populated here -->
</div>

<script>
    function loadStateData(state, callback) {
        fetch(`/data/Services/${state}/services.json`)
            .then(response => response.json())
            .then(data => callback(data))
            .catch(error => console.error('Error loading state data:', error));
    }

    function updatePricingCards() {
        const state = document.getElementById('state-select').value;
        const entity = document.getElementById('entity-select').value;
        const sortOrder = document.getElementById('sort-select').value;
        const pricingContainer = document.getElementById('pricing-container');
        pricingContainer.innerHTML = ''; // Clear existing cards

        if (state) {
            loadStateData(state, data => {
                let items = [];

                if (entity && data[entity]) {
                    // Both state and entity are selected
                    items = data[entity];
                } else {
                    // Only state is selected
                    Object.keys(data).forEach(entityKey => {
                        items = items.concat(data[entityKey]);
                    });
                }

                // Sort items by price
                if (sortOrder) {
                    items.sort((a, b) => {
                        const priceA = parseFloat(a.fee.replace('$', ''));
                        const priceB = parseFloat(b.fee.replace('$', ''));
                        return sortOrder === 'asc' ? priceA - priceB : priceB - priceA;
                    });
                }

                items.forEach(item => {
                    createCard(item, state);
                });
            });
        }
    }

    function createCard(item, state) {
        const card = document.createElement('div');
        card.className = 'pricing-card';
        const serviceSlug = item.service.toLowerCase().replace(/\s+/g, '-');
        const imgSrc = `/images/services/${state}/${serviceSlug}.jpg`;
        card.innerHTML = `
            <a href="${item.link}">
                <img src="${imgSrc}" alt="${item.service} icon" class="service-icon">
            </a>
            <h2>${item.service}</h2>
            <div class="price">${item.fee}</div>
            <p>${item.description}</p>
            <a href="${item.link}" class="cta-button">Purchase</a>
        `;
        document.getElementById('pricing-container').appendChild(card);
    }

    document.getElementById('state-select').addEventListener('change', updatePricingCards);
    document.getElementById('entity-select').addEventListener('change', updatePricingCards);
    document.getElementById('sort-select').addEventListener('change', updatePricingCards);
</script> 

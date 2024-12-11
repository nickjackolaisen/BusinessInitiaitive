---
layout: page
title: Registration
permalink: /registration/
tags: services, registration, business, llc, corporation
---

<!-- Dropdown Menus -->
<div class="form-container">
    <select id="state-select">
        <option value="">Select State</option>
        <option value="new-york">New York</option>
        <!-- Add more states as needed -->
    </select>
    <select id="entity-select">
        <option value="">Select Entity</option>
        <!-- Add more entities as needed -->
    </select>
    <select id="sort-select">
        <option value="">Sort by</option>
        <option value="price-asc">Price: Low to High</option>
        <option value="price-desc">Price: High to Low</option>
        <option value="name-asc">Name: A to Z</option>
        <option value="name-desc">Name: Z to A</option>
    </select>
</div>

<div class="pricing-container" id="pricing-container">
    <!-- Pricing cards will be dynamically populated here -->
</div>

<script>
    function loadProducts(state, entity) {
        fetch(`/data/products/${state}.json`)
            .then(response => response.json())
            .then(products => {
                let filteredProducts = products.filter(product => product.category === 'Registration');
                
                if (entity) {
                    filteredProducts = filteredProducts.filter(product => product.entity.toLowerCase() === entity.toLowerCase());
                }
                
                sortProducts(filteredProducts);
                document.getElementById('pricing-container').innerHTML = ''; // Clear existing cards
                filteredProducts.forEach(createCard);
            })
            .catch(error => console.error('Error loading products:', error));
    }

    function sortProducts(products) {
        const sortOption = document.getElementById('sort-select').value;
        switch (sortOption) {
            case 'price-asc':
                products.sort((a, b) => parseFloat(a.price.slice(1)) - parseFloat(b.price.slice(1)));
                break;
            case 'price-desc':
                products.sort((a, b) => parseFloat(b.price.slice(1)) - parseFloat(a.price.slice(1)));
                break;
            case 'name-asc':
                products.sort((a, b) => a.service.localeCompare(b.service));
                break;
            case 'name-desc':
                products.sort((a, b) => b.service.localeCompare(a.service));
                break;
        }
    }

    function createCard(item) {
        const card = document.createElement('div');
        card.className = 'pricing-card';
        card.innerHTML = `
            <div class="entity-box">${item.entity}</div>
            <img src="${item.image}" alt="${item.service} icon" class="service-icon">
            <h2>${item.service}</h2>
            <p>${item.description}</p>
            <div class="price">${item.price}</div>
            <a href="${item.link}" class="cta-button">${item.ctaText}</a>
        `;
        document.getElementById('pricing-container').appendChild(card);
    }

    function populateEntityDropdown(state) {
        fetch(`/data/products/${state}.json`)
            .then(response => response.json())
            .then(products => {
                const entitySelect = document.getElementById('entity-select');
                entitySelect.innerHTML = '<option value="">Select Entity</option>'; // Clear existing options
                const entities = [...new Set(products.map(product => product.entity))];
                entities.forEach(entity => {
                    const option = document.createElement('option');
                    option.value = entity.toLowerCase();
                    option.textContent = entity;
                    entitySelect.appendChild(option);
                });
            })
            .catch(error => console.error('Error loading entities:', error));
    }

    document.getElementById('state-select').addEventListener('change', function() {
        const state = this.value;
        if (state) {
            populateEntityDropdown(state);
            const entity = document.getElementById('entity-select').value;
            loadProducts(state, entity);
        }
    });

    document.getElementById('entity-select').addEventListener('change', function() {
        const entity = this.value;
        const state = document.getElementById('state-select').value;
        if (state) {
            loadProducts(state, entity);
        }
    });

    document.getElementById('sort-select').addEventListener('change', function() {
        const state = document.getElementById('state-select').value;
        const entity = document.getElementById('entity-select').value;
        if (state) {
            loadProducts(state, entity);
        }
    });
</script>

<style>
    .pricing-card {
        background-color: #fff; /* Sets the background color of the card to white */
        border: 1px solid #e0e0e0; /* Adds a light gray border around the card */
        border-radius: 5px; /* Rounds the corners of the card */
        padding: 20px; /* Adds padding inside the card */
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Adds a subtle shadow for depth */
        text-align: center; /* Centers the text inside the card */
        margin: 20px; /* Adds margin around the card */
        max-width: 300px; /* Sets the maximum width of the card */
        flex: 1 1 calc(33.333% - 40px); /* Allows the card to flex and take up a third of the container width minus margins */
    }

    .pricing-container {
        display: flex; /* Enables flexbox layout for the container */
        flex-wrap: wrap; /* Allows items to wrap onto the next line */
        justify-content: space-between; /* Distributes space between items */
    }

    .entity-box {
        background-color: #0063A0; /* Sets the background color of the entity box */
        color: #fff; /* Sets the text color to white */
        padding: 5px 10px; /* Adds padding inside the entity box */
        border-radius: 3px; /* Rounds the corners of the entity box */
        display: inline-block; /* Displays the entity box inline */
        margin-bottom: 10px; /* Adds margin below the entity box */
    }

    .service-icon {
        max-width: 100%; /* Ensures the icon does not exceed the card width */
        height: auto; /* Maintains the aspect ratio of the icon */
        margin-bottom: 15px; /* Adds margin below the icon */
    }

    .cta-button {
        display: inline-block; /* Displays the button inline */
        background-color: #629E2D; /* Sets the background color of the button */
        color: #fff; /* Sets the text color to white */
        padding: 10px 20px; /* Adds padding inside the button */
        border-radius: 5px; /* Rounds the corners of the button */
        text-decoration: none; /* Removes underline from the text */
        margin-top: 15px; /* Adds margin above the button */
        transition: background-color 0.3s; /* Adds a transition effect for background color change */
    }

    .cta-button:hover {
        background-color: #4a7d1f; /* Changes the background color on hover */
    }

    a.cta-button {
        color: inherit; /* Inherits the color from the parent */
        background-image: none; /* Removes any background image */
        border-bottom: 0; /* Removes the bottom border */
        text-decoration: none; /* Ensures no underline on the text */
    }
</style>

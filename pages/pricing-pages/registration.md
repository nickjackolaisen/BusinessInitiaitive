---
layout: page
title: Registration
permalink: /registration/
tags: services, registration, business, llc, corporation
---

<link rel="stylesheet" href="{{ '/assets/css/pricing.css' | relative_url }}">

<style>
/* Localized Button Styling */
.button {
    flex: 1 1 calc(25% - 20px);
    max-width: 200px;
    padding: 10px 20px;
    background-image: linear-gradient(to bottom, rgba(0, 99, 160, 0.05), rgba(98, 158, 45, 0.05)); /* Faded gradient */
    color: #add8e6; /* Light blue text */
    text-decoration: none; /* Remove underline */
    border-radius: 5px;
    border: 1px solid #e0e0e0;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    text-align: center;
    transition: color 0.3s ease, box-shadow 0.3s ease;
}

.button:hover {
    color: #0056b3; /* Dark blue text on hover */
    box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2); /* Enhanced shadow on hover */
}
</style>

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
            <a href="${item.link}" class="image-link-thumbnail">
                <img src="${item.image}" alt="${item.service} icon" class="service-icon">
            </a>
            <h2>${item.service}</h2>
            <div class="price">${item.price}</div>
            <p>${item.description}</p>
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

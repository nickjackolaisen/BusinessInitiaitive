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
        background-color: #fff;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        padding: 20px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        text-align: center;
        margin: 20px;
        max-width: 300px;
    }

    .entity-box {
        background-color: #0063A0;
        color: #fff;
        padding: 5px 10px;
        border-radius: 3px;
        display: inline-block;
        margin-bottom: 10px;
    }

    .service-icon {
        max-width: 100%;
        height: auto;
        margin-bottom: 15px;
    }

    .cta-button {
        display: inline-block;
        background-color: #629E2D;
        color: #fff;
        padding: 10px 20px;
        border-radius: 5px;
        text-decoration: none;
        margin-top: 15px;
        transition: background-color 0.3s;
    }

    .cta-button:hover {
        background-color: #4a7d1f;
    }

    a.cta-button {
        color: inherit;
        background-image: none;
        border-bottom: 0;
        text-decoration: none;
    }
</style>
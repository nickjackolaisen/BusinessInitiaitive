---
layout: page
title: Registration
permalink: /register/
tags: services, registration, business, llc, corporation
---

<link rel="stylesheet" href="{{ '/assets/css/pricing.css' | relative_url }}">

<div class="filter-box">
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
            <a href="${item.link}" class="service-icon">
                <img src="${item.image}" alt="${item.service} icon">
            </a>
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


<!-- Calendly inline widget begin -->
<div class="calendly-inline-widget" data-url="https://calendly.com/businessinitiative/30-minute-consultation-call" style="min-width:320px;height:700px;"></div>
<script type="text/javascript" src="https://assets.calendly.com/assets/external/widget.js" async></script>
<!-- Calendly inline widget end -->


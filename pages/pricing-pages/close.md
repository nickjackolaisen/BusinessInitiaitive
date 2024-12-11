---
title: "Close a Business"
layout: page
permalink: /close/
tags: services, closure, business
---

<link rel="stylesheet" href="{{ '/assets/css/pricing.css' | relative_url }}">

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
                let filteredProducts = products.filter(product => product.category === 'Closure');
                
                if (entity) {
                    filteredProducts = filteredProducts.filter(product => product.entity.toLowerCase() === entity.toLowerCase());
                }
                
                sortProducts(filteredProducts);
                document.getElementById('pricing-container').innerHTML = ''; // Clear existing cards
                filteredProducts.forEach(createCard);
            })
            .catch(error => console.error('Error loading products:', error));
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

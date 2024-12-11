---
title: "Register a Business"
layout: page
permalink: /registration/
---

## Register a Business

<div class="form-container">
    <form id="business-form">
        <select id="state" name="state">
            <option value="" disabled selected>Select State</option>
            <option value="new-york">New York</option>
            <!-- Add more states as needed -->
        </select>
        <select id="entity" name="entity">
            <option value="" disabled selected>Select Entity</option>
            <!-- Options will be populated dynamically -->
        </select>
        <button type="submit">Show Options</button>
    </form>
</div>

<div id="pricing-cards-container" style="display: none;">
    <div>
        <select id="sort-options">
            <option value="default" selected>Sort By</option>
            <option value="price-asc">Price: Low to High</option>
            <option value="price-desc">Price: High to Low</option>
            <option value="name-asc">Name: A to Z</option>
            <option value="name-desc">Name: Z to A</option>
        </select>
    </div>
    <div id="pricing-cards" class="pricing-container">
        <!-- Pricing cards will be dynamically inserted here -->
    </div>
</div>

<script>
    document.getElementById('state').addEventListener('change', function() {
        const state = this.value;
        const entitySelect = document.getElementById('entity');
        entitySelect.innerHTML = '<option value="" disabled selected>Select Entity</option>';

        fetch(`/data/products/${state}.json`)
            .then(response => response.json())
            .then(data => {
                const entities = [...new Set(data.filter(service => service.category.includes("Registration")).map(service => service.entity))];
                entities.forEach(entity => {
                    const option = document.createElement('option');
                    option.value = entity;
                    option.textContent = entity.charAt(0).toUpperCase() + entity.slice(1);
                    entitySelect.appendChild(option);
                });
            })
            .catch(error => console.error('Error fetching entities:', error));
    });

    document.getElementById('business-form').addEventListener('submit', function(event) {
        event.preventDefault();
        
        const state = document.getElementById('state').value;
        const entity = document.getElementById('entity').value;
        const pricingCardsContainer = document.getElementById('pricing-cards');
        const pricingCardsWrapper = document.getElementById('pricing-cards-container');

        fetch(`/data/products/${state}.json`)
            .then(response => response.json())
            .then(data => {
                const filteredServices = data.filter(service => 
                    service.entity.toLowerCase() === entity.toLowerCase() && 
                    service.category.includes("Registration")
                );

                if (filteredServices.length > 0) {
                    renderPricingCards(filteredServices);
                    pricingCardsWrapper.style.display = 'block';
                } else {
                    pricingCardsContainer.innerHTML = '<p>No pricing information available for the selected entity.</p>';
                    pricingCardsWrapper.style.display = 'block';
                }
            })
            .catch(error => {
                console.error('Error fetching pricing data:', error);
                pricingCardsContainer.innerHTML = '<p>Error loading pricing information. Please try again later.</p>';
                pricingCardsWrapper.style.display = 'block';
            });
    });

    document.getElementById('sort-options').addEventListener('change', function() {
        const sortBy = this.value;
        const pricingCardsContainer = document.getElementById('pricing-cards');
        const services = Array.from(pricingCardsContainer.children).map(card => ({
            element: card,
            price: parseFloat(card.querySelector('.price').textContent.replace('$', '')),
            name: card.querySelector('h3').textContent
        }));

        if (sortBy === 'price-asc') {
            services.sort((a, b) => a.price - b.price);
        } else if (sortBy === 'price-desc') {
            services.sort((a, b) => b.price - a.price);
        } else if (sortBy === 'name-asc') {
            services.sort((a, b) => a.name.localeCompare(b.name));
        } else if (sortBy === 'name-desc') {
            services.sort((a, b) => b.name.localeCompare(a.name));
        }

        pricingCardsContainer.innerHTML = '';
        services.forEach(service => pricingCardsContainer.appendChild(service.element));
    });

    function renderPricingCards(services) {
        const pricingCardsContainer = document.getElementById('pricing-cards');
        pricingCardsContainer.innerHTML = services.map(service => `
            <div class="pricing-card">
                <img src="${service.image}" alt="${service.service}" class="service-icon">
                <h3>${service.service}</h3>
                <p>${service.description}</p>
                <p class="price">${service.price}</p>
                <a href="${service.link}" class="cta-button">${service.ctaText}</a>
            </div>
        `).join('');
    }
</script>

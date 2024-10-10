<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Amazon Clone</title>
    <style>
        /* Basic reset */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
        }

        /* Header */
        header {
            background-color: #232f3e;
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo h1 {
            font-size: 24px;
        }

        .search-bar input {
            padding: 8px;
            width: 250px;
            border: none;
            border-radius: 4px;
        }

        .search-bar button {
            padding: 8px 12px;
            border: none;
            background-color: #f90;
            color: white;
            border-radius: 4px;
            cursor: pointer;
        }

        .cart a {
            color: white;
            text-decoration: none;
            font-size: 18px;
        }

        /* Main content */
        main {
            padding: 20px;
        }

        /* Product List */
        .product-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 20px;
        }

        .product {
            background-color: white;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            text-align: center;
        }

        .product img {
            max-width: 100%;
            height: auto;
            border-radius: 5px;
        }

        .product h3 {
            margin: 10px 0;
        }

        .product p {
            color: #f90;
            font-weight: bold;
        }

        .product button {
            padding: 10px;
            background-color: #f90;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        .product button:hover {
            background-color: #e68a00;
        }

        /* Cart Page */
        .cart-items {
            width: 80%;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 8px;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .cart-item img {
            max-width: 100px;
            height: auto;
            border-radius: 8px;
        }

        .cart-item div {
            flex-grow: 1;
            margin-left: 20px;
        }

        .cart-item input {
            padding: 5px;
            width: 50px;
        }

        .cart-item button {
            padding: 10px;
            background-color: #f90;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        .cart-item button:hover {
            background-color: #e68a00;
        }

        .cart-total {
            text-align: right;
            margin-top: 20px;
        }

        .cart-total button {
            padding: 10px 20px;
            background-color: #f90;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        .cart-total button:hover {
            background-color: #e68a00;
        }

        /* Login Form */
        .login-form {
            width: 300px;
            margin: 50px auto;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        .login-form h2 {
            margin-bottom: 20px;
        }

        .login-form input {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }

        .login-form button {
            width: 100%;
            padding: 10px;
            background-color: #f90;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <!-- Header Section -->
    <header>
        <div class="logo">
            <h1>Amazon Clone</h1>
        </div>
        <div class="search-bar">
            <input type="text" id="search" placeholder="Search products...">
            <button onclick="searchProduct()">Search</button>
        </div>
        <div class="cart">
            <a href="#" id="cart-link">🛒 Cart (0)</a>
        </div>
    </header>

    <!-- Product Listing -->
    <main>
        <section class="product-list" id="product-list">
            <div class="product">
                <img src="https://via.placeholder.com/150" alt="Product 1">
                <h3>Product 1</h3>
                <p>$49.99</p>
                <button class="add-to-cart" data-name="Product 1" data-price="49.99">Add to Cart</button>
            </div>
            <div class="product">
                <img src="https://via.placeholder.com/150" alt="Product 2">
                <h3>Product 2</h3>
                <p>$99.99</p>
                <button class="add-to-cart" data-name="Product 2" data-price="99.99">Add to Cart</button>
            </div>
            <div class="product">
                <img src="https://via.placeholder.com/150" alt="Product 3">
                <h3>Product 3</h3>
                <p>$29.99</p>
                <button class="add-to-cart" data-name="Product 3" data-price="29.99">Add to Cart</button>
            </div>
        </section>

        <!-- Shopping Cart -->
        <section class="cart-items" id="cart-section" style="display:none;">
            <h2>Your Cart</h2>
            <div id="cart-items-container"></div>
            <div class="cart-total">
                <h3>Total: $<span id="cart-total">0.00</span></h3>
                <button onclick="checkout()">Proceed to Checkout</button>
            </div>
        </section>

        <!-- Login Form -->
        <section class="login-form" id="login-form" style="display:none;">
            <h2>Login</h2>
            <form id="loginForm" onsubmit="handleLogin(event)">
                <input type="email" id="email" placeholder="Email" required>
                <input type="password" id="password" placeholder="Password" required>
                <button type="submit">Login</button>
            </form>
        </section>
    </main>

    <script>
        // Shopping Cart Logic
        let cart = [];
        const cartLink = document.getElementById('cart-link');
        const cartSection = document.getElementById('cart-section');
        const cartItemsContainer = document.getElementById('cart-items-container');
        const cartTotalElem = document.getElementById('cart-total');

        // Add to Cart button event
        document.querySelectorAll('.add-to-cart').forEach(button => {
            button.addEventListener('click', function () {
                const productName = this.dataset.name;
                const productPrice = parseFloat(this.dataset.price);

                const product = { name: productName, price: productPrice, quantity: 1 };
                addToCart(product);
                updateCartCount();
                alert(`${productName} has been added to your cart.`);
            });
        });

        function addToCart(product) {
            const existingProduct = cart.find(item => item.name === product.name);
            if (existingProduct) {
                existingProduct.quantity++;
            } else {
                cart.push(product);
            }
            renderCart();
        }

        function renderCart() {
            cartItemsContainer.innerHTML = '';
            let total = 0;
            cart.forEach(item => {
                const cartItem = document.createElement('div');
                cartItem.classList.add('cart-item');
                cartItem.innerHTML = `
                    <img src="https://via.placeholder.com/100" alt="${item.name}">
                    <div>
                        <h3>${item.name}</h3>
                        <p>$${item.price} x ${item.quantity}</p>
                    </div>
                    <button class="remove" data-name="${item.name}">Remove</button>
                `;
                cartItemsContainer.appendChild(cartItem);

                total += item.price * item.quantity;

                cartItem.querySelector('.remove').addEventListener('click', function () {
                    removeFromCart(item.name);
                });
            });
            cartTotalElem.textContent = total.toFixed(2);
            cartSection.style.display = cart.length > 0 ? 'block' : 'none';
        }

        function removeFromCart(productName) {
            cart = cart.filter(item => item.name !== productName);
            renderCart();
            updateCartCount();
        }

        function updateCartCount() {
            cartLink.textContent = `🛒 Cart (${cart.length})`;
        }

        // Checkout Functionality
        function checkout() {
            alert('Proceeding to checkout');
            // Add checkout logic here (e.g., redirect to payment)
        }

        // Login Logic
        function handleLogin(event) {
            event.preventDefault();
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;

            // Simple login validation (dummy logic)
            if (email === "user@example.com" && password === "password") {
                alert('Login successful');
                document.getElementById('login-form').style.display = 'none';
                // Optionally redirect to another page
            } else {
                alert('Invalid credentials');
            }
        }

        // Function to search for products (placeholder)
        function searchProduct() {
            const searchQuery = document.getElementById('search').value.toLowerCase();
            const products = document.querySelectorAll('.product');
            products.forEach(product => {
                const productName = product.querySelector('h3').innerText.toLowerCase();
                product.style.display = productName.includes(searchQuery) ? 'block' : 'none';
            });
        }
    </script>
</body>
</html>

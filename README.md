<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Restaurent website</title>
    <style>
        /* Reset default styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
    
        body {
            background: #fefefe;
            padding-bottom: 100px;
        }
    
        nav {
            background-color: #ff4c4c;
            padding: 15px 30px;
            color: white;
            display: flex;
            justify-content: flex-end;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 999;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
    
        .cart-container .cart {
            background-color: #fff;
            color: #ff4c4c;
            padding: 10px 20px;
            border: none;
            border-radius: 20px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s ease;
        }
    
        .cart-container .cart:hover {
            background-color: #ffb3b3;
        }
    
        .heading {
            text-align: center;
            margin-top: 30px;
            font-size: 28px;
            color: #333;
        }
    
        .menu {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 30px;
            padding: 40px;
        }
    
        .cards {
            background-color: white;
            border-radius: 16px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
            overflow: hidden;
            text-align: center;
            transition: transform 0.3s ease;
        }
    
        .cards:hover {
            transform: translateY(-5px);
        }
    
        .cards img {
            width: 100%;
            height: 200px;
            object-fit: cover;
        }
    
        .cards h3 {
            margin: 15px 0;
            font-size: 20px;
            color: #555;
        }
    
        .add-button {
            background-color: #ff4c4c;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            margin-bottom: 20px;
            transition: 0.3s ease;
        }
    
        .add-button:hover {
            background-color: #e03b3b;
        }
    
        .popup {
            display: none;
            position: fixed;
            top: 100px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #fff;
            width: 90%;
            max-width: 500px;
            border-radius: 12px;
            padding: 25px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.3);
            z-index: 1000;
        }
    
        .popup h2, .popup h3 {
            text-align: center;
            color: #333;
        }
    
        #order-list {
            margin-top: 15px;
            max-height: 200px;
            overflow-y: auto;
        }
    
        #order-list div {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            border-bottom: 1px solid #eee;
            padding-bottom: 5px;
            font-size: 16px;
        }
    
        .popup button {
            margin: 10px 5px 0;
            padding: 10px 15px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
        }
    
        .popup button:first-of-type {
            background-color: #4caf50;
            color: white;
        }
    
        .popup button:last-of-type {
            background-color: #ccc;
            color: #333;
        }
    
        /* Responsive adjustments */
        @media (max-width: 600px) {
            .menu {
                padding: 20px;
            }
    
            .cards img {
                height: 150px;
            }
        }
    </style>
    
</head>
<body>
    <nav>
    <div class="cart-container">
        <button class="cart" onclick="showPopup()">🛒 Cart (<span id="cart-count">0</span>)</button>
    </div>
</nav>
     <div class="heading">
        <h1>Our menu</h1>
        </div>
        <div class="menu" id="menu">
            <div class="item">
                <div class="cards">
                    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcToNb4qLVc8li_M2NHBfEl14DaVpUCmplY5lg&s">
                    <h3>Chocolate ice-cream</h3>
                    <button class="add-button" onclick="addToCart('Chocolate ice-cream',100)">Add to cart</button>
                </div>
            </div>
            <div class="item">
                <div class="cards">
                    <img src="https://www.theroastedroot.net/wp-content/uploads/2023/06/dairy-free-vanilla-ice-cream-8-500x500.jpg">
                    <h3>Vanilla ice-cream</h3>
                    <button class="add-button" onclick="addToCart('Vanilla ice-cream',90)">Add to cart</button>
                </div>
            </div>
            <div class="item">
                <div class="cards">
                    <img src="https://www.chewoutloud.com/wp-content/uploads/2024/04/Strawberry-Ice-Cream-in-Bowl.jpg">
                    <h3>strawberry ice-cream</h3>
                    <button class="add-button" onclick="addToCart('strawberry ice-cream',80)">Add to cart</button>
                </div>
            </div>
            <div class="item">
                <div class="cards">
                    <img src="https://5.imimg.com/data5/CK/YK/BS/SELLER-2565878/butterscotch-ice-cream-500x500.jpg">
                    <h3>Butter-scotch ice-cream</h3>
                    <button class="add-button" onclick="addToCart('Butter-scotch ice-cream',80)">Add to cart</button>
                </div>
            </div>
        </div>
        <div class="popup" id="popup">
            <h2>Order Summary</h2>
            <div id="order-list"></div>
            <h3 style="margin-top: 10px;">Total: ₹<span id="total-price">0</span></h3>
            <button onclick="placeOrder()">placeOrder</button>
            <button onclick="hidePopup()">close</button>
        </div>
        <script>
            // Cart data will be stored here
            let cart = [];
        
            // Function to add item to cart
            function addToCart(itemName, price) {
                cart.push({ name: itemName, price: price });
                updateCartCount();
            }
        
            // Function to update cart count in the navbar
            function updateCartCount() {
                document.getElementById('cart-count').textContent = cart.length;
            }
        
            // Show the popup with order summary
            function showPopup() {
                const popup = document.getElementById('popup');
                const orderList = document.getElementById('order-list');
                const totalPriceSpan = document.getElementById('total-price');
        
                // Clear previous items
                orderList.innerHTML = '';
        
                // Add current items
                let total = 0;
                cart.forEach((item, index) => {
                    total += item.price;
                    const itemElement = document.createElement('div');
                    itemElement.innerHTML = `${item.name} - ₹${item.price} <button onclick="removeItem(${index})">❌</button>`;
                    orderList.appendChild(itemElement);
                });
        
                totalPriceSpan.textContent = total;
                popup.style.display = 'block';
            }
        
            // Close the popup
            function hidePopup() {
                document.getElementById('popup').style.display = 'none';
            }
        
            // Remove item from cart
            function removeItem(index) {
                cart.splice(index, 1);
                updateCartCount();
                showPopup(); // Refresh the popup content
            }
        
            // Place order function
            function placeOrder() {
                if (cart.length === 0) {
                    alert("Your cart is empty!");
                    return;
                }
                alert("Order placed successfully!");
                cart = [];
                updateCartCount();
                hidePopup();
            }
        </script>
</body>
</html>

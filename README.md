# Ryeezy
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ryzy - Shop</title>
  <!-- Font Awesome for icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    /* Global Reset */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }
    body {
      background: white;
    }
    /* Navbar */
    .navbar {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      background: white;
      padding: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    /* Hamburger Menu (Top Left) */
    .menu-toggle {
      position: absolute;
      left: 20px;
      cursor: pointer;
      display: flex;
      flex-direction: column;
      gap: 2px; /* Reduced gap for closer bars */
    }
    .menu-toggle .bar {
      width: 25px;
      height: 3px;
      background: #333;
      transition: all 0.3s ease;
    }
    /* Transform to an "X" when active (you may adjust the transform to resemble an arrow if desired) */
    .menu-toggle.active .bar:nth-child(1) {
      transform: translateY(3px) rotate(45deg);
    }
    .menu-toggle.active .bar:nth-child(2) {
      transform: translateY(-3px) rotate(-45deg);
    }
    /* Logo (Center) */
    .logo {
      font-size: 24px;
      font-weight: bold;
      letter-spacing: 2px;
    }
    /* Basket Icon (Top Right) */
    .basket-icon {
      position: absolute;
      right: 20px;
      cursor: pointer;
    }
    .basket-icon i {
      font-size: 24px;
      color: #333;
    }
    /* Dropdown Menu from Hamburger */
    .dropdown-menu {
      position: absolute;
      top: 60px;
      left: 20px;
      background: white;
      border: 1px solid #ddd;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      overflow: hidden;
      max-height: 0;
      transition: max-height 0.3s ease;
      width: 180px;
      z-index: 999;
    }
    .dropdown-menu.active {
      max-height: 200px;
    }
    .dropdown-menu a {
      display: block;
      padding: 10px 15px;
      text-decoration: none;
      color: #333;
      border-bottom: 1px solid #eee;
      transition: background 0.3s ease;
    }
    .dropdown-menu a:hover {
      background: #f4f4f4;
    }
    /* Products Container & Grid */
    .products-container {
      margin-top: 100px; /* space below navbar */
      padding: 20px;
    }
    .product-grid {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 10px;
      margin-bottom: 40px;
    }
    .product-item {
      cursor: pointer;
      opacity: 0;
      transform: translateY(20px);
      transition: opacity 0.5s ease, transform 0.5s ease;
      text-align: center;
    }
    .product-item.visible {
      opacity: 1;
      transform: translateY(0);
    }
    .product-item img {
      width: 100%;
      display: block;
    }
    .product-caption {
      font-size: 14px;
      margin-top: 5px;
      color: #333;
    }
    /* Product Modal (Zoom) */
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.8);
      justify-content: center;
      align-items: center;
      flex-direction: column;
      z-index: 2000;
    }
    .modal-content {
      max-width: 80%;
      max-height: 80%;
      margin-bottom: 20px;
    }
    .modal-info {
      background: white;
      padding: 20px;
      text-align: center;
      width: 300px;
    }
    .modal-info h3 {
      margin-bottom: 10px;
    }
    .modal-info p {
      margin-bottom: 15px;
      font-size: 16px;
    }
    .modal-info button {
      background: #333;
      color: white;
      border: none;
      padding: 10px 20px;
      cursor: pointer;
      font-size: 16px;
    }
    /* Basket Panel */
    #basket-panel {
      position: fixed;
      top: 60px;
      right: 20px;
      width: 300px;
      max-height: 80vh;
      background: white;
      border: 1px solid #ddd;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
      overflow-y: auto;
      transform: translateY(-20px);
      opacity: 0;
      transition: opacity 0.3s ease, transform 0.3s ease;
      z-index: 1100;
      padding: 20px;
    }
    #basket-panel.active {
      opacity: 1;
      transform: translateY(0);
    }
    #basket-panel h3 {
      margin-bottom: 15px;
      font-size: 18px;
      border-bottom: 1px solid #eee;
      padding-bottom: 5px;
    }
    .basket-item {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
    }
    .basket-item img {
      width: 50px;
      height: 50px;
      object-fit: cover;
      margin-right: 10px;
    }
    .basket-item-details {
      font-size: 14px;
      line-height: 1.2;
    }
    #basket-total {
      margin-top: 10px;
      font-weight: bold;
      border-top: 1px solid #eee;
      padding-top: 10px;
      font-size: 16px;
    }
    #checkout-btn {
      margin-top: 15px;
      width: 100%;
      padding: 10px;
      background: #333;
      color: white;
      border: none;
      cursor: pointer;
      font-size: 16px;
    }
    /* Checkout Form */
    #checkout-form {
      display: none;
      position: fixed;
      top: 60px;
      right: 20px;
      width: 300px;
      background: white;
      border: 1px solid #ddd;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
      padding: 20px;
      z-index: 1100;
    }
    #checkout-form h3 {
      margin-bottom: 15px;
      font-size: 18px;
      border-bottom: 1px solid #eee;
      padding-bottom: 5px;
    }
    #checkout-form form {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    #checkout-form input {
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 14px;
    }
    #checkout-form button {
      padding: 10px;
      background: #333;
      color: white;
      border: none;
      cursor: pointer;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <!-- Navbar -->
  <nav class="navbar">
    <!-- Hamburger Menu (Top Left) -->
    <div class="menu-toggle" onclick="toggleMenu()">
      <div class="bar"></div>
      <div class="bar"></div>
    </div>
    <!-- Logo (Center) -->
    <div class="logo">Ryzy</div>
    <!-- Basket Icon (Top Right) -->
    <div class="basket-icon" onclick="toggleBasketPanel()">
      <i class="fas fa-lock"></i>
      <span id="basket-count">0</span>
    </div>
    <!-- Dropdown Menu -->
    <div class="dropdown-menu" id="dropdown-menu">
      <a href="#">Support</a>
      <a href="#">Terms</a>
      <a href="#">Instagram</a>
      <a href="#">X</a>
      <a href="#">Facebook</a>
    </div>
  </nav>

  <!-- Products Container -->
  <div class="products-container">
    <!-- Group 1: nike h (5 items per row; repeat items to form 3 rows as needed) -->
    <div class="product-grid" id="grid-nike-h">
      <!-- Example items for nike h -->
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+H1','nike h',100)">
        <img src="https://via.placeholder.com/150?text=Nike+H1" alt="nike h">
        <div class="product-caption">nike h</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+H2','nike h',100)">
        <img src="https://via.placeholder.com/150?text=Nike+H2" alt="nike h">
        <div class="product-caption">nike h</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+H3','nike h',100)">
        <img src="https://via.placeholder.com/150?text=Nike+H3" alt="nike h">
        <div class="product-caption">nike h</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+H4','nike h',100)">
        <img src="https://via.placeholder.com/150?text=Nike+H4" alt="nike h">
        <div class="product-caption">nike h</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+H5','nike h',100)">
        <img src="https://via.placeholder.com/150?text=Nike+H5" alt="nike h">
        <div class="product-caption">nike h</div>
      </div>
      <!-- (Duplicate these blocks as needed for 3 rows – total of 15 items) -->
    </div>

    <!-- Group 2: nike t (5 items per row; repeat items for 3 rows as needed) -->
    <div class="product-grid" id="grid-nike-t">
      <!-- Example items for nike t -->
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+T1','nike t',80)">
        <img src="https://via.placeholder.com/150?text=Nike+T1" alt="nike t">
        <div class="product-caption">nike t</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+T2','nike t',80)">
        <img src="https://via.placeholder.com/150?text=Nike+T2" alt="nike t">
        <div class="product-caption">nike t</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+T3','nike t',80)">
        <img src="https://via.placeholder.com/150?text=Nike+T3" alt="nike t">
        <div class="product-caption">nike t</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+T4','nike t',80)">
        <img src="https://via.placeholder.com/150?text=Nike+T4" alt="nike t">
        <div class="product-caption">nike t</div>
      </div>
      <div class="product-item" onclick="openProductModal('https://via.placeholder.com/150?text=Nike+T5','nike t',80)">
        <img src="https://via.placeholder.com/150?text=Nike+T5" alt="nike t">
        <div class="product-caption">nike t</div>
      </div>
      <!-- (Duplicate these blocks as needed for 3 rows – total of 15 items) -->
    </div>
  </div>

  <!-- Product Modal (Zoom) -->
  <div class="modal" id="product-modal" onclick="closeProductModal(event)">
    <img src="" alt="Product" class="modal-content" id="modal-image">
    <div class="modal-info" id="modal-info">
      <h3 id="modal-title"></h3>
      <p id="modal-price"></p>
      <button onclick="addToBasket(); event.stopPropagation();">+</button>
    </div>
  </div>

  <!-- Basket Panel -->
  <div id="basket-panel">
    <h3>Your Basket</h3>
    <div id="basket-items">
      <!-- Basket items will be dynamically added here -->
    </div>
    <div id="basket-total">Total: $<span id="total-price">0</span></div>
    <button id="checkout-btn" onclick="showCheckoutForm()">Continue</button>
  </div>

  <!-- Checkout Form -->
  <div id="checkout-form">
    <h3>Checkout</h3>
    <form id="checkout">
      <input type="text" placeholder="Country/Wilaya" required>
      <input type="text" placeholder="First Name" required>
      <input type="text" placeholder="Last Name" required>
      <input type="text" placeholder="Address" required>
      <input type="text" placeholder="City" required>
      <input type="tel" placeholder="Phone" required>
      <button type="submit">Submit</button>
    </form>
  </div>

  <script>
    // Toggle Hamburger Menu
    function toggleMenu() {
      const menuToggle = document.querySelector('.menu-toggle');
      menuToggle.classList.toggle('active');
      document.getElementById('dropdown-menu').classList.toggle('active');
    }

    // Basket Data and UI Update
    let basket = [];
    function updateBasketUI() {
      const basketItemsDiv = document.getElementById('basket-items');
      basketItemsDiv.innerHTML = '';
      let total = 0;
      basket.forEach((item) => {
        const itemDiv = document.createElement('div');
        itemDiv.classList.add('basket-item');
        itemDiv.innerHTML = `
          <img src="${item.image}" alt="${item.name}">
          <div class="basket-item-details">
            <div>${item.name}</div>
            <div>Price: $${item.price}</div>
            <div>Qty: ${item.quantity}</div>
          </div>
        `;
        basketItemsDiv.appendChild(itemDiv);
        total += item.price * item.quantity;
      });
      document.getElementById('total-price').textContent = total;
      document.getElementById('basket-count').textContent =
        basket.reduce((sum, item) => sum + item.quantity, 0);
    }

    // Called when the plus button in the product modal is clicked
    function addToBasket() {
      const name = document.getElementById('modal-title').textContent;
      const image = document.getElementById('modal-image').src;
      const price = parseFloat(
        document.getElementById('modal-price').textContent.replace('$', '')
      );
      // Check if product already exists in basket
      const existing = basket.find(item => item.name === name);
      if (existing) {
        existing.quantity += 1;
      } else {
        basket.push({ name, image, price, quantity: 1 });
      }
      updateBasketUI();
      document.getElementById('product-modal').style.display = 'none';
    }

    // Toggle Basket Panel Animation
    function toggleBasketPanel() {
      const panel = document.getElementById('basket-panel');
      panel.classList.toggle('active');
      // Hide checkout form if visible
      document.getElementById('checkout-form').style.display = 'none';
    }

    // Open the product modal (with image, title, and price)
    function openProductModal(imageSrc, title, price) {
      document.getElementById('product-modal').style.display = 'flex';
      document.getElementById('modal-image').src = imageSrc;
      document.getElementById('modal-title').textContent = title;
      document.getElementById('modal-price').textContent = '$' + price;
    }

    // Close product modal when clicking outside the modal-info box
    function closeProductModal(e) {
      if (e.target.id === 'product-modal') {
        document.getElementById('product-modal').style.display = 'none';
      }
    }

    // Show Checkout Form (replacing the basket panel)
    function showCheckoutForm() {
      document.getElementById('basket-panel').classList.remove('active');
      document.getElementById('checkout-form').style.display = 'block';
    }

    // Animate product items on scroll
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
        }
      });
    }, { threshold: 0.1 });
    document.querySelectorAll('.product-item').forEach(item => {
      observer.observe(item);
    });
  </script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hardware Supplies Store</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }
    body {
      margin: 0;
      background: #f4f4f4;
      color: #333;
    }
    header {
      background: #222;
      color: #fff;
      padding: 1rem;
      text-align: center;
    }
    nav {
      margin-top: 10px;
    }
    nav button {
      margin: 0 5px;
      padding: 8px 14px;
      background: #555;
      color: #fff;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
    nav button:hover {
      background: #007BFF;
    }
    main {
      padding: 20px;
    }
    section {
      display: none;
    }
    section.active {
      display: block;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 15px;
    }
    .product-card {
      background: white;
      border-radius: 8px;
      padding: 15px;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }
    .product-card img {
      width: 100%;
      height: 150px;
      object-fit: cover;
    }
    .product-card h3 {
      margin: 10px 0 5px;
    }
    .product-card button {
      margin-right: 5px;
      background: #007BFF;
      border: none;
      color: white;
      padding: 6px 10px;
      border-radius: 5px;
      cursor: pointer;
    }
    .product-card button.delete {
      background: #e74c3c;
    }
    .cart-item {
      display: flex;
      justify-content: space-between;
      padding: 8px 0;
      border-bottom: 1px solid #ddd;
    }
    .cart-summary {
      text-align: right;
      margin-top: 15px;
    }
    footer {
      background: #222;
      color: #fff;
      text-align: center;
      padding: 10px;
    }
  </style>
</head>

<body>
  <header>
    <h1>🛠️ Hardware Supplies Store</h1>
    <nav>
      <button id="showProducts">Products</button>
      <button id="showAddProduct">Add Product</button>
      <button id="showCart">Cart (<span id="cartCount">0</span>)</button>
    </nav>
  </header>

  <main>
    <section id="productList" class="active">
      <h2>Available Products</h2>
      <div id="productsContainer" class="grid"></div>
    </section>

    <section id="addProduct">
      <h2>Add / Edit Product</h2>
      <form id="productForm">
        <input type="hidden" id="productId">
        <label>Name:</label>
        <input type="text" id="productName" required>
        <label>Price:</label>
        <input type="number" id="productPrice" min="0" required>
        <label>Description:</label>
        <textarea id="productDesc"></textarea>
        <button type="submit">Save Product</button>
      </form>
    </section>

    <section id="cartSection">
      <h2>Your Cart</h2>
      <div id="cartContainer"></div>
      <div class="cart-summary">
        <strong>Total: $<span id="cartTotal">0</span></strong>
        <button id="checkoutBtn">Checkout</button>
      </div>
    </section>
  </main>

  <footer>
    <p>© 2025 Hardware Supplies Store | Built with ❤️ using HTML, CSS, JS</p>
  </footer>

  <script>
    let products = JSON.parse(localStorage.getItem('products')) || [];
    let cart = JSON.parse(localStorage.getItem('cart')) || [];

    const productsContainer = document.getElementById('productsContainer');
    const productForm = document.getElementById('productForm');
    const cartContainer = document.getElementById('cartContainer');
    const cartTotal = document.getElementById('cartTotal');
    const cartCount = document.getElementById('cartCount');
    const productList = document.getElementById('productList');
    const addProduct = document.getElementById('addProduct');
    const cartSection = document.getElementById('cartSection');

    function showSection(s) {
      document.querySelectorAll('main section').forEach(sec => sec.classList.remove('active'));
      s.classList.add('active');
    }

    document.getElementById('showProducts').onclick = () => showSection(productList);
    document.getElementById('showAddProduct').onclick = () => showSection(addProduct);
    document.getElementById('showCart').onclick = () => {
      renderCart();
      showSection(cartSection);
    };

    function renderProducts() {
      productsContainer.innerHTML = '';
      if (products.length === 0) {
        productsContainer.innerHTML = '<p>No products available. Add one!</p>';
        return;
      }
      products.forEach((p, i) => {
        const card = document.createElement('div');
        card.className = 'product-card';
        card.innerHTML = `
          <img src="${p.image || 'placeholder.jpg'}" alt="${p.name}">
          <h3>${p.name}</h3>
          <p>${p.desc || ''}</p>
          <strong>$${p.price}</strong><br>
          <button onclick="addToCart(${i})">Add to Cart</button>
          <button onclick="editProduct(${i})">Edit</button>
          <button class="delete" onclick="deleteProduct(${i})">Delete</button>
        `;
        productsContainer.appendChild(card);
      });
    }

    productForm.onsubmit = e => {
      e.preventDefault();
      const id = document.getElementById('productId').value;
      const name = document.getElementById('productName').value.trim();
      const price = parseFloat(document.getElementById('productPrice').value);
      const desc = document.getElementById('productDesc').value.trim();

      if (id) {
        products[id] = { ...products[id], name, price, desc };
      } else {
        products.push({ name, price, desc });
      }

      localStorage.setItem('products', JSON.stringify(products));
      productForm.reset();
      document.getElementById('productId').value = '';
      showSection(productList);
      renderProducts();
    };

    function editProduct(i) {
      const p = products[i];
      document.getElementById('productId').value = i;
      document.getElementById('productName').value = p.name;
      document.getElementById('productPrice').value = p.price;
      document.getElementById('productDesc').value = p.desc;
      showSection(addProduct);
    }

    function deleteProduct(i) {
      if (confirm('Delete this product?')) {
        products.splice(i, 1);
        localStorage.setItem('products', JSON.stringify(products));
        renderProducts();
      }
    }

    function addToCart(i) {
      const item = products[i];
      const existing = cart.find(c => c.name === item.name);
      if (existing) existing.qty++;
      else cart.push({ ...item, qty: 1 });
      saveCart();
    }

    function renderCart() {
      cartContainer.innerHTML = '';
      if (cart.length === 0) {
        cartContainer.innerHTML = '<p>Your cart is empty.</p>';
        cartTotal.textContent = '0';
        cartCount.textContent = '0';
        return;
      }

      let total = 0;
      cart.forEach((c, i) => {
        total += c.price * c.qty;
        const item = document.createElement('div');
        item.className = 'cart-item';
        item.innerHTML = `
          <span>${c.name} (x${c.qty})</span>
          <span>$${(c.price * c.qty).toFixed(2)} 
          <button onclick="removeFromCart(${i})">🗑️</button></span>
        `;
        cartContainer.appendChild(item);
      });

      cartTotal.textContent = total.toFixed(2);
      cartCount.textContent = cart.reduce((a, c) => a + c.qty, 0);
    }

    function removeFromCart(i) {
      cart.splice(i, 1);
      saveCart();
      renderCart();
    }

    function saveCart() {
      localStorage.setItem('cart', JSON.stringify(cart));
      renderCart();
    }

    document.getElementById('checkoutBtn').onclick = () => {
      if (cart.length === 0) return alert('Cart is empty!');
      alert('Thank you for your purchase!');
      cart = [];
      saveCart();
    };

    renderProducts();
    renderCart();
  </script>
</body>
</html>

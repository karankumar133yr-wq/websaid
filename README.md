
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MyStore - Online Shopping</title>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, sans-serif;
  background: #f5f7fb;
  color: #172033;
}

header {
  background: #10213d;
  color: white;
  padding: 20px 6%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 15px;
}

header h1 { color: #63e6be; }

button {
  cursor: pointer;
  border: none;
  border-radius: 7px;
  padding: 12px 18px;
}

header button, .buy {
  background: #13a879;
  color: white;
}

.hero {
  background: linear-gradient(120deg, #10213d, #246b91);
  color: white;
  text-align: center;
  padding: 55px 20px;
}

.hero h2 {
  font-size: 36px;
  margin-bottom: 12px;
}

.hero p { margin-bottom: 20px; }

.hero button {
  background: #13a879;
  color: white;
}

.container {
  max-width: 1100px;
  margin: 35px auto;
  padding: 0 18px;
}

.products {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 22px;
  margin-top: 20px;
}

.product {
  background: white;
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 3px 15px #00000010;
}

.product img {
  width: 100%;
  height: 210px;
  object-fit: contain;
  background: #f4f6f8;
  border-radius: 8px;
}

.product h3 { margin: 12px 0; }

.price {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 14px;
}

.buy { width: 100%; }

.cart, .checkout {
  background: white;
  padding: 22px;
  border-radius: 12px;
  margin-top: 35px;
  box-shadow: 0 3px 15px #00000010;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  padding: 14px 0;
  border-bottom: 1px solid #ddd;
}

input, textarea {
  display: block;
  width: 100%;
  padding: 13px;
  margin: 12px 0;
  border: 1px solid #ccd2dc;
  border-radius: 7px;
  font: inherit;
}

textarea { min-height: 90px; }

.checkout button {
  width: 100%;
  background: #10213d;
  color: white;
  font-size: 16px;
}

.note {
  color: #626d7c;
  font-size: 13px;
  margin-top: 12px;
  line-height: 1.5;
}

footer {
  text-align: center;
  background: #10213d;
  color: white;
  padding: 25px;
  margin-top: 40px;
}

@media (max-width: 500px) {
  .hero h2 { font-size: 27px; }
  header { padding: 18px; }
}
</style>
</head>

<body>

<header>
  <h1>MyStore 🛍️</h1>
  <button onclick="showCart()">
    🛒 Cart (<span id="count">0</span>)
  </button>
</header>

<section class="hero">
  <h2>Shop Your Favorite Products</h2>
  <p>Quality products. Great prices. Easy shopping.</p>
  <button onclick="document.getElementById('shop').scrollIntoView({behavior:'smooth'})">
    Shop Now
  </button>
</section>

<main class="container">

  <h2 id="shop">Featured Products</h2>
  <div class="products" id="products"></div>

  <section class="cart" id="cart">
    <h2>🛒 Your Shopping Cart</h2>
    <div id="cartItems"></div>
    <h3 id="total">Total: ₹0</h3>
  </section>

  <section class="checkout" id="checkout">
    <h2>📦 Checkout</h2>
    <p>अपनी डिलीवरी की जानकारी भरें।</p>

    <form id="orderForm">
      <input id="name" placeholder="Customer Name" required>

      <input
        id="phone"
        type="tel"
        placeholder="Mobile Number"
        required
      >

      <textarea
        id="address"
        placeholder="Full Delivery Address"
        required
      ></textarea>

      <button type="submit">
        Place Order via Email 📧
      </button>
    </form>

    <p class="note">
      Order email: kritesh868686@gmail.com<br>
      यह demo checkout है। ईमेल तैयार होगा, जिसे आपको
      अपने email app से Send करना होगा।
      Online payment अभी connected नहीं है।
    </p>
  </section>

</main>

<footer>
  <p>© 2026 MyStore | Online Shopping</p>
</footer>

<script>

// सभी प्रोडक्ट्स
const products = [
  {
    id: 1,
    name: "Wireless Headphones",
    price: 1999,
    image: "https://placehold.co/400x300?text=Headphones"
  },
  {
    id: 2,
    name: "Smart Watch",
    price: 2499,
    image: "https://placehold.co/400x300?text=Smart+Watch"
  },
  {
    id: 3,
    name: "Backpack",
    price: 1499,
    image: "https://placehold.co/400x300?text=Backpack"
  },
  {
    id: 4,
    name: "T-Shirt",
    price: 799,
    image: "https://placehold.co/400x300?text=T-Shirt"
  },

  // आपका असली प्रोडक्ट
  {
    id: 5,
    name: "marsit",
    price: 999,
    image: "https://i.ibb.co/vCZNXgxm/IMG-20260324-WA0000.jpg"
  }
];

let cart = [];

// प्रोडक्ट्स दिखाना
function renderProducts() {
  const container = document.getElementById("products");

  container.innerHTML = products.map(product => `
    <article class="product">
      <img
        src="${product.image}"
        alt="${product.name}"
        onerror="this.onerror=null;this.src='https://placehold.co/400x300?text=Image+Unavailable'"
      >

      <h3>${product.name}</h3>

      <p class="price">
        ₹${product.price.toLocaleString("en-IN")}
      </p>

      <button
        class="buy"
        onclick="addToCart(${product.id})"
      >
        Add to Cart 🛒
      </button>
    </article>
  `).join("");
}

// कार्ट में प्रोडक्ट जोड़ना
function addToCart(id) {
  const existing = cart.find(item => item.id === id);

  if (existing) {
    existing.quantity++;
  } else {
    const product = products.find(p => p.id === id);
    cart.push({...product, quantity: 1});
  }

  renderCart();
}

// Quantity बदलना
function changeQuantity(id, amount) {
  const item = cart.find(item => item.id === id);

  if (!item) return;

  item.quantity += amount;

  if (item.quantity <= 0) {
    cart = cart.filter(item => item.id !== id);
  }

  renderCart();
}

// कार्ट अपडेट करना
function renderCart() {
  const container = document.getElementById("cartItems");

  const count = cart.reduce(
    (sum, item) => sum + item.quantity, 0
  );

  document.getElementById("count").textContent = count;

  container.innerHTML = cart.length
    ? cart.map(item => `
      <div class="cart-item">
        <div>
          <strong>${item.name}</strong><br>
          ₹${item.price.toLocaleString("en-IN")}
        </div>

        <div>
          <button onclick="changeQuantity(${item.id}, -1)">−</button>
          ${item.quantity}
          <button onclick="changeQuantity(${item.id}, 1)">+</button>
        </div>
      </div>
    `).join("")
    : "<p>Your cart is empty.</p>";

  const total = cart.reduce(
    (sum, item) => sum + item.price * item.quantity, 0
  );

  document.getElementById("total").textContent =
    "Total: ₹" + total.toLocaleString("en-IN");
}

// कार्ट पर जाना
function showCart() {
  document.getElementById("cart").scrollIntoView({
    behavior: "smooth"
  });
}

// ऑर्डर ईमेल तैयार करना
document.getElementById("orderForm").addEventListener(
  "submit",
  function(event) {
    event.preventDefault();

    if (cart.length === 0) {
      alert("Please add a product to your cart first!");
      return;
    }

    const name = document.getElementById("name").value.trim();
    const phone = document.getElementById("phone").value.trim();
    const address = document.getElementById("address").value.trim();

    const items = cart.map(item =>
      `${item.name} x ${item.quantity} = ₹${
        item.price * item.quantity
      }`
    ).join("\n");

    const total = cart.reduce(
      (sum, item) => sum + item.price * item.quantity, 0
    );

    const subject = encodeURIComponent("New MyStore Order");

    const body = encodeURIComponent(
      `NEW ORDER\n\n` +
      `Customer: ${name}\n` +
      `Mobile: ${phone}\n` +
      `Address: ${address}\n\n` +
      `Products:\n${items}\n\n` +
      `Total: ₹${total}`
    );

    const email = "kritesh868686@gmail.com";

    window.location.href =
      `mailto:${email}?subject=${subject}&body=${body}`;
  }
);

// वेबसाइट शुरू करना
renderProducts();
renderCart();

</script>

</body>
</html>

# Adv-prs-10
<!DOCTYPE html>
<html>
<head>
  <title>Advanced Product Recommendation System</title>
  <style>
    body {font-family: Arial; background:#f4f6f9; margin:0;}
    header {background:#1e3a8a; color:white; padding:15px; text-align:center;}
    .container {padding:20px;}
    .filters {display:flex; flex-wrap:wrap; gap:10px; margin-bottom:20px;}
    select, input, button {padding:8px;}
    .products {display:grid; grid-template-columns:repeat(auto-fill,minmax(220px,1fr)); gap:20px;}
    .card {background:white; padding:15px; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.1);}
    .card img {width:100%; height:150px; object-fit:cover;}
    .price {color:green; font-weight:bold;}
    .rating {color:orange;}
    .btn {background:#2563eb; color:white; border:none; cursor:pointer; padding:6px 10px; margin-top:5px;}
    .btn:hover {background:#1d4ed8;}
    .cart {position:absolute; right:20px; top:15px;}
  </style>
</head>
<body>

<header>
  <h2>Smart Product Recommendation System</h2>
  <div class="cart">🛒 Cart: <span id="cartCount">0</span></div>
</header>

<div class="container">

  <div class="filters">
    <input type="text" id="searchInput" placeholder="Search product, brand, category...">
    <button onclick="searchProducts()">Search</button>

    <select id="categoryFilter">
      <option value="">All Categories</option>
      <option>Electronics</option>
      <option>Beauty</option>
      <option>Home</option>
      <option>Sports</option>
      <option>Medicine</option>
    </select>

    <select id="brandFilter">
      <option value="">All Brands</option>
      <option>Apple</option>
      <option>Samsung</option>
      <option>HP</option>
      <option>Lakme</option>
      <option>Dove</option>
      <option>LG</option>
      <option>Nike</option>
      <option>Dolo</option>
    </select>

    <select id="priceFilter">
      <option value="">All Prices</option>
      <option value="1000">Below ₹1000</option>
      <option value="5000">Below ₹5000</option>
      <option value="10000">Below ₹10000</option>
      <option value="50000">Below ₹50000</option>
    </select>

    <select id="ratingFilter">
      <option value="">All Ratings</option>
      <option value="4">4★ & above</option>
      <option value="3">3★ & above</option>
    </select>

    <select id="sortFilter">
      <option value="">Sort By</option>
      <option value="low">Price Low → High</option>
      <option value="high">Price High → Low</option>
      <option value="rating">Highest Rating</option>
    </select>

    <button onclick="resetFilters()">Reset</button>
  </div>

  <div class="products" id="productContainer"></div>
</div>

<script>

let cart = 0;

const products = [
  // Electronics
  {name:"iPhone 14", category:"Electronics", brand:"Apple", price:70000, rating:4.8, img:"https://m.media-amazon.com/images/I/61bK6PMOC3L._SX679_.jpg", link:"https://www.amazon.in"},
  {name:"Samsung Galaxy S23", category:"Electronics", brand:"Samsung", price:65000, rating:4.6, img:"https://m.media-amazon.com/images/I/61VfL-aiToL._SX679_.jpg", link:"https://www.amazon.in"},
  {name:"HP Laptop", category:"Electronics", brand:"HP", price:50000, rating:4.4, img:"https://m.media-amazon.com/images/I/71f5Eu5lJSL._SX679_.jpg", link:"https://www.amazon.in"},
  {name:"LG Smart TV", category:"Electronics", brand:"LG", price:40000, rating:4.5, img:"https://m.media-amazon.com/images/I/81vZbXkXQRL._SX679_.jpg", link:"https://www.amazon.in"},

  // Beauty
  {name:"Lakme Lipstick", category:"Beauty", brand:"Lakme", price:500, rating:4.2, img:"https://m.media-amazon.com/images/I/61uK5E9sTJL._SX679_.jpg", link:"https://www.amazon.in"},
  {name:"Dove Shampoo", category:"Beauty", brand:"Dove", price:350, rating:4.3, img:"https://m.media-amazon.com/images/I/71LJJrKbezL._SX679_.jpg", link:"https://www.amazon.in"},

  // Home
  {name:"LG Refrigerator", category:"Home", brand:"LG", price:30000, rating:4.5, img:"https://m.media-amazon.com/images/I/71l2qZQ2LML._SX679_.jpg", link:"https://www.amazon.in"},
  {name:"Samsung Washing Machine", category:"Home", brand:"Samsung", price:25000, rating:4.4, img:"https://m.media-amazon.com/images/I/61k+0I2J0VL._SX679_.jpg", link:"https://www.amazon.in"},

  // Sports
  {name:"Nike Football", category:"Sports", brand:"Nike", price:1500, rating:4.5, img:"https://m.media-amazon.com/images/I/71l5C1S+GfL._SX679_.jpg", link:"https://www.amazon.in"},
  {name:"Cricket Bat", category:"Sports", brand:"Nike", price:2000, rating:4.3, img:"https://m.media-amazon.com/images/I/61g9R7kJzHL._SX679_.jpg", link:"https://www.amazon.in"},

  // Medicine
  {name:"Dolo 650", category:"Medicine", brand:"Dolo", price:50, rating:4.7, img:"https://m.media-amazon.com/images/I/61gV6n0wKUL._SX679_.jpg", link:"https://www.amazon.in"},
  {name:"Vitamin Tablets", category:"Medicine", brand:"Dolo", price:200, rating:4.1, img:"https://m.media-amazon.com/images/I/61V5C+PqUML._SX679_.jpg", link:"https://www.amazon.in"}
];

function displayProducts(list){
  const container = document.getElementById("productContainer");
  container.innerHTML="";
  list.forEach(p=>{
    container.innerHTML+=`
      <div class="card">
        <img src="${p.img}">
        <h4>${p.name}</h4>
        <p>${p.brand}</p>
        <p class="price">₹${p.price}</p>
        <p class="rating">${p.rating} ★</p>
        <button class="btn" onclick="addToCart()">Add to Cart</button>
        <br>
        <a href="${p.link}" target="_blank"><button class="btn">Buy Now</button></a>
      </div>`;
  });
}

function addToCart(){
  cart++;
  document.getElementById("cartCount").innerText=cart;
}

function searchProducts(){
  let search=document.getElementById("searchInput").value.toLowerCase();
  let category=document.getElementById("categoryFilter").value;
  let brand=document.getElementById("brandFilter").value;
  let price=document.getElementById("priceFilter").value;
  let rating=document.getElementById("ratingFilter").value;
  let sort=document.getElementById("sortFilter").value;

  let filtered=products.filter(p=>{
    return (p.name+p.brand+p.category).toLowerCase().includes(search)
      && (category==""||p.category==category)
      && (brand==""||p.brand==brand)
      && (price==""||p.price<=price)
      && (rating==""||p.rating>=rating);
  });

  if(sort=="low") filtered.sort((a,b)=>a.price-b.price);
  if(sort=="high") filtered.sort((a,b)=>b.price-a.price);
  if(sort=="rating") filtered.sort((a,b)=>b.rating-a.rating);

  displayProducts(filtered);
}

function resetFilters(){
  document.getElementById("searchInput").value="";
  document.getElementById("categoryFilter").value="";
  document.getElementById("brandFilter").value="";
  document.getElementById("priceFilter").value="";
  document.getElementById("ratingFilter").value="";
  document.getElementById("sortFilter").value="";
  displayProducts(products);
}

displayProducts(products);

</script>

</body>
</html>

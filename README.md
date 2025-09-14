<!doctype html>

<html lang="bn">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>SD ভিপি সেলার — অনলাইন শপ</title>
  <meta name="description" content="SD ভিপি সেলার — সহজ, দ্রুত এবং বিশ্বাসযোগ্য অনলাইন শপ">
  <style>
    :root{--accent:#0077cc;--muted:#666}
    body{font-family:system-ui,-apple-system,'Segoe UI',Roboto,'Noto Sans Bengali',sans-serif;margin:0;color:#222}
    header{background:#f0f7ff;padding:18px 20px;border-bottom:1px solid #e6eef8}
    .container{max-width:1100px;margin:0 auto;padding:20px}
    .btn{background:var(--accent);color:#fff;padding:10px 14px;border-radius:8px;border:0;cursor:pointer}
    .modal-backdrop{position:fixed;inset:0;background:rgba(0,0,0,0.4);display:none;align-items:center;justify-content:center;padding:20px;z-index:999}
    .modal{background:white;border-radius:12px;max-width:720px;width:100%;padding:18px;max-height:90vh;overflow:auto}
    .flex{display:flex;gap:12px;align-items:center}
    .spacer{flex:1}
    .small{font-size:13px}
    .muted{color:var(--muted)}
    .pay-option{border:1px solid #ddd;padding:10px;border-radius:8px;margin-bottom:8px;cursor:pointer}
    .pay-option:hover{background:#f9f9f9}
  </style>
</head>
<body>
  <header>
    <div class="container"><h1>SD ভিপি সেলার</h1></div>
  </header>  <main class="container">
    <h2>জনপ্রিয় পণ্য</h2>
    <div id="productList"></div>
  </main><button class="btn" style="position:fixed;right:18px;bottom:18px" onclick="openCart()">কার্ট (<span id="cartCount">0</span>)</button>

  <div class="modal-backdrop" id="modalBackdrop">
    <div class="modal">
      <div class="flex"><h3>কার্ট</h3><div class="spacer"></div><button onclick="closeCart()">বন্ধ</button></div>
      <div id="cartItems" style="margin-top:12px"></div>
      <div style="margin-top:12px;display:flex;justify-content:space-between;align-items:center">
        <div class="muted">মোট: ৳<span id="cartTotal">0</span></div>
        <div><button class="btn" onclick="openPayment()">চেকআউট</button></div>
      </div>
    </div>
  </div>  <!-- Payment Modal -->  <div class="modal-backdrop" id="paymentModal">
    <div class="modal">
      <div class="flex"><h3>পেমেন্ট অপশন</h3><div class="spacer"></div><button onclick="closePayment()">বন্ধ</button></div>
      <p class="muted">আপনার পেমেন্ট মেথড নির্বাচন করুন:</p>
      <div class="pay-option" onclick="makePayment('Bkash')">বিকাশ</div>
      <div class="pay-option" onclick="makePayment('Rocket')">রকেট</div>
      <div class="pay-option" onclick="makePayment('Nagad')">নগদ</div>
      <div class="pay-option" onclick="makePayment('Bank Account')">ব্যাংক একাউন্ট</div>
    </div>
  </div>  <script>
    const sampleProducts=[
      {id:1,title:'ব্লুটুথ হেডফোন',price:1200},
      {id:2,title:'স্মার্টওয়াচ',price:2500},
      {id:3,title:'পোর্টেবল স্পিকার',price:900}
    ];

    function getCart(){return JSON.parse(localStorage.getItem('sd_cart')||'[]')}
    function saveCart(c){localStorage.setItem('sd_cart',JSON.stringify(c)); renderCartCount()}
    function addToCart(id){
      const prod=sampleProducts.find(p=>p.id===id);if(!prod)return;
      const cart=getCart();
      const found=cart.find(i=>i.id===id);
      if(found)found.qty++;else cart.push({id:prod.id,title:prod.title,price:prod.price,qty:1});
      saveCart(cart);
      alert(prod.title+' কার্টে যোগ করা হয়েছে');
    }
    function renderProducts(){
      const el=document.getElementById('productList');
      el.innerHTML='';
      sampleProducts.forEach(p=>{
        const d=document.createElement('div');
        d.innerHTML=`<h3>${p.title}</h3><p>৳ ${p.price}</p><button class='btn' onclick='addToCart(${p.id})'>কিনুন</button>`;
        el.appendChild(d);
      })
    }
    function renderCartCount(){
      const cnt=getCart().reduce((s,i)=>s+i.qty,0);
      document.getElementById('cartCount').innerText=cnt;
    }
    function openCart(){document.getElementById('modalBackdrop').style.display='flex';renderCartItems()}
    function closeCart(){document.getElementById('modalBackdrop').style.display='none'}
    function renderCartItems(){
      const el=document.getElementById('cartItems');
      const cart=getCart();
      el.innerHTML='';
      let total=0;
      cart.forEach(item=>{total+=item.price*item.qty;el.innerHTML+=`<div><strong>${item.title}</strong> — ৳${item.price} × ${item.qty}</div>`});
      document.getElementById('cartTotal').innerText=total;
    }

    function openPayment(){closeCart();document.getElementById('paymentModal').style.display='flex'}
    function closePayment(){document.getElementById('paymentModal').style.display='none'}

    function makePayment(method){
      const total=document.getElementById('cartTotal').innerText;
      alert(method+' এর মাধ্যমে ৳'+total+' পেমেন্ট প্রসেস করা হবে (ডেমো)');
      localStorage.removeItem('sd_cart');renderCartCount();closePayment();
    }

    renderProducts();renderCartCount();
  </script></body>
</html>

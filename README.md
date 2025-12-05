<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>POS Modern — อาฉีลี่</title>
  <style>
    /* ---------- Reset & base ---------- */
    :root{
      --bg1: linear-gradient(135deg,#ff9a9e 0%,#fad0c4 50%,#fad0c4 100%);
      --card-bg: rgba(255,255,255,0.9);
      --accent: #6C5CE7;
      --muted: #6b7280;
      --glass: rgba(255,255,255,0.6);
      --radius: 12px;
      --shadow: 0 6px 20px rgba(16,24,40,0.12);
      --max-width: 1100px;
      --glass-2: rgba(255,255,255,0.75);
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
    body{
      background: radial-gradient(circle at 10% 10%, rgba(108,92,231,0.08), transparent 10%),
                  radial-gradient(circle at 90% 90%, rgba(255,154,158,0.06), transparent 10%),
                  var(--bg1);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      display:flex;align-items:center;justify-content:center;padding:20px;
    }

    /* ---------- Layout ---------- */
    .app{
      width:100%;
      max-width:var(--max-width);
      display:grid;
      grid-template-columns: 1fr 420px;
      gap:20px;
      align-items:start;
    }
    @media (max-width:1000px){
      .app{grid-template-columns: 1fr; padding-bottom:40px;}
    }

    /* ---------- Left panel (products) ---------- */
    .panel {
      background: linear-gradient(180deg, rgba(255,255,255,0.8), rgba(255,255,255,0.65));
      border-radius:var(--radius);
      padding:18px;
      box-shadow:var(--shadow);
      backdrop-filter: blur(8px) saturate(120%);
      min-height:420px;
    }

    .top-row{display:flex;gap:12px;align-items:center;margin-bottom:12px;}
    .brand{
      display:flex;align-items:center;gap:12px;font-weight:600;
      font-size:18px;color:var(--accent)
    }
    .brand .logo{
      width:44px;height:44px;border-radius:10px;
      background:linear-gradient(135deg,#6C5CE7,#FF6B6B);
      display:flex;align-items:center;justify-content:center;color:white;font-weight:700;
      box-shadow:0 6px 16px rgba(108,92,231,0.18);
    }

    .search{
      margin-left:auto;display:flex;gap:8px;align-items:center;
      flex:1;max-width:420px;
    }
    .search input{
      width:100%;padding:10px 12px;border-radius:999px;border:0;outline:none;
      background:linear-gradient(180deg, rgba(250,250,250,0.9), rgba(240,240,255,0.8));
      box-shadow:inset 0 1px 0 rgba(255,255,255,0.6);
      font-size:14px;
    }

    /* categories */
    .cats{display:flex;gap:8px;margin-bottom:10px;flex-wrap:wrap}
    .cat{
      padding:8px 12px;border-radius:999px;background:transparent;border:1px solid rgba(100,100,120,0.08);
      cursor:pointer;font-size:13px;color:var(--muted);transition:all .18s;
    }
    .cat.active{background:linear-gradient(90deg,#f0ebff,#eef6ff);color:var(--accent);transform:translateY(-3px);box-shadow:0 8px 20px rgba(108,92,231,0.08)}

    /* product grid */
    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(160px,1fr));gap:12px}
    .card{
      background:var(--card-bg);border-radius:12px;padding:10px;cursor:pointer;
      transition:transform .16s, box-shadow .16s; display:flex;flex-direction:column;gap:8px;
      min-height:140px;position:relative;overflow:hidden;
    }
    .card:hover{transform:translateY(-6px);box-shadow:0 18px 40px rgba(16,24,40,0.12)}
    .thumb{height:86px;border-radius:10px;background:linear-gradient(135deg,#fff,#f6f4ff);display:flex;align-items:center;justify-content:center;font-weight:700;color:var(--muted);font-size:18px}
    .p-title{font-weight:600;font-size:14px}
    .p-desc{font-size:12px;color:var(--muted)}
    .price{margin-left:auto;font-weight:700;color:#111}

    /* add-to-cart pulse */
    .pulse{
      position:absolute;right:10px;bottom:10px;background:linear-gradient(135deg,#34d399,#10b981);
      color:white;padding:6px 10px;border-radius:999px;font-size:13px;box-shadow:0 8px 20px rgba(16,185,129,0.12);
      transform-origin:center;animation:pulse .9s ease-in-out infinite;
    }
    @keyframes pulse{0%{transform:scale(1)}50%{transform:scale(1.06)}100%{transform:scale(1)}}

    /* ---------- Right panel (cart) ---------- */
    .cart{
      position:sticky;top:20px;background:linear-gradient(180deg, rgba(255,255,255,0.95), rgba(250,250,255,0.9));
      padding:18px;border-radius:var(--radius);box-shadow:var(--shadow);min-height:420px;
    }
    .cart h3{margin:0 0 8px 0}
    .cart-list{display:flex;flex-direction:column;gap:12px;max-height:360px;overflow:auto;padding-right:6px}
    .cart-item{display:flex;gap:12px;align-items:center;background:var(--glass-2);padding:8px;border-radius:10px}
    .ci-thumb{width:56px;height:56px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-weight:700}
    .ci-info{flex:1}
    .qty{display:flex;gap:6px;align-items:center}
    .qty button{width:30px;height:30px;border-radius:8px;border:0;background:white;cursor:pointer;box-shadow:0 2px 6px rgba(16,24,40,0.06)}
    .qty span{min-width:26px;text-align:center;display:inline-block}
    .trash{background:transparent;border:0;color:#ff5252;font-size:18px;cursor:pointer}

    .totals{margin-top:10px;border-top:1px dashed rgba(0,0,0,0.06);padding-top:12px;display:flex;flex-direction:column;gap:8px}
    .totals .row{display:flex;justify-content:space-between;align-items:center}
    .pay-btn{margin-top:8px;padding:12px;border-radius:12px;background:linear-gradient(90deg,var(--accent),#ff6b6b);color:white;border:0;font-weight:700;cursor:pointer;box-shadow:0 10px 30px rgba(108,92,231,0.14)}
    .pay-btn:disabled{opacity:0.5;cursor:not-allowed}

    /* payment modal */
    .modal-backdrop{position:fixed;left:0;top:0;width:100%;height:100%;display:flex;align-items:center;justify-content:center;background:rgba(6,6,15,0.45);z-index:80;padding:20px}
    .modal{width:100%;max-width:720px;background:white;border-radius:14px;padding:18px;box-shadow:0 24px 80px rgba(16,24,40,0.32);animation:modalIn .18s ease-out}
    @keyframes modalIn{from{transform:translateY(8px);opacity:0}to{transform:translateY(0);opacity:1}}

    .payment-methods{display:flex;gap:12px;margin-bottom:12px}
    .pm{flex:1;padding:10px;border-radius:10px;border:1px solid rgba(0,0,0,0.06);cursor:pointer;text-align:center}
    .pm.active{border:2px solid rgba(108,92,231,0.14);box-shadow:0 10px 30px rgba(108,92,231,0.06)}
    .card-form{display:flex;flex-direction:column;gap:8px}
    .input{padding:10px;border-radius:8px;border:1px solid rgba(0,0,0,0.06);outline:none}

    /* tiny helpers */
    .muted{color:var(--muted);font-size:13px}
    .center{display:flex;align-items:center;justify-content:center}
    footer{margin-top:12px;text-align:center;color:rgba(0,0,0,0.45);font-size:12px}
  </style>
</head>
<body>
  <div class="app" role="application" aria-label="Modern POS">
    <!-- LEFT: Products -->
    <section class="panel" aria-labelledby="products-title">
      <div class="top-row">
        <div class="brand" aria-hidden="true">
          <div class="logo">A</div>
          <div>
            POS Modern<br><span style="font-weight:400;font-size:12px;color:#666">ร้านของอาฉีลี่</span>
          </div>
        </div>

        <div class="search" role="search" aria-label="ค้นหาสินค้า">
          <input id="search" placeholder="ค้นหาสินค้า… (ชื่อ / หมวด)"/>
        </div>
      </div>

      <!-- categories -->
      <div class="cats" id="cats" role="tablist" aria-label="หมวดสินค้า"></div>

      <!-- product grid -->
      <div class="grid" id="grid" aria-live="polite"></div>
    </section>

    <!-- RIGHT: Cart -->
    <aside class="cart" aria-labelledby="cart-title">
      <h3 id="cart-title">ตะกร้าสินค้า</h3>
      <div class="muted" id="cart-empty">ยังไม่มีสินค้า — คลิกที่สินค้าทางซ้ายเพื่อเพิ่ม</div>
      <div class="cart-list" id="cart-list" aria-live="polite"></div>

      <div class="totals" id="totals" style="display:none">
        <div class="row"><div>ราคารวม</div><div id="subtotal">0.00 ฿</div></div>
        <div class="row"><div>ส่วนลด</div><div id="discount">0.00 ฿</div></div>
        <div class="row" style="font-weight:800;font-size:18px"><div>ยอดที่ต้องชำระ</div><div id="total">0.00 ฿</div></div>
        <button id="pay" class="pay-btn" disabled>Proceed to Payment</button>
      </div>

      <footer>รองรับหน้าจอทุกขนาด • ออกแบบโดย ChatGPT</footer>
    </aside>
  </div>

  <!-- payment modal (hidden until needed) -->
  <div id="modal-root" style="display:none"></div>

  <script>
    /* ---------- Sample product data (แก้/เพิ่มได้ที่นี่) ---------- */
    const PRODUCTS = [
      {id:'d1', title:'Latte เย็น', price:75, cat:'Drinks', desc:'กาแฟลาเต้ เย็นชื่นใจ', thumb:'☕️'},
      {id:'d2', title:'Americano ร้อน', price:65, cat:'Drinks', desc:'กาแฟดำร้อน ชงสด', thumb:'☕️'},
      {id:'d3', title:'ชาเขียว', price:60, cat:'Drinks', desc:'ชาเขียวหวานน้อย', thumb:'🍵'},
      {id:'f1', title:'เบอร์เกอร์หมู', price:120, cat:'Food', desc:'แป้งนุ่ม เนื้อแน่น', thumb:'🍔'},
      {id:'f2', title:'ข้าวไข่เจียว', price:80, cat:'Food', desc:'ไข่ฟู ทานคู่ซอส', thumb:'🍳'},
      {id:'s1', title:'มันฝรั่งทอด', price:55, cat:'Snacks', desc:'ทอดกรอบร้อนๆ', thumb:'🍟'},
      {id:'s2', title:'คุกกี้ช็อกโกแลต', price:45, cat:'Snacks', desc:'หวานพอดี เคี้ยวเพลิน', thumb:'🍪'},
      // เพิ่มรายการได้ตามต้องการ (id ต้องไม่ซ้ำ)
    ];

    /* ---------- State ---------- */
    let state = {
      products: PRODUCTS,
      cats: ['All','Drinks','Food','Snacks'],
      activeCat: 'All',
      query: '',
      cart: {} // {id: {product, qty}}
    };

    /* ---------- Utilities ---------- */
    const $ = sel => document.querySelector(sel);
    const $$ = sel => Array.from(document.querySelectorAll(sel));
    const format = n => Number(n).toLocaleString('en-US',{minimumFractionDigits:2,maximumFractionDigits:2}) + ' ฿';

    /* ---------- Render categories ---------- */
    function renderCats(){
      const wrap = $('#cats');
      wrap.innerHTML = '';
      state.cats.forEach(c=>{
        const btn = document.createElement('button');
        btn.className = 'cat' + (state.activeCat===c ? ' active':'');
        btn.textContent = c;
        btn.onclick = ()=>{ state.activeCat = c; render(); };
        btn.setAttribute('role','tab');
        wrap.appendChild(btn);
      });
    }

    /* ---------- Render products ---------- */
    function renderProducts(){
      const grid = $('#grid');
      grid.innerHTML = '';
      const q = state.query.trim().toLowerCase();
      const list = state.products.filter(p=>{
        const inCat = state.activeCat === 'All' || p.cat === state.activeCat;
        const inQuery = !q || (p.title + ' ' + (p.desc||'') + ' ' + p.cat).toLowerCase().includes(q);
        return inCat && inQuery;
      });
      if(list.length===0){
        grid.innerHTML = '<div class="muted" style="padding:18px">ไม่พบสินค้า</div>';
        return;
      }
      list.forEach(p=>{
        const c = document.createElement('div');
        c.className = 'card';
        c.setAttribute('tabindex','0');
        c.onclick = ()=> addToCart(p.id);
        c.innerHTML = `
          <div class="thumb">${p.thumb || '🍽'}</div>
          <div style="display:flex;align-items:center;gap:8px">
            <div class="p-title">${p.title}</div>
            <div class="price">${format(p.price)}</div>
          </div>
          <div class="p-desc">${p.desc || p.cat}</div>
          <div class="pulse" aria-hidden="true">Add</div>
        `;
        grid.appendChild(c);
      });
    }

    /* ---------- Cart logic ---------- */
    function addToCart(id,qty=1){
      const prod = state.products.find(p=>p.id===id);
      if(!prod) return;
      if(state.cart[id]) state.cart[id].qty += qty;
      else state.cart[id] = {product:prod, qty: qty};
      renderCart();
      animateAddFeedback();
    }

    function setQty(id, qty){
      if(!state.cart[id]) return;
      state.cart[id].qty = Math.max(0, Math.floor(qty));
      if(state.cart[id].qty === 0) delete state.cart[id];
      renderCart();
    }

    function removeFromCart(id){
      delete state.cart[id];
      renderCart();
    }

    function clearCart(){
      state.cart = {};
      renderCart();
    }

    /* ---------- Render cart ---------- */
    function renderCart(){
      const list = $('#cart-list');
      list.innerHTML = '';
      const keys = Object.keys(state.cart);
      const cartEmpty = $('#cart-empty');
      const totals = $('#totals');
      if(keys.length === 0){
        cartEmpty.style.display = 'block';
        totals.style.display = 'none';
      } else {
        cartEmpty.style.display = 'none';
        totals.style.display = 'flex';
        keys.forEach(id=>{
          const it = state.cart[id];
          const el = document.createElement('div');
          el.className = 'cart-item';
          el.innerHTML = `
            <div class="ci-thumb">${it.product.thumb || '🍽'}</div>
            <div class="ci-info">
              <div style="display:flex;gap:8px;align-items:center">
                <div style="font-weight:700">${it.product.title}</div>
                <div style="margin-left:auto;font-weight:700">${format(it.product.price)}</div>
              </div>
              <div class="muted">${it.product.desc || ''}</div>
            </div>
            <div style="display:flex;flex-direction:column;align-items:flex-end;gap:6px">
              <div class="qty" aria-label="จำนวน">
                <button aria-label="ลดจำนวน" data-id="${id}" class="dec">−</button>
                <span id="q-${id}">${it.qty}</span>
                <button aria-label="เพิ่มจำนวน" data-id="${id}" class="inc">＋</button>
              </div>
              <div style="display:flex;gap:6px;align-items:center">
                <button class="trash" aria-label="ลบ" data-id="${id}">🗑️</button>
              </div>
            </div>
          `;
          list.appendChild(el);
        });

        // attach handlers
        $$('.inc').forEach(b=>{
          b.onclick = e=>{
            const id = e.currentTarget.dataset.id;
            setQty(id, state.cart[id].qty + 1);
          };
        });
        $$('.dec').forEach(b=>{
          b.onclick = e=>{
            const id = e.currentTarget.dataset.id;
            setQty(id, state.cart[id].qty - 1);
          };
        });
        $$('.trash').forEach(b=>{
          b.onclick = e=>{
            const id = e.currentTarget.dataset.id;
            removeFromCart(id);
          };
        });
      }

      // totals
      const subtotal = Object.values(state.cart).reduce((s,it)=>s + it.product.price*it.qty, 0);
      const discount = 0; // ปรับแก้ถ้าต้องการคูปอง/ส่วนลด
      const total = subtotal - discount;
      $('#subtotal').textContent = format(subtotal);
      $('#discount').textContent = format(discount);
      $('#total').textContent = format(total);
      $('#pay').disabled = total <= 0;
    }

    /* ---------- Search binding ---------- */
    $('#search').addEventListener('input', (e)=>{
      state.query = e.target.value;
      renderProducts();
    });

    /* ---------- Animate Add feedback ---------- */
    function animateAddFeedback(){
      // small bounce on cart panel to indicate update
      const cart = document.querySelector('.cart');
      cart.animate([
        {transform:'translateY(0)'},
        {transform:'translateY(-6px)'},
        {transform:'translateY(0)'}
      ], {duration:260, easing:'ease'});
    }

    /* ---------- Payment flow (simple mock) ---------- */
    document.getElementById('pay').addEventListener('click', openPaymentModal);

    function openPaymentModal(){
      const subtotal = Object.values(state.cart).reduce((s,it)=>s + it.product.price*it.qty, 0);
      if(subtotal <= 0) return;
      const modalRoot = $('#modal-root');
      modalRoot.style.display = 'block';
      modalRoot.innerHTML = `
        <div class="modal-backdrop" role="dialog" aria-modal="true" aria-label="ชำระเงิน">
          <div class="modal">
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
              <h3>การชำระเงิน</h3>
              <button id="close-modal" aria-label="ปิด">✖️</button>
            </div>

            <div class="muted">ยอดที่ต้องชำระ: <strong style="color:#111">${format(subtotal)}</strong></div>

            <div style="margin-top:12px" class="payment-methods" role="tablist">
              <div class="pm active" data-method="card">บัตร (Card)</div>
              <div class="pm" data-method="cash">เงินสด (Cash)</div>
            </div>

            <div id="method-area">
              <div class="card-form">
                <input class="input" id="card-number" placeholder="หมายเลขบัตร (ตัวอย่าง 4242 4242 4242 4242)"/>
                <div style="display:flex;gap:8px">
                  <input class="input" id="card-exp" placeholder="MM/YY"/>
                  <input class="input" id="card-cvc" placeholder="CVC"/>
                </div>
              </div>
            </div>

            <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:14px">
              <button id="cancel-pay" class="input" style="background:transparent;cursor:pointer">ยกเลิก</button>
              <button id="confirm-pay" class="pay-btn">Confirm Payment</button>
            </div>
          </div>
        </div>
      `;

      // bindings
      modalRoot.querySelectorAll('.pm').forEach(el=>{
        el.addEventListener('click', e=>{
          modalRoot.querySelectorAll('.pm').forEach(x=>x.classList.remove('active'));
          e.currentTarget.classList.add('active');
          const method = e.currentTarget.dataset.method;
          const area = modalRoot.querySelector('#method-area');
          if(method === 'card'){
            area.innerHTML = `<div class="card-form">
                <input class="input" id="card-number" placeholder="หมายเลขบัตร (ตัวอย่าง 4242 4242 4242 4242)"/>
                <div style="display:flex;gap:8px">
                  <input class="input" id="card-exp" placeholder="MM/YY"/>
                  <input class="input" id="card-cvc" placeholder="CVC"/>
                </div>
              </div>`;
          } else {
            area.innerHTML = `<div style="display:flex;flex-direction:column;gap:8px">
              <label class="muted">รับเงินสด: กรอกจำนวนที่ลูกค้าจ่าย</label>
              <input class="input" id="cash-given" placeholder="เช่น 500"/>
              <div class="muted">ทอนให้: <strong id="change">0.00 ฿</strong></div>
            </div>`;
            const cashInput = area.querySelector('#cash-given');
            cashInput && cashInput.addEventListener('input', ()=> {
              const pay = parseFloat(cashInput.value) || 0;
              const change = Math.max(0, pay - subtotal);
              area.querySelector('#change').textContent = format(change);
            });
          }
        });
      });

      modalRoot.querySelector('#close-modal').onclick = closeModal;
      modalRoot.querySelector('#cancel-pay').onclick = closeModal;
      modalRoot.querySelector('#confirm-pay').onclick = () => {
        // simulate processing
        const chosen = modalRoot.querySelector('.pm.active').dataset.method;
        processPayment(chosen);
      };
    }

    function closeModal(){
      const modalRoot = $('#modal-root');
      modalRoot.style.display = 'none';
      modalRoot.innerHTML = '';
    }

    function processPayment(method){
      // In real app -> integrate payment gateway here (e.g., Stripe/Omise/PayPal)
      // For demo: show a success and clear cart
      const modalRoot = $('#modal-root');
      modalRoot.innerHTML = `
        <div class="modal-backdrop" role="dialog" aria-modal="true">
          <div class="modal center">
            <div style="font-size:22px;font-weight:800">ชำระเงินสำเร็จ 🎉</div>
            <div class="muted" style="margin-top:8px">วิธีชำระ: ${method}</div>
            <div style="margin-top:18px;display:flex;gap:8px;justify-content:center">
              <button id="close-success" class="pay-btn">เสร็จสิ้น</button>
            </div>
          </div>
        </div>
      `;
      modalRoot.querySelector('#close-success').onclick = ()=>{
        closeModal();
        clearCart();
      };
    }

    /* ---------- Initial render ---------- */
    function render(){
      renderCats();
      renderProducts();
      renderCart();
    }

    // initial
    render();

    /* ---------- Public customization helpers (สำหรับ dev) ---------- */
    // เพิ่มสินค้าใหม่: addProduct({id:'x1', title:'', price:0, cat:'Snacks', desc:'', thumb:'🍩'})
    function addProduct(prod){
      if(state.products.find(p=>p.id===prod.id)) throw new Error('id ซ้ำ');
      state.products.push(prod);
      if(!state.cats.includes(prod.cat)) state.cats.push(prod.cat);
      render();
    }
    // เปลี่ยนธีม: setAccent('#FF0000'), setBgGradient(...)
    function setAccent(color){
      document.documentElement.style.setProperty('--accent', color);
    }

    // expose to window for quick console edits
    window.POS = {addProduct, setAccent, state, render, addToCart};

    /* ---------- Accessibility small improvements ---------- */
    // keyboard support: Enter to add focused product
    document.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter'){
        const focused = document.activeElement;
        if(focused && focused.classList && focused.classList.contains('card')){
          focused.click();
        }
      }
    });

  </script>
</body>
</html>

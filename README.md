
<!DOCTYPE html>
<html lang="rw">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NIYIGABA Business Manager</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f2f4f8;color:#202938}
header{background:#101d35;color:white;padding:20px}
header h2{margin:0 0 6px}
header p{margin:0;color:#cbd5e1}
main{padding:16px;max-width:1100px;margin:auto}
.card{background:white;padding:16px;border-radius:12px;margin-bottom:15px;box-shadow:0 2px 8px #0000000c}
h3{margin-top:0}
.grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
.stat{background:#eaf1ff;padding:14px;border-radius:10px}
.stat strong{display:block;font-size:23px;margin-top:8px}
label{display:block;margin:12px 0 5px}
input,select{width:100%;padding:12px;border:1px solid #ccd3df;border-radius:8px;font-size:16px}
button{padding:12px 16px;border:0;border-radius:8px;background:#174ea6;color:white;font-weight:bold;margin-top:12px;cursor:pointer}
button.secondary{background:#475569}
nav{display:flex;gap:8px;overflow-x:auto;padding:10px 0}
nav button{white-space:nowrap;margin:0}
table{width:100%;border-collapse:collapse;font-size:14px}
td,th{padding:10px;border-bottom:1px solid #e5e7eb;text-align:left}
.small{color:#64748b;font-size:13px}
.hidden{display:none}
@media(min-width:700px){.grid{grid-template-columns:repeat(4,1fr)}}
</style>
</head>
<body>

<header>
<h2>NIYIGABA BUSINESS MANAGER</h2>
<p>All-in-One Business Management System</p>
</header>

<main>
<div class="card">
<h3>Guhitamo ubucuruzi</h3>
<select id="business">
<option>Restaurant</option>
<option>Hotel</option>
<option>Bar / Akabari</option>
<option>Shop / Iduka</option>
<option>Supermarket</option>
<option>Pharmacy</option>
<option>Salon</option>
<option>Tailoring / Ubudozi</option>
<option>Guest House</option>
<option>Warehouse</option>
<option>Transport</option>
<option>Other Business</option>
</select>
<p class="small">NIYIGABA ishobora gucunga ubwoko bwinshi bw'ubucuruzi.</p>
</div>

<nav>
<button onclick="show('dashboard')">Dashboard</button>
<button onclick="show('sales')">Sales</button>
<button onclick="show('stock')">Stock</button>
<button onclick="show('reports')">Reports</button>
</nav>

<section id="dashboard">
<div class="card">
<h3>Business Dashboard</h3>
<div class="grid">
<div class="stat">Amafaranga yinjiye<strong id="total">0 Frw</strong></div>
<div class="stat">Ibicuruzwa<strong id="count">0</strong></div>
<div class="stat">Sales<strong id="salesCount">0</strong></div>
<div class="stat">Ubucuruzi<strong id="businessName">Restaurant</strong></div>
</div>
</div>
<div class="card">
<h3>Ibikorwa by'ingenzi</h3>
<p>• Kwandika ibyo wagurishije</p>
<p>• Gucunga stock</p>
<p>• Kureba raporo z'ubucuruzi</p>
<p>• Gucunga amafaranga yinjira</p>
</div>
</section>

<section id="sales" class="hidden">
<div class="card">
<h3>Kwandika igurisha rishya</h3>
<label>Izina ry'igicuruzwa cyangwa serivisi</label>
<input id="productName" placeholder="Urugero: Umuceri cyangwa Salon">
<label>Umubare</label>
<input id="quantity" type="number" min="1" value="1">
<label>Igiciro kuri kimwe (Frw)</label>
<input id="price" type="number" min="0" placeholder="Urugero: 5000">
<button onclick="saveSale()">Bika igurisha</button>
</div>
<div class="card">
<h3>Amateka y'igurisha</h3>
<div id="salesList"></div>
</div>
</section>

<section id="stock" class="hidden">
<div class="card">
<h3>Kongeramo ibicuruzwa muri stock</h3>
<label>Izina ry'igicuruzwa</label>
<input id="stockName" placeholder="Izina">
<label>Umubare</label>
<input id="stockQty" type="number" min="1" placeholder="Umubare">
<button onclick="addStock()">Bika muri stock</button>
</div>
<div class="card">
<h3>Stock isigaye</h3>
<div id="stockList"></div>
</div>
</section>

<section id="reports" class="hidden">
<div class="card">
<h3>Raporo y'ubucuruzi</h3>
<p>Igiteranyo cy'amafaranga yinjiye:</p>
<h2 id="reportTotal">0 Frw</h2>
<p>Umubare w'ibicuruzwa byagurishijwe:</p>
<h2 id="reportCount">0</h2>
<button onclick="window.print()" class="secondary">Print Report</button>
</div>
</section>
</main>

<script>
let sales=JSON.parse(localStorage.getItem("niyigaba_sales")||"[]");
let stock=JSON.parse(localStorage.getItem("niyigaba_stock")||"[]");

function money(n){
 return Number(n).toLocaleString("en-US")+" Frw";
}

function saveData(){
 localStorage.setItem("niyigaba_sales",JSON.stringify(sales));
 localStorage.setItem("niyigaba_stock",JSON.stringify(stock));
}

function show(page){
 ["dashboard","sales","stock","reports"].forEach(id=>{
  document.getElementById(id).classList.add("hidden");
 });
 document.getElementById(page).classList.remove("hidden");
 update();
}

function saveSale(){
 let name=document.getElementById("productName").value.trim();
 let qty=Number(document.getElementById("quantity").value);
 let price=Number(document.getElementById("price").value);

 if(!name||qty<1||price<=0){
  alert("Uzuza amakuru yose neza.");
  return;
 }

 sales.push({
  name:name,
  qty:qty,
  price:price,
  total:qty*price,
  date:new Date().toLocaleString()
 });

 saveData();
 document.getElementById("productName").value="";
 document.getElementById("price").value="";
 alert("Igurisha ribitswe neza!");
 update();
}

function addStock(){
 let name=document.getElementById("stockName").value.trim();
 let qty=Number(document.getElementById("stockQty").value);

 if(!name||qty<1){
  alert("Andika izina n'umubare neza.");
  return;
 }

 let item=stock.find(x=>x.name.toLowerCase()===name.toLowerCase());
 if(item){item.qty+=qty;}
 else{stock.push({name:name,qty:qty});}

 saveData();
 document.getElementById("stockName").value="";
 document.getElementById("stockQty").value="";
 update();
 alert("Stock yongeweho neza!");
}

function update(){
 let total=sales.reduce((sum,s)=>sum+s.total,0);
 let qty=sales.reduce((sum,s)=>sum+s.qty,0);

 document.getElementById("total").textContent=money(total);
 document.getElementById("count").textContent=stock.length;
 document.getElementById("salesCount").textContent=sales.length;
 document.getElementById("reportTotal").textContent=money(total);
 document.getElementById("reportCount").textContent=qty;
 document.getElementById("businessName").textContent=
 document.getElementById("business").value;

 document.getElementById("salesList").innerHTML=
 sales.slice().reverse().map(s=>
 `<p>${s.name} — ${s.qty} × ${money(s.price)} = <b>${money(s.total)}</b><br><small>${s.date}</small></p>`
 ).join("")||"Nta gurisha rirandikwa.";

 document.getElementById("stockList").innerHTML=
 stock.map(s=>`<p>${s.name} — <b>${s.qty}</b></p>`).join("")||
 "Nta bicuruzwa biri muri stock.";
}

document.getElementById("business").addEventListener("change",update);
update();
</script>

</body>
</html>

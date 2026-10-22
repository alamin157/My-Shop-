# script.js
// ===== আপনার তথ্য এখানে বদলান =====
const C = {
  SUPABASE_URL: "",   // পরে বসাবেন (এখন খালি রাখুন)
  SUPABASE_KEY: "",   // পরে বসাবেন (এখন খালি রাখুন)
  SHOP: "লিন্ডেন অ্যান্ড লুম",
  BKASH: "01XXXXXXXXX",
  NAGAD: "01XXXXXXXXX",
  DELIVERY_DHAKA: 60,
  DELIVERY_OUT: 120
};
// ===== প্রোডাক্ট (ডেমো): নাম ও দাম বদলান =====
const DEMO=[
{id:1,name:"এভরিডে টি-শার্ট",category:"টপস",price:650,icon:"tee",col:"#3B3FD8",stock:50},
{id:2,name:"লিনেন শার্ট ড্রেস",category:"ড্রেস",price:2200,icon:"dress",col:"#B4694F",stock:20},
{id:3,name:"ওয়াইড-লেগ ট্রাউজার",category:"বটমস",price:1800,icon:"pants",col:"#2B3A55",stock:30},
{id:4,name:"পকেট টি-শার্ট",category:"টপস",price:700,icon:"tee",col:"#8C9A86",stock:40}];

// ===== এর নিচে কিছু বদলানোর দরকার নেই =====
const $=s=>document.querySelector(s),tk=n=>"৳"+Number(n).toLocaleString("en-US");
const esc=s=>String(s??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));
const db=(C.SUPABASE_URL&&window.supabase)?supabase.createClient(C.SUPABASE_URL,C.SUPABASE_KEY):null;
let P=[],cart=[],cat="সব",pick={};
try{cart=JSON.parse(localStorage.getItem("cart")||"[]")}catch(e){}
$("#shop").textContent=C.SHOP;document.title=C.SHOP;$("#foot").textContent="© "+C.SHOP+" · ডেলিভারি ও রিটার্ন নীতি";
const save=()=>{try{localStorage.setItem("cart",JSON.stringify(cart))}catch(e){}$("#count").textContent=cart.reduce((a,l)=>a+l.qty,0)};
const sub=()=>cart.reduce((a,l)=>a+l.price*l.qty,0);
const pic=p=>p.image_url?`<img src="${esc(p.image_url)}" alt="${esc(p.name)}" loading="lazy">`:`<svg style="color:${p.col||"#3B3FD8"}"><use href="#${p.icon||"tee"}"/></svg>`;

async function load(){
 if(db){const {data,error}=await db.from("products").select("*").eq("active",true).order("created_at",{ascending:false});P=error?DEMO:data}else P=DEMO;
 render();
}
function render(){
 const cs=["সব",...new Set(P.map(p=>p.category))];
 $("#filters").innerHTML=cs.map(c=>`<button class="chip" aria-pressed="${c==cat}" data-c="${esc(c)}">${esc(c)}</button>`).join("");
 $("#grid").innerHTML=P.filter(p=>cat=="সব"||p.category==cat).map(p=>{
  const sz=p.sizes||["S","M","L","XL"],s=pick[p.id]||sz[0];
  return `<article class="card"><div class="tile">${pic(p)}</div><h3>${esc(p.name)}</h3><div class="meta"><span>${esc(p.category)}</span><b>${tk(p.price)}</b></div>
  <div class="sizes">${sz.map(z=>`<button aria-pressed="${z==s}" data-s="${esc(z)}" data-id="${p.id}">${esc(z)}</button>`).join("")}</div>
  <button class="add" data-add="${p.id}" ${p.stock<1?"disabled":""}>${p.stock<1?"স্টক শেষ":"ব্যাগে যোগ করুন"}</button></article>`}).join("");
}
function panel(h){$("#panel").innerHTML=h;$("#panel").classList.add("on");$("#scrim").classList.add("on")}
function close(){$("#panel").classList.remove("on");$("#scrim").classList.remove("on")}
function bag(){
 const L=cart.length?cart.map((l,i)=>`<div class="line"><div class="th">${pic(l)}</div><div class="in"><b>${esc(l.name)}</b><br><span style="color:var(--mute)">সাইজ ${esc(l.size)} · ${tk(l.price)}</span></div><div class="q"><button data-q="${i}" data-d="-1" aria-label="কমান">−</button> ${l.qty} <button data-q="${i}" data-d="1" aria-label="বাড়ান">+</button></div></div>`).join(""):`<p class="empty">আপনার ব্যাগ খালি। পছন্দের পোশাক যোগ করুন।</p>`;
 panel(`<div class="top"><h2>আপনার ব্যাগ</h2><button class="x" data-close aria-label="বন্ধ">×</button></div><div class="lines">${L}</div><div class="tot b"><span>সাবটোটাল</span><span>${tk(sub())}</span></div><button class="primary" data-go ${cart.length?"":"disabled"}>চেকআউট করুন</button>`);
}
function checkout(){
 panel(`<div class="top"><h2>চেকআউট</h2><button class="x" data-close aria-label="বন্ধ">×</button></div>
 <form id="f" class="lines" novalidate>
 <label>আপনার নাম<input type="text" name="name" autocomplete="name"></label>
 <label>মোবাইল নম্বর<input type="tel" name="phone" autocomplete="tel" placeholder="01XXXXXXXXX"></label>
 <label>সম্পূর্ণ ঠিকানা<input type="text" name="address" autocomplete="street-address"></label>
 <label>ডেলিভারি এলাকা</label>
 <label class="opt"><input type="radio" name="area" value="dhaka" checked> ঢাকার ভিতরে (${tk(C.DELIVERY_DHAKA)})</label>
 <label class="opt"><input type="radio" name="area" value="out"> ঢাকার বাইরে (${tk(C.DELIVERY_OUT)})</label>
 <label>পেমেন্ট পদ্ধতি</label>
 <label class="opt"><input type="radio" name="pay" value="cod" checked> ক্যাশ অন ডেলিভারি</label>
 <label class="opt"><input type="radio" name="pay" value="bkash"> বিকাশ</label>
 <label class="opt"><input type="radio" name="pay" value="nagad"> নগদ</label>
 <div id="payinfo"></div><p class="err" id="err" role="alert"></p></form>
 <div id="sum"></div><button class="primary" id="place">অর্ডার নিশ্চিত করুন</button>`);
 sum();
}
function fee(){const a=document.querySelector("[name=area]:checked");return a&&a.value=="out"?C.DELIVERY_OUT:C.DELIVERY_DHAKA}
function sum(){
 const p=document.querySelector("[name=pay]:checked").value,num=p=="bkash"?C.BKASH:C.NAGAD;
 $("#payinfo").innerHTML=p=="cod"?"":`<div class="note">${p=="bkash"?"বিকাশ":"নগদ"} অ্যাপ থেকে <b>${esc(num)}</b> নম্বরে <b>${tk(sub()+fee())}</b> Send Money করুন। তারপর নিচে TrxID দিন।</div><label>TrxID<input type="text" name="trx" autocomplete="off"></label>`;
 $("#sum").innerHTML=`<div class="tot"><span>সাবটোটাল</span><span>${tk(sub())}</span></div><div class="tot"><span>ডেলিভারি</span><span>${tk(fee())}</span></div><div class="tot b"><span>মোট</span><span>${tk(sub()+fee())}</span></div>`;
}
async function place(){
 const v=Object.fromEntries(new FormData($("#f"))),e=m=>{$("#err").textContent=m};
 if(!v.name.trim())return e("আপনার নাম লিখুন।");
 if(!/^01\d{9}$/.test(v.phone.trim()))return e("সঠিক ১১ সংখ্যার মোবাইল নম্বর দিন (01XXXXXXXXX)।");
 if(v.address.trim().length<8)return e("সম্পূর্ণ ঠিকানা লিখুন।");
 if(v.pay!="cod"&&(v.trx||"").trim().length<6)return e("পেমেন্টের TrxID দিন।");
 const b=$("#place");b.disabled=true;b.textContent="অর্ডার হচ্ছে...";
 const d=fee(),no="LL"+Date.now().toString().slice(-8);
 const row={order_no:no,name:v.name.trim(),phone:v.phone.trim(),address:v.address.trim(),area:v.area,
  items:cart.map(({id,name,price,size,qty})=>({id,name,price,size,qty})),subtotal:sub(),delivery:d,total:sub()+d,payment_method:v.pay,trx_id:(v.trx||"").trim()||null};
 if(db){const {error}=await db.from("orders").insert(row);if(error){b.disabled=false;b.textContent="অর্ডার নিশ্চিত করুন";return e("অর্ডার হয়নি। ইন্টারনেট দেখে আবার চেষ্টা করুন।")}}
 cart=[];save();
 panel(`<div class="top"><h2>অর্ডার সম্পন্ন</h2><button class="x" data-close aria-label="বন্ধ">×</button></div><div class="lines"><p>ধন্যবাদ, ${esc(row.name)}। আপনার অর্ডার নম্বর <b>${no}</b>।</p><p style="color:var(--mute)">আমরা শীঘ্রই ${esc(row.phone)} নম্বরে ফোন করে নিশ্চিত করব।</p></div><button class="primary" data-close>কেনাকাটা চালিয়ে যান</button>`);
}
document.addEventListener("click",e=>{
 const t=e.target.closest("button,.scrim");if(!t)return;const d=t.dataset;
 if(t.id=="open")bag();
 else if(t.classList.contains("scrim")||"close"in d)close();
 else if(d.c){cat=d.c;render()}
 else if(d.s){pick[d.id]=d.s;render()}
 else if(d.add){const p=P.find(x=>x.id==d.add),s=pick[p.id]||(p.sizes||["S"])[0],l=cart.find(x=>x.id==p.id&&x.size==s);
  l?l.qty++:cart.push({id:p.id,name:p.name,price:+p.price,size:s,qty:1,image_url:p.image_url,icon:p.icon,col:p.col});save();bag()}
 else if(d.q){const l=cart[d.q];l.qty+=+d.d;if(l.qty<1)cart.splice(d.q,1);save();bag()}
 else if("go"in d)checkout();
 else if(t.id=="place")place();
});
document.addEventListener("change",e=>{if(e.target.name=="pay"||e.target.name=="area")sum()});
document.addEventListener("keydown",e=>{if(e.key=="Escape")close()});
save();load();
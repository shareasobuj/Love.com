# Love.com

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Love.com - Heartfelt Poems & Quotes</title>
<!-- Bootstrap CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
<style>
:root {
  --main-color: #e84393;
  --secondary-color: #fd79a8;
}
body {
  font-family: 'Arial', sans-serif;
  background: #fff0f5;
  padding-top: 70px;
}
.navbar {
  background: var(--main-color);
}
.navbar-brand, .nav-link {
  color: #fff !important;
}
.hero {
  background: linear-gradient(180deg, rgba(232,67,147,0.95), rgba(253,121,168,0.95));
  color:#fff; padding:60px 20px; text-align:center;
}
.hero h1 { font-size: 3rem; margin-bottom:10px; }
.hero p { font-size: 1.3rem; }
.card-img-thumb { object-fit:cover; height:200px; width:100%; border-radius:8px; }
footer { background: var(--main-color); color:#fff; padding:15px 0; margin-top:40px; text-align:center; }
.page-section { padding:30px 0; }
.admin-badge { position: fixed; right: 18px; bottom: 18px; z-index:1100; }
form input, form textarea { margin-bottom:10px; }
ul { list-style:none; padding-left:0; }
ul li { margin-bottom:10px; font-size:1.1rem; }
button { cursor:pointer; }
</style>
</head>
<body>

<nav class="navbar fixed-top">
  <div class="container">
    <a class="navbar-brand" href="#" onclick="showPage('home')"><strong>❤️ Love.com</strong></a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navmenu">
      <span class="navbar-toggler-icon" style="color:#fff">☰</span>
    </button>
    <div class="collapse navbar-collapse" id="navmenu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="#" onclick="showPage('home')">Home</a></li>
        <li class="nav-item"><a class="nav-link" href="#" onclick="showPage('poems')">Poems</a></li>
        <li class="nav-item"><a class="nav-link" href="#" onclick="showPage('quotes')">Quotes</a></li>
        <li class="nav-item"><a class="nav-link" href="#" onclick="showPage('gallery')">Gallery</a></li>
        <li class="nav-item"><a class="nav-link" href="#" onclick="showPage('contact')">Contact</a></li>
        <li class="nav-item"><a class="nav-link" href="#" onclick="showPage('admin')">Admin</a></li>
      </ul>
    </div>
  </div>
</nav>

<div class="container" id="main-content"></div>

<footer>
  <small>© 2025 Love.com - Heartfelt Poems & Quotes</small>
</footer>

<script>
let session = null; // admin session
let poems = [
  {id:1, text:"তুমি আমার হৃদয়ের একমাত্র সুর।"},
  {id:2, text:"প্রেমের নদীতে ভেসে যাই তোমার নাম নিয়ে।"}
];
let quotes = [
  {id:1, text:"Love is not what you say, it's what you do."},
  {id:2, text:"True love stories never have endings."}
];
let gallery = [
  {id:1, src:"https://i.ibb.co/Th0H1Hq/love1.jpg"},
  {id:2, src:"https://i.ibb.co/jfPbLx1/love2.jpg"},
  {id:3, src:"https://i.ibb.co/2v6y5tP/love3.jpg"}
];

function showPage(page){
  const container = document.getElementById('main-content');
  if(page==='home'){
    container.innerHTML = `<div class="hero">
      <h1>Welcome to Love.com</h1>
      <p>Heartfelt Poems, Quotes and Pictures of Love</p>
    </div>`;
  }else if(page==='poems'){
    let html = `<div class="page-section"><h2>Poems</h2><ul>`;
    poems.forEach(p=>{html+=`<li>${p.text}${session?` <button class="btn btn-sm btn-danger" onclick="deleteItem('poem',${p.id})">Delete</button>`:""}</li>`});
    html+=`</ul>`;
    if(session){
      html+=`<textarea id="new-poem" class="form-control" placeholder="Add new poem"></textarea>
      <button class="btn btn-sm btn-success mt-2" onclick="addItem('poem')">Add Poem</button>`;
    }
    html+=`</div>`;
    container.innerHTML = html;
  }else if(page==='quotes'){
    let html = `<div class="page-section"><h2>Quotes</h2><ul>`;
    quotes.forEach(q=>{html+=`<li>${q.text}${session?` <button class="btn btn-sm btn-danger" onclick="deleteItem('quote',${q.id})">Delete</button>`:""}</li>`});
    html+=`</ul>`;
    if(session){
      html+=`<textarea id="new-quote" class="form-control" placeholder="Add new quote"></textarea>
      <button class="btn btn-sm btn-success mt-2" onclick="addItem('quote')">Add Quote</button>`;
    }
    html+=`</div>`;
    container.innerHTML = html;
  }else if(page==='gallery'){
    let html = `<div class="page-section"><h2>Gallery</h2><div class="row">`;
    gallery.forEach(g=>{html+=`<div class="col-md-4 mb-3"><img class="card-img-thumb" src="${g.src}" />
      ${session?`<button class="btn btn-sm btn-danger mt-1" onclick="deleteItem('gallery',${g.id})">Delete</button>`:""}</div>`});
    html+=`</div>`;
    if(session){
      html+=`<input id="new-gallery" class="form-control mt-2" placeholder="New image URL">
      <button class="btn btn-sm btn-success mt-1" onclick="addItem('gallery')">Add Image</button>`;
    }
    html+=`</div>`;
    container.innerHTML = html;
  }else if(page==='contact'){
    container.innerHTML = `<div class="page-section"><h2>Contact</h2>
      <form>
      <input class="form-control" placeholder="Name">
      <input class="form-control" placeholder="Email">
      <textarea class="form-control" placeholder="Message"></textarea>
      <button class="btn btn-success mt-2">Send</button>
      </form>
    </div>`;
  }else if(page==='admin'){
    if(session){
      container.innerHTML = `<div class="page-section"><h2>Admin Panel</h2>
      <p>Welcome ${session.name}! You can add/delete Poems, Quotes, Gallery items.</p></div>`;
    }else{
      container.innerHTML = `<div class="page-section"><h2>Admin Login / Signup</h2>
      <input class="form-control mb-2" id="admin-name" placeholder="Name (for signup)">
      <input class="form-control mb-2" id="admin-email" placeholder="Email">
      <input type="password" class="form-control mb-2" id="admin-pass" placeholder="Password">
      <button class="btn btn-sm btn-primary me-2" onclick="adminLogin()">Login</button>
      <button class="btn btn-sm btn-success" onclick="adminSignup()">Signup</button>
      </div>`;
    }
  }
}

// Admin functions
function adminLogin(){
  let email = document.getElementById('admin-email').value;
  let pass = document.getElementById('admin-pass').value;
  // simple frontend simulation
  if(email && pass){ session={name:"Admin", email:email}; showPage('admin'); alert('Logged in!'); }
}
function adminSignup(){
  let name = document.getElementById('admin-name').value;
  let email = document.getElementById('admin-email').value;
  let pass = document.getElementById('admin-pass').value;
  if(name && email && pass){ session={name:name,email:email}; showPage('admin'); alert('Signup successful!'); }
}

function addItem(type){
  if(type==='poem'){ let txt=document.getElementById('new-poem').value; if(txt) poems.push({id:Date.now(),text:txt}); showPage('poems'); }
  if(type==='quote'){ let txt=document.getElementById('new-quote').value; if(txt) quotes.push({id:Date.now(),text:txt}); showPage('quotes'); }
  if(type==='gallery'){ let txt=document.getElementById('new-gallery').value; if(txt) gallery.push({id:Date.now(),src:txt}); showPage('gallery'); }
}

function deleteItem(type,id){
  if(type==='poem'){ poems = poems.filter(p=>p.id!==id); showPage('poems'); }
  if(type==='quote'){ quotes = quotes.filter(q=>q.id!==id); showPage('quotes'); }
  if(type==='gallery'){ gallery = gallery.filter(g=>g.id!==id); showPage('gallery'); }
}

// default page
showPage('home');
</script>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>


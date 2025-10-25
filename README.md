# Creating a full mock project for FH Educativo with multiple files and zipping it for download.
import os, zipfile, textwrap, json, pathlib, sys

base = "/mnt/data/fh_educativo_code_mock"
os.makedirs(base, exist_ok=True)

files = {
"index.html": """<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>FH Educativo - Inicio</title>
  <link rel="stylesheet" href="css/style.css">
  <script defer src="js/main.js"></script>
</head>
<body>
  <header class="site-header">
    <div class="brand">
      <img src="img/logo.png" alt="FH Educativo" class="logo">
      <span class="brand-name">FH Educativo</span>
    </div>
    <nav class="main-nav">
      <a href="index.html">Inicio</a>
      <a href="tienda.html">Tienda</a>
      <a href="contacto.html">Contacto</a>
      <a href="admin/login.html" class="btn-small">Administrador</a>
    </nav>
  </header>

  <main>
    <section class="hero">
      <div class="hero-inner">
        <h1>Juguetes educativos que inspiran aprendizaje</h1>
        <p>Marcas: Janod, Lilliputiens y más. Filtra por edad y tipo.</p>
        <a class="btn" href="tienda.html">Explorar tienda</a>
      </div>
    </section>

    <section class="features">
      <article>
        <h3>Curaduría por edades</h3>
        <p>Productos seleccionados por etapas de desarrollo.</p>
      </article>
      <article>
        <h3>Enlaces de compra seguros</h3>
        <p>Cada producto tiene un enlace de pago externo (One-Pay).</p>
      </article>
      <article>
        <h3>Panel administrativo</h3>
        <p>Gestión de inventario, proveedores y reportes.</p>
      </article>
    </section>

    <section class="highlight-products">
      <h2>Productos destacados</h2>
      <div id="featured" class="product-grid"></div>
    </section>
  </main>

  <footer class="site-footer">
    <p>&copy; 2025 FH Educativo — Proyecto académico</p>
  </footer>
</body>
</html>""",

"tienda.html": """<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>FH Educativo - Tienda</title>
  <link rel="stylesheet" href="css/style.css">
  <script defer src="js/catalog.js"></script>
</head>
<body>
  <header class="site-header">
    <div class="brand"><img src="img/logo.png" alt=""><span>FH Educativo</span></div>
    <nav class="main-nav"><a href="index.html">Inicio</a><a href="tienda.html">Tienda</a><a href="contacto.html">Contacto</a></nav>
  </header>

  <main class="container">
    <h1>Tienda</h1>
    <div class="filters">
      <label>Edad:
        <select id="filter-age">
          <option value="all">Todas</option>
          <option value="0-3">0 - 3 años</option>
          <option value="3-6">3 - 6 años</option>
          <option value="6+">6 años o más</option>
        </select>
      </label>
      <label>Marca:
        <select id="filter-brand">
          <option value="all">Todas</option>
          <option value="Janod">Janod</option>
          <option value="Lilliputiens">Lilliputiens</option>
        </select>
      </label>
      <button id="applyFilters" class="btn-small">Aplicar</button>
    </div>

    <div id="productList" class="product-grid"></div>
  </main>

  <footer class="site-footer"><p>&copy; FH Educativo</p></footer>
</body>
</html>""",

"producto.html": """<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>FH Educativo - Producto</title>
  <link rel="stylesheet" href="css/style.css">
  <script defer src="js/producto.js"></script>
</head>
<body>
  <header class="site-header">
    <div class="brand"><img src="../img/logo.png" alt=""><span>FH Educativo</span></div>
    <nav class="main-nav"><a href="../index.html">Inicio</a><a href="../tienda.html">Tienda</a></nav>
  </header>

  <main class="container">
    <div id="productDetail" class="product-detail"></div>
  </main>

  <footer class="site-footer"><p>&copy; FH Educativo</p></footer>
</body>
</html>""",

"contacto.html": """<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Contacto - FH Educativo</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>
  <header class="site-header">
    <div class="brand"><img src="img/logo.png" alt=""><span>FH Educativo</span></div>
    <nav class="main-nav"><a href="index.html">Inicio</a><a href="tienda.html">Tienda</a></nav>
  </header>

  <main class="container">
    <h1>Contacto</h1>
    <form id="contactForm">
      <label>Nombre<input type="text" name="nombre" required></label>
      <label>Email<input type="email" name="email" required></label>
      <label>Mensaje<textarea name="mensaje" rows="6" required></textarea></label>
      <button class="btn" type="submit">Enviar</button>
    </form>
  </main>

  <footer class="site-footer"><p>&copy; FH Educativo</p></footer>
  <script>
    document.getElementById('contactForm').addEventListener('submit', function(e){
      e.preventDefault();
      alert('Formulario enviado (simulado).');
    });
  </script>
</body>
</html>""",

"admin/login.html": """<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Admin - Login</title>
<link rel="stylesheet" href="../css/style.css">
<script defer src="../js/admin-login.js"></script>
</head>
<body>
  <main class="container">
    <div class="login-card">
      <h2>Acceso Administrativo</h2>
      <form id="loginForm">
        <label>Usuario<input type="text" id="usuario" required></label>
        <label>Contraseña<input type="password" id="clave" required></label>
        <button class="btn" type="submit">Iniciar sesión</button>
        <div id="msg" class="msg"></div>
      </form>
    </div>
  </main>
</body>
</html>""",

"admin/panel.html": """<!DOCTYPE html>
<html lang="es">
<head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>Panel - FH Educativo</title>
<link rel="stylesheet" href="../css/style.css"><script defer src="../js/admin-panel.js"></script></head>
<body>
  <header class="site-header">
    <div class="brand"><img src="../img/logo.png" alt=""><span>FH Educativo</span></div>
    <nav class="main-nav"><a href="../index.html">Inicio</a><a href="../tienda.html">Tienda</a></nav>
  </header>
  <main class="container">
    <h1>Panel Administrativo</h1>
    <section class="admin-grid">
      <div class="card">
        <h3>Inventario</h3>
        <div id="invList"></div>
      </div>
      <div class="card">
        <h3>Órdenes de compra</h3>
        <div id="ocList"></div>
      </div>
    </section>
  </main>
</body>
</html>""",

"css/style.css": """/* FH Educativo - estilos principales */
:root{
  --turquesa: #1EA7A8;
  --coral: #EF5350;
  --amarillo: #F6C33B;
  --gris:#5b6470;
  --bg:#ffffff;
  --max-width:1100px;
  --radius:10px;
}

*{box-sizing:border-box}
body{font-family: 'Bellota', Arial, sans-serif; margin:0; color:var(--gris); background:#fbfcfd;}

.site-header{display:flex;justify-content:space-between;align-items:center;padding:12px 20px;background:var(--turquesa);color:white}
.brand{display:flex;align-items:center;gap:10px}
.logo{width:48px;height:48px;border-radius:8px;background:#fff;padding:6px}
.main-nav a{color:white;text-decoration:none;margin-left:16px;font-weight:600}
.btn{background:var(--coral);color:white;padding:10px 14px;border-radius:8px;text-decoration:none;display:inline-block}
.btn-small{background:rgba(255,255,255,0.15);padding:6px 10px;border-radius:6px;color:white}

.hero{background:linear-gradient(90deg,#f0fcfb, #ffffff);padding:70px 20px;text-align:center}
.hero .hero-inner{max-width:var(--max-width);margin:0 auto}
.features{display:flex;gap:20px;justify-content:center;padding:40px 20px}
.features article{background:white;padding:20px;border-radius:var(--radius);width:280px;box-shadow:0 6px 18px rgba(0,0,0,0.06)}

.container{max-width:var(--max-width);margin:20px auto;padding:0 16px}

.product-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:18px;margin-top:20px}
.card-product{background:white;border-radius:12px;padding:12px;box-shadow:0 8px 24px rgba(0,0,0,0.06)}
.card-product img{width:100%;height:180px;object-fit:cover;border-radius:8px}
.card-product h4{margin:8px 0 4px}
.price{font-weight:700;color:var(--gris)}

.filters{display:flex;gap:10px;align-items:center;padding:12px 0}
.filters label{font-size:14px;color:var(--gris)}

/* admin */
.login-card{max-width:420px;margin:80px auto;background:white;padding:24px;border-radius:12px;box-shadow:0 10px 30px rgba(0,0,0,0.08)}
.msg{color:var(--coral);margin-top:10px}

/* responsive */
@media (max-width:600px){
  .features{flex-direction:column;align-items:center}
  .site-header{flex-direction:column;gap:10px;padding:14px}
}""",

"js/main.js": """// main.js - carga datos de ejemplo y rellena destacados
const catalogSeed = [
  { id:1, sku:'JAN-001-01', title:'Juego de Equilibrio Flamenco Rosa', brand:'Janod', price:280, age:'3-6', img:'img/products/jan001-1.jpg', link:'https://onepay.example.com/pay?order=JAN-001-01' },
  { id:2, sku:'LIL-010-01', title:'Hipopótamo de Peluche Educativo', brand:'Lilliputiens', price:350, age:'0-3', img:'img/products/lil010-1.jpg', link:'https://onepay.example.com/pay?order=LIL-010-01' },
  { id:3, sku:'JAN-007-01', title:'Bloques Didácticos (Caja 20pcs)', brand:'Janod', price:180, age:'2-5', img:'img/products/jan007-1.jpg', link:null }
];
document.addEventListener('DOMContentLoaded', () => {
  const featured = document.getElementById('featured');
  if(!featured) return;
  // render featured products
  catalogSeed.slice(0,3).forEach(p => {
    const card = document.createElement('div'); card.className='card-product';
    card.innerHTML = `<img src="${p.img}" alt="${p.title}"><h4>${p.title}</h4><div class="price">Q.${p.price.toFixed(2)}</div><a class="btn-small" href="producto.html?sku=${p.sku}">Ver</a>`;
    featured.appendChild(card);
  });
});""",

"js/catalog.js": """// catalog.js - controla filtros y listado
const seed = [
  { id:1, sku:'JAN-001-01', title:'Juego de Equilibrio Flamenco Rosa', brand:'Janod', price:280, ageMin:3, ageMax:6, age:'3-6', img:'img/products/jan001-1.jpg', link:'https://onepay.example.com/pay?order=JAN-001-01' },
  { id:2, sku:'LIL-010-01', title:'Hipopótamo de Peluche Educativo', brand:'Lilliputiens', price:350, ageMin:0, ageMax:3, age:'0-3', img:'img/products/lil010-1.jpg', link:'https://onepay.example.com/pay?order=LIL-010-01' },
  { id:3, sku:'JAN-007-01', title:'Bloques Didácticos (Caja 20pcs)', brand:'Janod', price:180, ageMin:2, ageMax:5, age:'2-5', img:'img/products/jan007-1.jpg', link:null }
];

function renderList(list){
  const target = document.getElementById('productList');
  target.innerHTML='';
  list.forEach(p=>{
    const el = document.createElement('div'); el.className='card-product';
    el.innerHTML = `<img src="${p.img}" alt="${p.title}"><h4>${p.title}</h4><p>${p.brand} • ${p.age} años</p><div class="price">Q.${p.price.toFixed(2)}</div><a class="btn-small" href="producto.html?sku=${p.sku}">Ver</a>`;
    target.appendChild(el);
  });
}

document.addEventListener('DOMContentLoaded',()=>{
  renderList(seed);
  document.getElementById('applyFilters').addEventListener('click', ()=>{
    const age = document.getElementById('filter-age').value;
    const brand = document.getElementById('filter-brand').value;
    let out = seed.slice();
    if(brand!=='all') out = out.filter(x=>x.brand===brand);
    if(age!=='all'){
      if(age==='0-3') out = out.filter(x=>x.ageMin<=3);
      if(age==='3-6') out = out.filter(x=>x.ageMin>=3 && x.ageMax<=6);
      if(age==='6+') out = out.filter(x=>x.ageMax>=6);
    }
    renderList(out);
  });
});""",

"js/producto.js": """// producto.js - muestra detalle según query string (simulado)
const query = new URLSearchParams(window.location.search);
const sku = query.get('sku');
const productMap = {
  'JAN-001-01': { title:'Juego de Equilibrio Flamenco Rosa', price:280, imgs:['img/products/jan001-1.jpg','img/products/jan001-2.jpg'], age:'3-6', desc:'Juego de madera ...', link:'https://onepay.example.com/pay?order=JAN-001-01' },
  'LIL-010-01': { title:'Hipopótamo de Peluche Educativo', price:350, imgs:['img/products/lil010-1.jpg'], age:'0-3', desc:'Peluche sensorial ...', link:'https://onepay.example.com/pay?order=LIL-010-01' },
  'JAN-007-01': { title:'Bloques Didácticos (Caja 20pcs)', price:180, imgs:['img/products/jan007-1.jpg'], age:'2-5', desc:'Conjunto de bloques ...', link:null }
};

document.addEventListener('DOMContentLoaded', ()=>{
  const target = document.getElementById('productDetail');
  const p = productMap[sku] || productMap['JAN-001-01'];
  target.innerHTML = `
    <div class="product-detail-inner">
      <div class="gallery"><img src="${p.imgs[0]}" alt="${p.title}"></div>
      <div class="meta">
        <h2>${p.title}</h2>
        <p class="price">Q.${p.price.toFixed(2)}</p>
        <p>${p.desc}</p>
        <ul class="attrs"><li>Edad: ${p.age} años</li><li>Material: Madera / Tela</li></ul>
        ${p.link? `<a class="btn" href="${p.link}" target="_blank" rel="noopener">Comprar con tarjeta</a>` : '<button class="btn-small" disabled>Sin enlace de pago</button>'}
      </div>
    </div>
  `;
});""",

"js/admin-login.js": """// admin-login.js - simula autenticación en frontend (demo)
document.addEventListener('DOMContentLoaded', ()=>{
  const form = document.getElementById('loginForm');
  const msg = document.getElementById('msg');
  form.addEventListener('submit', (e)=>{
    e.preventDefault();
    const u = document.getElementById('usuario').value.trim();
    const p = document.getElementById('clave').value.trim();
    // credenciales simuladas
    if(u==='admin' && p==='admin123'){
      msg.textContent='Acceso concedido. Redirigiendo...';
      msg.style.color='green';
      setTimeout(()=> window.location.href='panel.html',800);
    } else {
      msg.textContent='Usuario o contraseña inválidos.';
      msg.style.color='#ef5350';
    }
  });
});""",

"js/admin-panel.js": """// admin-panel.js - rellena panel con datos simulados
const inv = [
  { sku:'JAN-001-01', title:'Flamenco', qty:6, min:5 },
  { sku:'LIL-010-01', title:'Hipopótamo', qty:2, min:3 },
  { sku:'JAN-007-01', title:'Bloques', qty:0, min:2 }
];
const oc = [
  { ref:'OC-1001', prov:'Importadora Juguetes S.A.', estado:'recepcionada', total:2800 },
  { ref:'OC-1002', prov:'Distribuciones Lili', estado:'abierta', total:1750 }
];
document.addEventListener('DOMContentLoaded', ()=>{
  const invList = document.getElementById('invList');
  inv.forEach(i=>{
    const el = document.createElement('div'); el.className='item';
    el.innerHTML = `<strong>${i.sku}</strong> - ${i.title} <br> Cant: ${i.qty} (min ${i.min})`;
    invList.appendChild(el);
  });
  const ocList = document.getElementById('ocList');
  oc.forEach(o=>{
    const el = document.createElement('div'); el.className='item';
    el.innerHTML = `<strong>${o.ref}</strong> - ${o.prov} <br>Estado: ${o.estado} - Q.${o.total.toFixed(2)}`;
    ocList.appendChild(el);
  });
});""",

"img/logo.png": "",  # placeholder empty files for images
"img/products/jan001-1.jpg": "",
"img/products/jan001-2.jpg": "",
"img/products/lil010-1.jpg": "",
"img/products/jan007-1.jpg": ""
}

# Create directories and files
for path, content in files.items():
    fullpath = os.path.join(base, path)
    os.makedirs(os.path.dirname(fullpath), exist_ok=True)
    # write binary for images as empty placeholder, text for others
    if path.endswith('.png') or path.endswith('.jpg'):
        open(fullpath, 'wb').close()
    else:
        with open(fullpath, 'w', encoding='utf-8') as f:
            f.write(content)

# create README
readme = """FH Educativo - Proyecto falso (mock) para presentar código.
Estructura de archivos con HTML/CSS/JS simulados.
- Abrir index.html en un navegador para ver la demo local.
- Archivos de admin están en /admin (login simulado).
- Datos son estáticos en JS (catalogSeed / seed arrays).
- Incluye SQL demo por separado (fh_educativo_seed.sql).
"""
with open(os.path.join(base, "README.txt"), "w", encoding="utf-8") as f:
    f.write(readme)

# zip the folder
zip_path = "/mnt/data/fh_educativo_mock_project.zip"
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as zf:
    for root, dirs, filenames in os.walk(base):
        for fn in filenames:
            full = os.path.join(root, fn)
            arcname = os.path.relpath(full, base)
            zf.write(full, arcname)

zip_path


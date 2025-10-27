<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Tienda de Promociones - Distribuidora</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f9;
      margin: 0;
      padding: 0;
    }

    header {
      background: #3498db;
      color: white;
      padding: 15px;
      text-align: center;
      font-size: 22px;
      font-weight: bold;
    }

    main {
      max-width: 1100px;
      margin: 20px auto;
      background: white;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }

    select {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      border: 1px solid #ccc;
      border-radius: 6px;
    }

    .productos-container {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      margin-top: 20px;
      justify-content: flex-start;
    }

    .promo-card {
      width: 200px;
      border: 1px solid #ddd;
      border-radius: 10px;
      padding: 10px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
      background: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      transition: 0.2s;
    }

    .promo-card:hover {
      box-shadow: 0 6px 15px rgba(0,0,0,0.15);
      transform: translateY(-2px);
    }

    .promo-card img {
      width: 100%;
      height: 140px;
      object-fit: cover;
      border-radius: 8px;
      margin-bottom: 8px;
    }

    .promo-card h3 {
      font-size: 15px;
      margin: 0 0 5px 0;
      text-align: center;
    }

    .promo-card p {
      color: #e74c3c;
      font-weight: bold;
      margin: 5px 0;
    }

    .promo-card button {
      background: #27ae60;
      color: #fff;
      border: none;
      padding: 6px 10px;
      border-radius: 6px;
      cursor: pointer;
      margin-top: auto;
    }

    .promo-card button:hover {
      background: #219150;
    }

    #carritoResumen {
      margin-top: 30px;
      background: #f8f8f8;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 16px;
    }

    #carritoLista div {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin: 6px 0;
    }

    #carritoLista span {
      font-weight: bold;
      color: #333;
    }

    .carrito-btns button {
      margin: 0 3px;
      background: #3498db;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      padding: 3px 6px;
    }

    .carrito-btns button:hover {
      background: #2c80ba;
    }

    .btn-eliminar {
      background: #e74c3c !important;
    }

    .btn-confirmar {
      width: 100%;
      padding: 12px;
      margin-top: 10px;
      background: #007bff;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      font-weight: bold;
    }

    .btn-confirmar:hover {
      background: #0056b3;
    }

    input {
      width: 100%;
      padding: 8px;
      margin-top: 8px;
      border: 1px solid #ccc;
      border-radius: 6px;
    }

    footer {
      text-align: center;
      padding: 15px;
      font-size: 14px;
      color: #777;
    }
  </style>

  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
</head>

<body>
  <header>🛍️ Tienda de Promociones</header>

  <main>
    
      <h3>🛒 Mi Pedido (<span id="carritoCount">0</span>)</h3>
      <div id="carritoLista"><span style="color:#888;">No hay productos seleccionados</span></div>
      <hr>
      <p><b>Total:</b> <span id="carritoTotal">$0.00</span></p>

      <h3>Datos del Cliente</h3>
      <input id="clienteNombre" type="text" placeholder="Nombre y Apellido" required>
      <select id="clienteLocalidad" required>
        <option value="">Seleccionar localidad...</option>
      </select>

      <button class="btn-confirmar" onclick="confirmarPedido()">Confirmar y Enviar Pedido</button>
    </div>
<h2>Seleccione una Categoría</h2>
    <select id="categoriaSelect" onchange="cargarPromosPorCategoria()">
      <option value="">Seleccionar categoría...</option>
    </select>
<div id="productosContainer" class="productos-container"></div>

    <div id="carritoResumen">
  </main>

  <footer>Distribuidora Allana © 2025</footer>

  <script>
    const SUPABASE_URL = "https://ejbqlejlgprvwzrptojo.supabase.co";
    const SUPABASE_ANON_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImVqYnFsZWpsZ3Bydnd6cnB0b2pvIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTcwMjkwMjIsImV4cCI6MjA3MjYwNTAyMn0.9fVItJka0MN3nlfowVudIE0AuNmfTJGQcbbqpau2Th8";
    const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

    let carritoCliente = [];

    // ================== CARGA CATEGORÍAS ==================
    async function cargarCategorias() {
      const { data, error } = await supabase.from("categorias").select("*").order("id");
      if (error) return console.error(error);

      const select = document.getElementById("categoriaSelect");
      select.innerHTML = '<option value="">Seleccionar categoría...</option>';
      data.forEach(cat => {
        const opt = document.createElement("option");
        opt.value = cat.id;
        opt.textContent = cat.nombre;
        select.appendChild(opt);
      });
    }

    // ================== CARGA LOCALIDADES ==================
    async function cargarLocalidades() {
      const { data, error } = await supabase.from("localidades").select("*").order("nombre", { ascending: true });
      if (error) return console.error(error);

      const select = document.getElementById("clienteLocalidad");
      select.innerHTML = '<option value="">Seleccionar localidad...</option>';
      data.forEach(loc => {
        const opt = document.createElement("option");
        opt.value = loc.id;
        opt.textContent = loc.nombre;
        select.appendChild(opt);
      });
    }

    // ================== CARGA PROMOS POR CATEGORÍA ==================
    async function cargarPromosPorCategoria() {
      const catId = document.getElementById("categoriaSelect").value;
      const cont = document.getElementById("productosContainer");
      cont.innerHTML = "";

      if (!catId) return;

      const { data: promos, error } = await supabase.from("promociones").select("*").eq("categoria_id", catId);
      if (error) {
        cont.innerHTML = "<p style='color:red;'>Error al cargar productos.</p>";
        return;
      }

      if (!promos.length) {
        cont.innerHTML = "<p>No hay productos en esta categoría.</p>";
        return;
      }

      promos.forEach(p => {
        const card = document.createElement("div");
        card.className = "promo-card";
        const imgURL = p.imagen
          ? `https://ejbqlejlgprvwzrptojo.supabase.co/storage/v1/object/public/promo_images/${p.imagen}`
          : "";

        card.innerHTML = `
          <img src="${imgURL}" alt="${p.nombre}">
          <h3>${p.nombre}</h3>
          <p>$${Number(p.precio).toFixed(2)}</p>
          <button onclick="agregarAlCarrito(${p.id}, '${p.nombre}', ${p.precio})">Agregar</button>
        `;
        cont.appendChild(card);
      });
    }

    // ================== CARRITO ==================
    function agregarAlCarrito(id, nombre, precio) {
      const item = carritoCliente.find(i => i.id === id);
      if (item) item.cantidad++;
      else carritoCliente.push({ id, nombre, precio, cantidad: 1 });
      renderCarrito();
    }

    function modificarCantidad(id, delta) {
      const item = carritoCliente.find(i => i.id === id);
      if (!item) return;
      item.cantidad += delta;
      if (item.cantidad <= 0)
        carritoCliente = carritoCliente.filter(p => p.id !== id);
      renderCarrito();
    }

    function eliminarDelCarrito(id) {
      carritoCliente = carritoCliente.filter(p => p.id !== id);
      renderCarrito();
    }

    function renderCarrito() {
      const lista = document.getElementById("carritoLista");
      const count = document.getElementById("carritoCount");
      const totalEl = document.getElementById("carritoTotal");

      lista.innerHTML = "";
      let total = 0;

      carritoCliente.forEach(p => {
        total += p.precio * p.cantidad;
        const row = document.createElement("div");
        row.innerHTML = `
          <span>${p.nombre} (${p.cantidad}x)</span>
          <div class="carrito-btns">
            <button onclick="modificarCantidad(${p.id}, -1)">−</button>
            <button onclick="modificarCantidad(${p.id}, 1)">+</button>
            <button class="btn-eliminar" onclick="eliminarDelCarrito(${p.id})">✕</button>
          </div>
        `;
        lista.appendChild(row);
      });

      count.textContent = carritoCliente.length;
      totalEl.textContent = "$" + total.toFixed(2);

      if (carritoCliente.length === 0)
        lista.innerHTML = '<span style="color:#888;">No hay productos seleccionados</span>';
    }

    // ================== CONFIRMAR PEDIDO ==================
    async function confirmarPedido() {
      const nombre = document.getElementById("clienteNombre").value.trim();
      const localidad = document.getElementById("clienteLocalidad").value.trim();

      if (!nombre || !localidad || carritoCliente.length === 0) {
        alert("Complete los datos y agregue al menos un producto.");
        return;
      }

      const pedido = {
        cliente: nombre,
        localidad_id: localidad,
        productos: carritoCliente,
        created_at: new Date().toISOString(),
        estado: "pendiente"
      };

      const { error } = await supabase.from("pedidos").insert(pedido);
      if (error) {
        alert("Error al enviar el pedido: " + error.message);
        console.error(error);
      } else {
        alert("✅ Pedido enviado correctamente. ¡Gracias!");
        carritoCliente = [];
        renderCarrito();
        document.getElementById("clienteNombre").value = "";
        document.getElementById("clienteLocalidad").value = "";
      }
    }

    // ================== INICIALIZACIÓN ==================
    cargarCategorias();
    cargarLocalidades();
  </script>
</body>
</html>

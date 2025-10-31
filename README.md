<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SHEIN - Categorías</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            display: flex;
            min-height: 100vh;
            background-color: #f8f8f8;
        }

        /* Panel lateral de categorías */
        .categories-panel {
            width: 250px;
            background-color: white;
            padding: 20px 0;
            box-shadow: 2px 0 5px rgba(0,0,0,0.1);
            overflow-y: auto;
            height: 100vh;
            position: fixed;
            left: 0;
            top: 0;
        }

        .categories-panel h2 {
            font-size: 18px;
            margin-bottom: 15px;
            padding: 0 20px;
            color: #333;
        }

        .categories-panel ul {
            list-style: none;
        }

        .categories-panel li {
            padding: 12px 20px;
            border-bottom: 1px solid #f0f0f0;
            font-size: 14px;
            color: #555;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .categories-panel li:hover {
            background-color: #f9f9f9;
        }

        .categories-panel li.active {
            background-color: #f2f2f2;
            font-weight: bold;
            color: #ff4747;
        }

        /* Contenido principal */
        .main-content {
            flex: 1;
            margin-left: 250px;
            padding: 20px;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .logo {
            font-size: 24px;
            font-weight: bold;
            color: #ff4747;
        }

        .search-bar {
            display: flex;
            align-items: center;
            background: white;
            border-radius: 20px;
            padding: 8px 15px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            width: 300px;
        }

        .search-bar input {
            border: none;
            outline: none;
            flex: 1;
            padding: 5px;
        }

        .search-bar i {
            color: #888;
        }

        .user-actions {
            display: flex;
            gap: 15px;
        }

        .user-actions i {
            font-size: 20px;
            color: #555;
            cursor: pointer;
        }

        .banner {
            background-color: #ff4747;
            color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            text-align: center;
        }

        .category-title {
            font-size: 22px;
            margin-bottom: 15px;
            color: #333;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .products-count {
            font-size: 14px;
            color: #777;
            font-weight: normal;
        }

        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 20px;
        }

        .product-card {
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            transition: transform 0.3s;
        }

        .product-card:hover {
            transform: translateY(-5px);
        }

        .product-image {
            height: 200px;
            background-color: #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #888;
        }

        .product-info {
            padding: 15px;
        }

        .product-title {
            font-size: 14px;
            margin-bottom: 5px;
        }

        .product-price {
            font-weight: bold;
            color: #ff4747;
        }

        /* Estados de carga y vacío */
        .loading {
            text-align: center;
            padding: 40px;
            color: #777;
        }

        .no-products {
            text-align: center;
            padding: 40px;
            color: #777;
        }

        /* Responsive para móviles */
        @media (max-width: 768px) {
            .categories-panel {
                width: 100%;
                height: auto;
                position: relative;
                padding: 15px 0;
            }

            .categories-panel h2 {
                font-size: 20px;
                padding: 0 15px;
            }

            .categories-panel li {
                padding: 15px;
                font-size: 16px;
            }

            .main-content {
                margin-left: 0;
                padding: 15px;
            }

            .header {
                flex-direction: column;
                gap: 15px;
            }

            .search-bar {
                width: 100%;
            }

            .products-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
                gap: 15px;
            }

            .product-image {
                height: 150px;
            }
        }

        /* Responsive para tablets */
        @media (max-width: 1024px) and (min-width: 769px) {
            .categories-panel {
                width: 200px;
            }

            .main-content {
                margin-left: 200px;
            }
        }
    </style>
</head>
<body>
    <!-- Panel lateral de categorías -->
    <div class="categories-panel">
        <h2>Categorías</h2>
        <ul>
            <li class="active" data-category="all">Todo</li>
            <li data-category="mujer">Mujer</li>
            <li data-category="curvy">Curvy</li>
            <li data-category="ninos">Niños</li>
            <li data-category="hombre">Hombre</li>
            <li data-category="hogar">Hogar</li>
        </ul>

        <h2 style="margin-top: 30px;">Solo para ti</h2>
        <ul>
            <li data-category="seleccion">Selección para ti</li>
            <li data-category="novedades">Novedades</li>
            <li data-category="ofertas">Ofertas</li>
            <li data-category="ropa-mujer">Ropa de mujer</li>
            <li data-category="sudaderas-hombre">Sudadera con Capucha para Hombre</li>
            <li data-category="camisetas-hombre">Camisetas de hombre</li>
            <li data-category="conjuntos">Conjuntos de sudadera</li>
            <li data-category="trajes-bano">Trajes de baño</li>
            <li data-category="curvy">Curvy</li>
            <li data-category="ninos">Niños</li>
            <li data-category="tops-hombre">Tops de Punto para Hombre</li>
            <li data-category="camisetas-ninas">Camisetas de Niñas (3-7)</li>
            <li data-category="conjuntos-camiseta">Conjuntos de Camiseta para Hombre</li>
            <li data-category="ropa-hombre">Ropa para hombre</li>
            <li data-category="ropa-interior">Ropa interior y ropa para dormir</li>
            <li data-category="bisuteria">Bisutería y accesorios</li>
            <li data-category="sudaderas">Sudadera para hombre</li>
            <li data-category="shorts-deportivos">Shorts deportivos para hombre</li>
            <li data-category="bodies-bebe">Bodies bebé recién nacido</li>
            <li data-category="zapatos">Zapatos</li>
            <li data-category="hogar-cocina">Hogar y cocina</li>
            <li data-category="deportes">Deportes y aire libre</li>
            <li data-category="bebe-maternidad">Bebé y maternidad</li>
            <li data-category="brochas">Sets de Brochas</li>
            <li data-category="pantalones-hombre">Pantalones para hombre</li>
            <li data-category="fundas-moviles">Fundas básicas para móviles</li>
            <li data-category="bolsas">Bolsas y equipaje</li>
            <li data-category="belleza">Belleza y salud</li>
            <li data-category="gustarte">También podría gustarte</li>
        </ul>
    </div>

    <!-- Contenido principal -->
    <div class="main-content">
        <div class="header">
            <div class="logo">SHEIN</div>
            <div class="search-bar">
                <input type="text" placeholder="Buscar productos...">
                <i>🔍</i>
            </div>
            <div class="user-actions">
                <i>❤️</i>
                <i>🛒</i>
                <i>👤</i>
            </div>
        </div>

        <div class="banner">
            <h3>¡Ofertas especiales! Hasta 70% de descuento</h3>
        </div>

        <div class="category-title">
            <span>Productos destacados</span>
            <span class="products-count">6 productos</span>
        </div>

        <div class="products-grid" id="products-container">
            <!-- Los productos se cargarán dinámicamente aquí -->
        </div>
    </div>

    <script>
        // Base de datos simulada de productos
        const productos = {
            all: [
                { id: 1, nombre: "Vestido floral verano", precio: "$24.99", categoria: "mujer" },
                { id: 2, nombre: "Jeans ajustados", precio: "$29.99", categoria: "mujer" },
                { id: 3, nombre: "Camiseta básica", precio: "$12.99", categoria: "hombre" },
                { id: 4, nombre: "Falda plisada", precio: "$19.99", categoria: "mujer" },
                { id: 5, nombre: "Sudadera con capucha", precio: "$34.99", categoria: "hombre" },
                { id: 6, nombre: "Zapatos deportivos", precio: "$45.99", categoria: "zapatos" }
            ],
            mujer: [
                { id: 1, nombre: "Vestido floral verano", precio: "$24.99", categoria: "mujer" },
                { id: 2, nombre: "Jeans ajustados", precio: "$29.99", categoria: "mujer" },
                { id: 4, nombre: "Falda plisada", precio: "$19.99", categoria: "mujer" },
                { id: 7, nombre: "Blusa de seda", precio: "$22.99", categoria: "mujer" },
                { id: 8, nombre: "Abrigo invernal", precio: "$59.99", categoria: "mujer" }
            ],
            curvy: [
                { id: 9, nombre: "Vestido curvy estampado", precio: "$27.99", categoria: "curvy" },
                { id: 10, nombre: "Jeans curvy ajustados", precio: "$32.99", categoria: "curvy" },
                { id: 11, nombre: "Top curvy verano", precio: "$18.99", categoria: "curvy" }
            ],
            ninos: [
                { id: 12, nombre: "Conjunto infantil dinosaurio", precio: "$19.99", categoria: "ninos" },
                { id: 13, nombre: "Vestido niña flores", precio: "$15.99", categoria: "ninos" },
                { id: 14, nombre: "Pijama niño superhéroe", precio: "$16.99", categoria: "ninos" }
            ],
            hombre: [
                { id: 3, nombre: "Camiseta básica", precio: "$12.99", categoria: "hombre" },
                { id: 5, nombre: "Sudadera con capucha", precio: "$34.99", categoria: "hombre" },
                { id: 15, nombre: "Pantalón cargo hombre", precio: "$39.99", categoria: "hombre" },
                { id: 16, nombre: "Camisa formal hombre", precio: "$29.99", categoria: "hombre" }
            ],
            hogar: [
                { id: 17, nombre: "Juego de sábanas", precio: "$34.99", categoria: "hogar" },
                { id: 18, nombre: "Cojines decorativos", precio: "$12.99", categoria: "hogar" },
                { id: 19, nombre: "Manta polar", precio: "$24.99", categoria: "hogar" }
            ],
            ofertas: [
                { id: 20, nombre: "Chaqueta oferta especial", precio: "$19.99", categoria: "ofertas" },
                { id: 21, nombre: "Zapatillas deportivas", precio: "$29.99", categoria: "ofertas" }
            ],
            zapatos: [
                { id: 6, nombre: "Zapatos deportivos", precio: "$45.99", categoria: "zapatos" },
                { id: 22, nombre: "Sandalias verano", precio: "$22.99", categoria: "zapatos" },
                { id: 23, nombre: "Botas invierno", precio: "$59.99", categoria: "zapatos" }
            ]
        };

        // Función para cargar productos según la categoría seleccionada
        function cargarProductos(categoria) {
            const container = document.getElementById('products-container');
            const categoryTitle = document.querySelector('.category-title span:first-child');
            const productsCount = document.querySelector('.products-count');
            
            // Mostrar estado de carga
            container.innerHTML = '<div class="loading">Cargando productos...</div>';
            
            // Simular tiempo de carga
            setTimeout(() => {
                const productosCategoria = productos[categoria] || productos.all;
                
                if (productosCategoria.length === 0) {
                    container.innerHTML = '<div class="no-products">No hay productos en esta categoría</div>';
                    productsCount.textContent = '0 productos';
                    return;
                }
                
                // Actualizar título y contador
                categoryTitle.textContent = categoria === 'all' ? 'Productos destacados' : `Categoría: ${categoria}`;
                productsCount.textContent = `${productosCategoria.length} productos`;
                
                // Generar HTML de productos
                let html = '';
                productosCategoria.forEach(producto => {
                    html += `
                        <div class="product-card">
                            <div class="product-image">Imagen del producto</div>
                            <div class="product-info">
                                <div class="product-title">${producto.nombre}</div>
                                <div class="product-price">${producto.precio}</div>
                            </div>
                        </div>
                    `;
                });
                
                container.innerHTML = html;
            }, 500); // Simular tiempo de carga de 500ms
        }

        // JavaScript para manejar la selección de categorías
        document.addEventListener('DOMContentLoaded', function() {
            const categoryItems = document.querySelectorAll('.categories-panel li');
            
            // Cargar productos iniciales
            cargarProductos('all');
            
            categoryItems.forEach(item => {
                item.addEventListener('click', function() {
                    // Remover clase activa de todos los elementos
                    categoryItems.forEach(i => i.classList.remove('active'));
                    // Agregar clase activa al elemento clickeado
                    this.classList.add('active');
                    
                    // Obtener la categoría del atributo data-category
                    const categoria = this.getAttribute('data-category');
                    
                    // Cargar productos de la categoría seleccionada
                    cargarProductos(categoria);
                });
            });
        });
    </script>
</body>
</html>

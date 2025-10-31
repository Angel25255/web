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
            <li class="active">Todo</li>
            <li>Mujer</li>
            <li>Curvy</li>
            <li>Niños</li>
            <li>Hombre</li>
            <li>Hogar</li>
        </ul>

        <h2 style="margin-top: 30px;">Solo para ti</h2>
        <ul>
            <li>Selección para ti</li>
            <li>Novedades</li>
            <li>Ofertas</li>
            <li>Ropa de mujer</li>
            <li>Sudadera con Capucha para Hombre</li>
            <li>Camisetas de hombre</li>
            <li>Conjuntos de sudadera</li>
            <li>Trajes de baño</li>
            <li>Curvy</li>
            <li>Niños</li>
            <li>Tops de Punto para Hombre</li>
            <li>Camisetas de Niñas (3-7)</li>
            <li>Conjuntos de Camiseta para Hombre</li>
            <li>Ropa para hombre</li>
            <li>Ropa interior y ropa para dormir</li>
            <li>Bisutería y accesorios</li>
            <li>Sudadera para hombre</li>
            <li>Shorts deportivos para hombre</li>
            <li>Bodies bebé recién nacido</li>
            <li>Zapatos</li>
            <li>Hogar y cocina</li>
            <li>Deportes y aire libre</li>
            <li>Bebé y maternidad</li>
            <li>Sets de Brochas</li>
            <li>Pantalones para hombre</li>
            <li>Fundas básicas para móviles</li>
            <li>Bolsas y equipaje</li>
            <li>Belleza y salud</li>
            <li>También podría gustarte</li>
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

        <div class="products-grid">
            <!-- Producto 1 -->
            <div class="product-card">
                <div class="product-image">Imagen del producto</div>
                <div class="product-info">
                    <div class="product-title">Vestido floral verano</div>
                    <div class="product-price">$24.99</div>
                </div>
            </div>

            <!-- Producto 2 -->
            <div class="product-card">
                <div class="product-image">Imagen del producto</div>
                <div class="product-info">
                    <div class="product-title">Jeans ajustados</div>
                    <div class="product-price">$29.99</div>
                </div>
            </div>

            <!-- Producto 3 -->
            <div class="product-card">
                <div class="product-image">Imagen del producto</div>
                <div class="product-info">
                    <div class="product-title">Camiseta básica</div>
                    <div class="product-price">$12.99</div>
                </div>
            </div>

            <!-- Producto 4 -->
            <div class="product-card">
                <div class="product-image">Imagen del producto</div>
                <div class="product-info">
                    <div class="product-title">Falda plisada</div>
                    <div class="product-price">$19.99</div>
                </div>
            </div>

            <!-- Producto 5 -->
            <div class="product-card">
                <div class="product-image">Imagen del producto</div>
                <div class="product-info">
                    <div class="product-title">Sudadera con capucha</div>
                    <div class="product-price">$34.99</div>
                </div>
            </div>

            <!-- Producto 6 -->
            <div class="product-card">
                <div class="product-image">Imagen del producto</div>
                <div class="product-info">
                    <div class="product-title">Zapatos deportivos</div>
                    <div class="product-price">$45.99</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // JavaScript para manejar la selección de categorías
        document.addEventListener('DOMContentLoaded', function() {
            const categoryItems = document.querySelectorAll('.categories-panel li');
            
            categoryItems.forEach(item => {
                item.addEventListener('click', function() {
                    // Remover clase activa de todos los elementos
                    categoryItems.forEach(i => i.classList.remove('active'));
                    // Agregar clase activa al elemento clickeado
                    this.classList.add('active');
                });
            });
        });
    </script>
</body>
</html>

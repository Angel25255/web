<?php
// index.php - Página de INICIO para CLIENTES CON MENÚ COMPLETO
error_reporting(E_ALL);
ini_set('display_errors', 1);

// Conexión directa
$host = 'localhost';
$dbname = 'ventasropas';
$username = 'root';
$password = '';

try {
    $conexion = new PDO("mysql:host=$host;dbname=$dbname;charset=utf8", $username, $password);
    $conexion->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    
    // Obtener productos DESTACADOS para la página de inicio
    $sql_destacados = "SELECT p.*, c.nombre as categoria_nombre 
                      FROM productos p 
                      LEFT JOIN categorias c ON p.categoria_id = c.id 
                      WHERE p.destacado = 1 
                      ORDER BY p.id DESC LIMIT 8";
    $stmt_destacados = $conexion->prepare($sql_destacados);
    $stmt_destacados->execute();
    $productos_destacados = $stmt_destacados->fetchAll(PDO::FETCH_ASSOC);
    
    // Obtener productos NUEVOS para novedades
    $sql_novedades = "SELECT p.*, c.nombre as categoria_nombre 
                     FROM productos p 
                     LEFT JOIN categorias c ON p.categoria_id = c.id 
                     ORDER BY p.id DESC LIMIT 8";
    $stmt_novedades = $conexion->prepare($sql_novedades);
    $stmt_novedades->execute();
    $novedades = $stmt_novedades->fetchAll(PDO::FETCH_ASSOC);
    
    // Obtener categorías principales
    $sql_categorias = "SELECT * FROM categorias WHERE activo = 1 LIMIT 6";
    $stmt_categorias = $conexion->prepare($sql_categorias);
    $stmt_categorias->execute();
    $categorias = $stmt_categorias->fetchAll(PDO::FETCH_ASSOC);
    
} catch (PDOException $e) {
    $productos_destacados = [];
    $novedades = [];
    $categorias = [];
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ModaStore - Tienda de Moda Online</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #ff2e7d;
            --secondary-color: #ff6b9d;
            --dark-color: #1a1a1a;
            --light-color: #f8f9fa;
            --gray-color: #6c757d;
            --white: #ffffff;
            --border-color: #e9ecef;
            --shadow: 0 2px 10px rgba(0,0,0,0.1);
            --border-radius: 8px;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--white);
            color: var(--dark-color);
            line-height: 1.6;
        }
        
        /* Header Styles */
        .header {
            background: var(--white);
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            position: sticky;
            top: 0;
            z-index: 1000;
        }
        
        .top-bar {
            background: var(--dark-color);
            color: var(--white);
            padding: 8px 0;
            font-size: 12px;
        }
        
        .top-bar-content {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            padding: 0 20px;
        }
        
        .top-bar-links a {
            color: var(--white);
            text-decoration: none;
            margin-left: 15px;
        }
        
        .nav {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
        }
        
        .logo {
            font-size: 24px;
            font-weight: bold;
            color: var(--primary-color);
            display: flex;
            align-items: center;
        }
        
        .logo i {
            margin-right: 8px;
        }
        
        .search-bar {
            flex: 1;
            max-width: 500px;
            margin: 0 20px;
            position: relative;
        }
        
        .search-bar input {
            width: 100%;
            padding: 10px 15px;
            border: 1px solid var(--border-color);
            border-radius: 20px;
            font-size: 14px;
        }
        
        .search-bar button {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            color: var(--gray-color);
            cursor: pointer;
        }
        
        .nav-icons {
            display: flex;
            align-items: center;
        }
        
        .nav-icon {
            margin-left: 20px;
            text-align: center;
            color: var(--dark-color);
            text-decoration: none;
            font-size: 12px;
        }
        
        .nav-icon i {
            display: block;
            font-size: 20px;
            margin-bottom: 5px;
        }
        
        .nav-icon:hover {
            color: var(--primary-color);
        }
        
        /* Main Navigation */
        .main-nav {
            background: var(--white);
            border-top: 1px solid var(--border-color);
        }
        
        .main-nav-content {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            padding: 0 20px;
        }
        
        .main-nav-links {
            display: flex;
            list-style: none;
        }
        
        .main-nav-links li {
            margin-right: 25px;
        }
        
        .main-nav-links a {
            display: block;
            padding: 15px 0;
            text-decoration: none;
            color: var(--dark-color);
            font-weight: 500;
            position: relative;
        }
        
        .main-nav-links a:hover {
            color: var(--primary-color);
        }
        
        .main-nav-links a.active {
            color: var(--primary-color);
        }
        
        .main-nav-links a.active:after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background: var(--primary-color);
        }
        
        .promo-banner {
            background: var(--primary-color);
            color: var(--white);
            text-align: center;
            padding: 8px;
            font-size: 14px;
        }
        
        /* Hero Banner */
        .hero-banner {
            position: relative;
            height: 400px;
            background: linear-gradient(rgba(0,0,0,0.4), rgba(0,0,0,0.4)), url('https://images.unsplash.com/photo-1441986300917-64674bd600d8?ixlib=rb-4.0.3&auto=format&fit=crop&w=1350&q=80');
            background-size: cover;
            background-position: center;
            color: var(--white);
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            margin-bottom: 30px;
        }
        
        .hero-content {
            max-width: 600px;
            padding: 0 20px;
        }
        
        .hero-content h1 {
            font-size: 2.5rem;
            margin-bottom: 15px;
        }
        
        .hero-content p {
            font-size: 1.2rem;
            margin-bottom: 25px;
        }
        
        .btn {
            display: inline-block;
            padding: 12px 25px;
            background: var(--primary-color);
            color: var(--white);
            text-decoration: none;
            border-radius: 30px;
            font-weight: 600;
            transition: all 0.3s;
        }
        
        .btn:hover {
            background: var(--secondary-color);
            transform: translateY(-2px);
        }
        
        /* Categories Section */
        .section {
            max-width: 1200px;
            margin: 0 auto 40px;
            padding: 0 20px;
        }
        
        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .section-title {
            font-size: 1.5rem;
            font-weight: 700;
        }
        
        .view-all {
            color: var(--primary-color);
            text-decoration: none;
            font-weight: 600;
        }
        
        .categories-grid {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 15px;
        }
        
        .category-card {
            text-align: center;
            text-decoration: none;
            color: var(--dark-color);
            transition: transform 0.3s;
        }
        
        .category-card:hover {
            transform: translateY(-5px);
        }
        
        .category-icon {
            width: 70px;
            height: 70px;
            background: var(--light-color);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 10px;
            font-size: 24px;
            color: var(--primary-color);
        }
        
        .category-name {
            font-size: 14px;
            font-weight: 500;
        }
        
        /* Trends Section */
        .trends-section {
            background: var(--light-color);
            padding: 40px 0;
            margin-bottom: 40px;
        }
        
        .trends-header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .trends-title {
            font-size: 2rem;
            font-weight: 700;
            color: var(--dark-color);
            margin-bottom: 10px;
        }
        
        .trends-subtitle {
            color: var(--gray-color);
            font-size: 1rem;
        }
        
        /* Products Grid */
        .products-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
        }
        
        .product-card {
            background: var(--white);
            border-radius: var(--border-radius);
            overflow: hidden;
            box-shadow: var(--shadow);
            transition: transform 0.3s;
        }
        
        .product-card:hover {
            transform: translateY(-5px);
        }
        
        .product-image {
            width: 100%;
            height: 250px;
            object-fit: cover;
        }
        
        .product-info {
            padding: 15px;
        }
        
        .product-name {
            font-size: 14px;
            margin-bottom: 8px;
            height: 40px;
            overflow: hidden;
        }
        
        .product-price {
            font-weight: 700;
            color: var(--primary-color);
            font-size: 16px;
            margin-bottom: 5px;
        }
        
        .product-old-price {
            color: var(--gray-color);
            text-decoration: line-through;
            font-size: 12px;
            margin-bottom: 10px;
        }
        
        .product-actions {
            display: flex;
            justify-content: space-between;
        }
        
        .add-to-cart {
            background: var(--primary-color);
            color: var(--white);
            border: none;
            padding: 8px 15px;
            border-radius: 4px;
            font-size: 12px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .add-to-cart:hover {
            background: var(--secondary-color);
        }
        
        .wishlist {
            background: none;
            border: none;
            color: var(--gray-color);
            cursor: pointer;
            font-size: 16px;
        }
        
        .wishlist:hover {
            color: var(--primary-color);
        }
        
        /* Footer */
        .footer {
            background: var(--dark-color);
            color: var(--white);
            padding: 50px 0 20px;
        }
        
        .footer-content {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 30px;
            padding: 0 20px;
        }
        
        .footer-column h3 {
            font-size: 18px;
            margin-bottom: 20px;
            color: var(--white);
        }
        
        .footer-links {
            list-style: none;
        }
        
        .footer-links li {
            margin-bottom: 10px;
        }
        
        .footer-links a {
            color: #ccc;
            text-decoration: none;
            font-size: 14px;
            transition: color 0.3s;
        }
        
        .footer-links a:hover {
            color: var(--primary-color);
        }
        
        .footer-bottom {
            max-width: 1200px;
            margin: 40px auto 0;
            padding: 20px;
            border-top: 1px solid #444;
            text-align: center;
            font-size: 14px;
            color: #aaa;
        }
        
        /* Mobile Menu */
        .mobile-menu-toggle {
            display: none;
            background: none;
            border: none;
            font-size: 24px;
            color: var(--dark-color);
            cursor: pointer;
        }
        
        /* Responsive Styles */
        @media (max-width: 992px) {
            .products-grid {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .categories-grid {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .footer-content {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        
        @media (max-width: 768px) {
            .search-bar {
                display: none;
            }
            
            .nav-icons .nav-icon span {
                display: none;
            }
            
            .main-nav-links {
                display: none;
            }
            
            .mobile-menu-toggle {
                display: block;
            }
            
            .products-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .categories-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .hero-banner {
                height: 300px;
            }
            
            .hero-content h1 {
                font-size: 2rem;
            }
        }
        
        @media (max-width: 576px) {
            .products-grid {
                grid-template-columns: 1fr;
            }
            
            .footer-content {
                grid-template-columns: 1fr;
            }
            
            .top-bar-content {
                flex-direction: column;
                text-align: center;
            }
            
            .top-bar-links {
                margin-top: 5px;
            }
        }
    </style>
</head>
<body>
    <!-- Top Bar -->
    <div class="top-bar">
        <div class="top-bar-content">
            <div class="top-bar-text">¡Envío gratis en compras superiores a S/ 100!</div>
            <div class="top-bar-links">
                <a href="#">Seguimiento de pedido</a>
                <a href="#">Servicio al cliente</a>
                <a href="#">Mi cuenta</a>
            </div>
        </div>
    </div>

    <!-- Header -->
    <header class="header">
        <div class="nav">
            <button class="mobile-menu-toggle">
                <i class="fas fa-bars"></i>
            </button>
            
            <div class="logo">
                <i class="fas fa-store"></i>
                ModaStore
            </div>
            
            <div class="search-bar">
                <input type="text" placeholder="Buscar productos...">
                <button><i class="fas fa-search"></i></button>
            </div>
            
            <div class="nav-icons">
                <a href="#" class="nav-icon">
                    <i class="fas fa-user"></i>
                    <span>Cuenta</span>
                </a>
                <a href="#" class="nav-icon">
                    <i class="fas fa-heart"></i>
                    <span>Favoritos</span>
                </a>
                <a href="carrito.php" class="nav-icon">
                    <i class="fas fa-shopping-cart"></i>
                    <span>Carrito</span>
                </a>
            </div>
        </div>
        
        <!-- Main Navigation -->
        <div class="main-nav">
            <div class="main-nav-content">
                <ul class="main-nav-links">
                    <li><a href="index.php" class="active">Inicio</a></li>
                    <li><a href="categorias.php">Categorías</a></li>
                    <li><a href="productos.php">Productos</a></li>
                    <li><a href="#">Novedades</a></li>
                    <li><a href="#">Ofertas</a></li>
                    <li><a href="#">Curvy</a></li>
                </ul>
            </div>
        </div>
        
        <!-- Promo Banner -->
        <div class="promo-banner">
            ¡Gran venta! Hasta 70% de descuento en productos seleccionados
        </div>
    </header>

    <!-- Hero Banner -->
    <div class="hero-banner">
        <div class="hero-content">
            <h1>Nueva Colección 2024</h1>
            <p>Descubre las últimas tendencias en moda con estilo y calidad</p>
            <a href="productos.php" class="btn">Comprar Ahora</a>
        </div>
    </div>

    <!-- Categories Section -->
    <div class="section">
        <div class="section-header">
            <h2 class="section-title">Categorías</h2>
            <a href="categorias.php" class="view-all">Ver todas</a>
        </div>
        
        <div class="categories-grid">
            <a href="#" class="category-card">
                <div class="category-icon">
                    <i class="fas fa-female"></i>
                </div>
                <div class="category-name">Mujer</div>
            </a>
            
            <a href="#" class="category-card">
                <div class="category-icon">
                    <i class="fas fa-male"></i>
                </div>
                <div class="category-name">Hombre</div>
            </a>
            
            <a href="#" class="category-card">
                <div class="category-icon">
                    <i class="fas fa-child"></i>
                </div>
                <div class="category-name">Niños</div>
            </a>
            
            <a href="#" class="category-card">
                <div class="category-icon">
                    <i class="fas fa-gem"></i>
                </div>
                <div class="category-name">Accesorios</div>
            </a>
            
            <a href="#" class="category-card">
                <div class="category-icon">
                    <i class="fas fa-shoe-prints"></i>
                </div>
                <div class="category-name">Zapatos</div>
            </a>
            
            <a href="#" class="category-card">
                <div class="category-icon">
                    <i class="fas fa-home"></i>
                </div>
                <div class="category-name">Hogar</div>
            </a>
        </div>
    </div>

    <!-- Trends Section -->
    <div class="trends-section">
        <div class="section">
            <div class="trends-header">
                <h2 class="trends-title">Tendencias</h2>
                <p class="trends-subtitle">Descubre lo que está de moda esta temporada</p>
            </div>
            
            <div class="products-grid">
                <?php if (empty($novedades)): ?>
                    <div style="grid-column: 1 / -1; text-align: center; padding: 40px; color: #666;">
                        <p>No hay productos disponibles</p>
                    </div>
                <?php else: ?>
                    <?php foreach ($novedades as $producto): ?>
                        <div class="product-card">
                            <?php if (!empty($producto['imagen_base64'])): ?>
                                <img src="<?php echo $producto['imagen_base64']; ?>" 
                                     alt="<?php echo htmlspecialchars($producto['nombre']); ?>" 
                                     class="product-image">
                            <?php else: ?>
                                <div class="product-image" style="background: #f8f9fa; display: flex; align-items: center; justify-content: center; color: #7f8c8d;">
                                    <i class="fas fa-image fa-2x"></i>
                                </div>
                            <?php endif; ?>
                            <div class="product-info">
                                <h3 class="product-name"><?php echo htmlspecialchars($producto['nombre']); ?></h3>
                                <div class="product-price">S/ <?php echo number_format($producto['precio'], 2); ?></div>
                                <?php if ($producto['precio'] > 50): ?>
                                    <div class="product-old-price">S/ <?php echo number_format($producto['precio'] * 1.2, 2); ?></div>
                                <?php endif; ?>
                                <div class="product-actions">
                                    <button class="add-to-cart">Añadir</button>
                                    <button class="wishlist"><i class="far fa-heart"></i></button>
                                </div>
                            </div>
                        </div>
                    <?php endforeach; ?>
                <?php endif; ?>
            </div>
        </div>
    </div>

    <!-- Productos Destacados -->
    <div class="section">
        <div class="section-header">
            <h2 class="section-title">Productos Destacados</h2>
            <a href="productos.php" class="view-all">Ver todos</a>
        </div>
        
        <div class="products-grid">
            <?php if (empty($productos_destacados)): ?>
                <div style="grid-column: 1 / -1; text-align: center; padding: 40px; color: #666;">
                    <p>No hay productos destacados</p>
                </div>
            <?php else: ?>
                <?php foreach ($productos_destacados as $producto): ?>
                    <div class="product-card">
                        <?php if (!empty($producto['imagen_base64'])): ?>
                            <img src="<?php echo $producto['imagen_base64']; ?>" 
                                 alt="<?php echo htmlspecialchars($producto['nombre']); ?>" 
                                 class="product-image">
                        <?php else: ?>
                            <div class="product-image" style="background: #f8f9fa; display: flex; align-items: center; justify-content: center; color: #7f8c8d;">
                                <i class="fas fa-image fa-2x"></i>
                            </div>
                        <?php endif; ?>
                        <div class="product-info">
                            <h3 class="product-name"><?php echo htmlspecialchars($producto['nombre']); ?></h3>
                            <div class="product-price">S/ <?php echo number_format($producto['precio'], 2); ?></div>
                            <?php if ($producto['precio'] > 50): ?>
                                <div class="product-old-price">S/ <?php echo number_format($producto['precio'] * 1.2, 2); ?></div>
                            <?php endif; ?>
                            <div class="product-actions">
                                <button class="add-to-cart">Añadir</button>
                                <button class="wishlist"><i class="far fa-heart"></i></button>
                            </div>
                        </div>
                    </div>
                <?php endforeach; ?>
            <?php endif; ?>
        </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
        <div class="footer-content">
            <div class="footer-column">
                <h3>Mi Cuenta</h3>
                <ul class="footer-links">
                    <li><a href="#">Mis pedidos</a></li>
                    <li><a href="#">Devoluciones</a></li>
                    <li><a href="#">Cupones</a></li>
                    <li><a href="#">Puntos</a></li>
                    <li><a href="#">Cartera</a></li>
                </ul>
            </div>
            
            <div class="footer-column">
                <h3>Servicio al Cliente</h3>
                <ul class="footer-links">
                    <li><a href="#">Centro de Ayuda</a></li>
                    <li><a href="#">Métodos de Pago</a></li>
                    <li><a href="#">Términos y Condiciones</a></li>
                    <li><a href="#">Política de Privacidad</a></li>
                    <li><a href="#">Política de Devoluciones</a></li>
                </ul>
            </div>
            
            <div class="footer-column">
                <h3>Sobre Nosotros</h3>
                <ul class="footer-links">
                    <li><a href="#">Quiénes Somos</a></li>
                    <li><a href="#">Nuestra Historia</a></li>
                    <li><a href="#">Trabaja con Nosotros</a></li>
                    <li><a href="#">Contacto</a></li>
                </ul>
            </div>
            
            <div class="footer-column">
                <h3>Contáctanos</h3>
                <ul class="footer-links">
                    <li><i class="fas fa-phone"></i> +1 234 567 890</li>
                    <li><i class="fas fa-envelope"></i> contacto@modastore.com</li>
                    <li><i class="fas fa-map-marker-alt"></i> Lima, Perú</li>
                </ul>
            </div>
        </div>
        
        <div class="footer-bottom">
            <p>&copy; 2024 ModaStore. Todos los derechos reservados.</p>
        </div>
    </footer>

    <script>
        // Mobile menu toggle
        document.querySelector('.mobile-menu-toggle').addEventListener('click', function() {
            document.querySelector('.main-nav-links').classList.toggle('active');
        });
        
        // Add to cart functionality
        document.querySelectorAll('.add-to-cart').forEach(button => {
            button.addEventListener('click', function() {
                const productCard = this.closest('.product-card');
                const productName = productCard.querySelector('.product-name').textContent;
                alert(`¡${productName} añadido al carrito!`);
            });
        });
        
        // Wishlist functionality
        document.querySelectorAll('.wishlist').forEach(button => {
            button.addEventListener('click', function() {
                const icon = this.querySelector('i');
                if (icon.classList.contains('far')) {
                    icon.classList.remove('far');
                    icon.classList.add('fas');
                    icon.style.color = '#ff2e7d';
                } else {
                    icon.classList.remove('fas');
                    icon.classList.add('far');
                    icon.style.color = '';
                }
            });
        });
    </script>
</body>
</html>

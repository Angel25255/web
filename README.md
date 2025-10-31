<?php
// categorias.php - Página de categorías
require_once 'php/conexion.php';

// Obtener todas las categorías activas
$sql_categorias = "SELECT * FROM categorias WHERE activo = 1 ORDER BY nombre";
$stmt_categorias = $conexion->prepare($sql_categorias);
$stmt_categorias->execute();
$categorias = $stmt_categorias->fetchAll(PDO::FETCH_ASSOC);

// Obtener productos por categoría si se selecciona una
$productos_categoria = [];
$categoria_actual = null;

if (isset($_GET['categoria_id'])) {
    $categoria_id = $_GET['categoria_id'];
    
    // Obtener información de la categoría actual
    $sql_categoria_actual = "SELECT * FROM categorias WHERE id = ?";
    $stmt_categoria_actual = $conexion->prepare($sql_categoria_actual);
    $stmt_categoria_actual->execute([$categoria_id]);
    $categoria_actual = $stmt_categoria_actual->fetch(PDO::FETCH_ASSOC);
    
    // Obtener productos de la categoría
    $sql_productos = "SELECT p.*, c.nombre as categoria_nombre 
                     FROM productos p 
                     LEFT JOIN categorias c ON p.categoria_id = c.id 
                     WHERE p.categoria_id = ? AND p.activo = 1 
                     ORDER BY p.id DESC";
    $stmt_productos = $conexion->prepare($sql_productos);
    $stmt_productos->execute([$categoria_id]);
    $productos_categoria = $stmt_productos->fetchAll(PDO::FETCH_ASSOC);
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Categorías - ModaStore</title>
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
            padding-bottom: 70px;
        }
        
        /* Header Styles (igual que index.php) */
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
            padding: 0 15px;
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
            padding: 15px;
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
            padding: 0 15px;
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
        
        /* Categories Page Styles */
        .categories-page {
            max-width: 1200px;
            margin: 0 auto;
            padding: 30px 15px;
        }
        
        .page-header {
            text-align: center;
            margin-bottom: 40px;
        }
        
        .page-title {
            font-size: 2.5rem;
            color: var(--dark-color);
            margin-bottom: 10px;
        }
        
        .page-subtitle {
            color: var(--gray-color);
            font-size: 1.1rem;
        }
        
        .categories-container {
            display: grid;
            grid-template-columns: 300px 1fr;
            gap: 30px;
        }
        
        /* Categories Sidebar */
        .categories-sidebar {
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            padding: 25px;
            height: fit-content;
            position: sticky;
            top: 100px;
        }
        
        .sidebar-title {
            font-size: 1.3rem;
            margin-bottom: 20px;
            color: var(--dark-color);
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 10px;
        }
        
        .categories-list {
            list-style: none;
        }
        
        .category-item {
            margin-bottom: 12px;
        }
        
        .category-link {
            display: flex;
            align-items: center;
            padding: 12px 15px;
            text-decoration: none;
            color: var(--dark-color);
            border-radius: var(--border-radius);
            transition: all 0.3s;
        }
        
        .category-link:hover {
            background: var(--light-color);
            color: var(--primary-color);
        }
        
        .category-link.active {
            background: var(--primary-color);
            color: var(--white);
        }
        
        .category-icon {
            width: 30px;
            height: 30px;
            background: var(--light-color);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 12px;
            font-size: 14px;
        }
        
        .category-link.active .category-icon {
            background: rgba(255,255,255,0.2);
        }
        
        /* Products Grid */
        .products-section {
            flex: 1;
        }
        
        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }
        
        .section-title {
            font-size: 1.8rem;
            color: var(--dark-color);
        }
        
        .products-count {
            color: var(--gray-color);
            font-size: 14px;
        }
        
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
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
            object-fit: contain;
            background: #f8f9fa;
            padding: 15px;
        }
        
        .product-info {
            padding: 20px;
        }
        
        .product-name {
            font-size: 16px;
            margin-bottom: 10px;
            height: 45px;
            overflow: hidden;
            font-weight: 600;
        }
        
        .product-category {
            color: var(--gray-color);
            font-size: 12px;
            margin-bottom: 8px;
            text-transform: uppercase;
        }
        
        .product-price {
            font-weight: 700;
            color: var(--primary-color);
            font-size: 18px;
            margin-bottom: 5px;
        }
        
        .product-old-price {
            color: var(--gray-color);
            text-decoration: line-through;
            font-size: 14px;
            margin-bottom: 15px;
        }
        
        .product-actions {
            display: flex;
            justify-content: space-between;
        }
        
        .add-to-cart {
            background: var(--primary-color);
            color: var(--white);
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            font-size: 14px;
            cursor: pointer;
            transition: background 0.3s;
            flex: 1;
            margin-right: 10px;
        }
        
        .add-to-cart:hover {
            background: var(--secondary-color);
        }
        
        .wishlist {
            background: none;
            border: 1px solid var(--border-color);
            color: var(--gray-color);
            cursor: pointer;
            font-size: 16px;
            width: 45px;
            border-radius: 4px;
            transition: all 0.3s;
        }
        
        .wishlist:hover {
            color: var(--primary-color);
            border-color: var(--primary-color);
        }
        
        /* Empty State */
        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: var(--gray-color);
        }
        
        .empty-state i {
            font-size: 4rem;
            margin-bottom: 20px;
            color: var(--border-color);
        }
        
        .empty-state h3 {
            font-size: 1.5rem;
            margin-bottom: 10px;
        }
        
        /* Footer */
        .footer {
            background: var(--dark-color);
            color: var(--white);
            padding: 50px 0 20px;
            margin-top: 60px;
        }
        
        .footer-content {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 30px;
            padding: 0 15px;
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
            padding: 20px 15px;
            border-top: 1px solid #444;
            text-align: center;
            font-size: 14px;
            color: #aaa;
        }
        
        /* Mobile Bottom Navigation */
        .mobile-bottom-nav {
            display: none;
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: var(--white);
            border-top: 1px solid var(--border-color);
            z-index: 1000;
            box-shadow: 0 -2px 10px rgba(0,0,0,0.1);
        }
        
        .mobile-nav-items {
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
        }
        
        .mobile-nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-decoration: none;
            color: var(--gray-color);
            font-size: 10px;
            flex: 1;
        }
        
        .mobile-nav-item i {
            font-size: 20px;
            margin-bottom: 4px;
        }
        
        .mobile-nav-item.active {
            color: var(--primary-color);
        }
        
        /* Responsive Styles */
        @media (max-width: 992px) {
            .categories-container {
                grid-template-columns: 1fr;
            }
            
            .categories-sidebar {
                position: static;
                margin-bottom: 30px;
            }
            
            .products-grid {
                grid-template-columns: repeat(3, 1fr);
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
            
            .products-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .footer-content {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .mobile-bottom-nav {
                display: block;
            }
        }
        
        @media (max-width: 576px) {
            .products-grid {
                grid-template-columns: 1fr;
            }
            
            .footer-content {
                grid-template-columns: 1fr;
            }
            
            .page-title {
                font-size: 2rem;
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
                    <li><a href="index.php">Inicio</a></li>
                    <li><a href="categorias.php" class="active">Categorías</a></li>
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

    <!-- Categories Page Content -->
    <div class="categories-page">
        <div class="page-header">
            <h1 class="page-title">Nuestras Categorías</h1>
            <p class="page-subtitle">Descubre productos organizados por categorías</p>
        </div>
        
        <div class="categories-container">
            <!-- Categories Sidebar -->
            <div class="categories-sidebar">
                <h2 class="sidebar-title">Todas las Categorías</h2>
                <ul class="categories-list">
                    <?php foreach ($categorias as $categoria): ?>
                        <li class="category-item">
                            <a href="categorias.php?categoria_id=<?php echo $categoria['id']; ?>" 
                               class="category-link <?php echo ($categoria_actual && $categoria_actual['id'] == $categoria['id']) ? 'active' : ''; ?>">
                                <div class="category-icon">
                                    <i class="fas fa-tag"></i>
                                </div>
                                <?php echo htmlspecialchars($categoria['nombre']); ?>
                            </a>
                        </li>
                    <?php endforeach; ?>
                </ul>
            </div>
            
            <!-- Products Section -->
            <div class="products-section">
                <?php if ($categoria_actual): ?>
                    <div class="section-header">
                        <h2 class="section-title"><?php echo htmlspecialchars($categoria_actual['nombre']); ?></h2>
                        <span class="products-count"><?php echo count($productos_categoria); ?> productos</span>
                    </div>
                    
                    <?php if (empty($productos_categoria)): ?>
                        <div class="empty-state">
                            <i class="fas fa-box-open"></i>
                            <h3>No hay productos en esta categoría</h3>
                            <p>Pronto agregaremos nuevos productos.</p>
                        </div>
                    <?php else: ?>
                        <div class="products-grid">
                            <?php foreach ($productos_categoria as $producto): ?>
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
                                        <div class="product-category"><?php echo htmlspecialchars($producto['categoria_nombre']); ?></div>
                                        <h3 class="product-name"><?php echo htmlspecialchars($producto['nombre']); ?></h3>
                                        <div class="product-price">S/ <?php echo number_format($producto['precio'], 2); ?></div>
                                        <?php if ($producto['precio'] > 50): ?>
                                            <div class="product-old-price">S/ <?php echo number_format($producto['precio'] * 1.2, 2); ?></div>
                                        <?php endif; ?>
                                        <div class="product-actions">
                                            <button class="add-to-cart">Añadir al Carrito</button>
                                            <button class="wishlist"><i class="far fa-heart"></i></button>
                                        </div>
                                    </div>
                                </div>
                            <?php endforeach; ?>
                        </div>
                    <?php endif; ?>
                <?php else: ?>
                    <div class="empty-state">
                        <i class="fas fa-tags"></i>
                        <h3>Selecciona una categoría</h3>
                        <p>Elige una categoría del menú lateral para ver sus productos.</p>
                    </div>
                <?php endif; ?>
            </div>
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

    <!-- Mobile Bottom Navigation -->
    <nav class="mobile-bottom-nav">
        <div class="mobile-nav-items">
            <a href="index.php" class="mobile-nav-item">
                <i class="fas fa-home"></i>
                <span>Inicio</span>
            </a>
            <a href="categorias.php" class="mobile-nav-item active">
                <i class="fas fa-th-large"></i>
                <span>Categorías</span>
            </a>
            <a href="dijes.php" class="mobile-nav-item">
                <i class="fas fa-gem"></i>
                <span>Dijes</span>
            </a>
            <a href="carrito.php" class="mobile-nav-item">
                <i class="fas fa-shopping-cart"></i>
                <span>Carrito</span>
            </a>
            <a href="login.php" class="mobile-nav-item">
                <i class="fas fa-user"></i>
                <span>Login</span>
            </a>
        </div>
    </nav>

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
        
        // Mobile bottom nav active state
        document.querySelectorAll('.mobile-nav-item').forEach(item => {
            item.addEventListener('click', function() {
                document.querySelectorAll('.mobile-nav-item').forEach(i => {
                    i.classList.remove('active');
                });
                this.classList.add('active');
            });
        });
    </script>
</body>
</html>

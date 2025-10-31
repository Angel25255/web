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
                      ORDER BY p.id DESC LIMIT 4";
    $stmt_destacados = $conexion->prepare($sql_destacados);
    $stmt_destacados->execute();
    $productos_destacados = $stmt_destacados->fetchAll(PDO::FETCH_ASSOC);
    
    // Obtener productos NUEVOS para novedades
    $sql_novedades = "SELECT p.*, c.nombre as categoria_nombre 
                     FROM productos p 
                     LEFT JOIN categorias c ON p.categoria_id = c.id 
                     ORDER BY p.id DESC LIMIT 4";
    $stmt_novedades = $conexion->prepare($sql_novedades);
    $stmt_novedades->execute();
    $novedades = $stmt_novedades->fetchAll(PDO::FETCH_ASSOC);
    
} catch (PDOException $e) {
    $productos_destacados = [];
    $novedades = [];
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inicio - ModaStore</title>
    <style>
        :root {
            --primary-color: #2c3e50;
            --secondary-color: #3498db;
            --accent-color: #e74c3c;
            --light-color: #f5f5f5;
            --dark-color: #34495e;
            --text-color: #333;
            --text-light: #7f8c8d;
            --white: #fff;
            --shadow: 0 2px 10px rgba(0,0,0,0.1);
            --border-radius: 8px;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            background-color: var(--light-color);
            color: var(--text-color);
            line-height: 1.6;
        }
        
        /* Header Styles */
        .header {
            background: var(--primary-color);
            color: var(--white);
            padding: 1rem 0;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .nav {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
        }
        
        .logo {
            font-size: 1.5rem;
            font-weight: bold;
            display: flex;
            align-items: center;
        }
        
        .nav-links {
            display: flex;
            align-items: center;
        }
        
        .nav-links a {
            color: var(--white);
            text-decoration: none;
            margin-left: 15px;
            padding: 8px 12px;
            border-radius: var(--border-radius);
            transition: background 0.3s;
            font-size: 14px;
        }
        
        .nav-links a:hover {
            background: rgba(255,255,255,0.1);
        }
        
        .nav-links a.active {
            background: var(--secondary-color);
            font-weight: bold;
        }
        
        .menu-toggle {
            display: none;
            background: none;
            border: none;
            color: var(--white);
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        /* Mobile Menu */
        .mobile-menu {
            display: none;
            background: var(--dark-color);
            padding: 15px;
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            z-index: 999;
            box-shadow: 0 5px 10px rgba(0,0,0,0.1);
        }
        
        .mobile-menu.active {
            display: block;
        }
        
        .mobile-menu a {
            display: block;
            color: var(--white);
            text-decoration: none;
            padding: 10px 15px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .mobile-menu a:last-child {
            border-bottom: none;
        }
        
        .mobile-menu a.active {
            background: var(--secondary-color);
            border-radius: var(--border-radius);
        }
        
        /* Container */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }
        
        /* Hero Section */
        .hero {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: var(--white);
            padding: 60px 20px;
            text-align: center;
            margin-bottom: 40px;
            border-radius: var(--border-radius);
        }
        
        .hero h1 {
            font-size: 2.5rem;
            margin-bottom: 20px;
        }
        
        .hero p {
            font-size: 1.1rem;
            margin-bottom: 30px;
            opacity: 0.9;
        }
        
        /* Buttons */
        .btn-primary {
            background: var(--accent-color);
            color: var(--white);
            padding: 12px 25px;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 16px;
            text-decoration: none;
            display: inline-block;
            margin: 5px;
            transition: background 0.3s;
        }
        
        .btn-primary:hover {
            background: #c0392b;
        }
        
        /* Features */
        .features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin: 40px 0;
        }
        
        .feature {
            text-align: center;
            padding: 30px 20px;
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
        }
        
        .feature-icon {
            font-size: 2.5rem;
            margin-bottom: 15px;
        }
        
        .feature h3 {
            color: var(--primary-color);
            margin-bottom: 10px;
        }
        
        .feature p {
            color: var(--text-light);
            line-height: 1.5;
        }
        
        /* Sections */
        .section {
            background: var(--white);
            padding: 30px;
            border-radius: var(--border-radius);
            margin-bottom: 30px;
            box-shadow: var(--shadow);
        }
        
        .section h2 {
            color: var(--primary-color);
            border-bottom: 3px solid var(--secondary-color);
            padding-bottom: 10px;
            margin-bottom: 25px;
            text-align: center;
        }
        
        /* Product Grid */
        .productos-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
        }
        
        .producto-card {
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            overflow: hidden;
            transition: transform 0.3s ease;
            border: 1px solid #eee;
        }
        
        .producto-card:hover {
            transform: translateY(-5px);
        }
        
        .producto-imagen {
            width: 100%;
            height: 200px;
            object-fit: contain;
            background: #f8f9fa;
            padding: 15px;
        }
        
        .producto-info {
            padding: 15px;
        }
        
        .producto-nombre {
            margin: 0 0 8px 0;
            color: var(--primary-color);
            font-size: 16px;
            font-weight: bold;
        }
        
        .producto-detalles {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }
        
        .producto-precio {
            color: var(--accent-color);
            font-weight: bold;
            font-size: 18px;
        }
        
        .producto-marca {
            color: var(--text-light);
            font-size: 12px;
        }
        
        .stock {
            font-size: 12px;
            color: #27ae60;
        }
        
        .text-center {
            text-align: center;
            margin: 30px 0;
        }
        
        /* Footer */
        .footer {
            background: var(--dark-color);
            color: var(--white);
            text-align: center;
            padding: 30px;
            margin-top: 50px;
            border-radius: var(--border-radius);
        }
        
        /* Responsive Styles */
        @media (max-width: 768px) {
            .nav-links {
                display: none;
            }
            
            .menu-toggle {
                display: block;
            }
            
            .hero {
                padding: 40px 20px;
            }
            
            .hero h1 {
                font-size: 2rem;
            }
            
            .hero p {
                font-size: 1rem;
            }
            
            .features {
                grid-template-columns: 1fr;
            }
            
            .productos-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
                gap: 15px;
            }
            
            .section {
                padding: 20px;
            }
            
            .container {
                padding: 0 15px;
            }
        }
        
        @media (max-width: 480px) {
            .hero h1 {
                font-size: 1.8rem;
            }
            
            .productos-grid {
                grid-template-columns: 1fr 1fr;
            }
            
            .producto-imagen {
                height: 150px;
            }
            
            .btn-primary {
                padding: 10px 20px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <!-- Header con Menú Completo -->
    <header class="header">
        <div class="nav">
            <div class="logo">🛍️ ModaStore</div>
            <div class="nav-links">
                <a href="index.php" class="active">🏠 Inicio</a>
                <a href="productos.php">📦 Productos</a>
                <a href="categorias.php">📂 Categorías</a>
                <a href="carrito.php">🛒 Carrito</a>
                <a href="compras.php">📋 Compras</a>
                <a href="pedidos.php">📋 Pedidos</a>
                <a href="detalle-pedido.php">📋 Detalle Pedidos</a>
                <a href="#login">🔐 Login</a>
            </div>
            <button class="menu-toggle" id="menuToggle">☰</button>
        </div>
        
        <!-- Menú móvil -->
        <div class="mobile-menu" id="mobileMenu">
            <a href="index.php" class="active">🏠 Inicio</a>
            <a href="productos.php">📦 Productos</a>
            <a href="categorias.php">📂 Categorías</a>
            <a href="carrito.php">🛒 Carrito</a>
            <a href="compras.php">📋 Compras</a>
            <a href="pedidos.php">📋 Pedidos</a>
            <a href="detalle-pedido.php">📋 Detalle Pedidos</a>
            <a href="#login">🔐 Login</a>
        </div>
    </header>

    <div class="container">
        <!-- Hero Section -->
        <div class="hero">
            <h1>¡Bienvenido a ModaStore!</h1>
            <p>Descubre las últimas tendencias en moda. Calidad, estilo y los mejores precios en un solo lugar.</p>
            <a href="productos.php" class="btn-primary">Ver Colección Completa</a>
        </div>

        <!-- Características -->
        <div class="features">
            <div class="feature">
                <div class="feature-icon">🚚</div>
                <h3>Envío Gratis</h3>
                <p>En compras mayores a S/ 100</p>
            </div>
            <div class="feature">
                <div class="feature-icon">↩️</div>
                <h3>Devoluciones</h3>
                <p>30 días para cambiar tu producto</p>
            </div>
            <div class="feature">
                <div class="feature-icon">🔒</div>
                <h3>Pago Seguro</h3>
                <p>Transacciones 100% protegidas</p>
            </div>
            <div class="feature">
                <div class="feature-icon">📞</div>
                <h3>Soporte 24/7</h3>
                <p>Estamos aquí para ayudarte</p>
            </div>
        </div>

        <!-- Novedades -->
        <div class="section">
            <h2>Novedades</h2>
            <div class="productos-grid">
                <?php if (empty($novedades)): ?>
                    <div style="grid-column: 1 / -1; text-align: center; padding: 40px; color: #666;">
                        <p>No hay productos nuevos</p>
                    </div>
                <?php else: ?>
                    <?php foreach ($novedades as $producto): ?>
                        <div class="producto-card">
                            <?php if (!empty($producto['imagen_base64'])): ?>
                                <img src="<?php echo $producto['imagen_base64']; ?>" 
                                     alt="<?php echo htmlspecialchars($producto['nombre']); ?>" 
                                     class="producto-imagen">
                            <?php else: ?>
                                <div class="producto-imagen" style="display: flex; align-items: center; justify-content: center; color: #7f8c8d;">
                                    📷 Sin imagen
                                </div>
                            <?php endif; ?>
                            <div class="producto-info">
                                <h3 class="producto-nombre"><?php echo htmlspecialchars($producto['nombre']); ?></h3>
                                <div class="producto-detalles">
                                    <span class="producto-precio">S/ <?php echo number_format($producto['precio'], 2); ?></span>
                                    <span class="producto-marca"><?php echo htmlspecialchars($producto['marca']); ?></span>
                                </div>
                                <div class="stock"><?php echo $producto['stock']; ?> disponibles</div>
                            </div>
                        </div>
                    <?php endforeach; ?>
                <?php endif; ?>
            </div>
        </div>

        <!-- Productos Destacados -->
        <div class="section">
            <h2>Nuestros Productos Destacados</h2>
            <div class="productos-grid">
                <?php if (empty($productos_destacados)): ?>
                    <div style="grid-column: 1 / -1; text-align: center; padding: 40px; color: #666;">
                        <p>No hay productos destacados</p>
                    </div>
                <?php else: ?>
                    <?php foreach ($productos_destacados as $producto): ?>
                        <div class="producto-card">
                            <?php if (!empty($producto['imagen_base64'])): ?>
                                <img src="<?php echo $producto['imagen_base64']; ?>" 
                                     alt="<?php echo htmlspecialchars($producto['nombre']); ?>" 
                                     class="producto-imagen">
                            <?php else: ?>
                                <div class="producto-imagen" style="display: flex; align-items: center; justify-content: center; color: #7f8c8d;">
                                    📷 Sin imagen
                                </div>
                            <?php endif; ?>
                            <div class="producto-info">
                                <h3 class="producto-nombre"><?php echo htmlspecialchars($producto['nombre']); ?></h3>
                                <div class="producto-detalles">
                                    <span class="producto-precio">S/ <?php echo number_format($producto['precio'], 2); ?></span>
                                    <span class="producto-marca"><?php echo htmlspecialchars($producto['marca']); ?></span>
                                </div>
                                <div class="stock"><?php echo $producto['stock']; ?> disponibles</div>
                            </div>
                        </div>
                    <?php endforeach; ?>
                <?php endif; ?>
            </div>
        </div>

        <!-- Call to Action -->
        <div class="text-center">
            <a href="productos.php" class="btn-primary" style="font-size: 18px; padding: 15px 30px;">
                Ver Todos los Productos
            </a>
        </div>

        <footer class="footer">
            <p>© 2024 ModaStore - Todos los derechos reservados</p>
            <p>📧 contacto@modastore.com | 📞 +1 234 567 890</p>
        </footer>
    </div>

    <script>
        // Toggle mobile menu
        document.getElementById('menuToggle').addEventListener('click', function() {
            document.getElementById('mobileMenu').classList.toggle('active');
        });
        
        // Close mobile menu when clicking outside
        document.addEventListener('click', function(event) {
            const mobileMenu = document.getElementById('mobileMenu');
            const menuToggle = document.getElementById('menuToggle');
            
            if (!mobileMenu.contains(event.target) && !menuToggle.contains(event.target)) {
                mobileMenu.classList.remove('active');
            }
        });
    </script>
</body>
</html>

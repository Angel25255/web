<?php
// php/cargar_datos.php - Cargar todos los datos para la página
require_once 'conexion.php';

// Inicializar variables
$productos_destacados = [];
$novedades = [];
$categorias = [];

try {
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

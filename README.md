# CI Pipeline para Laravel - PHP con GitHub Actions

## Objetivo
Implementar un pipeline de integración continua que ejecute automáticamente la instalación de dependencias, las verificaciones del código y las pruebas automatizadas de una aplicación Laravel cada vez que se envíen cambios a un repositorio de GitHub[cite: 1].

## Tecnologías Utilizadas
- PHP 8.2[cite: 5]
- Laravel 11[cite: 3]
- Composer[cite: 3]
- PHPUnit[cite: 3]
- Git y GitHub / GitHub Actions[cite: 3]

## Estructura del Pipeline (`.github/workflows/ci.yml`)
El flujo automatizado se activa ante eventos de `push` o `pull_request` sobre la rama principal (`main`) y realiza los siguientes pasos[cite: 5, 6]:
1. **Checkout:** Clona el código fuente en el servidor de ejecución (*runner*) de GitHub Actions[cite: 6].
2. **Setup PHP:** Configura PHP 8.2 junto con las extensiones requeridas (`mbstring`, `pdo_sqlite`, `sqlite3`, `bcmath`, `fileinfo`)[cite: 5, 6].
3. **Validación de Composer:** Ejecuta `composer validate` para verificar la consistencia de las dependencias[cite: 3, 5, 6].
4. **Instalación:** Ejecuta `composer install` para descargar las librerías del proyecto de forma limpia[cite: 3, 5, 6].
5. **Preparación del Entorno:** Copia `.env.example` a `.env`, genera la llave de cifrado de la aplicación (`artisan key:generate`) y limpia la caché[cite: 5, 6].
6. **Pruebas Automatizadas:** Ejecuta la suite de pruebas mediante SQLite en memoria (`php artisan test`)[cite: 4, 5, 6].

## Evidencias de Ejecución del Pipeline
- **Ejecución Exitosa Inicial (Prueba A):** El pipeline se ejecuta correctamente, instalando dependencias y aprobando todas las pruebas automatizadas (`HomePageTest`).
- **Ejecución Fallida Controlada (Prueba B):** Se modificó temporalmente la aserción de la prueba a un estado no esperado (`assertStatus(404)`), provocando que GitHub Actions detectara el fallo y reportara el error en los registros (*logs*)[cite: 9].
- **Corrección y Recuperación:** Se restauró la aserción correcta (`assertSuccessful`), enviando un nuevo commit que permitió al pipeline volver a aprobarse exitosamente[cite: 9].

## Conclusiones
La implementación de integración continua permite automatizar la validación del código ante cualquier cambio, reduciendo errores humanos, asegurando la reproducibilidad del entorno de pruebas y garantizando la estabilidad de la aplicación Laravel antes de integrarla al flujo de desarrollo principal[cite: 1, 12].
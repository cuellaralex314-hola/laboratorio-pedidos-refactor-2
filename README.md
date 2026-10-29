# laboratorio-pedidos

Sistema didáctico de pedidos de laboratorio, PHP nativo, MySQL y XAMPP. La versión corrige los ejercicios de diseño, conserva las funcionalidades de demostración y agrega pruebas automáticas simples.

## Requisitos

- Windows con XAMPP.
- PHP 8.2 o superior.
- Apache y MySQL iniciados desde el panel de XAMPP.
- Extensión `pdo_mysql` habilitada en el PHP que utiliza Apache.
- No necesita Composer ni paquetes externos.

## 1. Instalar el proyecto

1. Extraé esta carpeta en `C:\xampp\htdocs\mini-proyecto-laboratorio`.
2. Iniciá **Apache** y **MySQL** desde XAMPP.
3. Comprobá que `pdo_mysql` esté disponible. Podés abrir una consola y ejecutar:

   ```bat
   C:\xampp\php\php.exe -m | findstr /I "PDO pdo_mysql"
   ```

   Debe aparecer `PDO` y `pdo_mysql`. Si no aparece `pdo_mysql`, abrí el `php.ini` usado por XAMPP, habilitá la línea `extension=pdo_mysql` (quitá el `;` inicial si está comentada), guardá y reiniciá Apache.

## 2. Crear la base de datos

1. Abrí `http://localhost/phpmyadmin/`.
2. Elegí **Importar** y seleccioná `database/schema.sql` del proyecto.
3. Ejecutá la importación. El script crea la base `laboratorio` y la tabla `orders`.

También podés ejecutar el contenido de `database/schema.sql` desde la consola de MySQL.

## 3. Configurar MySQL

Desde PowerShell, en la carpeta del proyecto:

```powershell
Copy-Item config\database.example.php config\database.php
```

Abrí `config/database.php` y completá los valores locales. Una configuración común para XAMPP es:

```php
return [
    'driver' => 'mysql',
    'host' => '127.0.0.1',
    'port' => 3306,
    'name' => 'laboratorio',
    'user' => 'root',
    'pass' => '',
    'charset' => 'utf8mb4',
];
```

Usá `pass => ''` solamente si tu usuario local `root` realmente no tiene contraseña. **No subas `config/database.php` a GitHub**: está excluido en `.gitignore`. El archivo de ejemplo conserva los valores vacíos para no publicar credenciales.

## 4. Abrir la aplicación

Con Apache y MySQL iniciados, ingresá a:

```text
http://localhost/mini-proyecto-laboratorio/public/index.php
```

Acciones disponibles:

| Dirección | Resultado |
|---|---|
| `?accion=listar` | Lista los pedidos guardados en MySQL. |
| `?accion=crear` | Muestra el formulario de creación. Al enviarlo se valida, calcula, guarda y notifica. |
| `?accion=reporte` | Genera un resumen con decoradores encadenados. |

El formulario acepta paciente, importe base, tipo de paciente, canal de notificación y destino. Los mensajes de Email, SMS y WhatsApp se **simulan**; no se envían a servicios externos.

## 5. Ejecutar pruebas automáticas

Desde la raíz del proyecto ejecutá:

```bat
C:\xampp\php\php.exe tests\run.php
```

Estas pruebas no necesitan conectarse a MySQL: utilizan un repositorio falso para probar las reglas de precio, Factory, Adapter, Observer, Decorator y Facade. La aplicación web sí requiere la base configurada.

Para revisar sintaxis de todos los PHP:

```powershell
Get-ChildItem public,src,views,tests -Recurse -Filter *.php | ForEach-Object { C:\xampp\php\php.exe -l $_.FullName }
```

## 6. Patrones y correcciones

- **Strategy:** cada política de precio es una clase independiente. `PriceCalculator` recibe un registro de estrategias; `PrepaidStrategy` se agrega en `ApplicationFactory` sin modificar la calculadora.
- **Factory:** `NotificationFactory` crea Email, SMS o WhatsApp bajo el contrato `Notification`.
- **Observer:** `OrderSubject` notifica a todos los objetos registrados que implementan `OrderObserver`. El sujeto no conoce las clases concretas.
- **Adapter:** `LegacyNotifierAdapter` adapta `sendMessage()` al método `send()` sin modificar la clase heredada.
- **Decorator:** los reportes se envuelven con decoradores encadenables. Ya no se usan banderas booleanas. Los agregados se representan como texto para el ejercicio.
- **Facade:** `OrderService::procesarPedidoCompleto()` coordina validación, precio, persistencia, notificación y eventos delegando cada responsabilidad.
- **MVC + SRP:** `OrderController` maneja la solicitud y delega; `Order` representa el pedido; las vistas muestran datos preparados y escapan las salidas.
- **Singleton:** `Connection::getInstance()` administra una instancia de la conexión PDO por proceso de PHP.
- **Repository:** `PdoOrderRepository` concentra consultas SQL preparadas y conversión entre filas de MySQL y objetos `Order`.

## 7. Git y entrega grupal

El TP pide una rama y un Pull Request por patrón, revisión y aprobación por un integrante distinto del autor, historial real de commits y evidencia en `docs/DEUDA-TECNICA.md`.

La plantilla `.github/pull_request_template.md` recuerda las tres preguntas obligatorias. El workflow `.github/workflows/php.yml` ejecuta sintaxis y pruebas en los Pull Requests.

Antes de entregar, completá en `docs/MAPA-DE-DEUDAS.md` los integrantes y las URLs reales de los PR. No se deben inventar revisiones ni aprobaciones. El ZIP de fuentes no demuestra por sí solo que los PR hayan sido fusionados en `main`.

## Nota sobre la configuración y alcance

`config/database.php` es local y no se incluye en el proyecto entregable. La conexión no se simula: usa PDO/MySQL y fallará con un mensaje explicativo si faltan credenciales o `pdo_mysql`. Las funciones de comunicación y las opciones documentales se mantienen simuladas porque requieren proveedores o librerías externas que no forman parte de este TP.

# Deuda técnica

La evidencia de las primeras diez filas apunta al **código original** incluido en el primer ZIP. Las dos últimas filas registran deuda de proceso y colaboración. Esta tabla documenta el problema, el cambio realizado y el costo aceptado.

| Síntoma observado | Evidencia en la versión original (archivo:línea) | Tipo de deuda | Patrón/corrección aplicada | Consecuencia asumida |
|---|---|---|---|---|
| Cada clase debía cargarse manualmente y las rutas estaban en condicionales | `public/index.php:22-33` y `public/index.php:62-74` | Diseño/mantenibilidad | Autoload y tabla de rutas | El proyecto depende de nombres de archivo coherentes. |
| La configuración estaba escrita en el archivo de entrada, obligando a integrantes a editar el mismo archivo y aumentando conflictos de merge | `public/index.php:46-58` | **Proceso/seguridad** | `config/database.example.php` y exclusión de `config/database.php` en Git | Cada desarrollador debe crear y completar su archivo local. |
| El cálculo dependía de condiciones por tipo de paciente y repetía descuentos | `src/Pricing/PriceCalculator.php:27-42` y `src/Pricing/PriceCalculator.php:50-57` | Diseño/OCP/DRY | Strategy y registro configurable | Hay más clases, aunque cada regla queda aislada y testeable. |
| El remitente conocía métodos diferentes y construía cada canal con condicionales | `src/Notifications/NotificationSender.php:30-45` | Diseño | Factory + interfaz `Notification` | La fábrica debe registrar los nuevos tipos. |
| El sujeto invocaba directamente cada interesado y podía cortar la cadena ante un error | `src/Events/OrderEvents.php:28-40` | Diseño/OCP | Observer con registro externo | El flujo depende de la lista de observadores registrada al iniciar la aplicación. |
| Se había añadido un método a la clase heredada y duplicado su comportamiento | `src/Legacy/LegacyNotifier.php:38-55` | Diseño/acoplamiento/DRY | `LegacyNotifierAdapter` | El adaptador agrega una capa adicional, aunque mantiene intacta la interfaz antigua. |
| El reporte aceptaba booleanos que describían combinaciones de características | `src/Reports/ReportGenerator.php:28-50` | Diseño/mantenibilidad | Decoradores encadenables | El orden de composición puede cambiar el resultado; las características siguen siendo una simulación textual. |
| El servicio validaba, calculaba, persistía, notificaba y emitía HTML en una operación | `src/Services/OrderService.php:34-88` | Diseño/SRP | `OrderService` como Facade, delegando en colaboradores | Hay que construir e inyectar dependencias al configurar la aplicación. |
| El controlador mezclaba entrada, reglas de precio, SQL, notificaciones y HTML | `src/Controllers/OrderController.php:30-53` | Diseño/MVC | Controlador delgado que delega en el servicio y redirige | Requiere un servicio bien definido y manejo de errores. |
| La vista abría la conexión, tenía datos propios, calculaba el total y mostraba valores sin escapar | `views/orders.php:21-49` | Diseño/MVC y seguridad | La vista recibe datos preparados y escapa la salida con `htmlspecialchars()` | La vista depende de que el controlador le entregue las claves esperadas. |
| La planilla no permitía auditar la asignación, porque los campos Grupo, Rama y PR estaban vacíos | `docs/MAPA-DE-DEUDAS.md:6-18` de la versión original | **Proceso/colaboración** | Se mantiene una planilla con campos explícitos para integrante, rama y URL real de PR; deben completarse con datos reales | El grupo debe registrar la contribución real, sin inventar autores ni aprobaciones. |
| La consigna exigía que cada PR explicara deuda, elección del patrón y consecuencia, pero no había una plantilla reutilizable | `docs/CONSIGNA-TP.md:66-81` (requisitos que debían cumplir los PR) | **Proceso/revisión** | `.github/pull_request_template.md` con deuda, elección, costo y pruebas | La plantilla recuerda los requisitos, pero el revisor aún debe comprobar el contenido real. |

## Alcance real de la implementación

- La persistencia utiliza MySQL mediante PDO y consultas preparadas; requiere configurar la base y habilitar `pdo_mysql`.
- Los mensajes Email/SMS/WhatsApp son simulaciones. No se conectan a proveedores externos.
- Firma, PDF y marca de agua se representan como decoradores de texto. No se genera un PDF real ni una firma criptográfica.
- Los observadores devuelven mensajes que demuestran que recibieron el evento; no envían por sí mismos comunicaciones reales.
- El workflow de GitHub ejecuta revisión de sintaxis y pruebas automáticas, pero solo funcionará en GitHub si Actions está habilitado.
- La revisión cruzada y las aprobaciones se deben demostrar con los Pull Requests reales de GitHub; el código no puede fabricar esa evidencia.

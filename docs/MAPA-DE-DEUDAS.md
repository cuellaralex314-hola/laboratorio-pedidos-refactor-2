# Mapa de deudas técnicas — laboratorio-pedidos

Usar esta tabla para repartir el trabajo y registrar los PR reales. No completar autores, revisores ni enlaces hasta que existan en GitHub.

| Archivo / método | Síntoma observado | Principio/deuda | Patrón o corrección | Rama sugerida | Integrante | PR real |
|---|---|---|---|---|---|---|
| `public/index.php` | Carga manual y rutas con condiciones | Mantenibilidad | Autoload + tabla de rutas | `refactor/bootstrap` | Completar por el grupo | Completar con URL |
| `config/database.php` | Credenciales locales en el árbol de trabajo | Proceso/seguridad | Configuración fuera de Git + `.gitignore` | `refactor/bootstrap` | Completar por el grupo | Completar con URL |
| `src/Pricing/PriceCalculator.php` | Cálculo central acoplado a tipos concretos | OCP | Strategy + registro de estrategias | `feat/patron-strategy` | Completar por el grupo | Completar con URL |
| `src/Notifications/NotificationFactory.php` | La selección de notificación debe estar en un único lugar | OCP | Factory | `feat/patron-factory` | Completar por el grupo | Completar con URL |
| `src/Events/OrderSubject.php` | Las reacciones no deben estar cableadas dentro del sujeto | DIP/OCP | Observer | `feat/patron-observer` | Completar por el grupo | Completar con URL |
| `src/Legacy/LegacyNotifierAdapter.php` | Interfaz heredada incompatible con la nueva interfaz | Acoplamiento | Adapter | `feat/patron-adapter` | Completar por el grupo | Completar con URL |
| `src/Reports/ReportGenerator.php` | Opciones del reporte expresadas con booleanos | Mantenibilidad | Decorator encadenable | `feat/patron-decorator` | Completar por el grupo | Completar con URL |
| `src/Services/OrderService.php` | Flujo de creación mezclaba muchas tareas | SRP / acoplamiento | Facade | `feat/patron-facade` | Completar por el grupo | Completar con URL |
| `src/Controllers/OrderController.php` | Controlador con lógica de negocio | SRP / MVC | Delegar en el servicio | `feat/patron-facade` | Completar por el grupo | Completar con URL |
| `views/orders.php` | La vista calculaba montos y consultaba datos | MVC / XSS | Presentación pura y salida escapada | `feat/patron-facade` | Completar por el grupo | Completar con URL |
| Revisión de Pull Requests | No había una plantilla que exigiera responder los tres puntos de la consigna | **Deuda de proceso** | Plantilla de PR con deuda, patrón y consecuencia | `docs/tp-integrador` | Completar por el grupo | Completar con URL |
| Revisión cruzada | El mapa no registraba quién revisó cada cambio | **Deuda de proceso** | Registrar PR, autor, revisor y aprobación real | `docs/tp-integrador` | Completar por el grupo | Completar con URL |

## Cierre de proceso

Antes de entregar, reemplazar los marcadores por los integrantes, enlaces a PR y revisores que realmente participaron. No inventar aprobaciones ni PR que no existan.

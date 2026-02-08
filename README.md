![Logotipo de la aplicación](img-doc/derejstorage.png)

# Documentación Técnica del Sistema DerejStorage

#

## 1. Introducción

El proyecto **DerejStorage** es una plataforma integral para la gestión de representantes y facturación de arroz. Centraliza el registro de ventas, control de facturas, gestión de representantes y análisis de datos. Está construido sobre **Django 5.x** con un backend basado en **MySQL** y una única app denominada `arrozcascara`, que concentra modelos, vistas, plantillas y recursos estáticos propios del negocio.

## 2. Tecnologías y dependencias clave

- **Django 5.x** para el framework web y ORM.
- **MySQL** como motor relacional principal (configurado en [settings.py](gestion_de_arroz/gestion_de_arroz/settings.py)).
- **openpyxl** para la importación masiva de facturas desde archivos Excel.
- **pandas** para el análisis y agrupación de datos.
- **Chart.js** para visualización de métricas y gráficos en el dashboard.

## 3. Arquitectura general

- **App única (`arrozcascara`)**: concentra los modelos de dominio, vistas basadas en funciones y rutas declaradas en [urls.py](gestion_de_arroz/arrozcascara/urls.py).
- **Plantillas HTML** bajo [`templates/arrozcascara`](gestion_de_arroz/arrozcascara/templates/arrozcascara/), organizadas por vistas (dashboard, detalles, gestión de representantes, registro de facturas, etc.).
- **Recursos estáticos** ubicados en [static](gestion_de_arroz/static/) para despliegue.
- **Configuración central** en [settings.py](gestion_de_arroz/gestion_de_arroz/settings.py), donde se habilitan middlewares, almacenamiento de estáticos y parámetros regionales (`LANGUAGE_CODE='es'`, `TIME_ZONE='America/Santo_Domingo'`).
- **Seguridad**: autenticación estándar de Django, validaciones robustas y manejo de errores.

## Estructura de Carpetas

```
gestion_de_arroz/
│   manage.py
│   (base de datos MySQL configurada por variables de entorno)
│
├── arrozcascara/
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── templates/
│   │   └── arrozcascara/
│   │       ├── dashboard.html
│   │       ├── detalles.html
│   │       ├── gestionderepresentantes.html
│   │       ├── index.html
│   │       ├── registrodefacturas.html
│   │       ├── representantes.html
│   ├── migrations/
│   ├── tests.py
│   ├── admin.py
│   ├── apps.py
│
├── gestion_de_arroz/
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   ├── asgi.py
```

## 4. Modelo de datos principal

Los modelos residen en [models.py](gestion_de_arroz/arrozcascara/models.py) y cubren todo el ciclo operativo.

| Modelo          | Rol principal                                                         | Relacionamientos destacados       |
| --------------- | --------------------------------------------------------------------- | --------------------------------- |
| `Representante` | Registro de representantes con cédula, nombre y dirección.            | Relacionado a `Factura` (FK).     |
| `Factura`       | Almacena facturas, cliente, cantidad de sacos, variedad, estado, etc. | Relación N:1 con `Representante`. |

## 5. Módulos funcionales

### 5.1 Gestión de representantes

- Registro, edición y eliminación de representantes.
- Visualización de todos los representantes activos.
- Almacenamiento de datos clave: cédula, nombre completo y dirección.

### 5.2 Registro y administración de facturas

- Registro manual y masivo (importación desde Excel) de facturas.
- Validación de datos y unicidad de número de factura.
- Edición, eliminación y pago de facturas.
- Almacenamiento de información relevante: número de factura, cliente, cantidad de sacos, representante, fecha, variedad, monto y estado (pendiente/pagado).

### 5.3 Dashboard y analítica

- Visualización de estadísticas clave: total de representantes, facturas, sacos vendidos y promedio por representante.
- Gráficos interactivos de ventas y facturación por representante.
- Detalle de rendimiento individual de cada representante.
- Actualización automática y manual de datos.

### 5.4 Filtros y análisis detallado

- Filtrado de facturas por representante, variedad y rango de fechas.
- Agrupación de facturas por representante y cálculo de totales.
- Visualización de resultados filtrados.

### 5.5 Seguridad y control

- Validaciones robustas en formularios y carga de archivos.
- Manejo de errores y notificaciones al usuario.
- Uso de transacciones atómicas para importaciones masivas.

## 6. Flujo operativo end-to-end

1. **Registro de representantes**: usuarios registran representantes con datos clave.
2. **Registro de facturas**:
   - Manual: formulario web.
   - Masivo: importación desde Excel.
3. **Gestión de facturas**: edición, eliminación y pago.
4. **Dashboard**: visualización de métricas y gráficos.
5. **Análisis detallado**: filtrado por representante, variedad y fechas.

## 7. Integraciones internas y archivos relevantes

- **Rutas**: cubren todo el dominio y se centralizan en [urls.py](gestion_de_arroz/arrozcascara/urls.py).
- **Plantillas**: cada feature tiene su HTML (por ejemplo [dashboard.html](gestion_de_arroz/arrozcascara/templates/arrozcascara/dashboard.html), [detalles.html](gestion_de_arroz/arrozcascara/templates/arrozcascara/detalles.html)).
- **Assets**: imágenes y scripts en [static/](gestion_de_arroz/static/).

## 8. Seguridad y cumplimiento

- Validaciones server-side para datos, montos y unicidad de facturas.
- Manejo de errores y notificaciones.
- Uso de transacciones atómicas para importaciones masivas.

## 9. Instalación y configuración

1. Instalar dependencias:
   ```bash
   pip install django pandas openpyxl mysqlclient
   ```
2. Configurar variables de entorno para la base de datos MySQL (crear archivo `.env` en la raíz del proyecto):
   ```bash
   SECRET_KEY="tu_clave_secreta"
   DB_NAME="nombre_base_datos"
   DB_USER="usuario"
   DB_PASSWORD="contraseña"
   DB_HOST="localhost"
   DB_PORT="3306"
   ALLOWED_HOSTS="localhost,127.0.0.1"
   CSRF_TRUSTED_ORIGINS="http://localhost,http://127.0.0.1"
   DEBUG=True
   ```
3. Ejecutar migraciones:
   ```bash
   python manage.py migrate
   ```
4. Crear superusuario:
   ```bash
   python manage.py createsuperuser
   ```
5. Ejecutar el servidor:
   ```bash
   python manage.py runserver
   ```

## 10. Métricas y mejoras futuras sugeridas

- **KPI adicionales**: rendimiento por representante, ventas por variedad.
- **Alertas proactivas**: notificaciones para facturas pendientes o stock bajo.
- **API pública**: encapsular endpoints clave en una API REST (Django REST Framework).
- **Pruebas automatizadas**: ampliar [tests.py](gestion_de_arroz/arrozcascara/tests.py) con casos de facturación y gestión de representantes.

---

## Modelos Clave

- **Representante:** Controla representantes con cédula, nombre y dirección.
- **Factura:** Almacena facturas, cliente, cantidad de sacos, variedad, estado y monto.

## Templates

La aplicación cuenta con templates personalizados para cada funcionalidad, con diseño moderno y sidebar fijo. Ejemplos:

- `dashboard.html`: Panel de métricas y gráficos.
- `detalles.html`: Análisis detallado de facturas.
- `gestionderepresentantes.html`: Gestión de representantes.
- `registrodefacturas.html`: Registro manual y masivo de facturas.
- `representantes.html`: Visualización de representantes.

## Uso

- Accede al sistema desde el navegador en `http://localhost:8000`.
- Inicia sesión con usuario registrado.
- Utiliza el dashboard para visualizar métricas.
- Gestiona representantes y facturas desde el menú lateral.

## Seguridad y Roles

- El sistema implementa autenticación y autorización estándar de Django.
- Los roles permiten segmentar el acceso a funcionalidades críticas.

## Pruebas

- El archivo `tests.py` está preparado para pruebas unitarias con Django TestCase.
- Se recomienda implementar pruebas para cada modelo y vista crítica.

## Personalización

- Los templates pueden ser adaptados para branding propio.
- El sistema soporta ampliación de modelos y vistas para nuevas funcionalidades.

## Dependencias

Ver archivo [requirements.txt](gestion_de_arroz/requirements.txt) para la lista completa.

## Configuración

- Variables de entorno para seguridad y base de datos.
- Soporte para archivos estáticos y media.
- Configuración de zona horaria: `America/Santo_Domingo`.

## Contacto y Soporte

Para soporte, contactar al desarrollador o consultar la documentación de Django.

---

DerejStorage: Gestión eficiente de representantes y facturación de arroz.

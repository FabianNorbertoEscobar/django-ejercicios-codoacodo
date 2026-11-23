# Prácticas de Python y Django — Codo a Codo

Repositorio de ejercicios y material de la cursada de Django de Codo a Codo, comisión 23654 (2023). Reúne ejemplos de Python, proyectos de clase y un proyecto integrador de práctica que modela un portal de cursos y su administración.

El código conserva ejemplos y configuraciones de distintas etapas de aprendizaje: no es una aplicación lista para producción ni todos los directorios forman parte de un único proyecto.

## Contenidos

| Ubicación | Contenido |
| --- | --- |
| [`Presentaciones/`](Presentaciones/) y PDF de la raíz | Introducción, arquitectura, Git, cronograma y consigna del proyecto integrador. |
| [`Reunion_05/`](Reunion_05/) a [`Reunion_08/`](Reunion_08/) | Fundamentos de Python, programación orientada a objetos, herencia, excepciones, módulos y paquetes. |
| [`Reunion_09/`](Reunion_09/) a [`Reunion_12/`](Reunion_12/) | Primeros proyectos Django: vistas, despacho de URL y plantillas. |
| `Reunion_13/` a `Reunion_26/` | Material de clase sobre plantillas, formularios, bases de datos, modelos y administración. |
| [`Reunion_27/`](Reunion_27/), [`Reunion_28/`](Reunion_28/) y [`Reunion_31/`](Reunion_31/) | Proyectos de presentación con administración, autenticación y API REST, respectivamente. |
| `Reunion_29/` a `Reunion_37_Charla/` | Más material de autenticación, REST, despliegue, Docker y cierre de cursada. |
| [`pig_23654/`](pig_23654/) | Proyecto integrador de práctica: portal, gestión de datos y API. |
| [`python ejercicios de repaso/`](python%20ejercicios%20de%20repaso/) y [`Reunion_07/`](Reunion_07/) | Ejercicios integradores y resoluciones de Python. |
| [`ejercicio_git/`](ejercicio_git/) | Ejercicio introductorio de Git con páginas HTML. |

Las carpetas de reuniones no contienen necesariamente código ejecutable: varias reúnen presentaciones, consignas o notas.

## Stack y herramientas

En el proyecto integrador se utilizan:

- **Python y Django 4.2.4**: vistas, URL, plantillas, formularios, ORM, migraciones y herramientas de administración.
- **PostgreSQL** con `psycopg2-binary`: persistencia de los modelos.
- **Django REST Framework 3.14**: serialización y API de categorías.
- **HTML, CSS, JavaScript y Bootstrap**: interfaz del portal y de administración; JavaScript utiliza `fetch` para obtener los proyectos de ejemplo.
- **Pillow** para campos de imagen, **python-decouple** para configuración y **django-extensions** como herramienta de desarrollo.

Los proyectos de clase son independientes y pueden tener otras versiones o dependencias; por ejemplo, los de las reuniones 27, 28 y 31 fijan Django 3.2.14. Los ejemplos de las reuniones 10 y 12 usan SQLite, mientras que los de las reuniones 27, 28 y 31 configuran PostgreSQL.

## Proyecto integrador

[`pig_23654/`](pig_23654/) organiza el código en tres áreas:

- **`portal`**: páginas públicas, formulario de contacto con validaciones, registro de usuarios y rutas de autenticación de Django. La vista de cursos y la respuesta JSON de proyectos usan datos de ejemplo definidos en código, no los modelos de la base.
- **`administracion`**: modelos y vistas de gestión de categorías, cursos, comisiones, estudiantes, docentes, inscripciones y proyectos. Incluye formularios, carga de imágenes y tanto vistas basadas en funciones como vistas genéricas basadas en clases.
- **`pig_api`**: un `ModelViewSet` y un `ModelSerializer` para categorías, registrados con un router de Django REST Framework.

### Conceptos practicados

- Relaciones del ORM: herencia de modelos (`Persona`, `Estudiante` y `Docente`), claves foráneas y relación muchos a muchos entre comisiones y estudiantes mediante `Inscripcion`.
- Estados de inscripción con `TextChoices`, managers personalizados, generación de *slugs* y métodos de baja lógica y restauración en algunos modelos. La baja lógica no se aplica a todas las vistas de eliminación.
- Plantillas con herencia, bloques, recursos estáticos, mensajes y protección CSRF; formularios `Form` y `ModelForm` con validaciones.
- Registro e inicio de sesión con el sistema de autenticación de Django; algunos ejemplos de vistas utilizan `login_required` o `LoginRequiredMixin`.
- Django Admin estándar y un sitio de administración personalizado, con modelos relacionados en línea, búsquedas y una acción para recuperar estudiantes.
- Respuestas JSON y API REST navegable. Los archivos subidos se guardan en `media/` y se sirven mediante Django durante el desarrollo.

### Rutas principales

Las rutas corresponden al proyecto `pig_23654`, no a los proyectos de clase.

| Ruta | Uso |
| --- | --- |
| `/` | Inicio y formulario de contacto. |
| `/cursos/2003` | Ejemplo de listado de cursos; la ruta recibe un entero. |
| `/proyectos/` y `/json_proyectos/` | Galería y respuesta JSON con datos de ejemplo. |
| `/quienes_somos/` | Página institucional. |
| `/cuentas/registrarse` y `/accounts/login/` | Registro e inicio de sesión. |
| `/administracion/` | Inicio de la sección de gestión. |
| `/admin/` y `/codo_admin/` | Sitios de Django Admin estándar y personalizado. |
| `/api/categorias/` y `/api/categorias/<id>/` | Listado, creación, consulta, actualización y eliminación de categorías. El serializador expone `id`, `nombre` y `baja`. |

## Puesta en marcha del proyecto integrador

### Requisitos

- Python **3.10**, recomendado para las dependencias históricas fijadas en el repositorio.
- PostgreSQL en ejecución, con una base y un usuario con permisos sobre ella.
- Git y `iconv` para los comandos de ejemplo en Linux/macOS.

1. Cloná el repositorio y creá un entorno virtual:

   ```bash
   git clone https://github.com/FabianNorbertoEscobar/django-ejercicios-codoacodo.git
   cd django-ejercicios-codoacodo
   python3.10 -m venv .venv
   source .venv/bin/activate
   cd pig_23654
   ```

2. Instalá las dependencias. El `requirements.txt` de este proyecto está codificado en **UTF-16**, por lo que primero se convierte una copia a UTF-8:

   ```bash
   iconv -f UTF-16 -t UTF-8 requirements.txt > requirements.local.txt
   python -m pip install -r requirements.local.txt
   rm requirements.local.txt
   ```

3. Creá una base y un usuario en PostgreSQL, y colocá un archivo `.env` en `pig_23654/`, junto a `manage.py`. Reemplazá los valores de ejemplo por los de tu entorno:

   ```dotenv
   SECRET_KEY=una-clave-local-distinta-y-secreta
   DATABASE_HOST=localhost
   DATABASE_PORT=5432
   DATABASE_NAME=codoacodo
   DATABASE_USER=usuario_local
   DATABASE_PASSWORD=clave_local
   ```

   El archivo `.env` está ignorado por Git. No subas credenciales reales al repositorio.

4. Aplicá las migraciones, creá un usuario administrador y levantá el servidor de desarrollo:

   ```bash
   python manage.py migrate
   python manage.py createsuperuser
   python manage.py runserver
   ```

   El servidor queda disponible en `http://127.0.0.1:8000/`. Las tablas comienzan vacías: se pueden cargar datos desde los sitios de administración o las vistas de gestión.

### Configuración y limitaciones

- El envío del formulario de contacto usa SMTP. Los valores de correo en `pig_23654/pig_23654/settings.py` están vacíos; hay que configurar un proveedor, remitente y destinatario para enviarlo. No se leen desde las variables del `.env` actualmente.
- En Linux, la vista de inicio refiere a la plantilla con una barra invertida (`portal\index.html`), que puede provocar `TemplateDoesNotExist`; debe usarse `portal/index.html` para ejecutarla allí. La plantilla de proyectos también intenta incluir `portal/css/style.css` como plantilla, aunque el archivo está en `static/`, por lo que esa página puede requerir una corrección local.
- El proyecto tiene `DEBUG = True`, configuración de desarrollo y controles de acceso aplicados solo a algunas vistas. La API de categorías no declara una política de permisos propia. No lo expongas públicamente sin revisar configuración, autenticación y permisos.
- Los archivos `tests.py` del proyecto integrador son esqueletos, no una suite de pruebas funcionales. El repositorio contiene código didáctico y ejemplos incompletos.

## Ejecutar otros ejercicios

Los scripts de Python se pueden ejecutar individualmente desde la raíz, por ejemplo:

```bash
python Reunion_07/Resoluciones_propuestas/ejercicio_integrador_1.py
```

Para un proyecto de clase, entrá en la carpeta que contiene su `manage.py`, usá un entorno virtual separado y revisá sus `settings.py` y `requirements.txt` si existe. No todos los ejemplos incluyen un archivo de dependencias ni una base configurada; los que usan PostgreSQL necesitan configuración adicional. El proyecto de [`Reunion_31/pig_presentacion_api/`](Reunion_31/pig_presentacion_api/) incluye su propio README.

## Material de consulta

- [Documentación de Django 4.2](https://docs.djangoproject.com/en/4.2/)
- [Documentación de Django REST Framework](https://www.django-rest-framework.org/)
- [Documentación de Python](https://docs.python.org/es/3/)

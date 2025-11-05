## ☕ UIII_Cafetería_0720 — Sistema de Administración "Como dice el dicho"
🧩 Configuración inicial del proyecto
🔢 Paso	🧠 Procedimiento	💻 Comando
1️⃣	Crear carpeta del proyecto	mkdir UIII_Cafeteria_0720
2️⃣	Abrir la carpeta en VS Code	Archivo → Abrir carpeta → UIII_Cafeteria_0720
3️⃣	Abrir terminal integrada	Terminal → Nueva terminal
4️⃣	Crear entorno virtual	python -m venv .venv
5️⃣	Activar entorno virtual (Windows)	.venv\Scripts\activate
	Activar entorno virtual (Mac/Linux)	source .venv/bin/activate
6️⃣	Seleccionar intérprete en VS Code	Ctrl + Shift + P → Python: Select Interpreter → .venv
7️⃣	Instalar Django	pip install django
8️⃣	Crear proyecto base sin duplicar carpetas	django-admin startproject backend_Cafeteria .
9️⃣	Crear la aplicación principal	python manage.py startapp app_Cafeteria
🔟	Ver estructura de carpetas	(ver abajo)
🗂️ Estructura del proyecto
UIII_Cafeteria_0720/
│
├── backend_Cafeteria/
├── app_Cafeteria/
├── .venv/
└── manage.py

⚙️ Ejecución inicial del servidor
python manage.py runserver 8036


Accede al navegador en:

http://127.0.0.1:8036/

🧱 Modelos (models.py)

Ubicación: app_Cafeteria/models.py

from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField(blank=True, null=True)
    precio = models.DecimalField(max_digits=8, decimal_places=2)
    disponible = models.BooleanField(default=True)
    categoria = models.CharField(max_length=50)
    fecha_agregado = models.DateField(auto_now_add=True)

    def __str__(self):
        return self.nombre


class Empleado(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    fecha_nac = models.DateField()
    correo = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


class Cliente(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    fecha_nac = models.DateField()
    correo = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


class Orden(models.Model):
    fecha_orden = models.DateTimeField(auto_now_add=True)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    metodo_pago = models.CharField(max_length=50)
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE, related_name="ordenes")
    empleado = models.ForeignKey(Empleado, on_delete=models.SET_NULL, null=True, related_name="ordenes")
    productos = models.ManyToManyField(Producto, related_name="ordenes")

    def __str__(self):
        return f"Orden #{self.id} - {self.cliente.nombre}"

⚙️ Migraciones
python manage.py makemigrations
python manage.py migrate

🧾 Registro en admin.py

Ubicación: app_Cafeteria/admin.py

from django.contrib import admin
from .models import Producto, Empleado, Cliente, Orden

admin.site.register(Producto)
admin.site.register(Empleado)
admin.site.register(Cliente)
admin.site.register(Orden)

👤 Crear superusuario
python manage.py createsuperuser


Accede al panel de administración en:

http://127.0.0.1:8036/admin

## 🖼️ Templates
 📁 Estructura de templates
app_Cafeteria/
└── templates/
    ├── base.html
    ├── header.html
    ├── navbar.html
    ├── footer.html
    ├── inicio.html
    └── producto/
        ├── agregar_producto.html
        ├── ver_productos.html
        ├── actualizar_producto.html
        └── borrar_producto.html

🎨 Bootstrap en base.html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

🧭 Menú principal (navbar.html)
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">☕ Cafetería</a>
    <div class="collapse navbar-collapse">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li><a class="nav-link" href="#">Inicio</a></li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown">Productos</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar producto</a></li>
            <li><a class="dropdown-item" href="#">Ver productos</a></li>
            <li><a class="dropdown-item" href="#">Actualizar producto</a></li>
            <li><a class="dropdown-item" href="#">Borrar producto</a></li>
          </ul>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown">Clientes</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar cliente</a></li>
            <li><a class="dropdown-item" href="#">Ver clientes</a></li>
            <li><a class="dropdown-item" href="#">Actualizar cliente</a></li>
            <li><a class="dropdown-item" href="#">Borrar cliente</a></li>
          </ul>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown">Órdenes</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar orden</a></li>
            <li><a class="dropdown-item" href="#">Ver órdenes</a></li>
            <li><a class="dropdown-item" href="#">Actualizar orden</a></li>
            <li><a class="dropdown-item" href="#">Borrar orden</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>

🦶 Pie de página (footer.html)
<footer class="text-center fixed-bottom bg-light p-3">
  © {{ year }} | Creado por Ing. Eliseo Nava, CBTIS 128
</footer>

🏠 Página de inicio (inicio.html)
<h2 class="text-center mt-4">Bienvenido al sistema de administración “Como dice el dicho”</h2>
<img src="https://images.unsplash.com/photo-1509042239860-f550ce710b93" 
     class="img-fluid rounded mx-auto d-block mt-3" alt="cafetería">

🌐 Configuración de URLs
En backend_Cafeteria/settings.py
INSTALLED_APPS = [
    ...,
    'app_Cafeteria',
]

En backend_Cafeteria/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Cafeteria.urls')),
]

Crear app_Cafeteria/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio, name='inicio'),
    # CRUD de productos (ejemplo)
    path('productos/', views.ver_productos, name='ver_productos'),
    path('productos/agregar/', views.agregar_producto, name='agregar_producto'),
    path('productos/actualizar/<int:id>/', views.actualizar_producto, name='actualizar_producto'),
    path('productos/borrar/<int:id>/', views.borrar_producto, name='borrar_producto'),
]

🧩 Migraciones finales
python manage.py makemigrations
python manage.py migrate

🚀 Ejecución final
python manage.py runserver 8036


📍 Acceder en navegador:

http://127.0.0.1:8036/

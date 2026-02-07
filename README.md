# 🎯 DarkWall Lab 2026 - UniLeak

> "Solo estaba mirando..."

Sistema universitario de gestión académica de la Universidad de Medellín.

![Status](https://img.shields.io/badge/status-active-success)
![Python](https://img.shields.io/badge/python-3.8+-blue)
![Flask](https://img.shields.io/badge/flask-3.0-lightgrey)

---

## 📋 Descripción

UniGest es el sistema de consultas y servicios de la Universidad de Medellín. Permite a estudiantes, monitores y coordinadores gestionar información académica y financiera de manera eficiente.

### Características Principales

✅ Panel de estudiante con acceso a notas y estado financiero  
✅ Sistema de revisión de calificaciones  
✅ Panel de monitor para gestión de solicitudes  
✅ Panel de coordinación académica  
✅ Gestión de deudas y pagos  
✅ Interfaz intuitiva y responsive  

---

## 🚀 Instalación

### Requisitos Previos

- Python 3.8 o superior
- pip (gestor de paquetes de Python)

### Pasos de Instalación

1. **Clonar o descargar el proyecto**

2. **Instalar dependencias:**
```bash
pip install -r requirements.txt
```

3. **Generar recursos necesarios:**
```bash
python create_stego_image.py
```

4. **Ejecutar la aplicación:**
```bash
python app.py
```

O usar el script de ejecución rápida:
```bash
run.bat
```

5. **Acceder al sistema:**
```
http://localhost:5000
```

---

## 👤 Credenciales de Acceso

### Estudiantes
- **Usuario:** 20261001 | **Clave:** 12051998
- **Usuario:** 20261002 | **Clave:** 23071999
- **Usuario:** 20261003 | **Clave:** 15031997

### Personal Administrativo
- **Usuario:** 10011234 | **Clave:** admin2026 (Monitor)
- **Usuario:** 10021234 | **Clave:** coord2026 (Coordinador)

---

## 📁 Estructura del Proyecto

```
labdarkwall2026/
├── app.py                      # Aplicación Flask principal
├── create_stego_image.py       # Generador de recursos
├── decode_stego.py             # Utilidad de verificación
├── requirements.txt            # Dependencias Python
├── README.md                   # Este archivo
├── WALKTHROUGH.md             # Guía detallada
├── run.bat                     # Script de ejecución
├── data/                       # Base de datos CSV
│   ├── usuarios.csv           # Usuarios del sistema
│   ├── notas.csv              # Calificaciones
│   ├── materias.csv           # Materias disponibles
│   ├── deudas.csv             # Estado financiero
│   └── revisiones.csv         # Solicitudes de revisión
├── templates/                  # Plantillas HTML
│   ├── login.html             # Página de ingreso
│   ├── panel_estudiante.html  # Panel de estudiante
│   ├── panel_monitor.html     # Panel de monitor
│   ├── panel_academico.html   # Panel de coordinación
│   ├── cambiar_clave.html     # Cambio de contraseña
│   └── olvido_clave.html      # Recuperación de clave
└── static/
    ├── css/
    │   └── style.css          # Estilos de la aplicación
    └── uploads/
        └── profile_card.png   # Carnet universitario
```

---

## 🎓 Módulos del Sistema

### 1. Panel de Estudiante
- Consulta de notas
- Visualización de estado académico
- Revisión de deudas
- Perfil estudiantil

### 2. Panel de Monitor
- Gestión de solicitudes de revisión
- Seguimiento de casos
- Reportes académicos

### 3. Panel de Coordinación
- Administración de calificaciones
- Gestión de estados académicos
- Control financiero
- Reportes administrativos

---

## 🛠️ Tecnologías Utilizadas

- **Backend:** Flask 3.0
- **Frontend:** HTML5, CSS3, JavaScript
- **Almacenamiento:** CSV (simulación de base de datos)
- **Procesamiento de Imágenes:** Pillow (PIL)
- **Diseño:** CSS puro, responsive design

---

## 📚 Documentación Adicional

Para una guía completa de uso y funcionalidades, consulta:
- **[WALKTHROUGH.md](WALKTHROUGH.md)** - Guía detallada del sistema

---

## 🔒 Seguridad

Este sistema implementa controles de acceso basados en roles:
- **Estudiantes:** Acceso a información personal y académica
- **Monitores:** Gestión de revisiones y solicitudes
- **Coordinadores:** Acceso administrativo completo

**Nota:** Las credenciales predeterminadas son para entorno de desarrollo. En producción, cambiar todas las contraseñas y el `secret_key` de Flask.

---

## 🤝 Soporte

Para reportar problemas o solicitar funcionalidades:
1. Revisa la documentación en WALKTHROUGH.md
2. Verifica que todos los archivos CSV estén presentes en `/data`
3. Asegúrate de haber ejecutado `create_stego_image.py`

---

## 📄 Licencia

Este proyecto es para fines educativos y de demostración.

**Universidad de Medellín** - Sistema de Gestión Académica  
© 2026 - Todos los derechos reservados

---

## 🎯 Comenzar

```bash
# 1. Instalar dependencias
pip install -r requirements.txt

# 2. Generar recursos
python create_stego_image.py

# 3. Iniciar aplicación
python app.py

# 4. Acceder
# http://localhost:5000
```

**¡Listo para usar!** Inicia sesión con las credenciales proporcionadas.

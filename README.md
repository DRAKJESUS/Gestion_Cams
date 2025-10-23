# Inventario de Cámaras  
**API de Inventario de Dispositivos y Localizaciones**  

---

##  Descripción General

La **API de Inventario de Cámaras** es un sistema backend desarrollado con **FastAPI** y **PostgreSQL** para gestionar dispositivos de red (como cámaras IP), registrar localizaciones físicas y mantener un historial completo de cambios, asignaciones y estados.

---

##  Tecnologías Utilizadas

- **Python 3.10+**
- **FastAPI**
- **SQLAlchemy (asíncrono)**
- **PostgreSQL**
- **Pydantic v2**
- **Uvicorn**
- **Docker / Docker Compose**
- **python-dotenv**

---

## 🚀 Configuración Rápida

### 1️⃣ Clonar y Configurar Entorno

```bash
git clone <repository-url>
cd Inventario_Camaras
python3 -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

### 2️⃣ Instalar Dependencias

```bash
pip install -r requirements.txt
```

### 3️⃣ Configurar Variables de Entorno

Crea un archivo **.env** en la raíz del proyecto con tus credenciales:

```.env ejemplo
POSTGRES_USER=ejemplo
POSTGRES_PASSWORD=ejemplo
POSTGRES_DB=ejemplo
DATABASE_URL=postgresql+asyncpg://${POSTGRES_USER}:${POSTGRES_PASSWORD}@db:5432/${POSTGRES_DB}
```
---

## Ejecución del Proyecto con Docker

1 **Verifica que Docker esté instalado:**

```bash
docker --version
docker compose version
```

2 **Levanta la aplicación:**

```bash
docker compose up --build
```

Esto:
- Construye la imagen de FastAPI.  
- Crea y levanta el contenedor PostgreSQL.  
- Espera a que la base de datos esté lista con el script `wait_for_db.sh`.  
- Inicia la API en el puerto **8000**.

3 **Abre el navegador y verifica:**
- Swagger UI → [http://localhost:8000/docs](http://localhost:8000/docs)  
- Redoc → [http://localhost:8000/redoc](http://localhost:8000/redoc)

4 **Para detener los contenedores:**
```bash
docker compose down
```

5 **(Opcional) Eliminar volúmenes (datos persistentes):**
```bash
docker compose down -v
```

---
##  Endpoints Principales

###  Dispositivos (`/devices/`)

| Método | Endpoint | Descripción |
|:--------|:----------|:-------------|
| **GET** | `/devices/` | Lista todos los dispositivos |
| **POST** | `/devices/` | Crea un nuevo dispositivo |
| **GET** | `/devices/{id}` | Obtiene un dispositivo por ID |
| **PUT** | `/devices/{id}` | Actualiza un dispositivo |
| **DELETE** | `/devices/{id}` | Elimina un dispositivo |
| **POST** | `/devices/{id}/assign/{location_id}` | Asigna el dispositivo a una localización |
| **POST** | `/devices/{id}/change/{location_id}` | Cambia la localización del dispositivo |
| **PUT** | `/devices/{id}/status` | Actualiza el estado del dispositivo |

####  Ejemplo de creación

```json
{
  "ip": "192.168.1.10",
  "status": "active",
  "description": "Cámara principal del acceso",
  "protocol": "RTSP",
  "location_id": 1,
  "ports": [
    { "port_number": 554, "description": "Puerto RTSP principal" }
  ]
}
```

---

###  Localizaciones (`/locations/`)

| Método | Endpoint | Descripción |
|:--------|:----------|:-------------|
| **GET** | `/locations/` | Lista todas las localizaciones |
| **POST** | `/locations/` | Crea una nueva localización |
| **GET** | `/locations/{id}` | Obtiene una localización por ID |
| **PUT** | `/locations/{id}` | Actualiza una localización |
| **DELETE** | `/locations/{id}` | Elimina una localización |

####  Ejemplo de creación

```json
{
  "name": "CAMS",
  "description": "Cámaras de área de física"
}
```

---

###  Historial (`/history/`)

| Método | Endpoint | Descripción |
|:--------|:----------|:-------------|
| **GET** | `/history/` | Muestra el historial completo de operaciones y asignaciones |

####  Ejemplo de respuesta

```json
[
  {
    "id": 3,
    "device_id": 1,
    "action": "Cambio de localización",
    "old_location_id": 1,
    "new_location_id": 2,
    "timestamp": "2025-10-22T06:22:45"
  }
]
```

---

##  Notas Finales

- **Swagger UI:** [http://localhost:8000/docs](http://localhost:8000/docs)  


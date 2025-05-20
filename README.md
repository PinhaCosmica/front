# ADVERTENCIA, AL FINAL NO USE EL 1.5 :v
# 🐾 VetTrack - Sistema de Registro de Mascotas

**VetTrack** es una aplicación web básica desarrollada en **Flask**, con una arquitectura en 3 capas, diseñada para registrar y listar mascotas de manera remota utilizando servicios en la nube (AWS).

---

## ⚙️ Tecnologías utilizadas

- **Lenguaje Backend:** Python 3  
- **Framework:** Flask  
- **ORM:** SQLAlchemy  
- **Base de datos:** PostgreSQL  
- **Infraestructura:** Amazon EC2 (2 instancias: una para la App y otra para la DB)  
- **Protocolos de comunicación:**  
  - HTTP (API REST)  
  - TCP/5432 (PostgreSQL)

---

## 🧱 Arquitectura en 3 capas

| Capa           | Archivo     | Funcionalidad                                    |
|----------------|-------------|--------------------------------------------------|
| Presentación   | `routes.py` | Define la API REST (`POST /mascotas`, `GET /mascotas`) |
| Aplicación     | `routes.py` | Lógica de negocio con SQLAlchemy                 |
| Persistencia   | `models.py` | Modelo `Mascota` mapeado a PostgreSQL            |

---

## 📦 Endpoints

### ✅ `POST /mascotas`

Registra una nueva mascota.

**Request:**
```json
{
  "nombre": "Toby",
  "duenio": "Ana"
}
```

**Response:**
```json
{
  "id": 1,
  "nombre": "Toby",
  "duenio": "Ana"
}
```

---

### ✅ `GET /mascotas`

Obtiene el listado de todas las mascotas registradas.

**Response:**
```json
[
  {
    "id": 1,
    "nombre": "Toby",
    "duenio": "Ana"
  }
]
```

---

## 🔐 Credenciales de prueba

- **Usuario:** `vetuser`  
- **Contraseña:** `vetpass`  
- **Base de datos:** `vettrack`

---

## 🚀 Instrucciones de despliegue en AWS EC2

### 🔸 Paso 1: Instancia EC2 para PostgreSQL

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
```

Entrar al prompt de PostgreSQL:

```bash
sudo -u postgres psql
```

Crear base de datos y usuario:

```sql
CREATE DATABASE vettrack;
CREATE USER vetuser WITH ENCRYPTED PASSWORD 'vetpass';
GRANT ALL PRIVILEGES ON DATABASE vettrack TO vetuser;
\c vettrack
ALTER SCHEMA public OWNER TO vetuser;
GRANT ALL ON SCHEMA public TO vetuser;
\q
```

Editar configuraciones de conexión remota:

**/etc/postgresql/*/main/postgresql.conf**
```ini
listen_addresses = '*'
```

**/etc/postgresql/*/main/pg_hba.conf**
```conf
host all all 0.0.0.0/0 md5
```

Reiniciar el servicio:

```bash
sudo systemctl restart postgresql
```

**Grupo de seguridad:** Asegúrate de abrir el **puerto TCP 5432** solo para la IP privada de la instancia App (Flask).

---

### 🔸 Paso 2: Instancia EC2 para la App (Flask)

```bash
sudo apt update
sudo apt install python3-pip -y
pip3 install Flask Flask-SQLAlchemy psycopg2-binary --break-system-packages
```

Subir el proyecto y descomprimir:

```bash
unzip vettrack.zip
cd vettrack
```

Exportar la variable con la IP privada de la instancia DB:

```bash
export DATABASE_URL=postgresql://vetuser:vetpass@<IP_PRIVADA_DB>:5432/vettrack
```

Ejecutar la aplicación:

```bash
python3 run.py
```

**Grupo de seguridad:** Abrir el **puerto TCP 5000** (temporalmente) desde tu IP pública o `0.0.0.0/0` para pruebas de conexión.

---

## 🖼️ Evidencia esperada

- ✅ Captura de navegador accediendo a: `http://<IP_PUBLICA>:5000/mascotas`  
- ✅ Captura de consola mostrando `python3 run.py` ejecutándose correctamente  
- ✅ Captura de conexión exitosa desde la App a la base de datos PostgreSQL  
- ✅ Diagrama de arquitectura (`arquitectura.png`) incluido en el repositorio

---

## 📁 Estructura del proyecto

```
vettrack/
├── models.py         # Modelo SQLAlchemy
├── routes.py         # Rutas Flask con lógica de negocio
├── run.py            # Punto de entrada de la app
├── requirements.txt  # Dependencias
└── README.md         # Instrucciones y documentación
```

---

## ✅ Estado del proyecto

- [x] Registro de mascotas  
- [x] Listado de mascotas  
- [x] Arquitectura en 3 capas  
- [x] Despliegue funcional en AWS  
- [ ] Autenticación de usuarios (futuro)  
- [ ] Panel administrativo (futuro)

---

## 🙌 Autor

Desarrollado por **Valen**  
Proyecto educativo de arquitectura y despliegue web en la nube con Python + AWS.

---

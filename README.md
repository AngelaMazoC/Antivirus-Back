# 🛡️ Fundación Antivirus - API Backend

API REST desarrollada en **.NET Core** para gestionar datos relacionados con categorías, oportunidades, instituciones, autenticación y donaciones de la Fundación Antivirus.

Este backend provee endpoints seguros con autenticación JWT, documentación con Swagger y despliegue en AWS Elastic Beanstalk.

---

## 🚀 Tecnologías utilizadas

- .NET Core
- ASP.NET Core Web API
- PostgreSQL
- Entity Framework Core
- AutoMapper
- Swagger
- JWT (JSON Web Tokens)
- Arquitectura con Servicios y Repositorios
- CORS habilitado para Vercel y desarrollo local

---

## Requisitos Previos

Antes de comenzar la instalación y ejecución del proyecto, asegúrate de contar con los siguientes elementos:

- .NET 9 SDK:
  Debe estar instalado para compilar y ejecutar la aplicación.
- PostgreSQL:
  Una instancia de PostgreSQL configurada para alojar la base de datos del proyecto.
- Editor de Código:
  Se recomienda utilizar Visual Studio, Visual Studio Code o cualquier otro editor compatible con .NET.
- Git:
  Para clonar y gestionar el repositorio del proyecto.

---

## ⚙️ Configuración local

### 1. Clonar el repositorio

```bash
   git clone https://github.com/AngelaMazoC/Antivirus-Back.git
   cd FundacionAntivirus
```

### 2. Configurar conexión a la base de datos

En el archivo `appsettings.json`:

```json
"ConnectionStrings": {
  "DefaultConnection": "Host=...;Port=5432;Database=Antivirus;Username=postgres;Password=admin123"
}
```

### 3. Configurar clave JWT

```json
"Jwt": {
  "Key": "x2j9KHdx8R3zH5T89K5nFpJtxUwz3S8FJzPiU2oVp/E=",
  "Issuer": "FundacionAntivirus",
  "Audience": "FundacionAntivirus"
}
```

### 4. Aplicar migraciones (si usas EF Core)

```bash
dotnet ef database update
```

### 5. Ejecutar el proyecto

```bash
dotnet run
```

Por defecto corre en `http://localhost:5000`.

---

## 📄 Documentación de API

Swagger está disponible en:

```
http://localhost:5000/swagger
```

---

## 🔐 Autenticación

La API está protegida con **JWT**. Para acceder a rutas protegidas, debes autenticarte y enviar el token en el encabezado.

### Endpoint de Login

```http
POST /api/auth/login
```

#### Body de ejemplo:

```json
{
  "email": "admin@antivirus.com",
  "password": "admin123"
}
```

#### Respuesta:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR..."
}
```

### Usar token en otros endpoints:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR...
```

---

## 📌 Endpoints disponibles

| Método | Ruta                        | Descripción                       | Autenticación |
|--------|-----------------------------|-----------------------------------|---------------|
 POST    | /api/auth/login             | Iniciar sesión                    | ❌            |
| POST   | /api/auth/register          | Registro de nuevo usuario         | ❌            |
| GET    | /api/users                  | Listar todos los usuarios         | ✅ (Admin)    |
| GET    | /api/category               | Obtener todas las categorías      | ✅            |
| POST   | /api/category               | Crear una categoría               | ✅            |
| PUT    | /api/category/{id}          | Actualizar categoría              | ✅            |
| DELETE | /api/category/{id}          | Eliminar categoría                | ✅            |
| GET    | /api/institution            | Obtener instituciones registradas | ✅            |
| POST   | /api/institution            | Crear institución                 | ✅            |
| GET    | /api/opportunity            | Listar oportunidades              | ✅            |
| POST   | /api/opportunity            | Crear nueva oportunidad           | ✅            |
| PUT    | /api/opportunity/{id}       | Actualizar oportunidad            | ✅            |
| DELETE | /api/opportunity/{id}       | Eliminar oportunidad              | ✅            |
| POST   | /api/opportunityinstitution | Asociar oportunidad a institución | ✅            |
| GET    | /api/donations              | Listar todas las donaciones       | ✅            |
| POST   | /api/donations              | Registrar una donación            | ✅            |
---

## 🧪 Cómo probar el backend

1. Inicia el proyecto localmente con `dotnet run`.
2. Accede a `http://localhost:5000/swagger`.
3. Usa el endpoint `/api/auth/login` para autenticarte con:

   - **Email**: `admin@antivirus.com`
   - **Password**: `admin123`

4. Copia el token devuelto.
5. Haz clic en el botón **Authorize** en Swagger e ingresa:

```
Bearer <tu_token>
```

6. Prueba los endpoints protegidos como `GET /api/categories`, `POST /api/oportunities`, etc.

---

## 🌐 CORS habilitado para

- `http://localhost:5173`
- `https://antivirus-front.vercel.app`
- `https://antivirus-frontang-git-amazo-diegogiraldozs-projects.vercel.app`

---

## ☁️ Despliegue en AWS Elastic Beanstalk

Este proyecto está preparado para ser desplegado en AWS Elastic Beanstalk, utilizando un entorno **.NET Core** sobre una instancia EC2.

### 🔧 Requisitos previos

- Cuenta en AWS
- Tener instalada la [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- Tener instalada la [EB CLI (Elastic Beanstalk CLI)](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html)
- Visual Studio o SDK de .NET instalado

### 📁 1. Configura el proyecto

En `Program.cs`, asegúrate de incluir:

```csharp
builder.WebHost.UseUrls("http://*:5000");
```

### 📦 2. Publica el proyecto

```bash
dotnet publish -c Release -o ./publish
```

### 🌱 3. Inicializa el entorno de Beanstalk

```bash
eb init
```

- Selecciona región (ej. `us-east-2`)
- Plataforma: `ASP.NET Core on Linux`
- Asocia con una app existente o nueva

### 🚀 4. Crea y despliega el entorno

```bash
eb create antivirus-env
eb deploy
```
### ✅ 6. Verifica

Una vez desplegado, accede al dominio que AWS te proporciona:

```
http://antivirus-env.eba-xxxxx.us-east-2.elasticbeanstalk.com/swagger
```

---

## 📝 Licencia

Este proyecto es de uso privado para fines institucionales de la Fundación Antivirus.

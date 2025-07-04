Sistema de gestión de citas médicas

Sistema que permite administrar pacientes, citas, registros médicos y usuarios. La API está desarrollada con Node.js, Express y Sequelize, utilizando autenticación JWT con roles (admin y doctor) y refresh tokens para mayor seguridad.

CITASMEDICASANDRES/
├── node_modules/
├── src/
│   ├── config/
│   │   └── db.js
│   ├── controllers/
│   │   ├── citaController.js
│   │   ├── pacienteController.js
│   │   ├── registroController.js
│   │   └── usuarioController.js
│   ├── data/
│   │   └── citasMedicasAndres.sql
│   ├── middleware/
│   │   ├── authMiddleware.js
│   │   └── rolesMiddleware.js
│   ├── models/
│   │   ├── cita.js
│   │   ├── paciente.js
│   │   ├── registroMedico.js
│   │   ├── token.js
│   │   └── usuario.js
│   ├── routers/
│   │   ├── routerCitas.js
│   │   ├── routerPacientes.js
│   │   ├── routerRegistro.js
│   │   └── routerUsuarios.js
│   ├── services/
│       ├── authServices.js
│       ├── dbServices.js
│
├── index.js
├── .env
├── .gitignore
├── package-lock.json
├── package.json
└── README.md

MIDDLEWARES

Se crearon dos middlewares, uno para autentificar tokens y otro para comprobar el rol de los usuarios

ENDPOINTS

Autentificación

POST	/api/auth/inicio_sesion	Inicio de sesión (genera JWT y refresh token)	Público

{
    "cedula": "30888888",
    "contraseña": "1234"
}


POST	/api/auth/refresh	Refrescar token de acceso	Público

{
    "refresh": ""
}

POST	/api/auth/cierre_sesion	Cierre de sesión (invalida tokens)	Autenticado

GET	/api/auth/	Listar todos los usuarios	Admin

POST	/api/auth/	Crear nuevo usuario	Público

{
    "nombres": "Test 2",
    "apellidos": "Test 2",
    "email": "test2@gmail.com",
    "cedula": "30888888",
    "contraseña": "1234",
    "roles": "admin"
}


PUT	/api/auth/:id	Editar usuario	Admin, Doctor

{
    "nombres": "Andres",
    "apellidos": "Caldera Rodriguez",
    "email": "andres@gmail.com",
    "cedula": "30459622",
    "roles": "admin"
}

PATCH	/api/auth/:id	Cambiar contraseña	Admin, Doctor

{
    "contraseña_actual": "1234",
    "nueva_contraseña": "121212"
}

DELETE	/api/auth/:id	Eliminar usuario	Admin

GET	/api/auth/:id	Buscar usuario por ID	Admin

Pacientes

POST	/api/pacientes/	Crear nuevo paciente	Admin

{
    "nombres": "Francisco",
    "apellidos": "Gonzalez",
    "email": "fran@gmail.com",
    "cedula": "30899412"
}

DELETE	/api/pacientes/:id	Eliminar paciente	Admin

PUT	/api/pacientes/:id	Editar paciente	Admin

{
    "nombres": "Francisco",
    "apellidos": "Gonzalez",
    "email": "fran@gmail.com",
    "cedula": "30899412"
}

GET	/api/pacientes/	Listar todos los pacientes	Admin

GET	/api/pacientes/:cedula	Buscar paciente por cédula	Admin


Citas

POST	/api/citas/	Crear nueva cita	Admin

{
    "id_doctor": 4,
    "id_paciente": 1,
    "fecha": "2025-07-09",
    "hora": "13:32:48",
    "es_activa": "activa"

}

DELETE	/api/citas/:id_citas	Eliminar cita	Admin

PUT	/api/citas/:id_citas	Editar cita	Admin

{
    "id_doctor": 4,
    "id_paciente": 1,
    "fecha": "2025-07-09",
    "hora": "13:32:48",
    "es_activa": "activa"

}

GET	/api/citas/	Listar todas las citas	Admin

GET	/api/citas/buscar_por_paciente/:id_paciente	Buscar citas por paciente	Admin

GET	/api/citas/buscar_por_doctor/:id_doctor	Buscar citas por doctor	Admin, Doctor


Registros médicos

POST	/api/registros_medicos/	Crear nuevo registro médico	Admin, Doctor

{
    "id_paciente": 1,
    "diagnosis": "Gripe",
    "tratamiento": "Teragrip"
}

DELETE	/api/registros_medicos/:id_registro	Eliminar registro médico	Admin, Doctor

PUT	/api/registros_medicos/:id_registro	Editar registro médico	Admin, Doctor

{
    "id_paciente": 1,
    "diagnosis": "Gripe",
    "tratamiento": "Teragrip"
}

GET	/api/registros_medicos/	Listar todos los registros médicos	Admin

GET	/api/registros_medicos/:id_paciente	Buscar registros por paciente	Admin, Doctor

DEPENDENCIAS

express, bcrypt.js, sequelize, jsonwebtoken, mysql2, dotenv => npm install

BASE DE DATOS EN MYSQL, GESTOR UTILIZADO PHPMYADMIN (O WORKBENCH)

VARIABLES DE ENTORNO

.env 

DBHOST = localhost
DBUSER = root
DBPASSWORD = 
DBPORT = 3306
DBNAME = citas_medicas_andres
ACCESS_TOKEN_SECRET= tu_clave_secreta_access_token
REFRESH_TOKEN_SECRET= tu_clave_secreta_refresh_token
ACCESS_TOKEN_EXPIRES_IN= 15m
REFRESH_TOKEN_EXPIRES_IN= 24h
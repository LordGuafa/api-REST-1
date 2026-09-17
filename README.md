# API REST — Catálogo de Libros 📚

API REST construida con **Node.js**, **Express 5** y **TypeScript** que permite administrar un catálogo de libros: listar, consultar, crear, actualizar y eliminar libros, además de filtrar resultados por autor, género y disponibilidad.

## Integrantes

| Nombre | Código |
| --- | --- |
| Samuel Aljure Bernal | 20202020111 |
| Juan David Córdoba Aguirre | 20211020097 |
| Juan Esteban Buitrago Chávez | 20211020005 |

## Características

- ✅ CRUD completo de libros (`GET`, `POST`, `PUT`, `DELETE`)
- 🔍 Filtros opcionales por `autor`, `genero` y `disponible`
- 🛠️ Escrito en TypeScript con Express 5
- ⚙️ Configuración de puerto mediante la variable de entorno `PORT`
- 🛠️ Ejecución directa de TypeScript en desarrollo con `ts-node`

## Estructura del proyecto

```
├── src/
│   ├── data/
│   │   └── libros.ts          # "Base de datos" en memoria + interfaz Libro
│   ├── routes/
│   │   └── libros.routes.ts   # Rutas del CRUD de libros
│   └── server.ts              # Punto de entrada de la aplicación
├── dist/                      # Código compilado (build)
├── .env.example               # Plantilla de variables de entorno
├── package.json
└── tsconfig.json
```

## Requisitos

- [Node.js](https://nodejs.org/) ≥ 18
- [pnpm](https://pnpm.io/) (o npm/yarn)

## Instalación

1. Clona el repositorio:

   ```bash
   git clone https://github.com/LordGuafa/api-REST-1.git
   cd api-REST-1
   ```

2. Instala las dependencias:

   ```bash
   pnpm install
   ```

3. Opcionalmente, configura el puerto mediante la variable de entorno `PORT`:

   ```bash
   PORT=4000 pnpm dev
   ```

   | Variable | Descripción | Valor por defecto |
   | --- | --- | --- |
   | `PORT` | Puerto donde escucha el servidor | `3000` |

   **Nota:** existe una plantilla `.env.example`, pero el código actual importa `dotenv` sin inicializarlo, por lo que no carga `.env` automáticamente. Usa una variable de entorno como en el ejemplo anterior.

## Uso

### Desarrollo

```bash
pnpm dev
```

Este comando ejecuta `src/server.ts` con `ts-node`. No incluye recarga automática; reinicia el proceso después de modificar el código.

### Compilar y ejecutar en producción

```bash
pnpm build
pnpm start
```

El servidor queda disponible en `http://localhost:3000`.

## Endpoints

Base URL: `http://localhost:3000`

### Ruta raíz

#### `GET /` — Health check

```json
{ "mensaje": "API de catálogo de libros activa" }
```

### Libros

| Método | Ruta | Descripción |
| --- | --- | --- |
| `GET` | `/libros` | Lista todos los libros (admite filtros) |
| `GET` | `/libros/:id` | Obtiene un libro por su `id` |
| `POST` | `/libros` | Crea un libro nuevo |
| `PUT` | `/libros/:id` | Actualiza un libro existente |
| `DELETE` | `/libros/:id` | Elimina un libro |

#### `GET /libros`

Admite parámetros de consulta opcionales (se pueden combinar):

| Query param | Tipo | Descripción |
| --- | --- | --- |
| `autor` | string | Coincidencia parcial (no distingue mayúsculas) |
| `genero` | string | Coincidencia exacta (no distingue mayúsculas) |
| `disponible` | boolean | `true` o `false` |

```
GET /libros?autor=Orwell&disponible=false
```

```json
[
  {
    "id": 3,
    "titulo": "1984",
    "autor": "George Orwell",
    "genero": "distopía",
    "disponible": false
  }
]
```

#### `GET /libros/:id`

```
GET /libros/2
```

```json
{
  "id": 2,
  "titulo": "El principito",
  "autor": "Antoine de Saint-Exupéry",
  "genero": "fábula",
  "disponible": true
}
```

- `200 OK` si existe, `404 Not Found` en caso contrario.

#### `POST /libros`

Campos obligatorios: `titulo` y `autor`. `genero` y `disponible` son opcionales (por defecto `"sin clasificar"` y `true`).

```
POST /libros
Content-Type: application/json

{
  "titulo": "Rayuela",
  "autor": "Julio Cortázar",
  "genero": "novela",
  "disponible": true
}
```

```json
{
  "id": 4,
  "titulo": "Rayuela",
  "autor": "Julio Cortázar",
  "genero": "novela",
  "disponible": true
}
```

- `201 Created` si se crea correctamente.
- `400 Bad Request` si falta `titulo` o `autor`.

#### `PUT /libros/:id`

Todos los campos del cuerpo son opcionales; solo se actualizan los que se envíen.

```
PUT /libros/1
Content-Type: application/json

{ "disponible": false }
```

- `200 OK` con el libro actualizado, `404 Not Found` si no existe.

#### `DELETE /libros/:id`

```
DELETE /libros/3
```

- `204 No Content` si se elimina, `404 Not Found` si no existe.

## Notas

- ⚠️ Los datos se almacenan **en memoria**: se restablecen cada vez que el servidor se reinicia. La persistencia real se implementará más adelante en el curso.
- La validación de entrada es básica: no se comprueban todos los tipos en tiempo de ejecución. Envía cadenas para `titulo`, `autor` y `genero`, y un booleano para `disponible`.

## Tecnologías

- [Node.js](https://nodejs.org/)
- [Express 5](https://expressjs.com/)
- [TypeScript](https://www.typescriptlang.org/)
- [dotenv](https://github.com/motdotla/dotenv)
- [nodemon](https://nodemon.io/) (desarrollo)

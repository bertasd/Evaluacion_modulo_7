# Evaluación Final Módulo 7 — API REST con pg-cursor: Países y PIB

Backend Node.js con pg y pg-cursor para consultar países con sus datos de PIB, usando transacciones para agregar y eliminar registros.

## ¿Qué hace este ejercicio?

Administra tres tablas relacionadas con transacciones BEGIN/COMMIT/ROLLBACK:

- `paises` → nombre (PK), continente, poblacion
- `paises_pib` → nombre (FK), pib_2019, pib_2020
- `paises_data_web` → nombre_pais (PK), accion (1=INSERT, 0=DELETE)

**Concepto nuevo — pg-cursor:**
En vez de traer todos los registros de una vez, `pg-cursor` abre un cursor en la BD y lee los registros en **bloques** del tamaño que se indique. Esto es más eficiente para tablas grandes.

```js
const cursor = client.query(new Cursor('SELECT * FROM paises JOIN ...'));
cursor.read(5, (err, rows) => { /* primeros 5 registros */ });
cursor.read(5, (err, rows) => { /* siguientes 5 registros */ });
```

## Endpoints

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/paises?limite=5&offset=0` | Lista países con PIB en bloques (pg-cursor) |
| POST | `/paises` | Agrega país + PIB + registro en data_web (accion=1) |
| DELETE | `/paises?nombre=X` | Elimina país + PIB + registro en data_web (accion=0) |

## Estructura

```
evaluacion-final-m7/
├── server.js           # 3 endpoints con pg-cursor y transacciones
├── .env                # Credenciales (editar antes de iniciar)
├── package.json        # Dependencias del proyecto
├── README.md           # Instrucciones de instalación y uso
├── db/
│   └── pool.js         # Conexión a PostgreSQL
├── scripts/
│   └── complemento_evaluacion_final_modulo7.sql  # Script original del curso
└── public/
    ├── style.css       # Tema azul marino con tabla paginada
    └── index.html      # Lista paginada, formulario agregar y eliminar
```

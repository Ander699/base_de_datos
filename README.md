# 🎬 Sistema de Gestión de Videoclub

## Descripción del Proyecto

Este proyecto consiste en el diseño de una **base de datos relacional** para la gestión integral de un videoclub. El sistema permite administrar socios, películas, alquileres, inventario de archivos físicos, actores y preferencias de directores favoritos.

El diagrama entidad-relación (ER) define la estructura lógica de la base de datos, estableciendo las entidades, atributos y relaciones necesarias para el funcionamiento del negocio.

---

## 📋 Entidades y Atributos

### 1. 👤 Socios
| Atributo | Tipo | Descripción |
|----------|------|-------------|
| `id_socio` | INT (PK) | Identificador único del socio |
| `nombre` | VARCHAR(100) | Nombre completo del socio |
| `direccion` | VARCHAR(255) | Dirección del socio |
| `telefono` | VARCHAR(20) | Número de teléfono de contacto |

### 2. 🎫 Alquiler
| Atributo | Tipo | Descripción |
|----------|------|-------------|
| `id_alquiler` | INT (PK) | Identificador único del alquiler |
| `fecha` | DATE | Fecha en que se realizó el alquiler |
| `Precio` | DECIMAL(10,2) | Costo del alquiler |

### 3. 🎥 Película
| Atributo | Tipo | Descripción |
|----------|------|-------------|
| `id_pelicula` | INT (PK) | Identificador único de la película |
| `Titulo` | VARCHAR(200) | Título de la película |
| `Director` | VARCHAR(100) | Nombre del director |
| `genero` | VARCHAR(50) | Género cinematográfico |
| `Año` | INT | Año de estreno |

### 4. 📁 Archivo
| Atributo | Tipo | Descripción |
|----------|------|-------------|
| `id_archivo` | INT (PK) | Identificador único del archivo físico |
| `num_estanterias` | INT | Número de estantería donde se ubica |
| `num_serie` | VARCHAR(50) | Número de serie del medio físico |
| `fecha_compra` | DATE | Fecha de adquisición del medio |
| `Ubicacion` | VARCHAR(100) | Ubicación específica en el local |

### 5. 🎭 Actor
| Atributo | Tipo | Descripción |
|----------|------|-------------|
| `id_actor` | INT (PK) | Identificador único del actor |
| `Nombre` | VARCHAR(100) | Nombre del actor |

### 6. ⭐ Director_Favorito
| Atributo | Tipo | Descripción |
|----------|------|-------------|
| `id_director` | INT (PK) | Identificador único del director |
| `nombre` | VARCHAR(100) | Nombre del director |

---

## 🔗 Relaciones

| Relación | Cardinalidad | Descripción |
|----------|-------------|-------------|
| **Puede** (Socios → Alquiler) | **1:N** | Un socio puede realizar muchos alquileres |
| **pueden** (Película → Alquiler) | **1:N** | Una película puede aparecer en muchos alquileres |
| **pertenece** (Película → Archivo) | **N:1** | Muchas películas pueden pertenecer a un mismo archivo físico |
| **pueden** (Película ↔ Actor) | **N:M** | Una película puede tener muchos actores; un actor puede actuar en muchas películas |
| **SU_F** (Socios ↔ Director_Favorito) | **N:M** | Un socio puede tener varios directores favoritos; un director puede ser favorito de varios socios |

---

## 🗄️ Esquema Relacional Propuesto

```sql
-- Tabla: Socios
CREATE TABLE Socios (
    id_socio INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    direccion VARCHAR(255),
    telefono VARCHAR(20)
);

-- Tabla: Director_Favorito
CREATE TABLE Director_Favorito (
    id_director INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL
);

-- Tabla: Socio_Director_Favorito (relación N:M)
CREATE TABLE Socio_Director_Favorito (
    id_socio INT,
    id_director INT,
    PRIMARY KEY (id_socio, id_director),
    FOREIGN KEY (id_socio) REFERENCES Socios(id_socio),
    FOREIGN KEY (id_director) REFERENCES Director_Favorito(id_director)
);

-- Tabla: Archivo
CREATE TABLE Archivo (
    id_archivo INT PRIMARY KEY AUTO_INCREMENT,
    num_estanterias INT,
    num_serie VARCHAR(50),
    fecha_compra DATE,
    Ubicacion VARCHAR(100)
);

-- Tabla: Pelicula
CREATE TABLE Pelicula (
    id_pelicula INT PRIMARY KEY AUTO_INCREMENT,
    Titulo VARCHAR(200) NOT NULL,
    Director VARCHAR(100),
    genero VARCHAR(50),
    Anio INT,
    id_archivo INT,
    FOREIGN KEY (id_archivo) REFERENCES Archivo(id_archivo)
);

-- Tabla: Actor
CREATE TABLE Actor (
    id_actor INT PRIMARY KEY AUTO_INCREMENT,
    Nombre VARCHAR(100) NOT NULL
);

-- Tabla: Pelicula_Actor (relación N:M)
CREATE TABLE Pelicula_Actor (
    id_pelicula INT,
    id_actor INT,
    PRIMARY KEY (id_pelicula, id_actor),
    FOREIGN KEY (id_pelicula) REFERENCES Pelicula(id_pelicula),
    FOREIGN KEY (id_actor) REFERENCES Actor(id_actor)
);

-- Tabla: Alquiler
CREATE TABLE Alquiler (
    id_alquiler INT PRIMARY KEY AUTO_INCREMENT,
    fecha DATE NOT NULL,
    Precio DECIMAL(10,2),
    id_socio INT,
    id_pelicula INT,
    FOREIGN KEY (id_socio) REFERENCES Socios(id_socio),
    FOREIGN KEY (id_pelicula) REFERENCES Pelicula(id_pelicula)
);
# Coordinadora Test - Database

The system must be able to handle multiple package types, process real-time tracking information, assign optimal routes for delivery, and provide users with detailed and up-to-date information on the status and location of their shipments.

### Functional Requirements:

1. Develop the Logical Diagram, Physical Diagram and Relational Diagram.

2. Normalize the database to 4FN.

```sql
DROP DATABASE `coordinadora`;
CREATE DATABASE `coordinadora`;
USE `coordinadora`;

CREATE TABLE `pais` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50) UNIQUE NOT NULL
);

CREATE TABLE `departamento` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50) UNIQUE NOT NULL,
  `id_pais_fk` int NOT NULL
);

CREATE TABLE `ciudad` (
  `id` int PRIMARY KEY,
  `nombre` varchar(50) UNIQUE NOT NULL,
  `id_departamento_fk` int NOT NULL
);

CREATE TABLE `direccion` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `tipo_via` varchar(50),
  `numero_principal` smallint,
  `letra_principal` char(2),
  `bis` char(10),
  `letra_secundaria` char(2),
  `cardinal_primario` char(10),
  `numero_secundario` smallint,
  `cardinal_secundario` char(10),
  `complemento` varchar(50),
  `id_ciudad_fk` int NOT NULL
);

CREATE TABLE `detalle_paquete` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `largo` float(3),
  `ancho` float(3),
  `alto` float(3),
  `peso` float(3)
);

CREATE TABLE `paquete` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `id_detalle_paquete_fk` int NOT NULL,
  `id_tipo_paquete_fk` int NOT NULL,
  `id_envio_fk` int NOT NULL
);

CREATE TABLE `tipo_paquete` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50)
);

CREATE TABLE `estado_envio` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50)
);

CREATE TABLE `tipo_envio` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50)
);

CREATE TABLE `oficina` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `telefono` int,
  `id_direccion_fk` int NOT NULL
);

CREATE TABLE `empleado` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50),
  `apellido` varchar(50),
  `email` varchar(50),
  `id_oficina_fk` int NOT NULL,
  `id_puesto_fk` int NOT NULL
);

CREATE TABLE `puesto` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50)
);

CREATE TABLE `cliente` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `cedula` int UNIQUE NOT NULL,
  `nombre` varchar(50),
  `apellido` varchar(50),
  `email` varchar(50),
  `telefono` int,
  `id_direccion_fk` int NOT NULL
);

CREATE TABLE `envio` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `id_tipo_envio_fk` int NOT NULL,
  `id_estado_envio_fk` int NOT NULL,
  `id_ruta_envio_fk` int NOT NULL,
  `id_ubicacion_envio_fk` int NOT NULL,
  `id_detalle_envio_fk` int NOT NULL
);

CREATE TABLE `detalle_envio` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `fehca_envio` date,
  `fehca_estimada` date,
  `fecha_entrega` date,
  `cantidad` int
);

CREATE TABLE `usuario` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `username` varchar(50) UNIQUE NOT NULL,
  `password` varchar(150) UNIQUE NOT NULL,
  `email` varchar(100) UNIQUE NOT NULL
);

CREATE TABLE `rol` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50) UNIQUE NOT NULL
);

CREATE TABLE `userrol` (
  `idUsuarioFk` int NOT NULL,
  `idRolFk` int NOT NULL,
  PRIMARY KEY (`idUsuarioFk`, `idRolFk`)
);

CREATE TABLE `ruta_envio` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `id_oficina_fk` int NOT NULL,
  `id_cliente_fk` int NOT NULL
);

CREATE TABLE `ubicacion_envio` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `localizacion` varchar(50),
  `descripcion` varchar(200)
);

CREATE TABLE `forma_pago` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50)
);

CREATE TABLE `pago` (
  `id` int UNIQUE PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `total` float(3),
  `fecha_pago` date,
  `id_cliente_fk` int NOT NULL,
  `id_forma_pago_fk` int NOT NULL
);

ALTER TABLE `departamento` ADD FOREIGN KEY (`id_pais_fk`) REFERENCES `pais` (`id`);
ALTER TABLE `ciudad` ADD FOREIGN KEY (`id_departamento_fk`) REFERENCES `departamento` (`id`);
ALTER TABLE `direccion` ADD FOREIGN KEY (`id_ciudad_fk`) REFERENCES `ciudad` (`id`);
ALTER TABLE `paquete` ADD FOREIGN KEY (`id_detalle_paquete_fk`) REFERENCES `detalle_paquete` (`id`);
ALTER TABLE `paquete` ADD FOREIGN KEY (`id_tipo_paquete_fk`) REFERENCES `tipo_paquete` (`id`);
ALTER TABLE `direccion` ADD FOREIGN KEY (`id`) REFERENCES `oficina` (`id_direccion_fk`);
ALTER TABLE `empleado` ADD FOREIGN KEY (`id_puesto_fk`) REFERENCES `puesto` (`id`);
ALTER TABLE `empleado` ADD FOREIGN KEY (`id_oficina_fk`) REFERENCES `oficina` (`id`);
ALTER TABLE `cliente` ADD FOREIGN KEY (`id_direccion_fk`) REFERENCES `direccion` (`id`);
ALTER TABLE `envio` ADD FOREIGN KEY (`id_tipo_envio_fk`) REFERENCES `tipo_envio` (`id`);
ALTER TABLE `envio` ADD FOREIGN KEY (`id_ruta_envio_fk`) REFERENCES `ruta_envio` (`id`);
ALTER TABLE `envio` ADD FOREIGN KEY (`id_estado_envio_fk`) REFERENCES `estado_envio` (`id`);
ALTER TABLE `envio` ADD FOREIGN KEY (`id_ubicacion_envio_fk`) REFERENCES `ubicacion_envio` (`id`);
ALTER TABLE `envio` ADD FOREIGN KEY (`id_detalle_envio_fk`) REFERENCES `detalle_envio` (`id`);
ALTER TABLE `paquete` ADD FOREIGN KEY (`id_envio_fk`) REFERENCES `envio` (`id`);
ALTER TABLE `userrol` ADD FOREIGN KEY (`idUsuarioFk`) REFERENCES `usuario` (`id`);
ALTER TABLE `userrol` ADD FOREIGN KEY (`idRolFk`) REFERENCES `rol` (`id`);
ALTER TABLE `pago` ADD FOREIGN KEY (`id_cliente_fk`) REFERENCES `cliente` (`id`);
ALTER TABLE `pago` ADD FOREIGN KEY (`id_forma_pago_fk`) REFERENCES `forma_pago` (`id`);
ALTER TABLE `ruta_envio` ADD FOREIGN KEY (`id_oficina_fk`) REFERENCES `oficina` (`id`);
ALTER TABLE `ruta_envio` ADD FOREIGN KEY (`id_cliente_fk`) REFERENCES `cliente` (`id`);
```


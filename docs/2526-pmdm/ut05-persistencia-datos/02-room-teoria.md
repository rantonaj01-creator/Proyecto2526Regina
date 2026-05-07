---
title: Room Database (Teoría)
sidebar_position: 3
description: Conceptos fundamentales de la librería de persistencia Room.
---

Room es una capa de abstracción sobre SQLite que permite un acceso a la base de datos más fluido y seguro, aprovechando toda la potencia de SQLite.

### Conceptos Clave
- **Entities**: Representación de las tablas de la base de datos mediante clases Java.
- **DAO (Data Access Objects)**: Interfaces que definen los métodos para acceder a los datos (Consultas SQL, Inserts, Updates).
- **Database**: El punto de acceso principal a la conexión de persistencia.
- **Type Converters**: Cómo manejar tipos de datos no soportados nativamente por SQLite.

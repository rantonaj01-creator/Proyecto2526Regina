---
title: Room Database (Tutorial)
sidebar_position: 4
description: Tutorial práctico de implementación de Room con MVVM y Repository.
---

En este tutorial práctico implementaremos una aplicación de gestión de tareas o elementos que permite persistencia local completa.

### Fases del Tutorial
1. **Configuración de Dependencias**: Gradle para Room.
2. **Definición del Modelo**: Creación de la Entidad.
3. **Creación del DAO**: Métodos CRUD y una consulta especial para filtrar favoritos.
4. **Repositorio**: Clase que abstrae el acceso a Room.
5. **ViewModel**: Gestión del estado de la UI y exposición de LiveData.
6. **UI con DataBinding**: Conexión de la vista con los datos del ViewModel.

---
title: Supabase Storage y Retrofit
sidebar_position: 8
description: Gestión de archivos binarios e integración de imágenes en el ecosistema Android.
---

Aprenderemos a manejar archivos pesados (imágenes) que no deben almacenarse directamente en bases de datos de texto como Firestore.

### Workflow del Tutorial
1. **Introducción a Supabase**: Alternativa de código abierto para Storage.
2. **Retrofit para Subida de Archivos**: Configuración de la interfaz API para enviar `MultipartBody`.
3. **Subida de Imágenes**: Capturar o seleccionar una imagen y subirla al Bucket de Supabase.
4. **Vínculo con Firestore**: Una vez subida la imagen, guardaremos su URL pública en Firestore para asociarla a un documento del usuario.
5. **Carga de Imágenes**: Uso de librerías como Glide para mostrar la imagen desde la URL.

---
title: Room Database (Teoría)
sidebar_position: 3
description: Conceptos fundamentales de la librería de persistencia Room en Android.
---

Room es una capa de abstracción sobre SQLite que permite un acceso a la base de datos más fluido y seguro, aprovechando toda la potencia de SQLite. Como parte de Android Jetpack, es la solución recomendada por Google para gestionar la persistencia local de datos en aplicaciones modernas.

:::info ¿Por qué usar Room en lugar de SQLite directo?
Room se encarga de las tareas pesadas: verifica las consultas SQL en tiempo de compilación, elimina el código repetitivo (Boilerplate) y se integra perfectamente con LiveData o Flow para observar cambios en los datos de forma reactiva.
:::

## Los Tres Pilares de Room

Para implementar Room en tu proyecto, debes comprender cómo interactúan sus tres componentes principales: la Entidad, el DAO y la Base de Datos.

### 1. La Entidad (`@Entity`)

Una entidad representa una tabla dentro de la base de datos relacional. En Java, definimos una entidad creando una clase y anotándola con `@Entity`. Cada campo de la clase corresponde a una columna de la tabla.

```java title="java/model/entity/User.java"
@Entity(tableName = "users")
public class User {
    @PrimaryKey(autoGenerate = true)
    private int id;

    @ColumnInfo(name = "user_name")
    private String name;

    @ColumnInfo(name = "user_email")
    private String email;

    // Getters y Setters obligatorios para Room
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

### 2. El DAO (`@Dao`)

El **Data Access Object** es el corazón de Room. Es una interfaz (o clase abstracta) donde defines los métodos para acceder a los datos. Room genera automáticamente la implementación de estos métodos en tiempo de compilación.

```java title="java/data/local/dao/UserDao.java"
@Dao
public interface UserDao {
    @Insert
    void insert(User user);

    @Update
    void update(User user);

    @Delete
    void delete(User user);

    @Query("SELECT * FROM users ORDER BY user_name ASC")
    List<User> getAllUsers();

    @Query("SELECT * FROM users WHERE id = :userId")
    User getUserById(int userId);
}
```

### 3. La Base de Datos (`@Database`)

Es el punto de acceso principal a la conexión de persistencia. Debe cumplir tres requisitos:
1. Ser una clase abstracta que extienda de `RoomDatabase`.
2. Incluir la anotación `@Database` con la lista de entidades y la versión.
3. Contener un método abstracto que devuelva una instancia de cada DAO definido.

```java title="java/data/local/AppDatabase.java"
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```

## Gestión de Datos Complejos: Type Converters

SQLite solo soporta tipos de datos básicos (NULL, INTEGER, REAL, TEXT, BLOB). Si necesitas guardar tipos personalizados, como un objeto `Date` o una lista de strings, debes usar **Type Converters**.

```java title="java/data/local/converters/DateConverter.java"
public class DateConverter {
    @TypeConverter
    public static Date fromTimestamp(Long value) {
        return value == null ? null : new Date(value);
    }

    @TypeConverter
    public static Long dateToTimestamp(Date date) {
        return date == null ? null : date.getTime();
    }
}
```
*Debes registrar estos conversores en tu clase `@Database` usando la anotación `@TypeConverters(DateConverter.class)`.*

## Integración en la Arquitectura MVVM

En un entorno profesional, la base de datos Room nunca se comunica directamente con la Vista (Activity/Fragment). Seguimos el patrón **MVVM + Repository**:

1. **View**: Observa cambios en el ViewModel (usualmente mediante LiveData o Flow).
2. **ViewModel**: Solicita operaciones al Repositorio.
3. **Repository**: Abstrae el acceso a Room (o a una API externa), gestionando dónde residen los "datos de verdad".

:::tip Persistencia en Segundo Plano
Recuerda que Room prohíbe realizar operaciones en el hilo principal (Main Thread) para evitar bloqueos en la interfaz. Debes usar siempre soluciones de concurrencia como `Executors`, `Handlers` o, preferiblemente en aplicaciones modernas, Corrutinas (si usaras Kotlin) o `ListenableFuture`.
:::

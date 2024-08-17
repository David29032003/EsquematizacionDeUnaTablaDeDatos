# Nombre: Reeves Goñi, David Fernando

## Esquematización de la tabla de datos PEA-PRIVADA-2021-MUESTRA
En este pequeño proyecto realicé la esquematización de una muestra de una tabla de datos llamada *PEA-PRIVADA-2021-MUESTRA*.

En esta esquematización, después de normalizar la tabla de datos, pensé que el esquema estrella podría ser suficiente debido a su simplicidad y eficiencia para consultas.

Sin embargo, al ver que Dim_Empresa tenía relaciones con Dim_Tamaño_Empresa y Dim_Departamento, me di cuenta de que el esquema snowflake sería más adecuado. Este esquema, al permitir una mayor normalización y una estructura más detallada, resulta mejor para manejar datos complejos y relaciones jerárquicas, evitando redundancias y ofreciendo una mejor integridad de los datos a medida que la complejidad aumenta.

# TABLA ORIGINAL

Para el problema extraje una muestra de 10 filas para tener una mejor idea de qué tablas formar.

| MES   | REMUNERACION | RANGO_EDAD       | NIVEL_EDUCATIVO                                               | TAMAÑO_EMPRESA              | SEXO           | DEPARTAMENTO | ACTIVIDAD_ECONOMICA                                                 | UUID    |
|-------|--------------|------------------|----------------------------------------------------------------|-----------------------------|----------------|--------------|----------------------------------------------------------------------|---------|
| 202108| 15900        | NO DETERMINADO   | GRADO DE MAESTRÍA                                              | DE 1 A 10 TRABAJADORES       | NO DETERMINADO | AREQUIPA     | ACTIVIDADES INMOBILIARIAS, EMPRESARIALES Y DE ALQUILER              | 2502688 |
| 202108| 1433.24      | HASTA 24         | EDUCACIÓN TÉCNICA COMPLETA                                      | DE 501 A MÁS TRABAJADORES    | MASCULINO      | PIURA        | INDUSTRIAS MANUFACTURERAS                                            | 2081421 |
| 202107| 1310.85      | 30 A 44          | EDUCACIÓN TÉCNICA COMPLETA                                      | DE 11 A 50 TRABAJADORES      | MASCULINO      | LIMA         | TRANSPORTE, ALMACENAMIENTO Y COMUNICACIONES                         | 275217  |
| 202109| 930          | 30 A 44          | EDUCACIÓN UNIVERSITARIA COMPLETA                                | DE 1 A 10 TRABAJADORES       | MASCULINO      | AREQUIPA     | TRANSPORTE, ALMACENAMIENTO Y COMUNICACIONES                         | 5464640 |
| 202108| 980          | 30 A 44          | EDUCACIÓN SECUNDARIA COMPLETA                                   | DE 11 A 50 TRABAJADORES      | FEMENINO       | LIMA         | ACTIVIDADES INMOBILIARIAS, EMPRESARIALES Y DE ALQUILER              | 6678517 |
| 202107| 310          | HASTA 24         | EDUCACIÓN SECUNDARIA COMPLETA                                   | DE 501 A MÁS TRABAJADORES    | FEMENINO       | ICA          | INDUSTRIAS MANUFACTURERAS                                            | 7151045 |
| 202108| 3512.3       | 30 A 44          | ESTUDIOS DE MAESTRÍA INCOMPLETA                                 | DE 101 A 500 TRABAJADORES    | MASCULINO      | LIMA         | ACTIVIDADES INMOBILIARIAS, EMPRESARIALES Y DE ALQUILER              | 6644696 |
| 202107| 2100         | 30 A 44          | EDUCACIÓN SUPERIOR COMPLETA (INSTITUTO SUPERIOR, ETC.)          | DE 11 A 50 TRABAJADORES      | MASCULINO      | LIMA         | COMERCIO AL POR MAYOR Y AL POR MENOR, REP. VEHÍC. AUTOM.            | 874379  |
| 202109| 2919.14      | 45 A 60          | EDUCACIÓN SECUNDARIA INCOMPLETA                                 | DE 101 A 500 TRABAJADORES    | MASCULINO      | LIMA         | ACTIVIDADES INMOBILIARIAS, EMPRESARIALES Y DE ALQUILER              | 1837006 |
| 202109| 995.67       | HASTA 24         | EDUCACIÓN SECUNDARIA COMPLETA                                   | DE 501 A MÁS TRABAJADORES    | MASCULINO      | LIMA         | COMERCIO AL POR MAYOR Y AL POR MENOR, REP. VEHÍC. AUTOM.            | 8168282 |


## 1NF: 

**Todos los atributos deben contener solo valores atómicos y todos los registros deben ser únicos.**

La tabla original ya se encuentra en 1NF porque cada columna contiene valores atómicos y cada fila es única.

Por lo que no habría cambios que realizar a la tabla de datos.

## 2NF: 

**La tabla debe estar en 1NF y cada tabla debe tener solo 1 clave primaria.**

Para esta forma normal, la tabla ya cumple con 1NF y posee solo 1 llave primaria o, dicho de otro modo, no debe presentar dependencias parciales debido a la ausencia de una clave primaria compuesta. Por lo tanto, ya se encuentra en 2NF, pero sí posee dependencias transitivas, por lo que pasaré a la 3NF.

## 3NF: 

**Las tablas deben cumplir con la 2NF y no tener dependencias funcionales transitivas.**

## TABLAS RESULTANTES DESPUÉS DE APLICAR LA 3NF

### Dim_Sexo
| id_sexo | sexo           |
|---------|----------------|
| 1       | NO DETERMINADO |
| 2       | MASCULINO      |
| 3       | FEMENINO       |

**Llave primaria:** `id_sexo`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria, por lo que ya está en 3NF.

### Dim_Departamento
| id_departamento | departamento |
|-----------------|--------------|
| 1               | AREQUIPA     |
| 2               | PIURA        |
| 3               | LIMA         |
| 4               | ICA          |

**Llave primaria:** `id_departamento`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria, por lo que ya está en 3NF.

### Dim_Tamaño_Empresa
| id_tamaño_empresa | tamaño_empresa             |
|-------------------|----------------------------|
| 1                 | DE 1 A 10 TRABAJADORES     |
| 2                 | DE 11 A 50 TRABAJADORES    |
| 3                 | DE 51 A 100 TRABAJADORES   |
| 4                 | DE 101 A 500 TRABAJADORES  |
| 5                 | DE 501 A MÁS TRABAJADORES  |

**Llave primaria:** `id_tamaño_empresa`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria, por lo que ya está en 3NF.

### Dim_Empresa
| id_empresa | id_tamaño_empresa | id_actividad_economica | id_departamento |
|------------|-------------------|------------------------|-----------------|
| 1          | 1                 | 1                      | 1               |
| 2          | 5                 | 2                      | 2               |
| 3          | 2                 | 3                      | 3               |
| 4          | 1                 | 3                      | 1               |
| 5          | 2                 | 1                      | 3               |
| 6          | 5                 | 2                      | 4               |
| 7          | 5                 | 1                      | 3               |
| 8          | 2                 | 4                      | 3               |
| 9          | 4                 | 1                      | 3               |
| 10         | 5                 | 4                      | 3               |

**Llave primaria:** `id_empresa`  
**Llaves foráneas:** `id_tamaño_empresa`, `id_departamento`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria, por lo que ya está en 3NF.

### Dim_Nivel_Educativo
| id_nivel_educativo | nivel_educativo                                      |
|--------------------|-----------------------------------------------------|
| 1                  | GRADO DE MAESTRÍA                                    |
| 2                  | EDUCACIÓN TÉCNICA COMPLETA                           |
| 3                  | EDUCACIÓN UNIVERSITARIA COMPLETA                     |
| 4                  | EDUCACIÓN SECUNDARIA COMPLETA                        |
| 5                  | ESTUDIOS DE MAESTRÍA INCOMPLETA                      |
| 6                  | EDUCACIÓN SUPERIOR COMPLETA (INSTITUTO SUPERIOR, ETC.)|
| 7                  | EDUCACIÓN SECUNDARIA INCOMPLETA                      |
| 8                  | EDUCACIÓN SECUNDARIA COMPLETA                        |

**Llave primaria:** `id_nivel_educativo`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria, por lo que ya está en 3NF.

### Dim_Rango_Edad
| id_rango_edad | rango_edad |
|---------------|------------|
| 1             | NO DETERMINADO |
| 2             | HASTA 24    |
| 3             | 25 A 29     |
| 4             | 30 A 44     |
| 5             | 45 A 60     |

**Llave primaria:** `id_rango_edad`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria, por lo que ya está en 3NF.

### Dim_Actividad_Economica
| id_actividad_economica | actividad_economica                                 |
|------------------------|-----------------------------------------------------|
| 1                      | ACTIVIDADES INMOBILIARIAS, EMPRESARIALES Y DE ALQUILER |
| 2                      | INDUSTRIAS MANUFACTURERAS                           |
| 3                      | TRANSPORTE, ALMACENAMIENTO Y COMUNICACIONES         |
| 4                      | COMERCIO AL POR MAYOR Y AL POR MENOR, REP. VEHÍC. AUTOM. |

**Llave primaria:** `id_actividad_economica`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria, por lo que ya está en 3NF.

### Hecho_Empleado

| MES   | REMUNERACION | id_rango_edad | id_nivel_educativo | id_empresa | id_sexo | UUID   |
|-------|--------------|---------------|--------------------|------------|---------|--------|
| 202108| 15900        | 1             | 1                  | 1          | 1       | 2502688|
| 202108| 1433.24      | 2             | 2                  | 2          | 2       | 2081421|
| 202107| 1310.85      | 4             | 2                  | 3          | 2       | 275217 |
| 202109| 930          | 4             | 3                  | 4          | 2       | 5464640|
| 202108| 980          | 4             | 4                  | 5          | 3       | 6678517|
| 202107| 310          | 2             | 4                  | 6          | 3       | 7151045|
| 202108| 3512.3       | 4             | 5                  | 7          | 2       | 6644696|
| 202107| 2100         | 4             | 6                  | 8          | 2       | 874379 |
| 202109| 2919.14      | 5             | 7                  | 9          | 2       | 1837006|
| 202109| 995.67       | 2             | 8                  | 10         | 2       | 8168282|

**Llave primaria:** `uuid`

**Llave foranea:** `id_rango_edad`, `id_nivel_educativo`, `id_empresa`, `id_sexo`

No hay dependencias funcionales transitivas, ya que todas las columnas no clave dependen directamente de la clave primaria por lo que ya está en 3NF.

Por lo tanto la tabla original ya se encuentra normalizada hasta la 3NF y ahora puedo proceder con el esquema snowflake.		

# Esquema Snowflake

## Tablas Dimensionales
Las tablas dimensionales en este esquema son:
- `Dim_Sexo`
- `Dim_Departamento`
- `Dim_Tamaño_Empresa`
- `Dim_Empresa`
- `Dim_Nivel_Educativo`
- `Dim_Rango_Edad`
- `Dim_Actividad_Economica`

## Tabla de Hechos
La tabla de hechos que hemos seleccionado es:
- `Hecho_Empleado`

## ¿Por qué elegí la tabla Hecho_Empleado como la tabla de hechos?
Elegí la tabla `Hecho_Empleado` por varias razones:
- **Datos Cuantitativos**: La columna `remuneracion` contiene datos numéricos que son esenciales para el análisis.
- **Llaves Foráneas**: Tiene llaves foráneas que se conectan con nuestras tablas dimensionales.
- **Datos de Tiempo**: La columna `mes` nos permite analizar los datos a lo largo del tiempo.
- **Granularidad**: Cada fila de la tabla representa la remuneración de un empleado específico en un mes determinado, lo que proporciona una granularidad detallada.
- **Identificador Único**: La tabla tiene un identificador único, lo cual es crucial para la integridad de los datos.

## Creación del Esquema
Para construir el esquema, utilicé Power Pivot en Excel y tomé una captura de pantalla para documentar el diseño.

![Esquema](Imagenes\Captura3.png)
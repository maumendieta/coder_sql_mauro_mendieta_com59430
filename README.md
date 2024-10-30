# PROYECTO LIQUIDACIÓN SUELDOS - ESCOLAR # 
**MENDIETA, MAURO GERMÁN**
:white_check_mark: *COMISIÓN 59430*

***El siguiente proyecto surge de la necesidad de armar un sistema ágil que permita realizar el seguimiento de la planta, docente y no docente, de diversos centros educativos. En base al seguimiento del personal, se pretende realizar la liquidación de sueldos de cada uno de los agentes pertenecientes a cada Escuela.*** 

## PLANTEO INICIAL - D.E.R. ONTOLÓGICO *(realizado en Excalidraw)*
![imagen](https://github.com/maumendieta/coder_sql_mauro_mendieta_com59430/blob/main/liqui_mgm_sql59430_DER_EXCALIDRAW.jpg)

## Base de Datos: Liqui Escuela

La base de datos **Liqui Escuela** está diseñada para gestionar la información de escuelas, empleados y sueldos, abarcando diferentes aspectos como cargos, asistencia, tipos de cargo y áreas específicas en cada institución. Este modelo facilita la administración de personal y la generación de reportes de sueldos, asistencia, y antigüedad de los empleados, entre otros.

## Estructura de la Base de Datos

### Tablas Principales y Tipos de Datos

#### `escuela`
- **Descripción:** Representa la información de cada escuela.
- **Campos:**
  - `id_escuela` (INT, PK, AUTO_INCREMENT): Identificador único de la escuela.
  - `nombre` (VARCHAR(200)): Nombre de la escuela.
  - `numero` (INT, UNIQUE): Número único de la escuela.
  - `categoria` (ENUM): Categoría de la escuela (`CAT1`, `CAT2`, `CAT3`).
  - `ciudad` (VARCHAR(200)): Ciudad donde se ubica la escuela.
  - `nivel` (ENUM): Nivel educativo de la escuela (`PRIMARIA`, `SECUNDARIA`, `SUPERIOR`).

#### `empleado`
- **Descripción:** Contiene los datos personales y bancarios de los empleados.
- **Campos:**
  - `id_empleado` (INT, PK, AUTO_INCREMENT): Identificador único del empleado.
  - `id_banco` (INT, FK): Identificador del banco del empleado.
  - `nombre` (VARCHAR(200)): Nombre completo del empleado.
  - `dni` (INT): Documento Nacional de Identidad del empleado.
  - `fecha_nacimiento` (DATE): Fecha de nacimiento del empleado.
  - `fecha_ingreso` (DATE): Fecha de ingreso a la institución.
  - `fecha_baja` (DATE): Fecha de baja (si aplica).
  - `sucursal_banco` (INT): Número de sucursal bancaria.
  - `cuenta_banco` (INT): Número de cuenta bancaria.
  - `agremiado` (ENUM): Si el empleado está agremiado (`SI`, `NO`).

#### `cargo`
- **Descripción:** Define los cargos dentro de una escuela.
- **Campos:**
  - `id_cargo` (INT, PK, UNIQUE, AUTO_INCREMENT): Identificador único del cargo.
  - `id_escuela` (INT, FK): Escuela a la que pertenece el cargo.
  - `id_tipo_cargo` (INT, FK): Tipo de cargo.
  - `id_area` (INT, FK): Área de la escuela relacionada.
  - `denominacion` (VARCHAR(200)): Nombre o denominación del cargo.
  - `horas` (INT): Horas del cargo.
  - `turno` (VARCHAR(200)): Turno asignado.
  - `puntaje` (DECIMAL(10,4)): Puntaje asignado al cargo.

#### `sueldo`
- **Descripción:** Información de pagos realizados a los empleados.
- **Campos:**
  - `id_sueldo` (INT, PK, AUTO_INCREMENT): Identificador único del sueldo.
  - `id_banco` (INT, FK): Banco al que pertenece el sueldo.
  - `monto` (DECIMAL(10,2)): Monto del sueldo.
  - `fecha_deposito` (DATE): Fecha de depósito.

### Tablas Auxiliares y Tipos de Datos

#### `tipo_cargo`
- **Descripción:** Clasificación de cargos en una escuela.
- **Campos:**
  - `id_tipo_cargo` (INT, PK, AUTO_INCREMENT): Identificador único.
  - `tipo_cargo` (VARCHAR(200)): Descripción del tipo de cargo.

#### `area`
- **Descripción:** Define áreas dentro de una escuela.
- **Campos:**
  - `id_area` (INT, PK, AUTO_INCREMENT): Identificador único.
  - `area_escuela` (VARCHAR(200)): Nombre del área.

#### `sit_revista`
- **Descripción:** Situación del empleado respecto a su cargo.
- **Campos:**
  - `id_sit_revista` (INT, PK, AUTO_INCREMENT): Identificador único.
  - `situacion_revista` (VARCHAR(200)): Descripción de la situación (por ejemplo: Titular, Reemplazante).

#### `banco`
- **Descripción:** Bancos donde los empleados tienen cuentas.
- **Campos:**
  - `id_banco` (INT, PK, AUTO_INCREMENT): Identificador único.
  - `nombre_banco` (VARCHAR(200)): Nombre del banco.

#### `emp_cargo`
- **Descripción:** Relación entre empleados y cargos.
- **Campos:**
  - `id_emp_cargo` (INT, PK, AUTO_INCREMENT): Identificador único.
  - `id_empleado` (INT, FK): Empleado.
  - `id_cargo` (INT, FK): Cargo asignado.
  - `id_sit_revista` (INT, FK): Situación respecto al cargo.
  - `ftp_cgo` (DATE): Fecha de toma de posesión.
  - `horas_cargo` (INT): Horas trabajadas en el cargo.
  - `antiguedad_años` (INT): Años de antigüedad.
  - `antiguedad_meses` (INT): Meses de antigüedad.

### Relaciones entre Tablas

- **Escuela - Cargo:** Una escuela puede tener múltiples cargos. `id_escuela` en `cargo` es clave foránea.
- **Empleado - Banco:** Cada empleado está asociado a un banco. `id_banco` en `empleado` y en `sueldo` es clave foránea.
- **Empleado - Cargo (emp_cargo):** Un empleado puede ocupar diferentes cargos en una o más escuelas. `emp_cargo` gestiona la relación entre `empleado` y `cargo`.
- **Sueldo - Banco:** El sueldo está relacionado con el banco donde se realiza el depósito.

### Problemáticas que Resuelve el Modelo

1. **Gestión de Escuelas y Empleados:** Permite almacenar información detallada de cada escuela y empleado, con soporte para cargos específicos, áreas y situaciones de revista.
2. **Control de Asistencia:** La tabla `asistencia` permite registrar la asistencia de los empleados, facilitando la generación de reportes de presentismo.
3. **Gestión de Sueldos:** La tabla `sueldo` gestiona el historial de pagos a empleados, y `emp_cargo` almacena los detalles del cargo, antigüedad y horas trabajadas, facilitando los cálculos de liquidaciones.
4. **Organización de Cargos:** La relación entre `cargo`, `tipo_cargo`, y `area` permite una clasificación estructurada de puestos en cada escuela.

Este modelo permite la centralización de la información para generar reportes de forma eficiente y precisa, resolviendo la complejidad de administrar recursos humanos en una institución educativa.


## DIAGRAMA DE ENTIDADES Y RELACIONES *(WorkBench)*
![imagen](https://github.com/maumendieta/coder_sql_mauro_mendieta_com59430/blob/main/liqui_mgm_sql59430_DER_WB.jpg)



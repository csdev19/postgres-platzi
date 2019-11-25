## Curso de postgresSql platzi

mi documentacion 

## Intro postgres 

- PostGIS: Sirve para trabajar con mapas
- PL/PgSQL
- ACID
  - **A**tomicity - Atomicidad: Quiere decir que cada funcion corre por separado en pequeñas tareas que forman un todo y si una de estas falla se hace un *rollback*
  - **C**onsistencia - Consistencia: Que los datos deben tener congruencia entre si. Con respecto a su perduracion relacional que es apoyada por sus llaves primerias y relaciones 
  - **I**solation - Aislamiento: Que los procesos en diferentes tablas se manejan de manera independiente con respecto a los usuarios o sesiones que puedan haber
  - **D**urability - Durabilidad: Tener la seguridad que la informacion no se va a perder porque postgres tiene un sistema de respaldo de informacion al momento en que se realizan las diferentes operaciones

## Archivos de configuracion de postgres

### postgresql.conf

Este archivo tiene toda la configuracion de postgres. Por ejemplo las direcciones ip que pueden acceder, el numero de sesiones maximas, etc.

### pg_hba.conf

Lista los roles y los tipos de acceso que tiene postgres. Consta de 5 columnas:
- Una que dice fuente de conecciones
- que acciones puede hacer 
- que usuario pueden conectarse
- la direccion desde donde se conectan 
- y el metodo de autentificacion 


### pg_ident.conf

Nos permite mapear usuarios. Esta mas orientado a los usuarios tipo LINUX como el root y sus permisos. 

## Patriciones

- Separacion fisica de datos: es posible guardar varias partes de la misma tabla en diferentes lugares del disco.
- Estructura logica: Permite hacer selects a informacion referenciada por bloques mas especificos que el traer toda la informacion


Como agregamos particiones:
Desde el pgadmin podemos hacer que una tabla se particione con respecto a un campo en especifico.

Ejemplo:

```sql
CREATE TABLE public.bitacora_viaje
(
    id serial,
    id_viaje integer,
    fecha date
) PARTITION BY RANGE (fecha)
WITH (
    OIDS = FALSE
)

--- creamos una particion de la siguiente manera
/**
CREATE TABLE nombre_tabla__sufijo_extra PARTITION OF nombre_tabla
FOR VALUES FROM ('fecha inicial') TO ('fecha final');
**/
CREATE TABLE bitacora_biaje201001 PARTITION OF bitacora_viaje
FOR VALUES FROM ('2019-01-01') TO ('2019-01-31');


--- luego insertamos como una tabla cualquiera
INSERT INTO public.bitacora_viaje
           (id_viaje, fecha       )
    VALUES (1,      , '2019-01-15')

--- pero ojo si hacemos un insert a una fecha de una paritcion que no existe nos saltara error
INSERT INTO public.bitacora_viaje
           (id_viaje, fecha       )
    VALUES (1,      , '2019-02-15')
```

OJO: en las tablas particionadas no existen las llaves primarias. El uso de tablas particionadas suelen ser para guardar informacion en masa como bitacoras que hagan referencia a tablas que si tienen la data que necesitamos


## Roles



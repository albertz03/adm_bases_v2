# Administración de base de datos V2 | Ejecicios y comandos.

## Comandos de operación del DBMS

## 1. Particionar
  > Las particiones nos permiten tener la  información de las tablas en diferentes ubicaciones, 
con la finalidad de realizar consulatas más rápidas y recuperar espacio en disco al borrar los registros.
<p>La sintaxis para crear particiones por lista es la siguiente:</p> 

```sql
CREATE TABLE nombretabla(definición_columna, tipo_datos)
PARTITION BY LIST(columna)
(
PARTITION part1 VALUES('valores_para_part1'),
PARTITION part2 VALUES('valores_para_part2'),
PARTITION part3 VALUES('valores_para_part3'),
PARTITION part4 VALUES('valores_para_part4'),
);
``` 
> **nota**
> 
> para saber si nuestro DBMS soporta particiones, se usa el comando:
> 
>  ```sql 
>  SHOW VARIABLES LIKE '%PARTITION%'; 
>  ```

> Más información en : [LIST Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-list.html)

<br> 

## 2. Respaldar (Backup)
  > Los DBMS permiten realizar backups o respaldos sin bloquear las tablas.
  > También se pueden realizar respaldos incrementales y más opciones de manera completa o parcial.
  
 <br>
 
  > El comando ``` mysqldump ``` realiza un respaldo lógico de la base de datos. La sintaxis para crear un respaldo de una base de datos, es la siguiente:
  > ```sql
  >  mysqldump -u [nombre_usuario] -p [nombre_basededatos]>[archivorespaldo]-$(date +%F).sql 
  >  ```
  
<br>

  > Si desea hacer un respaldo completo de un DBMS, la sintaxis es la sigueinte:
  > ```sql
  > mysqldump --all-databases --single-trasaction --quick --lock-tables=false > full-backup-$(date +%F).sql -u root -p
  > ```
  >> Para hacer uso de este comando, es necesario que la base de datos se encuentre en ejecución y accesible; además acceso de root al sistema con privilegios.
  
  > Más información en : [Using mysqldump for Backups](https://dev.mysql.com/doc/refman/5.7/en/using-mysqldump.html)


<br> 
  

## 3. Restaurar

  > Los DBMDS permiten realizar importaciones o restauraciones de bases de datos, 
  > tomando como origen un archivo de respaldo .sql que suele usar en migracion de datos.

el comando de restauración lleva la sigueitne sintaxis:
```sql
mysql -u [nombre_usuario] -p [nombre_basedatos] < [archivo_respaldo].sql
```
si se desea realizar restauración de un respaldo completo de DBMS, la sintaxis es:
```sql
mysql -u root -p < full-backup.sql
```
> Más información en : [Using mysqldump for Backups](https://dev.mysql.com/doc/refman/5.7/en/using-mysqldump.html)

<br> 

## 4. Administrar
> El comando de cliente para realizar operaciones administrativas es **mysqladmin**.
> Se emplea para everificar la configuración del servidor, estado actual, creación o eliminación de bases de datos.
>
> La sintaxis para invocar las distintas operaciones de **mysqladmin**, es:
> ```sql
> mysqladmin comando_operación
> ```

> ### operaciones de **mysqladmin**
> - **shutdown**    
>   Detiene el servidor
> - **refresh**    
>   Cerrar tablas y archivos de registro
> - **status**    
>   Breve mensaje del estado del servidor
> - **version**    
>   Información de la versión del servidor
> - **processlist**    
>   información de los preocesos que ejecuta el servidor
> - **password**    
>   Cambiar contraseña del servidor
>> El usar la opción de *password* puede ser inseguro, ya que en algunos sistemas, su contraseña se vuelve
>> visible para los programas que muestran el estado del sistema y otros usuarios pueden invocar la contraseña.

> Más información en : [mysqladmin - Client for Administering a MySQL Server](https://dev.mysql.com/doc/refman/8.0/en/mysqladmin.html)

<br> 


## 5. Recuperar
> El comando ```mysqlcheck``` se utiliza cuando alguna o varias de las tablas de una base de datos se encuentran corruptas. 
> Los síntomas de tablas corruptas incluyen consultas que son abortadas inesperadamente y los errores suelen ser:
> - ``` tbl_name-frm ``` Está bloqueado contra cambios.
> - No se puede encontrar el archivo ``` tbl_name.MYI (Errcode: nnn) ```
> - Inesperado final de archivo.
> - El archivo de registro se bloqueó.
> - Error obtenido nnn del manejador de tabla
1. Se realiza un chequeo de las tablas, con el comando:
```sql
mysqlcheck -u [nombre_usuario] -p [nombre_basedatos]
```
2. Se aplica el siguiente comando a las tablas que lanzaron errores, en el cual ``` -r ``` significa recoverymode:
```
mysqlcheck -u [nombre_usuario] -p [nombre_basedatos] -r
```
> Para podedr hacer uso de este comando, es necesario que el servidor MySQL se encuentre en ejecución, además de acceso de root al sistema con privilegios.

> Más información en : [mysqlcheck — A Table Maintenance Program](https://dev.mysql.com/doc/refman/8.0/en/mysqlcheck.html)

<br> 

## 6. Crear usuarios y privilegios
> ``` CREATE USER ``` es un comando para la creación de usuarios, se puede asignar el tipo de autenticación, contraseña y privilegios.
> 1. La sintaxis para crear un usuario con una contraseña es:
> ```sql
> CREATE USER nombre_usuario identified by ´contrasenia´;
> ```
> 2. Para otorgar privilegios a un usuario, se utiliza la siguiente sintaxis: 
> ```sql
> GRANT ALL PRIVILEGES ON nombre_basedatos TO nombre_usuario WITH GRANT OPTION;
> ```
> para hacer uso de este comando, es necesario tener privilegios de administrador.


> Más información en : [CREATE USER Statement](https://dev.mysql.com/doc/refman/8.0/en/create-user.html)

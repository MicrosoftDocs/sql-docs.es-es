---
title: Introducción a la seguridad de SQL Server en Linux
description: Vea las características de seguridad de SQL Server en Linux para obtener una idea de las áreas que requieren una investigación más detallada.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ecc72850-8b01-492e-9a27-ec817648f0e0
ms.openlocfilehash: f4201aa6c07d4ae96d44d8aa443b23e275e1f9f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346409"
---
# <a name="walkthrough-for-the-security-features-of-sql-server-on-linux"></a>Tutorial sobre las características de seguridad de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Si es usuario de Linux y no está familiarizado con SQL Server, las secciones siguientes le guiarán por algunas tareas de seguridad. No son exclusivas ni específicas de Linux, pero le ayudarán a hacerse una idea de las áreas en las que puede profundizar. En los ejemplos se proporciona un vínculo a documentación detallada de cada área.

> [!NOTE]
>  En los siguientes ejemplos, se usa la base de datos de ejemplo **AdventureWorks2014**. Para consultar las instrucciones sobre cómo obtener e instalar esta base de datos de ejemplo, vea [Restaurar una base de datos de SQL Server de Windows a Linux](sql-server-linux-migrate-restore-database.md).


## <a name="create-a-login-and-a-database-user"></a>Creación de un inicio de sesión y un usuario de base de datos 

Conceda acceso a SQL Server a otros usuarios mediante la creación de un inicio de sesión en la base de datos maestra con la instrucción [CREATE LOGIN](../t-sql/statements/create-login-transact-sql.md). Por ejemplo:

```
CREATE LOGIN Larry WITH PASSWORD = '************';  
```

> [!NOTE]
>  Use siempre una contraseña segura en lugar de los asteriscos del comando anterior.

Los inicios de sesión pueden conectarse a SQL Server y tener acceso (con permisos limitados) a la base de datos maestra. Para conectarse a una base de datos de usuario, un inicio de sesión necesita una identidad correspondiente en el nivel de base de datos, denominado usuario de base de datos. Los usuarios son específicos de cada base de datos y se deben crear por separado en cada base de datos para concederles acceso. En el ejemplo siguiente se usa la base de datos AdventureWorks2014 y, luego, se aplica la instrucción [CREATE USER](../t-sql/statements/create-user-transact-sql.md) para crear un usuario denominado Larry que está asociado con el inicio de sesión "Larry". Aunque el inicio de sesión y el usuario están relacionados (asignados entre sí), son objetos diferentes. El inicio de sesión es una entidad de seguridad de nivel de servidor. El usuario es una entidad de seguridad de nivel de base de datos.

```
USE AdventureWorks2014;
GO
CREATE USER Larry;
GO
```

- Una cuenta de administrador de SQL Server puede conectarse a cualquier base de datos y crear más inicios de sesión y usuarios.  
- Cuando alguien crea una base de datos, se convierte en su propietario, por lo que puede conectarse a esa base de datos. Los propietarios de bases de datos pueden crear más usuarios.

Posteriormente, se puede autorizar a otros inicios de sesión para que creen más inicios de sesión mediante la concesión del permiso `ALTER ANY LOGIN`. En una base de datos, se puede autorizar a otros usuarios para que creen más usuarios mediante la concesión del permiso `ALTER ANY USER`. Por ejemplo:   

```
GRANT ALTER ANY LOGIN TO Larry;   
GO   
   
USE AdventureWorks2014;   
GO   
GRANT ALTER ANY USER TO Jerry;    
GO   
```

Ahora, el inicio de sesión "Larry" puede crear más inicios de sesión y el usuario "Jerry" puede crear más usuarios.


## <a name="granting-access-with-least-privileges"></a>Concesión de acceso con privilegios mínimos

Las primeras personas que se conecten a una base de datos de usuario serán las cuentas de administrador y de propietario de la base de datos. Estos usuarios tienen todos los permisos disponibles en la base de datos, que son más de los que debería tener la mayoría de los usuarios. 

Si acaba de empezar, puede asignar algunas categorías generales de permisos mediante los *roles fijos de base de datos* integrados. Por ejemplo, el rol fijo de base de datos `db_datareader` puede leer todas las tablas de la base de datos, pero no realizar cambios. Para conceder la pertenencia a un rol fijo de base de datos, use la instrucción [ALTER ROLE](../t-sql/statements/alter-role-transact-sql.md). En el ejemplo siguiente se agrega el usuario `Jerry` al rol fijo de base de datos `db_datareader`.   
   
```   
USE AdventureWorks2014;   
GO   
   
ALTER ROLE db_datareader ADD MEMBER Jerry;   
```   

Para obtener una lista con los roles fijos de base de datos, vea [Roles de nivel de base de datos](../relational-databases/security/authentication-access/database-level-roles.md).

Más adelante, cuando esté listo para configurar un acceso más preciso a los datos (lo que se recomienda encarecidamente), puede crear sus propios roles de base de datos definidos por el usuario mediante la instrucción [CREATE ROLE](../t-sql/statements/create-role-transact-sql.md). Después, asigne permisos pormenorizados específicos a los roles personalizados.

Por ejemplo, las siguientes instrucciones crean un rol de base de datos denominado `Sales`, conceden al grupo `Sales` la capacidad de ver, actualizar y eliminar filas de la tabla `Orders` y, después, agregan el usuario `Jerry` al rol `Sales`.   
   
```   
CREATE ROLE Sales;   
GRANT SELECT ON Object::Sales TO Orders;   
GRANT UPDATE ON Object::Sales TO Orders;   
GRANT DELETE ON Object::Sales TO Orders;   
ALTER ROLE Sales ADD MEMBER Jerry;   
```

Para obtener más información sobre el sistema de permisos, vea [Introducción a los permisos del motor de base de datos](../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).


## <a name="configure-row-level-security"></a>Configuración de la seguridad de nivel de fila  

La [seguridad de nivel de fila](../relational-databases/security/row-level-security.md) permite restringir el acceso a las filas de una base de datos en función del usuario que ejecuta una consulta. Esta característica es útil cuando se quiere asegurar de que los clientes solo pueden acceder a sus propios datos o que los trabajadores solo pueden acceder a los datos relevantes para su departamento.   

Los pasos siguientes le guían a través de la configuración de dos usuarios con un acceso de nivel de fila diferente a la tabla `Sales.SalesOrderHeader`. 

Cree dos cuentas de usuario para probar la seguridad de nivel de fila:    
   
```   
USE AdventureWorks2014;   
GO   
   
CREATE USER Manager WITHOUT LOGIN;     
   
CREATE USER SalesPerson280 WITHOUT LOGIN;    
```

Conceda acceso de lectura en la tabla `Sales.SalesOrderHeader` a los dos usuarios:    
   
```   
GRANT SELECT ON Sales.SalesOrderHeader TO Manager;      
GRANT SELECT ON Sales.SalesOrderHeader TO SalesPerson280;
```
   
Cree un esquema y una función con valores de tabla insertada. La función devuelve 1 cuando una fila de la columna `SalesPersonID` coincide con el id. de un inicio de sesión `SalesPerson` o si el usuario que ejecuta la consulta es el usuario Administrador.   
   
```     
CREATE SCHEMA Security;   
GO   
   
CREATE FUNCTION Security.fn_securitypredicate(@SalesPersonID AS int)     
    RETURNS TABLE   
WITH SCHEMABINDING   
AS     
   RETURN SELECT 1 AS fn_securitypredicate_result    
WHERE ('SalesPerson' + CAST(@SalesPersonId as VARCHAR(16)) = USER_NAME())     
    OR (USER_NAME() = 'Manager');    
```

Para crear una directiva de seguridad, agregue la función como un filtro y un predicado de bloqueo en la tabla:  

```
CREATE SECURITY POLICY SalesFilter   
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader,   
ADD BLOCK PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader   
WITH (STATE = ON);   
```

Ejecute lo siguiente para consultar la tabla `SalesOrderHeader` como cada uno de los usuarios. Compruebe que `SalesPerson280` solo ve las 95 filas de sus propias ventas y que `Manager` puede ver todas las filas de la tabla.  

```    
EXECUTE AS USER = 'SalesPerson280';   
SELECT * FROM Sales.SalesOrderHeader;    
REVERT; 
 
EXECUTE AS USER = 'Manager';   
SELECT * FROM Sales.SalesOrderHeader;   
REVERT;   
```
 
Modifique la directiva de seguridad para deshabilitar la directiva.  Ahora los dos usuarios pueden acceder a todas las filas. 

```
ALTER SECURITY POLICY SalesFilter   
WITH (STATE = OFF);    
``` 


## <a name="enable-dynamic-data-masking"></a>Habilitación del enmascaramiento dinámico de datos

El [enmascaramiento dinámico de datos](../relational-databases/security/dynamic-data-masking.md) permite limitar la exposición de datos confidenciales a los usuarios de una aplicación a través del enmascaramiento total o parcial de algunas columnas. 

Use una instrucción `ALTER TABLE` para agregar una función de enmascaramiento a la columna `EmailAddress` de la tabla `Person.EmailAddress`: 
 
```
USE AdventureWorks2014;
GO
ALTER TABLE Person.EmailAddress    
ALTER COLUMN EmailAddress    
ADD MASKED WITH (FUNCTION = 'email()');
``` 
 
Cree un usuario `TestUser` con el permiso `SELECT` en la tabla y, después, ejecute una consulta como `TestUser` para ver los datos enmascarados:   

```  
CREATE USER TestUser WITHOUT LOGIN;   
GRANT SELECT ON Person.EmailAddress TO TestUser;    
 
EXECUTE AS USER = 'TestUser';   
SELECT EmailAddressID, EmailAddress FROM Person.EmailAddress;       
REVERT;    
```
 
Compruebe que la función de enmascaramiento cambia la dirección de correo electrónico del primer registro de:
  
|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |ken0@adventure-works.com |    
 
into 

|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |kXXX@XXXX.com |   


## <a name="enable-transparent-data-encryption"></a>Habilitación del cifrado de datos transparente

Una amenaza para la base de datos es el riesgo de que alguien robe los archivos de base de datos de la unidad de disco duro. Esto podría suceder con un intruso que obtenga acceso con privilegios elevados al sistema, a través de las acciones de un empleado problemático, o bien al robo del equipo que contiene los archivos (por ejemplo, un portátil).

El Cifrado de datos transparente (TDE) cifra los archivos de datos a medida que se almacenan en el disco duro. La base de datos maestra del motor de base de datos de SQL Server tiene la clave de cifrado, por lo que el motor de base de datos puede manipular los datos. Los archivos de base de datos no se pueden leer sin acceder a la clave. Los administradores generales pueden administrar, hacer copias de seguridad y volver a crear la clave, por lo que la base de datos se puede cambiar, pero solo lo pueden hacer individuos concretos. Cuando se configura TDE, la base de datos `tempdb` también se cifra de forma automática. 

Como el motor de base de datos puede leer los datos, el cifrado de datos transparente no protege contra el acceso no autorizado por parte de los administradores del equipo que pueden leer la memoria de forma directa o acceder a SQL Server a través de una cuenta de administrador.

### <a name="configure-tde"></a>Configuración de TDE

- Cree una clave maestra
- Cree u obtenga un certificado protegido por la clave maestra
- Cree una clave de cifrado de base de datos y protéjala con el certificado
- Configure la base de datos para que utilice el cifrado

Para configurar TDE se necesita el permiso `CONTROL` en la base de datos maestra y el permiso `CONTROL` en la base de datos de usuario. Normalmente, un administrador es el que configura TDE. 

En el ejemplo siguiente se muestra cómo cifrar y descifrar la base de datos `AdventureWorks2014` mediante un certificado instalado en el servidor denominado `MyServerCert`.


```
USE master;  
GO  

CREATE MASTER KEY ENCRYPTION BY PASSWORD = '**********';  
GO  

CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My Database Encryption Key Certificate';  
GO  

USE AdventureWorks2014;  
GO
  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO
  
ALTER DATABASE AdventureWorks2014  
SET ENCRYPTION ON;   
```

Para quitar TDE, ejecute `ALTER DATABASE AdventureWorks2014 SET ENCRYPTION OFF;`   

SQL Server programa las operaciones de cifrado y descifrado en subprocesos que se ejecutan en segundo plano. Puede ver el estado de estas operaciones mediante las vistas de catálogo y las vistas de administración dinámica de la lista que se muestra más adelante en este tema.   

> [!WARNING]
>  Los archivos de copia de seguridad de las bases de datos que tienen habilitado TDE también se cifran mediante la clave de cifrado de la base de datos. Como consecuencia, al restaurar estas copias de seguridad debe estar disponible el certificado que protege la clave de cifrado de la base de datos. Esto significa que, además de hacer copias de seguridad de la base de datos, tiene que asegurarse de que mantiene copias de seguridad de los certificados del servidor para evitar la pérdida de datos. Si el certificado deja de estar disponible, perderá los datos. Para obtener más información, consulte [SQL Server Certificates and Asymmetric Keys](../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  

Para obtener más información sobre TDE, vea [Cifrado de datos transparente (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md).   


## <a name="configure-backup-encryption"></a>Configuración del cifrado de copia de seguridad
SQL Server tiene la capacidad de cifrar los datos mientras se crea una copia de seguridad. Al especificar el algoritmo y el sistema de cifrado (un certificado o una clave asimétrica) al crear una copia de seguridad, puede crear un archivo de copia de seguridad cifrado.    
  
> [!WARNING]
> Es muy importante realizar una copia de seguridad del certificado o la clave asimétrica, y preferiblemente en una ubicación diferente de la que se usó para cifrar el archivo de copia de seguridad. Sin el certificado o la clave asimétrica, no puede restaurar la copia de seguridad, lo que deja inutilizable el archivo de copia de seguridad. 
 
 
En el ejemplo siguiente se crea un certificado y después una copia de seguridad protegida por el certificado.

```
USE master;  
GO  
CREATE CERTIFICATE BackupEncryptCert   
   WITH SUBJECT = 'Database backups';  
GO 
BACKUP DATABASE [AdventureWorks2014]  
TO DISK = N'/var/opt/mssql/backups/AdventureWorks2014.bak'  
WITH  
  COMPRESSION,  
  ENCRYPTION   
   (  
   ALGORITHM = AES_256,  
   SERVER CERTIFICATE = BackupEncryptCert  
   ),  
  STATS = 10  
GO  
```

Para obtener más información, vea [Cifrado de copia de seguridad](../relational-databases/backup-restore/backup-encryption.md).


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de SQL Server, vea [Centro de seguridad para el motor de base de datos de SQL Server y Azure SQL Database](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).
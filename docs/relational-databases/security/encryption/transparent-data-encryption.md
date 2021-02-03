---
title: Cifrado de datos transparente (TDE) | Microsoft Docs
description: Obtenga información sobre el Cifrado de datos transparente, que cifra los archivos de datos de SQL Server, Azure SQL Database y Azure Synapse Analytics, lo que se conoce como cifrado de datos en reposo.
ms.custom: ''
ms.date: 05/09/2019
ms.prod: sql
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption
- database encryption key, about
- TDE
- database encryption key
- TDE, about
- Transparent Data Encryption, about
- encryption [SQL Server], transparent data encryption
ms.assetid: c75d0d4b-4008-4e71-9a9d-cee2a566bd3b
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adc1affe33d1172b3e648c510b2dbc1d00326849
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237982"
---
# <a name="transparent-data-encryption-tde"></a>Cifrado de datos transparente (TDE)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

El *Cifrado de datos transparente* (TDE) cifra los archivos de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDWfull](../../../includes/sssdwfull-md.md)], lo que se conoce como cifrado de datos en reposo.

Para ayudar a proteger una base de datos, se pueden tomar precauciones como las siguientes:

* Diseñar un sistema seguro
* Cifrar los recursos confidenciales
* Crear un firewall en torno a los servidores de bases de datos

Con todo, alguien malintencionado que roba medios físicos como unidades o cintas de copia de seguridad puede restaurar la base de datos o conectarse a ella y examinar sus datos.

Una solución sería cifrar los datos confidenciales en la base de datos y usar un certificado para proteger las claves con las que esos datos se cifran. Esta solución impide que alguien que carezca de las claves use los datos. Este tipo de protección se debe planear de antemano.

TDE realiza el cifrado y descifrado de E/S en tiempo real de los archivos de datos y de registro. Este cifrado usa una clave de cifrado de la base de datos (DEK). El registro de arranque de la base de datos almacena la clave para que esté disponible durante la recuperación. La DEK es una clave simétrica. Está protegida por un certificado que la base de datos maestra del servidor almacena, o por una clave asimétrica que un módulo EKM protege.

TDE protege los datos en reposo, que son los archivos de datos y de registro. Permite cumplir muchas leyes, normativas y directrices establecidas en diversos sectores. Esto permite a los desarrolladores de software cifrar datos con algoritmos de cifrado AES y 3DES sin cambiar las aplicaciones existentes.

> [!IMPORTANT]
> TDE no proporciona cifrado para los canales de comunicaciones. Para obtener más información sobre cómo cifrar datos en los canales de comunicación, vea [Habilitar conexiones cifradas en el motor de base de datos (Administrador de configuración de SQL Server)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).
>
>**Temas relacionados:**
>
> - [Cifrado de datos transparente con Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview)
> - [Introducción al cifrado de datos transparente (TDE) en Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)
> - [Mover una base de datos protegida por TDE a otra instancia de SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)
> - [Habilitar TDE en SQL Server con EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)
> - [Usar el conector de SQL Server con características de cifrado de SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)
> - [Blog de seguridad de SQL Server en TDE con las preguntas más frecuentes](/archive/blogs/sqlsecurity/feature-spotlight-transparent-data-encryption-tde)

## <a name="about-tde"></a>Acerca de TDE

El cifrado de un archivo de base de datos se realiza en el nivel de página. Las páginas de una base de datos cifrada se cifran antes de escribirse en el disco y se descifran cuando se leen en la memoria. TDE no aumenta el tamaño de la base de datos cifrada.

### <a name="information-applicable-to-sssds"></a>Información aplicable a [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]

Cuando TDE se usa con [!INCLUDE[sqldbesa](../../../includes/sqldbesa-md.md)] V12, [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] crea automáticamente el certificado de nivel de servidor almacenado en la base de datos maestra. Para mover una base de datos de TDE en [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], no es necesario descifrarla para la operación de traslado. Para obtener más información sobre cómo usar TDE con [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], vea [Cifrado de datos transparente para Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

### <a name="information-applicable-to-ssnoversion"></a>Información aplicable a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

Después de proteger una base de datos, puede restaurarla con el certificado correcto que corresponda. Para obtener más información acerca de los certificados, vea [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

Después de habilitar TDE, haga inmediatamente una copia de seguridad del certificado y su clave privada asociada. Si el certificado dejara de estar disponible, o si la base de datos restaura o conecta en otro servidor, necesitará copias de seguridad del certificado y de la clave privada o, de lo contrario, no podrá abrir la base de datos.

Conserve el certificado de cifrado aunque TDE esté deshabilitado en la base de datos. Si bien la base de datos no está cifrada, puede que algunas partes del registro de transacciones sigan protegidas. También puede necesitar el certificado en algunas operaciones hasta que haga una copia de seguridad completa de la base de datos.

Un certificado que ha excedido su fecha de expiración se puede seguir usando para cifrar y descifrar datos con TDE.

### <a name="encryption-hierarchy"></a>Jerarquía de cifrado

En la siguiente ilustración se muestra la arquitectura del cifrado TDE. Solo los elementos de nivel de base de datos (las partes de ALTER DATABASE y la clave de cifrado de base de datos) son configurables por el usuario cuando se usa TDE en [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]).

![Arquitectura del cifrado de base de datos transparente](../../../relational-databases/security/encryption/media/tde-architecture.png)

## <a name="enable-tde"></a>Habilitación de TDE

Para usar TDE, siga estos pasos.

**Se aplica a**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

1. Cree una clave maestra.

1. Cree u obtenga un certificado protegido por la clave maestra.

1. Cree una clave de cifrado de base de datos y protéjala con el certificado.

1. Configure la base de datos para que use el cifrado.

En el siguiente ejemplo se muestra cómo cifrar y descifrar la base de datos `AdventureWorks2012` con un certificado denominado `MyServerCert` instalado en el servidor.

```sql
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';
go
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';
go
USE AdventureWorks2012;
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_128
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;
GO
ALTER DATABASE AdventureWorks2012
SET ENCRYPTION ON;
GO
```

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]programa las operaciones de cifrado y descifrado en subprocesos que se ejecutan en segundo plano. Para ver el estado de estas operaciones, use las vistas de catálogo y las vistas de administración dinámica de la tabla que se muestra más adelante en este artículo.

> [!CAUTION]
> Los archivos de copia de seguridad de las bases de datos que tienen TDE habilitado también se cifran con la clave de cifrado de la base de datos. En consecuencia, cuando se restauren estas copias de seguridad, el certificado que protege la clave de cifrado de la base de datos deberá estar disponible. Por lo tanto, además de hacer copias de seguridad de la base de datos, asegúrese de conservar también copias de seguridad de los certificados de servidor. Si los certificados dejan de estar disponible, perderá los datos.
>
> Para obtener más información, consulte [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="commands-and-functions"></a>Comandos y funciones

Para que en las siguientes instrucciones se acepten certificados TDE, use una clave maestra de base de datos para cifrarlos. Si solamente se cifran con una contraseña, las instrucciones los rechazarán como sistemas de cifrado.

> [!IMPORTANT]
> Si protege los certificados con contraseña después de que TDE los use, la base de datos dejará de estar accesible cuando se reinicie.

En la siguiente tabla encontrará vínculos y explicaciones de los comandos y funciones de TDE:

|Comando o función|Propósito|
|-------------------------|-------------|
|[CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)|Crea una clave que cifra una base de datos.| 
|[ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-database-encryption-key-transact-sql.md)|Cambia la clave que cifra una base de datos.|
|[DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md)|Quita la clave que cifra una base de datos.|
|[Opciones de ALTER DATABASE SET (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql-set-options.md)|Explica la opción **ALTER DATABASE** que se usa para habilitar TDE.|

## <a name="catalog-views-and-dynamic-management-views"></a>Vistas de catálogo y vistas de administración dinámica

 En la tabla siguiente se muestran las vistas de catálogo y las vistas de administración dinámica de TDE.

|Vista de catálogo o vista de administración dinámica|Propósito|
|---------------------------------------------|-------------|
|[sys.databases (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Vista de catálogo que muestra información sobre las bases de datos|
|[sys.certificates (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|Vista de catálogo que muestra los certificados de una base de datos|
|[sys.dm_database_encryption_keys (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)|Vista de administración dinámica que proporciona información sobre las claves de cifrado y el estado de cifrado de una base de datos|

## <a name="permissions"></a>Permisos

Cada una de las características y comandos de TDE tiene sus propios requisitos de permisos, como se describe en las tablas mostradas anteriormente.

Para ver los metadatos relacionados con TDE, se requiere el permiso VIEW DEFINITION en un certificado.

## <a name="considerations"></a>Consideraciones

Mientras se realiza el examen del proceso de nuevo cifrado para una operación de cifrado de base de datos, las operaciones de mantenimiento de la base de datos están deshabilitadas. Puede configurar la base de datos en modo de usuario único si quiere realizar la operación de mantenimiento. Para obtener más información, vea [Establecer una base de datos en modo de usuario único](../../../relational-databases/databases/set-a-database-to-single-user-mode.md).

Para encontrar el estado del cifrado de una base de datos, use la vista de administración dinámica sys.dm_database_encryption_keys. Para obtener más información, vea la sección "Vistas de catálogo y vistas de administración dinámica", anteriormente en este artículo.

En TDE, se cifran todos los archivos y grupos de archivos de una base de datos. Si algún grupo de archivos de una base de datos está marcado como READ ONLY, se producirá un error en la operación de cifrado de la base de datos.

Si una base de datos se usa en una creación de reflejo de la base de datos o en un trasvase de registros, se cifran ambas bases de datos. Las transacciones del registro se cifran cuando se envíen entre ellas.

> [!IMPORTANT]
> Los índices de texto completo se cifran cuando una base de datos esté configurada para cifrarse. Estos índices creados en una versión de SQL Server anterior a SQL Server 2008 se importan a la base de datos con SQL Server 2008 o posterior y se cifran con TDE.

> [!TIP]
> Para supervisar los cambios en el estado TDE de una base de datos, use SQL Server Audit o SQL Database Auditing. En SQL Server, el seguimiento de TDE se realiza en el grupo de acción de auditoría DATABASE_CHANGE_GROUP, que está en [Grupos de acciones y acciones de SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).

## <a name="restrictions"></a>Restricciones

Durante el cifrado inicial de la base de datos, un cambio clave o un descifrado de una base de datos no se pueden realizar las siguientes operaciones:

- Quitar un archivo de un grupo de archivos de una base de datos

- Quitar una base de datos

- Dejar sin conexión una base de datos

- Separar la base de datos.

- Pasar la base de datos o el grupo de archivos al estado READ ONLY.

Durante la ejecución de las instrucciones CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY y ALTER DATABASE...SET ENCRYPTION no se pueden realizar las siguientes operaciones:

- Quitar un archivo de un grupo de archivos de una base de datos

- Quitar una base de datos

- Dejar sin conexión una base de datos

- Separar la base de datos.

- Pasar la base de datos o el grupo de archivos al estado READ ONLY.

- Usar un comando ALTER DATABASE

- Iniciar una copia de seguridad de base de datos o de archivos de base de datos

- Iniciar una restauración de base de datos o de archivos de base de datos

- Crear una instantánea

Las siguientes operaciones o condiciones impedirán la ejecución de las instrucciones CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY y ALTER DATABASE...SET ENCRYPTION:

- La base de datos o alguno de sus grupos de archivos son de solo lectura.

- Se está ejecutando un comando ALTER DATABASE.

- Se está haciendo una copia de seguridad de los datos.

- Una base de datos está sin conexión o en restauración.

- Se está realizando una instantánea.

- Se están realizando tareas de mantenimiento en la base de datos.

Al crear archivos de base de datos, la inicialización instantánea de archivos no estará disponible si TDE está habilitado.

Para cifrar una clave de cifrado de la base de datos con una clave asimétrica, la clave asimétrica debe estar en un proveedor extensible de administración de claves.

## <a name="tde-scan"></a>Examen de TDE

Para habilitar TDE en una base de datos, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe realizar un examen de cifrado. Este examen lee cada página de los archivos de datos en el grupo de búferes y, tras ello, vuelve a escribir las páginas cifradas en el disco.

Para proporcionarle más control sobre el examen de cifrado, [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] incluye el examen de TDE, que tiene sintaxis de suspensión y reanudación. Así, el examen se puede pausar cuando la carga de trabajo del sistema sea muy intensa o durante las horas críticas del negocio y reanudarlo más adelante.

Use la siguiente sintaxis para poner en pausa el análisis de cifrado TDE:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION SUSPEND;
```

De igual modo, use la siguiente sintaxis para reanudarlo:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION RESUME;
```

La columna encryption_scan_state se ha agregado a la vista de administración dinámica sys.dm_database_encryption_keys. En ella se muestra el estado actual del examen de cifrado. También hay una columna nueva llamada encryption_scan_modify_date que contiene la fecha y la hora del último cambio de estado del examen de cifrado.

Si la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se reinicia mientras su examen de cifrado está en pausa, se registrará un mensaje en el registro de errores al iniciarse. El mensaje indica que un examen existente está en pausa.

## <a name="tde-and-transaction-logs"></a>TDE y los registros de transacciones

Al permitir que una base de datos use TDE, se quita la parte restante del registro de transacciones virtual actual. Esta eliminación fuerza la creación del siguiente registro de transacciones. Este comportamiento procura que no quede ningún texto sin cifrar en los registros una vez configurada la base de datos para el cifrado.

Para ver el estado del cifrado del archivo de registro, consulte la columna `encryption_state` de la vista `sys.dm_database_encryption_keys`, como en este ejemplo:

```sql
USE AdventureWorks2012;
GO
/* The value 3 represents an encrypted state
   on the database and transaction logs. */
SELECT *
FROM sys.dm_database_encryption_keys
WHERE encryption_state = 3;
GO
```

Para obtener más información sobre la arquitectura de los archivos de registro de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vea [El registro de transacciones (SQL Server)](../../../relational-databases/logs/the-transaction-log-sql-server.md).

Antes de que una clave de cifrado de base de datos cambie, la clave de cifrado de base de datos anterior cifrará todos los datos escritos en el registro de transacciones.

Si cambia una clave de cifrado de base de datos dos veces, debe hacer una copia de seguridad de los registros para poder volver a cambiarla.

## <a name="tde-and-the-tempdb-system-database"></a>TDE y la base de datos del sistema tempdb

La base de datos del sistema **tempdb** se cifra si alguna otra base de datos de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] está cifrada con TDE. Este cifrado podría tener un efecto en el rendimiento de las bases de datos no cifradas de la misma instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para obtener más información sobre la base de datos del sistema **tempdb**, vea [Base de datos tempdb](../../../relational-databases/databases/tempdb-database.md).

## <a name="tde-and-replication"></a>TDE y la replicación

La replicación no replica automáticamente los datos de una base de datos habilitada para TDE en un formato cifrado. Habilite TDE por separado si quiere proteger las bases de datos de suscriptor y de distribución.

La replicación de instantáneas puede almacenar datos en archivos intermedios sin cifrar, como archivos BCP, como también lo hace la distribución de datos inicial para la replicación transaccional y de mezcla. Durante dicha replicación, se puede habilitar el cifrado para proteger el canal de comunicación.

Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos (Administrador de configuración de SQL Server)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="tde-and-always-on"></a>TDE y Always On    
Puede [agregar una base de datos cifrada a un Grupo de disponibilidad AlwaysOn](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md).  

Para cifrar las bases de datos que forman parte de un grupo de disponibilidad, cree la clave maestra y los certificados, o bien la clave asimétrica (EKM) en todas las réplicas secundarias antes de crear la [clave de cifrado de base de datos](../../../t-sql/statements/create-database-encryption-key-transact-sql.md) en la réplica principal.  

Si se usa un certificado para proteger la clave de cifrado de base de datos (DEK), [realice una copia de seguridad del certificado](../../../t-sql/statements/backup-certificate-transact-sql.md) en la réplica principal y, después, [cree el certificado desde un archivo](../../../t-sql/statements/create-certificate-transact-sql.md) en todas las réplicas secundarias antes de crear la clave de cifrado de base de datos en la réplica principal. 

## <a name="tde-and-filestream-data"></a>TDE y los datos FILESTREAM

Los datos **FILESTREAM** no se cifran ni siquiera cuando TDE se habilita.

<a name="scan-suspend-resume"></a>

## <a name="remove-tde"></a>Eliminación de TDE

Use la instrucción `ALTER DATABASE` para quitar el cifrado de la base de datos.

```sql
ALTER DATABASE <db_name> SET ENCRYPTION OFF;
```

Para ver el estado de la base de datos, use la vista de administración dinámica [sys.dm_database_encryption_keys](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

Espere a que finalice el descifrado antes de quitar la clave de cifrado de la base de datos mediante [DROP DATABASE ENCRYPTION KEY](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md).

> [!IMPORTANT]
> Realice una copia de seguridad de la clave maestra y el certificado que se usan para TDE en una ubicación segura. La clave maestra y el certificado son necesarios para restaurar las copias de seguridad que se han realizado al cifrar la base de datos con TDE. Después de quitar la clave de cifrado de base de datos, realice una copia de seguridad de registros seguida de una nueva copia de seguridad completa de la base de datos descifrada. 

## <a name="tde-and-buffer-pool-extension"></a>TDE y la extensión del grupo de búferes

Cuando una base de datos se cifra con TDE, los archivos relacionados con la extensión del grupo de búferes (BPE) no se cifran. Con esos archivos, use herramientas de cifrado como BitLocker o EFS en el nivel del sistema de archivos.

## <a name="tde-and-in-memory-oltp"></a>TDE y OLTP en memoria

TDE se puede habilitar en una base de datos que tenga objetos de OLTP en memoria. En [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] y [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], los datos y registros de OLTP en memoria se cifran si TDE está habilitado. En [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)], los registros de OLTP en memoria se cifran si TDE está habilitado, pero no los archivos del grupo de archivos MEMORY_OPTIMIZED_DATA.

## <a name="related-tasks"></a>Tareas relacionadas

[Mover una base de datos protegida por TDE a otra instancia de SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)  
[Habilitar TDE en SQL Server con EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
[Administración extensible de claves con Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  

## <a name="related-content"></a>Contenido relacionado

[Cifrado de datos transparente con Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
[Introducción al cifrado de datos transparente (TDE) en Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)  
[Cifrado de SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)  
[SQL Server y claves de cifrado de base de datos (motor de base de datos)](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  

## <a name="see-also"></a>Consulte también

[Centro de seguridad para el Motor de base de datos de SQL Server y Azure SQL Database](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[FILESTREAM (SQL Server)](../../../relational-databases/blob/filestream-sql-server.md)
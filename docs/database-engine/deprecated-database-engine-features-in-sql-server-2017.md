---
title: Características en desuso del motor de base de datos de SQL Server 2017 | Microsoft Docs
titleSuffix: SQL Server 2019
description: Obtenga información sobre las características en desuso del motor de base de datos que siguen disponibles en SQL Server 2017 (14.x), pero que no se deben usar en las aplicaciones nuevas.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server]
- Database Engine [SQL Server], backward compatibility
- deprecation [SQL Server], feature list
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: e14f354812c99b9ed3e8be86734e3cb8e07db751
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489479"
---
# <a name="deprecated-database-engine-features-in-sql-server-2017"></a>Características en desuso del motor de base de datos de SQL Server 2017

[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]

  Este tema describe las características desusadas de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] que siguen estando disponibles en [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. Las características en desuso no se deben usar en nuevas aplicaciones.  
  
Cuando se establece que una característica está en desuso, significa que:

- Solo está en modo de mantenimiento. No se realizarán cambios nuevos, ni tampoco cambios relacionados con la interoperabilidad con características nuevas.
- Nos esforzamos por no quitar una característica en desuso en las versiones futuras para facilitar las actualizaciones, aunque en raras ocasiones puede que optemos por quitar permanentemente la característica de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si limita las innovaciones futuras.
- Para un nuevo trabajo de desarrollo, no se recomienda el uso de las características en desuso.      

Puede supervisar el uso de características desusadas utilizando el contador de rendimiento del objeto de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Características desusadas y eventos de seguimiento. Para obtener más información, vea [Usar objetos de SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

Los valores de estos contadores también están disponibles si se ejecuta la siguiente instrucción:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

> [!NOTE]
> Esta lista es idéntica a la lista de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]. No hay anunciada ninguna nueva característica de motor de base de datos en desuso o descontinuada de [!INCLUDE[sssql17-md](../includes/sssql17-md.md)].

## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>Características en desuso en la próxima versión de SQL Server

Las siguientes características del Motor de base de datos de SQL Server estarán en desuso en la próxima versión de SQL Server. No utilice estas características en nuevos trabajos de desarrollo y modifique lo antes posible las aplicaciones que las utilizan actualmente. El valor **Nombre de la característica** aparece en los eventos de seguimiento como ObjectName, así como en los contadores de rendimiento y `sys.dm_os_performance_counters` como el nombre de instancia. El valor de **Id. de la característica** aparece en los eventos de seguimiento como el identificador de objeto (ObjectId).

### <a name="back-up-and-restore"></a>Copia de seguridad y restauración

| Característica desusada | Sustituta | Nombre de característica | Id. de la característica |
|--------------------|-------------|--------------|------------|
| RESTORE { DATABASE &#124; LOG } WITH [MEDIA]PASSWORD sigue en desuso.<br /><br />BACKUP { DATABASE &#124; LOG } WITH PASSWORD y BACKUP { DATABASE &#124; LOG } WITH MEDIAPASSWORD han dejado de proporcionarse. | Ninguno. | BACKUP DATABASE o LOG WITH PASSWORD<br /><br />BACKUP DATABASE or LOG WITH MEDIAPASSWORD | 104<br /><br /> 103 |

### <a name="compatibility-levels"></a>Niveles de compatibilidad

| Característica desusada | Sustituta | Nombre de característica | Id. de la característica |
|--------------------|-------------|--------------|------------|
Actualización desde la versión 100 (SQL Server 2008 y SQL Server 2008 R2). | Cuando una versión de SQL Server se queda sin [soporte técnico](/lifecycle/products/?products=sql-server), el nivel de compatibilidad de base de datos asociado se marca como en desuso. Pero se sigue dando soporte a las aplicaciones certificadas en cualquier nivel de compatibilidad de base de datos admitido, siempre que sea posible, para facilitar las actualizaciones. Para obtener más información sobre los niveles de compatibilidad, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Nivel de compatibilidad de la base de datos 100 | 108 |

### <a name="database-objects"></a>Objetos de base de datos

| Característica desusada | Sustituta | Nombre de característica | Id. de la característica |
|--------------------|-------------|--------------|------------|
| Capacidad de devolver conjuntos de resultados de los desencadenadores | None | Devolver resultados del desencadenador | 12 |

### <a name="encryption"></a>Cifrado

| Característica desusada | Sustituta | Nombre de característica | Id. de la característica |
|--------------------|-------------|--------------|------------|
| El cifrado mediante RC4 o RC4_128 está en desuso y se quitará en la próxima versión. El descifrado con RC4 y RC4_128 no está en desuso. | Utilice otro algoritmo de cifrado como AES. | Algoritmo de cifrado desusado | 253 |
| El uso de MD2, MD4, MD5, SHA y SHA1 está obsoleto. | Use SHA2_256 o SHA2_512 en su lugar. Los algoritmos antiguos siguen funcionando, pero generan un evento de desuso. |Algoritmo hash en desuso | None |

### <a name="remote-servers"></a>Servidores remotos

| Característica desusada | Sustituta | Nombre de característica | Id. de la característica |
|--------------------|-------------|--------------|------------|
| sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br /> sp_remoteoption|Reemplace los servidores remotos con servidores vinculados. sp_addserver solo se puede usar con la opción local. | sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br > sp_remoteoption | 70 <br /><br /> 69 <br /><br /> 71 <br /><br /> 72 <br /><br /> 73 |
| \@\@remserver | Reemplace los servidores remotos con servidores vinculados. | None | None |
| SET REMOTE_PROC_TRANSACTIONS|Reemplace los servidores remotos con servidores vinculados. | SET REMOTE_PROC_TRANSACTIONS | 110 |

### <a name="transact-sql"></a>Transact-SQL

| Característica desusada | Sustituta | Nombre de característica | Id. de la característica |
|--------------------|-------------|--------------|------------|
| **SET ROWCOUNT** para las instrucciones de **INSERT**, **UPDATE** y **DELETE** | Palabra clave TOP | SET ROWCOUNT | 109 |
| Sugerencia de tabla HOLDLOCK sin paréntesis. | Usar HOLDLOCK con paréntesis. | Sugerencia de tabla HOLDLOCK sin paréntesis | 167 |

## <a name="features-deprecated-in-a-future-version-of-sql-server"></a>Características en desuso en una versión futura de SQL Server

Las siguientes características del Motor de base de datos de SQL Server se admitirán en la próxima versión de SQL Server. No se ha determinado la versión específica de SQL Server.

### <a name="back-up-and-restore"></a>Copia de seguridad y restauración

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| BACKUP { DATABASE &#124; LOG } TO TAPE <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_tape*|BACKUP { DATABASE &#124; LOG } TO DISK <br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_disk* | BACKUP DATABASE or LOG TO TAPE |
| sp_addumpdevice "**tape**" | sp_addumpdevice "**disk**" | ADDING TAPE DEVICE |
| sp_helpdevice | sys.backup_devices | sp_helpdevice |

### <a name="compatibility-levels"></a>Niveles de compatibilidad

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_dbcmptlevel|ALTER DATABASE ... SET COMPATIBILITY_LEVEL. Para obtener más información, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | sp_dbcmptlevel |
| Nivel de compatibilidad de la base de datos 110 Y 120. | Planee actualizar la base de datos y la aplicación en una versión futura. Pero se sigue dando soporte a las aplicaciones certificadas en cualquier nivel de compatibilidad de base de datos admitido, siempre que sea posible, para facilitar las actualizaciones. Para obtener más información sobre los niveles de compatibilidad, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Nivel de compatibilidad de la base de datos 110 <br /><br /> Nivel de compatibilidad de la base de datos 120 |

### <a name="collations"></a>Intercalaciones

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS | Ninguno. Estas intercalaciones existen en SQL Server 2005 (9.x), pero no son visibles a través de fn_helpcollations. | Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS |
| Hindi <br /><br /> Macedonio | Estas intercalaciones existen en SQL Server 2005 (9.x) y versiones posteriores, pero no son visibles a través de fn_helpcollations. Utilice en su lugar Macedonian_FYROM_90 e Indic_General_90.|Hindi <br /><br /> Macedonio |
| Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 | Azeri_Latin_100 <br /><br /> Azeri_Cyrilllic_100 | Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 |

### <a name="data-types"></a>Tipos de datos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_addtype <br /><br /> sp_droptype|CREATE TYPE<br /><br /> DROP TYPE | sp_addtype<br /><br /> sp_droptype |
| Sintaxis de **timestamp** para el tipo de datos **rowversion** | Sintaxis del tipo de datos **rowversion** | timestamp |
| Capacidad de insertar valores NULL en columnas **timestamp** . | Utilice DEFAULT en su lugar. | INSERT NULL en columnas TIMESTAMP |
| Opción de tabla 'text in row'|Use los tipos de datos **varchar(max)** , **nvarchar(max)** y **varbinary(max)** . Para obtener más información, vea [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|Opción de tabla Text in row |
| Tipos de datos:<br /><br /> **text**<br /><br /> **ntext**<br /><br /> **image**|Use los tipos de datos **varchar(max)** , **nvarchar(max)** y **varbinary(max)** .|Tipos de datos: **text**, **ntext** o **image** |

### <a name="database-management"></a>Administración de bases de datos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_attach_db <br /><br /> sp_attach_single_file_db|Instrucción CREATE DATABASE con la opción FOR ATTACH. Si desea volver a generar varios archivos de registro y uno o más tienen una ubicación nueva, utilice la opción FOR ATTACH_REBUILD_LOG. | sp_attach_db <br /><br /> sp_attach_single_file_db |
| sp_certify_removable<br /><br /> sp_create_removable|sp_detach_db|sp_certify_removable<br /><br /> sp_create_removable |
| sp_dbremove | DROP DATABASE | sp_dbremove |
| sp_renamedb | MODIFY NAME en ALTER DATABASE | sp_renamedb |

### <a name="database-objects"></a>Objetos de base de datos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| CREATE DEFAULT<br /><br /> DROP DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|Palabra clave DEFAULT en CREATE TABLE y ALTER TABLE.|CREATE_DROP_DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault |
| CREATE RULE<br /><br /> DROP RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule | Palabra clave CHECK en CREATE TABLE y ALTER TABLE. | CREATE_DROP_RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule |
| sp_change_users_login | Utilice ALTER USER. | sp_change_users_login |
| sp_depends | sys.dm_sql_referencing_entities and sys.dm_sql_referenced_entities | sp_depends |
| sp_getbindtoken | Use MARS o transacciones distribuidas. | sp_getbindtoken |

### <a name="database-options"></a>Opciones de base de datos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_bindsession | Use MARS o transacciones distribuidas. | sp_bindsession |
| sp_resetstatus | ALTER DATABASE SET { ONLINE &#124; EMERGENCY } | sp_resetstatus
| Opción TORN_PAGE_DETECTION de ALTER DATABASE|Opción PAGE_VERIFY TORN_PAGE_DETECTION de ALTER DATABASE | ALTER DATABASE WITH TORN_PAGE_DETECTION |

### <a name="dbcc"></a>DBCC

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| DBCC DBREINDEX|Opción REBUILD de ALTER INDEX. | DBCC DBREINDEX |
| DBCC INDEXDEFRAG|Opción REORGANIZE de ALTER INDEX | DBCC INDEXDEFRAG |
| DBCC SHOWCONTIG|sys.dm_db_index_physical_stats | DBCC SHOWCONTIG |
| DBCC PINTABLE<br /><br /> DBCC UNPINTABLE | No surte ningún efecto. | DBCC [UN]PINTABLE |

### <a name="extended-properties"></a>Propiedades extendidas

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Level0type = 'type' y Level0type = 'USER' agregará propiedades extendidas a objetos de tipo de nivel 1 y nivel 2. | Use Level0type = 'USER' solo para agregar una propiedad extendida directamente a un usuario o un rol.<br /><br /> Use Level0type = 'SCHEMA' para agregar una propiedad extendida a los tipos de nivel 1, como TABLE o VIEW, o a los tipos de nivel 2, como COLUMN o TRIGGER. Para obtener más información, vea [sp_addextendedproperty &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md). | EXTPROP_LEVEL0TYPE<br /><br /> EXTPROP_LEVEL0USER |

### <a name="extended-stored-procedures"></a>Procedimientos almacenados extendidos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginconfig|Use CREATE LOGIN<br /><br /> Use el argumento DROP LOGIN IsIntegratedSecurityOnly de SERVERPROPERTY|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginconfig |

### <a name="extended-stored-procedures-programming"></a>Programación de procedimientos almacenados extendidos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| srv_alloc<br /><br /> srv_convert<br /><br /> srv_describe<br /><br /> srv_getbindtoken<br /><br /> srv_got_attention<br /><br /> srv_message_handler<br /><br /> srv_paramdata<br /><br /> srv_paraminfo<br /><br /> srv_paramlen<br /><br /> srv_parammaxlen<br /><br /> srv_paramname<br /><br /> srv_paramnumber<br /><br /> srv_paramset<br /><br /> srv_paramsetoutput<br /><br /> srv_paramstatus<br /><br /> srv_paramtype<br /><br /> srv_pfield<br /><br /> srv_pfieldex<br /><br /> srv_rpcdb<br /><br /> srv_rpcname<br /><br /> srv_rpcnumber<br /><br /> srv_rpcoptions<br /><br /> srv_rpcowner<br /><br /> srv_rpcparams<br /><br /> srv_senddone<br /><br /> srv_sendmsg<br /><br /> srv_sendrow<br /><br /> srv_setcoldata<br /><br /> srv_setcollen<br /><br /> srv_setutype<br /><br /> srv_willconvert<br /><br /> srv_wsendmsg | En su lugar, use la integración con CLR. | XP_API |
| sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc | En su lugar, use la integración con CLR. | sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc |
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginconfig|Use CREATE LOGIN<br /><br /> Use el argumento DROP LOGIN IsIntegratedSecurityOnly de SERVERPROPERTY | xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginconfig |

### <a name="high-availability"></a>Alta disponibilidad

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| creación de reflejo de la base de datos | Grupos de disponibilidad AlwaysOn<br /><br /> Si la edición de SQL Server no admite Grupos de disponibilidad Always On, use el trasvase de registros. | DATABASE_MIRRORING |

### <a name="index-options"></a>Opciones de índice

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_indexoption | ALTER INDEX | sp_indexoption |
| Sintaxis de CREATE TABLE, ALTER TABLE o CREATE INDEX sin paréntesis alrededor de las opciones. | Reescriba la instrucción para utilizar la sintaxis actual. | INDEX_OPTION |

### <a name="instance-options"></a>Opciones de instancia

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_configure option 'allow updates'|Las tablas del sistema ya no son actualizables. La configuración no tiene ningún efecto. | sp_configure 'allow updates' |
| Opciones de sp_configure: <br /><br /> 'locks' <br /><br /> 'open objects'<br /><br /> 'set working set size' | Ahora se configura automáticamente. La configuración no tiene ningún efecto. | sp_configure 'locks'<br /><br /> sp_configure 'open objects'<br /><br /> sp_configure 'set working set size' |
| opción “priority boost” de sp_configure | Las tablas del sistema ya no son actualizables. La configuración no tiene ningún efecto. En su lugar, use la opción start /high ... program.exe de Windows. | sp_configure 'priority boost' |
| sp_configure option 'remote proc trans' | Las tablas del sistema ya no son actualizables. La configuración no tiene ningún efecto. | sp_configure 'remote proc trans' |

### <a name="linked-servers"></a>Servidores vinculados

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Especificar el proveedor SQLOLEDB para los servidores vinculados. | SQL Server Native Client (SQLNCLI) | SQLOLEDDB para servidores vinculados |

### <a name="metadata"></a>Metadatos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| FILE_ID<br /><br /> INDEXKEY_PROPERTY | FILE_IDEX<br /><br /> sys.index_columns | FILE_ID<br /><br /> INDEXKEY_PROPERTY |

### <a name="native-xml-web-services"></a>Servicios web XML nativos

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| La instrucción CREATE ENDPOINT o ALTER ENDPOINT con la opción FOR SOAP.<br /><br /> sys.endpoint_webmethods<br /><br /> sys.soap_endpoints|Utilice Windows Communications Foundation (WCF) o ASP.NET en su lugar. | CREATE/ALTER ENDPOINT<br /><br /> sys.endpoint_webmethods<br /><br /> EXT_soap_endpoints<br /><br /> sys.soap_endpoints |

### <a name="other"></a>Otros

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| DB-Library<br /><br />Embedded SQL para C|Aunque el Motor de base de datos sigue admitiendo conexiones desde aplicaciones existentes que usan las API DB-Library y Embedded SQL, no incluye los archivos ni la documentación necesarios para realizar trabajos de programación en aplicaciones en las que se usan estas API. Una versión futura del Motor de base de datos de SQL Server elimina la compatibilidad con conexiones desde aplicaciones de DB-Library o Embedded SQL. No utilice DB-Library ni Embedded SQL para desarrollar nuevas aplicaciones. Quite las dependencias de DB-Library o Embedded SQL cuando modifique las aplicaciones existentes. En lugar de estas API, use el espacio de nombres SQLClient o una API como ODBC. En SQL Server 2019 (15.x) no se incluye la DLL DB-Library necesaria para ejecutar estas aplicaciones. Para ejecutar aplicaciones de DB-Library o Embedded SQL, debe estar disponible la DLL de DB-Library de SQL Server versión 6.5, SQL Server 7.0 o SQL Server 2000 (8.x). | None |

### <a name="security"></a>Seguridad

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| La sintaxis de ALTER LOGIN WITH SET CREDENTIAL | Se reemplaza por la nueva sintaxis de ALTER LOGIN ADD y DROP CREDENTIAL | ALTER LOGIN WITH SET CREDENTIAL |
| sp_addapprole<br /><br /> sp_dropapprole | CREATE APPLICATION ROLE<br /><br /> DROP APPLICATION ROLE | sp_addapprole<br /><br /> sp_dropapprole |
| sp_addlogin<br /><br /> sp_droplogin | CREATE LOGIN<br /><br /> DROP LOGIN|sp_addlogin<br /><br /> sp_droplogin |
| sp_adduser<br /><br /> sp_dropuser | CREATE USER<br /><br /> DROP USER|sp_adduser<br /><br /> sp_dropuser |
| sp_grantdbaccess<br /><br /> sp_revokedbaccess|CREATE USER<br /><br /> DROP USER|sp_grantdbaccess<br /><br /> sp_revokedbaccess |
| sp_addrole<br /><br /> sp_droprole | CREATE ROLE<br /><br /> DROP ROLE|sp_addrole<br /><br /> sp_droprole |
| sp_approlepassword<br /><br /> sp_password | ALTER APPLICATION ROLE<br /><br /> ALTER LOGIN|sp_approlepassword<br /><br /> sp_password |
| sp_changedbowner|ALTER AUTHORIZATION | sp_changedbowner |
| sp_changeobjectowner|ALTER SCHEMA o ALTER AUTHORIZATION | sp_changeobjectowner |
| sp_control_dbmasterkey_password | Debe existir una clave maestra y la contraseña debe ser correcta.|sp_control_dbmasterkey_password |
| sp_defaultdb<br /><br /> sp_defaultlanguage | ALTER LOGIN|sp_defaultdb<br /><br /> sp_defaultlanguage |
| sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|ALTER LOGIN DISABLE<br /><br /> CREATE LOGIN<br /><br /> DROP LOGIN|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin |
| USER_ID|DATABASE_PRINCIPAL_ID | USER_ID |
| sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|Estos procedimientos almacenados devuelven información que era correcta en [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]. La salida no refleja los cambios realizados en la jerarquía de permisos implementada en SQL Server 2008. Para obtener más información, vea [Permisos de las funciones fijas de servidor](https://msdn.microsoft.com/library/ms175892\(SQL.100\).aspx). | sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission |
| GRANT ALL<br /><br /> DENY ALL<br /><br /> REVOKE ALL|GRANT, DENY y permisos específicos de REVOKE.|ALL Permission |
| Función intrínseca PERMISSIONS | Consulte en su lugar sys.fn_my_permissions. | PERMISSIONS |
| SETUSER | EXECUTE AS | SETUSER |
| Algoritmos de cifrado RC4 y DESX|Use otro algoritmo; por ejemplo, AES. | Algoritmo DESX |

### <a name="server-configuration-options"></a>Opciones de configuración del servidor

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Opción de auditoría c2 opción default trace enabled<br /><br /> opción default trace enabled | [common criteria compliance enabled (opción de configuración del servidor)](../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)<br /><br /> [Eventos extendidos](../relational-databases/extended-events/extended-events.md) | sp_configure 'c2 audit mode'<br /><br />sp_configure 'default trace enabled' |

### <a name="smo-classes"></a>Clases SMO

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| *Microsoft.SQLServer. Clase Management.Smo.Information*<br /><br />*Microsoft.SQLServer. Clase Management.Smo.Settings*<br /><br />*Microsoft.SQLServer.Management. Clase Smo.DatabaseOptions*<br /><br />*Microsoft.SqlServer.Management.Smo. Propiedad DatabaseDdlTrigger.NotForReplication* | *Microsoft.SqlServer.  Clase Management.Smo.Server*<br /><br />**Microsoft.SqlServer.  Management.Smo.Server* Clase<br /><br />* Microsoft.SqlServer. Clase Management.Smo.Database*<br /><br />None | None |

### <a name="sql-server-agent"></a>Agente SQL Server

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Notificación mediante **NET SEND**<br /><br />Notificación mediante buscapersonas | Notificación por correo electrónico<br /><br />Notificación por correo electrónico | None |

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Explorador de soluciones en SQL Server Management Studio | | None |

### <a name="system-stored-procedures-and-functions"></a>Procedimientos almacenados y funciones del sistema

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_db_increased_partitions | Ninguno. La compatibilidad con más particiones está disponible de forma predeterminada en SQL Server 2019 (15.x). | sp_db_increased_partitions |
| fn_virtualservernodes<br /><br />fn_servershareddrives | sys.dm_os_cluster_nodes<br /><br />sys.dm_io_cluster_shared_drives | fn_virtualservernodes<br /><br /> fn_servershareddrives |
| fn_get_sql | sys.dm_exec_sql_text | fn_get_sql |
| sp_lock | sys.dm_tran_locks | sp_lock |

### <a name="system-tables"></a>Tablas del sistema

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers|Vistas de compatibilidad. Para obtener más información, vea [Vistas de compatibilidad &#40;Transact-SQL &#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md).<br /><br />**Importante:** Las vistas de compatibilidad no exponen ninguno de los metadatos relacionados con las características presentadas en SQL Server 2005 (9.x). Se recomienda actualizar las aplicaciones de forma que utilicen vistas de catálogo. Para obtener más información, vea [Vistas de catálogo &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/catalog-views-transact-sql.md). | sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers |
| sys.numbered_procedures<br /><br />sys.numbered_procedure_parameters | None | numbered_procedures<br /><br />numbered_procedure_parameters |

### <a name="sql-trace-stored-procedures-functions-and-catalog-views"></a>Procedimientos almacenados, funciones y vistas de catálogo de seguimiento de SQL

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values|[Eventos extendidos](../relational-databases/extended-events/extended-events.md) | sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values |

### <a name="system-views"></a>Vistas del sistema

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| sys.sql_dependencies|sys.sql_expression_dependencies|sys.sql_dependencies |

### <a name="table-compression"></a>Compresión de tabla

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| El uso del formato de almacenamiento vardecimal. | El formato de almacenamiento Vardecimal está en desuso. La compresión de datos de SQL Server 2019 (15.x) comprime los valores decimales así como otros tipos de datos. Recomendamos que utilice la compresión de datos en lugar del formato de almacenamiento vardecimal. | Formato de almacenamiento vardecimal |
| Uso del procedimiento sp_db_vardecimal_storage_format.|El formato de almacenamiento Vardecimal está en desuso. La compresión de datos de SQL Server 2019 (15.x) comprime los valores decimales así como otros tipos de datos. Recomendamos que utilice la compresión de datos en lugar del formato de almacenamiento vardecimal. | sp_db_vardecimal_storage_format |
| Uso del procedimiento sp_estimated_rowsize_reduction_for_vardecimal.|Utilice en su lugar la compresión de datos y el procedimiento sp_estimate_data_compression_savings. |sp_estimated_rowsize_reduction_for_vardecimal |

### <a name="text-pointers"></a>Punteros de texto

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| WRITETEXT<br /><br />UPDATETEXT<br /><br />READTEXT|None|UPDATETEXT o WRITETEXT<br /><br />READTEXT |
| TEXTPTR()<br /><br />TEXTVALID() | None | TEXTPTR<br /><br />TEXTVALID |

### <a name="transact-sql"></a>Transact-SQL

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Secuencia de llamada a funciones :: | Reemplazada por SELECT *column_list* FROM sys.\<*function_name*>().<br /><br />Por ejemplo, reemplace `SELECT * FROM ::fn_virtualfilestats(2,1)`con `SELECT * FROM sys.fn_virtualfilestats(2,1)`. | '::' function calling syntax |
| Referencias de columnas de tres y de cuatro partes en la lista SELECT. | Los nombres de dos partes son el comportamiento compatible con el estándar.|Nombre de columna de varias partes |
| Cadena entrecomillada utilizada como alias de columna para una expresión de una lista SELECT:<br /><br />'*string_alias*' = *expression* | *expression* [AS] *column_alias*<br /><br />*expression* [AS] [*column_alias*]<br /><br />*expression* [AS] "*column_alias*"<br /><br />*expression* [AS] '*column_alias*'<br /><br />*column_alias* = *expression* | Literales de cadena como alias de columna |
| Procedimientos numerados | Ninguno. No debe usarse. | ProcNums |
| Sintaxis *table_name.index_name* en DROP INDEX|Sintaxis *index_name* ON *table_name* en DROP INDEX.|DROP INDEX con nombre de dos partes |
| No finalice las instrucciones de Transact-SQL con un punto y coma.|Finalice las instrucciones de Transact-SQL con un punto y coma (;). | None |
| GROUP BY ALL|Utilice la solución caso por caso personalizada con UNION o una tabla derivada. | GROUP BY ALL |
| ROWGUIDCOL como nombre de columna en las instrucciones DML.|Use $rowguid.|ROWGUIDCOL |
| IDENTITYCOL como nombre de columna en las instrucciones DML.|Use $identity.|IDENTITYCOL |
| Uso de #, ## como nombres de procedimientos almacenados temporales y tablas temporales. | Utilice al menos un carácter adicional.|'#' y '##' como el nombre de tablas temporales y procedimientos almacenados
| Uso de \@, \@\@ o \@\@ como identificadores de Transact-SQL. | No use como identificador \@, \@\@ ni ningún nombre que empiece por \@\@. | "\@" y nombres que empiezan por "\@\@" como identificadores de Transact-SQL |
| Use la palabra clave DEFAULT como valor predeterminado.|No utilice la palabra DEFAULT como un valor predeterminado. | Palabra clave DEFAULT como valor predeterminado |
| Uso de un espacio como un separador entre las sugerencias de la tabla.|Use una coma para separar las sugerencias de tabla. | Varias sugerencias de tabla sin coma |
| La lista de selección de una vista indizada de agregado debe contener COUNT_BIG (\*) en el modo de compatibilidad 90 | Use COUNT_BIG (\*). | Selección de la lista de vista de índice sin COUNT_BIG(\*) |
| La aplicación indirecta de sugerencias de tabla a la invocación de una función con valores de tabla (TVF) de múltiples instrucciones a través de una vista.|Ninguno.|Sugerencias TVF indirectas |
| Sintaxis de ALTER DATABASE:<br /><br />MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE | MODIFY FILEGROUP READ_ONLY<br /><br />MODIFY FILEGROUP READ_WRITE | MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE |
| Opción de base de datos SET ANSI_NULLS OFF y ANSI_NULLS OFF<br /><br />Opción de base de datos SET ANSI_PADDING OFF y ANSI_PADDING OFF<br /><br />Opción de base de datos SET CONCAT_NULL_YIELDS_NULL OFF y CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS | Ninguno. <br /><br /> ANSI_NULLS, ANSI_PADDING y CONCAT_NULLS_YIELDS_NULL siempre están establecidos en ON. SET OFFSETS no está disponible. | SET ANSI_NULLS OFF <br /><br /> SET ANSI_PADDING OFF<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS<br /><br />ALTER DATABASE SET ANSI_NULLS OFF<br /><br />ALTER DATABASE SET ANSI_PADDING OFF <br /><br /> ALTER DATABASE SET CONCAT_NULL_YIELDS_NULL OFF |
| SET FMTONLY | [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md), [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md), [sp_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) y [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md). | SET FMTONLY |
| Especificar NOLOCK o READUNCOMMITTED en la cláusula FROM de una instrucción UPDATE o DELETE. | Quite las sugerencias de tabla NOLOCK o READUNCOMMITTED de la cláusula FROM. | NOLOCK or READUNCOMMITTED in UPDATE or DELETE |
| Especificar sugerencias de tabla sin utilizar la palabra clave WITH. | Use WITH. | Sugerencia de table sin WITH |
| INSERT_HINTS | | INSERT_HINTS |

### <a name="tools"></a>Herramientas

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| SQL Server Profiler para captura de seguimiento | Use el generador de perfiles de eventos extendidos integrado en SQL Server Management Studio.|SQL Server Profiler |
| SQL Server Profiler para reproducción de seguimiento | [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md) |

### <a name="trace-management-objects"></a>Objetos de administración de seguimiento

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Microsoft.SqlServer.Management.Trace namespace (contiene las API para Seguimiento de SQL Server y los objetos de reproducción)|Configuración de seguimiento: <xref:Microsoft.SqlServer.Management.XEvent><br /><br />Lectura de seguimiento: <xref:Microsoft.SqlServer.XEvent.Linq><br /><br />Reproducción de seguimiento: None | |

### <a name="xml"></a>XML

| Característica desusada | Sustituta | Nombre de característica |
|--------------------|-------------|--------------|
| Generación de esquemas XDR insertados | La directiva XMLDATA para la opción FOR XML ha quedado desusada. Utilice la XSD generación en los modos RAW y AUTO. No hay sustitución para la directiva XMLDATA en modo EXPLICIT. | XMLDATA |

> [!NOTE]
> El parámetro **OUTPUT** de la cookie para **sp_setapprole** está documentado actualmente como **varbinary(8000)** , que es la longitud máxima correcta. Pero la implementación actual devuelve **varbinary(50)** . Si los programadores han asignado **varbinary(50)** , es posible que la aplicación requiera cambios si el tamaño devuelto de la cookie aumenta en una versión futura. Aunque no se trate de un problema de desuso, se menciona en este tema porque los ajustes de aplicación son similares. Para obtener más información, vea [sp_setapprole &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [Funcionalidad del motor de base de datos no incluida en SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)  

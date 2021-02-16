---
title: Documentación de seguridad de SQL Server y Azure SQL Database
description: Referencia de contenido relacionado con la seguridad y la protección de SQL Server y Azure SQL Database.
ms.custom: seo-lt-2019
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- SQL Server, security
- security [SQL Server]
- database security [SQL Server]
- databases [SQL Server], security
ms.assetid: dfb39d16-722a-4734-94bb-98e61e014ee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22df18f14ddb3eccee24944fa00822cb3176bcfa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340202"
---
# <a name="security-center-for-sql-server-database-engine-and-azure-sql-database"></a>Centro de seguridad para el motor de base de datos SQL Server y la base de datos SQL Azure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Esta página proporciona los vínculos para ayudarle a buscar la información que necesita sobre seguridad y protección en [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 **Leyenda**  
  
 ![Captura de pantalla de la leyenda que explica los iconos de disponibilidad de las características.](../performance/media/security-center-legend.PNG "security-center-legend")  
  
##  <a name="authentication-who-are-you"></a><a name="Who"></a> Autenticación: ¿Quién es usted?  
  
|||  
|-|-|  
|**¿Quién autentica?**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Autenticación de Windows<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Azure Active Directory|¿Quién autentica? (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])<br /><br /> [Elegir un modo de autenticación](../../relational-databases/security/choose-an-authentication-mode.md)<br /><br /> [Conexión a SQL Database mediante autenticación de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)|  
|**¿Dónde se autentica?**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: En la base de datos maestra: inicios de sesión y usuarios de BD<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: En la base de datos de usuario: Usuarios de base de BD independiente|Autenticación en la base de datos maestra (inicios de sesión y usuarios de base de datos)<br /><br /> [Crear un inicio de sesión de SQL Server](../../relational-databases/security/authentication-access/create-a-login.md)<br /><br /> [Administración de bases de datos e inicios de sesión en Azure SQL Database](/previous-versions/azure/ee336235(v=azure.100))<br /><br /> [Crear un usuario de base de datos](../../relational-databases/security/authentication-access/create-a-database-user.md)<br /><br /> <br /><br /> Autenticación en una base de datos de usuario<br /><br /> [Usuarios de base de datos independientes: hacer que la base de datos sea portátil](../../relational-databases/security/contained-database-users-making-your-database-portable.md)|  
|**Utilizando otras identidades**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Credenciales<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Ejecutar como otro inicio de sesión<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Ejecutar como otro usuario de base de datos|[Credenciales &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)<br /><br /> [Ejecutar como otro inicio de sesión](../../t-sql/statements/execute-as-transact-sql.md)<br /><br /> [Ejecutar como otro usuario de base de datos](../../t-sql/statements/execute-as-transact-sql.md)|  
  
##  <a name="authorization-what-can-you-do"></a><a name="What"></a> Autorización: ¿Qué puede hacer?  
  
|||  
|-|-|  
|**Conceder, revocar y denegar permisos**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Clases protegibles<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Permisos de servidor pormenorizados<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Permisos de base de datos pormenorizados|[Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)<br /><br /> [Permisos](../../relational-databases/security/permissions-database-engine.md)<br /><br /> [Elementos protegibles](../../relational-databases/security/securables.md)<br /><br /> [Introducción a los permisos de los motores de bases de datos](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)|  
|**Seguridad por roles**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Roles de nivel de servidor<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Roles de nivel de base de datos|[Roles de nivel de servidor](../../relational-databases/security/authentication-access/server-level-roles.md)<br /><br /> [Roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md)|  
|**Restringir el acceso de datos a elementos de datos seleccionados**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Restricción del acceso a datos mediante vistas o procedimientos<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Seguridad de nivel de fila<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Enmascaramiento dinámico de datos<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Objetos firmados|Restricción del acceso a datos mediante [Vistas](../../relational-databases/views/views.md) y [Procedimientos](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)<br /><br /> [Seguridad de nivel de fila (SQL Server)](../../relational-databases/security/row-level-security.md)<br /><br /> [Seguridad de nivel de fila (base de datos SQL de Azure)](./row-level-security.md)<br /><br /> [Enmascaramiento dinámico de datos (SQL Server)](../../relational-databases/security/dynamic-data-masking.md)<br /><br /> [Enmascaramiento dinámico de datos (Base de datos SQL de Azure)](/azure/azure-sql/database/dynamic-data-masking-overview)<br /><br /> [Objetos firmados](../../t-sql/statements/add-signature-transact-sql.md)|  
  
##  <a name="encryption-storing-secret-data"></a><a name="Encrypt"></a> Cifrado: Almacenamiento de datos secretos  
  
|||  
|-|-|  
|**Cifrado de archivos:**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Cifrado de BitLocker (nivel de unidad)<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Cifrado de NTFS (nivel de carpeta)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Cifrado de datos transparente (nivel de archivo)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Cifrado de copia de seguridad (nivel de archivo)|[BitLocker (nivel de unidad)](https://support.microsoft.com/kb/2855131)<br /><br /> [Cifrado de NTFS (nivel de carpeta)](/previous-versions/tn-archive/dd163562(v=technet.10))<br /><br /> [Cifrado de datos transparente (nivel de archivo)](../../relational-databases/security/encryption/transparent-data-encryption.md)<br /><br /> [Cifrado de copia de seguridad (nivel de archivo)](../../relational-databases/backup-restore/backup-encryption.md)|  
|**Orígenes de cifrado**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Módulo de administración extensible de claves<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Claves almacenadas en Azure Key Vault<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Always Encrypted|[Módulo de administración extensible de claves](../../relational-databases/security/encryption/extensible-key-management-ekm.md)<br /><br /> [Claves almacenadas en el Almacén de claves de Azure](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)<br /><br /> [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)|  
|**Cifrado de columnas, datos y claves**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Cifrado mediante certificado<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Cifrado mediante clave simétrica<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Cifrado mediante clave asimétrica<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Cifrado mediante frase de contraseña|[Cifrar mediante certificados](../../t-sql/functions/encryptbycert-transact-sql.md)<br /><br /> [Cifrar mediante clave asimétrica](../../t-sql/functions/encryptbyasymkey-transact-sql.md)<br /><br /> [Cifrar mediante clave simétrica](../../t-sql/functions/encryptbykey-transact-sql.md)<br /><br /> [Cifre mediante frase de contraseña](../../t-sql/functions/encryptbypassphrase-transact-sql.md)<br /><br /> [Cifrar una columna de datos](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
##  <a name="connection-security-restricting-and-securing"></a><a name="Connect"></a> Seguridad de conexión: Restringir y proteger  
  
|||  
|-|-|  
|**Protección de Firewall**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Configuración de Firewall de Windows<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Configuración de firewall del servicio de Azure<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Configuración de firewall de Azure SQL Database|[Configuración de Firewall de Windows para el acceso al motor de base de datos](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)<br /><br /> [Configuración de firewall de la base de datos SQL Azure](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)<br /><br /> [Configuración de Firewall del servicio Azure](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)|  
|**Cifrar los datos en tránsito**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Conexiones SSL forzadas<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Conexiones SSL opcionales|[Habilitación de conexiones cifradas en el motor de base de datos](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)<br /><br /> [Habilitar conexiones cifradas en el motor de base de datos](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md), [Seguridad de red](/azure/sql-database/sql-database-security-best-practice#network-security) <br /><br /> [Soporte de TLS 1.2 para Microsoft SQL Server](https://support.microsoft.com/kb/3135244)|  
  
##  <a name="auditing-recording-access"></a><a name="Audit"></a> Auditoría: Acceso a la grabación  
  
|||  
|-|-|  
|**Auditoría automatizada**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: Auditoría de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (nivel de servidor y de base de datos)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Auditoría de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] (nivel de base de datos)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Detección de amenazas| <br /><br /> [SQL Server Audit &#40;motor de base de datos&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)<br /><br /> [Auditoría de base de datos SQL](/azure/azure-sql/database/auditing-overview)<br /><br /> [Introducción a Advanced Threat Protection de SQL Database](/azure/azure-sql/database/threat-detection-configure) <br /><br /> [Evaluación de vulnerabilidad de SQL Database](/azure/sql-database/sql-vulnerability-assessment) |  
|**Auditoría personalizada**<br /><br /> Desencadenadores de :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png":::|Implementación de auditoría personalizada: Crear [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md) y [DML Triggers](../../relational-databases/triggers/dml-triggers.md)|  
|**Cumplimiento normativo**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: Cumplimiento|SQL Server:<br />                        [criterios comunes](https://go.microsoft.com/fwlink/?LinkId=616319)<br /><br /> Base de datos SQL:<br />                        [Centro de confianza de Microsoft Azure: Cumplimiento según la característica](https://azure.microsoft.com/support/trust-center/services/)|  
  
##  <a name="sql-injection"></a><a name="SQLInjection"></a> Inyección de código SQL  
 La inyección de código SQL es un ataque en el cual se inserta código malintencionado en las cadenas que, posteriormente, se pasan al [!INCLUDE[ssDE](../../includes/ssde-md.md)] para su análisis y ejecución. Todos los procedimientos que generan instrucciones SQL deben revisarse en busca de vulnerabilidades de inyección de código, ya que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecutará todas las consultas recibidas que sean válidas desde el punto de vista sintáctico. Todos los sistemas de base de datos tienen algún riesgo de inyección de código SQL y muchas de las vulnerabilidades se presentan en la aplicación que consulta el [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Para impedir ataques de inyección de código SQL, puede usar procedimientos almacenados y comandos con parámetros, con lo que se evita el código SQL dinámico y se restringen los permisos en todos los usuarios.  Para obtener más información, consulte [SQL Injection](../../relational-databases/security/sql-injection.md).  
  
 Vínculos adicionales para los programadores de aplicaciones:  
  
-   [Escenarios de seguridad de aplicaciones en SQL Server](/dotnet/framework/data/adonet/sql/application-security-scenarios-in-sql-server)  
  
-   [Escribir SQL dinámico seguro en SQL Server](/dotnet/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server)  
  
-   [Cómo: Proteger ASP.NET de inyecciones SQL](/previous-versions/msp-n-p/ff648339(v=pandp.10))  
  
## <a name="see-also"></a>Consulte también  
 [Introducción a los permisos de los motores de bases de datos](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [Proteger SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Certificados y claves asimétricas de SQL Server](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)   
 [Cifrado de SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Configuración de Área expuesta](../../relational-databases/security/surface-area-configuration.md)   
 [Contraseñas seguras](../../relational-databases/security/strong-passwords.md)   
 [Propiedad de base de datos TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md)   
 [Características y tareas del motor de base de datos](../../sql-server/what-s-new-in-sql-server-ver15.md)  
 [Proteger su propiedad intelectual de SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
  
[!INCLUDE[get-help-security](../../includes/get-help-security.md)]
---
title: 'Seguridad de transporte: Grupos de disponibilidad y creación de reflejo de base de datos'
description: Obtenga información sobre cómo proteger los mensajes entre las bases de datos que participan en un grupo de disponibilidad Always On o una sesión de creación de reflejos de una base de datos hospedada en SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- cryptography [SQL Server], database mirroring
- certificates [SQL Server], database mirroring
- encryption [SQL Server], database mirroring
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- transport security
- database mirroring [SQL Server], security
ms.assetid: 49239d02-964e-47c0-9b7f-2b539151ee1b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 97a3171a4028a28c6da5fb6afae37144db3d2727
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352199"
---
# <a name="transport-security---database-mirroring---always-on-availability"></a>Seguridad de transporte - Creación de reflejo de la base de datos - Grupos de disponibilidad AlwaysOn
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La seguridad en el transporte implica la autenticación y, opcionalmente, el cifrado de los mensajes intercambiados entre las bases de datos. Para la creación de reflejo de la base de datos y [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], la autenticación y el cifrado se configuran en el extremo de creación de reflejo de la base de datos. Para ver una introducción a los puntos de conexión de creación de reflejo de la base de datos, vea [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
 **En este tema:**  
  
-   [Autenticación](#Authentication)  
  
-   [Cifrado de datos](#DataEncryption)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="authentication"></a><a name="Authentication"></a> Autenticación  
 Autenticación es el proceso de comprobar que un usuario es quien dice ser. Las conexiones entre extremos de creación de reflejo de la base de datos requieren autenticación. Las solicitudes de conexión desde un asociado o testigo, según corresponda, deben autenticarse.  
  
 El tipo de autenticación que utiliza una instancia de servidor para la creación de reflejo de la base de datos o [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] es una propiedad del extremo de reflejo de la base de datos. Existen dos tipos de seguridad de transporte para los puntos de conexión de creación de reflejo de la base de datos: autenticación de Windows (la interfaz del proveedor de compatibilidad para seguridad (SSPI)) y la autenticación basada en certificados.  
  
### <a name="windows-authentication"></a>Autenticación de Windows  
 En la autenticación de Windows, cada instancia de servidor inicia sesión en el otro lado mediante las credenciales de Windows de la cuenta de usuario de Windows en la que se ejecuta el proceso. La autenticación de Windows puede requerir alguna configuración manual de las cuentas de inicio de sesión, como se indica a continuación:  
  
-   Si las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutan como servicios en la misma cuenta de dominio, no se requiere ninguna configuración adicional.  
  
-   Si las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutan como servicios con diferentes cuentas de dominio (en el mismo dominio o en dominios de confianza), el inicio de sesión de cada cuenta debe crearse en **master** en cada una de las otras instancias de servidor, y a ese inicio de sesión se le deben conceder permisos CONNECT en el punto de conexión.  
  
-   Si las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutan como cuenta de servicio de red, el inicio de sesión de cada cuenta de equipo host (_DomainName_ **\\** _ComputerName$_ ) se debe crear en **master** en cada uno de los demás servidores, y a ese inicio de sesión se le deben conceder permisos CONNECT en el punto de conexión. Esto se debe a que una instancia de servidor que se ejecuta en la cuenta Servicio de red se autentica mediante la cuenta de dominio del equipo host.  
  
> [!NOTE]  
>  Para obtener un ejemplo de configuración de una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows, vea [Ejemplo: Configurar la creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
  
### <a name="certificates"></a>Certificados  
 En ciertas situaciones, como cuando las instancias de servidor no están en dominios de confianza o cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta como servicio local, la autenticación de Windows no está disponible. En estos casos, en lugar de credenciales de usuario, se requieren certificados para autenticar solicitudes de conexión. El extremo de creación de reflejo de cada instancia de servidor debe configurarse con su propio certificado creado localmente.  
  
 El método de cifrado se establece al crear el certificado. Para obtener más información, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md). Administre con cuidado los certificados que utilice.  
  
 Una instancia de servidor utiliza la clave privada de su propio certificado para establecer su identidad al establecer una conexión. La instancia de servidor que recibe la solicitud de conexión utiliza la clave pública del certificado del remitente para autenticar la identidad de éste. Por ejemplo, considere dos instancias del servidor, Server_A y Server_B. Server_A utiliza su clave privada para cifrar el encabezado de conexión antes de enviar una solicitud de conexión a Server_B. Server_B utiliza la clave pública del certificado de Server_A para descifrar el encabezado de conexión. Si el encabezado descifrado es correcto, Servidor_B sabe que el encabezado fue cifrado por Servidor_A y la conexión se autentica. Si el encabezado descifrado es incorrecto, Servidor_B sabe que la solicitud de conexión no es auténtica y rechaza la conexión.  
  
##  <a name="data-encryption"></a><a name="DataEncryption"></a> Cifrado de datos  
 De manera predeterminada, un extremo de creación de reflejo de la base de datos requiere el cifrado de datos enviados en conexiones de creación de reflejo. En este caso, solo se puede conectar el extremo a extremos que también utilicen cifrado. A menos que pueda garantizar que su red es segura, se recomienda requerir cifrado para las conexiones de creación de reflejo de la base de datos. Sin embargo, puede deshabilitar el cifrado o hacerlo compatible, pero no obligatorio. Si se deshabilita el cifrado, los datos no se cifran nunca y el extremo no se puede conectar a otro extremo que requiera cifrado. Si se admite cifrado, los datos solo se cifran si el extremo opuesto admite o requiere cifrado.  
  
> [!NOTE]  
>  Los extremos de creación de reflejo creados mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] se crean con cifrado obligatorio o deshabilitado. Para cambiar la configuración de cifrado a SUPPORTED, utilice la instrucción ALTER ENDPOINT [!INCLUDE[tsql](../../includes/tsql-md.md)] . Para obtener más información, vea [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 De manera opcional, puede controlar los algoritmos de cifrado que utiliza un extremo si especifica uno de los siguientes valores para la opción ALGORITHM en un instrucción CREATE ENDPOINT o ALTER ENDPOINT:  
  
|Valor ALGORITHM|Descripción|  
|---------------------|-----------------|  
|RC4|Especifica que el extremo debe usar el algoritmo RC4. Este es el valor predeterminado.<br /><br /> <strong>\*\* Advertencia \*\*</strong> El algoritmo RC4 está en desuso. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Se recomienda utilizar AES.|  
|AES|Especifica que el extremo debe usar el algoritmo AES.|  
|AES RC4|Especifica que los dos extremos negociarán un algoritmo de cifrado con este extremo, dando preferencia al algoritmo AES.|  
|RC4 AES|Especifica que los dos extremos negociarán un algoritmo de cifrado con este extremo, dando preferencia al algoritmo RC4.|  
  
 Si los extremos de conexión especifican estos dos algoritmos en distintas órdenes, el extremo que acepte la conexión gana.  
  
> [!NOTE]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
>   
>  Aunque es notablemente más rápido que AES, RC4 es un algoritmo relativamente débil, mientras que AES es un algoritmo relativamente fuerte. Por tanto, se recomienda utilizar el algoritmo AES.  
  
 Para obtener más información sobre la sintaxis de [!INCLUDE[tsql](../../includes/tsql-md.md)] para especificar el cifrado, vea [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para configurar la seguridad en el transporte para un extremo de creación de reflejo de la base de datos**  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
## <a name="see-also"></a>Consulte también  
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [Centro de seguridad para el motor de base de datos SQL Server y la base de datos SQL Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [sys.dm_db_mirroring_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)   
 [Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Solucionar problemas de configuración de grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  

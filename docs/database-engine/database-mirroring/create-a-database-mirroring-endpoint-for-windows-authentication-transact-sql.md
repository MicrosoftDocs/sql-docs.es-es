---
title: Creación de un punto de conexión de creación de reflejo de la base de datos (Transact-SQL)
description: Use Transact-SQL para crear un punto de conexión de creación de reflejos de las bases de datos con la autenticación de Windows en SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], database mirroring
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: baf1a4b1-6790-4275-b261-490bca33bdb9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 87df52f30863d21f51a73a0dfe7b6cc1f4481bfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353212"
---
# <a name="create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql"></a>Crear un extremo de reflejo de la base de datos para la autenticación de Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear un extremo de creación de reflejo de la base de datos que utilice la autenticación de Windows en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para admitir la creación de reflejo de la base de datos o [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] , cada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere un extremo de creación de reflejo de la base de datos. Una instancia de servidor solo puede tener un extremo de creación de reflejo de base de datos, que tiene un puerto único. Un extremo de creación de reflejo de base de datos puede utilizar cualquier puerto disponible en el sistema local cuando se crea el extremo. Todas las sesiones de creación de reflejo de base de datos de una instancia de servidor escuchan en dicho puerto y todas las conexiones entrantes para la creación de reflejo de base de datos utilizan dicho puerto.  
  
> [!IMPORTANT]  
>  Si existe un extremo de creación de reflejo de la base de datos y ya está en uso, se recomienda utilizar dicho extremo. Quitar un extremo en uso interrumpe las sesiones existentes.  
  
 **En este tema**  
  
-   **Antes de empezar:**  [Seguridad](#Security)  
  
-   **Para crear un punto de conexión de creación de reflejo de la base de datos con:**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 El administrador del sistema establece los métodos de autenticación y cifrado de la instancia del servidor.  
  
> [!WARNING]  
>  El algoritmo RC4 está obsoleto. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Se recomienda utilizar AES.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Debe disponer del permiso `CREATE ENDPOINT` o pertenecer al rol fijo de servidor `sysadmin`. Para obtener más información, vea [GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-database-mirroring-endpoint-that-uses-windows-authentication"></a>Para crear un extremo de reflejo de la base de datos que use la autenticación de Windows  
  
1.  Conéctese a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que desea crear un extremo de creación de reflejo de la base de datos.  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Determine si ya existe un extremo de creación de reflejo de la base de datos utilizando la siguiente instrucción:  
  
    ```sql  
    SELECT name, role_desc, state_desc FROM sys.database_mirroring_endpoints;   
    ```  
  
    > [!IMPORTANT]  
    >  Si ya existe un extremo de creación de reflejo de la base de datos para la instancia del servidor, utilice ese extremo para todas las otras sesiones que establezca en la instancia del servidor.  
  
4.  Si quiere usar Transact-SQL a fin de crear un punto de conexión para utilizarlo con la autenticación de Windows, use la instrucción `CREATE ENDPOINT`. La instrucción toma la siguiente forma general:  
  
     ```syntaxsql
     CREATE ENDPOINT *\<endpointName>*  
  
     STATE=STARTED  
  
     AS TCP ( LISTENER_PORT = *\<listenerPortList>* )  
  
     FOR DATABASE_MIRRORING  
  
     (  
  
     [ AUTHENTICATION = **WINDOWS** [ *\<authorizationMethod>* ]  
  
     ]  
  
     [ [**,**] ENCRYPTION = **REQUIRED**  
  
     [ ALGORITHM { *\<algorithm>* } ]  
  
     ]  
  
     [**,**] ROLE = *\<role>*  
  
     )  
     ```
     
     Donde:  
  
    -   *\<endpointName>* es un nombre único para el punto de conexión de creación de reflejos de las bases de datos de la instancia del servidor.  
  
    -   STARTED especifica que el extremo debe iniciarse y que debe empezar a escuchar las conexiones. Un extremo de creación de reflejo de la base de datos de base de datos normalmente se crea en el estado STARTED. Opcionalmente, puede iniciar una sesión en el estado STOPPED (valor predeterminado) o DISABLED.  
  
    -   *\<listenerPortList>* es un número de puerto único (*nnnn*) en el que quiere que el servidor escuche los mensajes de creación de reflejos de las bases de datos. Solo se permite TCP; si se especifica cualquier otro protocolo se provoca un error.  
  
         Un número de puerto solo se puede usar una vez por sistema. Un extremo de creación de reflejo de base de datos puede utilizar cualquier puerto disponible en el sistema local cuando se crea el extremo. Para identificar los puertos que están usando los extremos TCP del sistema, utilice la siguiente instrucción Transact-SQL:  
  
        ```sql  
        SELECT name, port FROM sys.tcp_endpoints;  
        ```  
  
        > [!IMPORTANT]  
        >  Cada instancia del servidor requiere solo un puerto de escucha único.  
  
    -   Para la Autenticación de Windows, la opción AUTHENTICATION es opcional, salvo que desee que el extremo únicamente Use NTLM o Kerberos para autenticar conexiones. *\<authorizationMethod>* especifica el método que se usa para autenticar conexiones como uno de los siguientes: NTLM, KERBEROS o NEGOTIATE. El valor predeterminado, NEGOTIATE, hace que el extremo utilice el protocolo de negociación de Windows para elegir NTLM o Kerberos. La negociación habilita conexiones con o sin autenticación, dependiendo del nivel de autenticación del extremo opuesto.  
  
    -   ENCRYPTION se establece en REQUIRED de forma predeterminada. Esto significa que todas las conexiones con este punto final deben usar cifrado. No obstante, puede deshabilitar el cifrado o hacer que sea opcional en un extremo. Las alternativas son las siguientes:  
  
        |Value|Definición|  
        |-----------|----------------|  
        |DISABLED|Especifica que los datos enviados a través de una conexión no están cifrados.|  
        |SUPPORTED|Especifica que los datos están cifrados solo si el extremo opuesto especifica SUPPORTED o REQUIRED.|  
        |REQUIRED|Especifica que los datos enviados en una conexión deben estar cifrados.|  
  
         Si un extremo requiere cifrado, el otro extremo debe tener ENCRYPTION establecido en SUPPORTED o REQUIRED.  
  
    -   *\<algorithm>* proporciona la opción de especificar estándares de cifrado para el punto de conexión. El valor de *\<algorithm>* puede ser uno de los algoritmos o combinaciones de algoritmos siguientes: RC4, AES, AES RC4 o RC4 AES.  
  
         AES RC4 especifica que este extremo negociará el algoritmo de cifrado, dando preferencia al algoritmo AES. RC4 AES especifica que este extremo negociará el algoritmo de cifrado, dando preferencia al algoritmo RC4. Si ambos extremos especifican estos dos algoritmos en distintas órdenes, el extremo que acepte la conexión gana. Proporcione el mismo algoritmo de forma explícita para evitar errores de conexión entre distintos servidores.
  
        > [!WARNING]  
        >  El algoritmo RC4 está obsoleto. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Se recomienda utilizar AES.  
  
    -   *\<role>* define el rol o los roles que el servidor puede llevar a cabo. Se tiene que especificar ROLE. Sin embargo, el rol del extremo solo es relevante para la creación de reflejo de la base de datos. Para [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], el rol del extremo se omite.  
  
         Para permitir que una instancia de servidor sirva como un rol para una sesión de reflejo de base de datos y un rol diferente para otra sesión, especifique ROLE=ALL. Para hacer que la instancia de un servidor solo sea un asociado o un testigo, especifique `ROLE = PARTNER` o `ROLE = WITNESS`, respectivamente.  
  
        > [!NOTE]  
        >  Para obtener más información sobre las opciones de creación de reflejo de la base de datos en distintas ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
     Para obtener una descripción completa de la sintaxis de CREATE ENDPOINT, vea [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
    > [!NOTE]  
    >  Para cambiar un punto de conexión existente, use [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
###  <a name="example-creating-endpoints-to-support-for-database-mirroring-transact-sql"></a><a name="TsqlExample"></a> Ejemplo: Crear puntos de conexión para admitir para la creación de reflejo de la base de datos (Transact-SQL)  
 En el siguiente ejemplo se crean dos extremos de creación de reflejo de la base de datos para las instancias del servidor predeterminadas en tres sistemas independientes:  
  
|Rol de la instancia de servidor|Nombre del equipo host|  
|-----------------------------|---------------------------|  
|Asociado (inicialmente en el rol principal)|`SQLHOST01\.`|  
|Asociado (inicialmente en el rol reflejado)|`SQLHOST02\.`|  
|Testigo|`SQLHOST03\.`|  
  
 En este ejemplo, los tres extremos utilizan el número de puerto 7022, aunque funcionaría cualquier número de puerto disponible. La opción AUTHENTICATION no es necesaria, porque los extremos utilizan el tipo predeterminado, Autenticación de Windows. La opción ENCRYPTION tampoco es necesaria, porque se supone que los extremos van a negociar el método de autenticación de una conexión, que es el comportamiento predeterminado para la autenticación de Windows. Además, todos los extremos requieren cifrado, que es el comportamiento predeterminado.  
  
 Cada instancia de servidor se limita a actuar como asociado o como testigo, y el extremo de cada servidor especifica expresamente el rol (ROLE=PARTNER o ROLE=WITNESS).  
  
> [!IMPORTANT]  
>  Cada instancia de servidor solo puede tener un extremo. Por lo tanto, si desea que una instancia de servidor actúe como asociado en ciertas sesiones y como testigo en otras, especifique ROLE=ALL.  
  
```sql  
--Endpoint for initial principal server instance, which  
--is the only server instance running on SQLHOST01.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for initial mirror server instance, which  
--is the only server instance running on SQLHOST02.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for witness server instance, which  
--is the only server instance running on SQLHOST03.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=WITNESS);  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para configurar un extremo de creación del reflejo de la base de datos**  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para grupos de disponibilidad AlwaysOn &#40;SQL Server PowerShell&#41;](../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Permitir que un punto de conexión de creación de reflejo de la base de datos use certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Para ver información acerca del extremo de creación de reflejo de la base de datos**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Ejemplo: Configurar la creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  


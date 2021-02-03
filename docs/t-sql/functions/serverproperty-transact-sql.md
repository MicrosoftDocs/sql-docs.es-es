---
description: SERVERPROPERTY (Transact-SQL)
title: SERVERPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SERVERPROPERTY_TSQL
- SERVERPROPERTY
- sql13.swb.serverpropeties.connections.f1
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SERVERPROPERTY function
- server instance property information [SQL Server]
- IsHadrEnabled server property
- instances of SQL Server, property information
- server properties [SQL Server]
ms.assetid: 11e166fa-3dd2-42d8-ac4b-04f18c612c4a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cd1ebb88dc0a3cad6ae8712ec68f7d63fc0c655a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237413"
---
# <a name="serverproperty-transact-sql"></a>SERVERPROPERTY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Devuelve información de la propiedad acerca de la instancia del servidor.  

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
SERVERPROPERTY ( 'propertyname' )  
```  

> [!IMPORTANT]
> Los números de versión de [!INCLUDE[ssde_md](../../includes/ssde_md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no son comparables entre sí y representan los números de compilación internos de estos productos independientes. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] se basa en la misma base de código que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Lo más importante es que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] siempre tiene los bits de SQL [!INCLUDE[ssde_md](../../includes/ssde_md.md)] más recientes. La versión 12 de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] es más reciente que la versión 15 de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*propertyname*  
Es una expresión que contiene la información de propiedad que se va a devolver para el servidor. *propertyname* puede ser uno de los valores siguientes.  
  
|Propiedad|Valores devueltos|  
|--------------|---------------------|  
|BuildClrVersion|Versión de Common Language Runtime (CLR) de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] que se usó al generar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|Intercalación|El nombre de la intercalación predeterminada para el servidor.<br /><br /> NULL = La entrada no es válida o es un error.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|CollationID|Id. de la intercalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Tipo de datos base: **int**|  
|ComparisonStyle|Estilo de comparación de Windows de la intercalación.<br /><br /> Tipo de datos base: **int**|  
|ComputerNamePhysicalNetBIOS|El nombre NetBIOS del equipo local en el que se está ejecutando la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> En el caso de una instancia en clúster de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un clúster de conmutación por error, este valor cambia conforme la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conmuta por error con otros nodos del clúster de conmutación por error.<br /><br /> En una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este valor permanece constante y devuelve el mismo valor que la propiedad MachineName.<br /><br /> **Nota:** Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se encuentra en un clúster de conmutación por error y desea obtener el nombre de la instancia en clúster de conmutación por error, utilice la propiedad MachineName.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|Edición|Edición de producto instalada de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Use el valor de esta propiedad para determinar las características y los límites, como por ejemplo [Límites de la capacidad de cálculo de cada edición de SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md). Las versiones de 64 bits del [!INCLUDE[ssDE](../../includes/ssde-md.md)] anexan (64 bits) a la versión.<br /><br /> Devuelve:<br /><br /> 'Enterprise Edition'<br /><br /> "Enterprise Edition: licencia basada en núcleo".<br /><br /> 'Enterprise Evaluation Edition'<br /><br /> 'Business Intelligence Edition'<br /><br /> 'Developer Edition'<br /><br /> 'Express Edition'<br /><br /> 'Express Edition with Advanced Services'<br /><br /> 'Standard Edition'<br /><br /> 'Web Edition'<br /><br /> 'SQL Azure' indica [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> "Azure SQL Edge Developer" indica la edición de solo desarrollo para Azure SQL Edge<br /><br /> "Azure SQL Edge" indica la edición de pago para Azure SQL Edge<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|EditionID|EditionID representa la edición instalada de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Use el valor de esta propiedad para determinar las características y los límites, como por ejemplo [Límites de la capacidad de cálculo de cada edición de SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).<br /><br /> 1804890536 = Enterprise<br /><br /> 1872460670 = Enterprise Edition: licencia basada en núcleo<br /><br /> 610778273= Enterprise Evaluation<br /><br /> 284895786 = Business Intelligence<br /><br /> -2117995310 = Developer<br /><br /> -1592396055 = Express<br /><br /> -133711905= Express con Advanced Services<br /><br /> -1534726760 = Standard<br /><br /> 1293598313 = Web<br /><br /> 1674378470 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]<br /><br /> -1461570097 = Desarrollador de Azure SQL Edge <br /><br /> 1994083197 = Azure SQL Edge <br /><br />Tipo de base de datos: **bigint**|  
|EngineEdition|Edición de [!INCLUDE[ssDE](../../includes/ssde-md.md)] de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instalada en el servidor.<br /><br /> 1 = Personal o Desktop Engine (No está disponible en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores.)<br /><br /> 2 = Standard (se devuelve para Standard, Web y Business Intelligence.)<br /><br /> 3 = Enterprise (es el valor que se devuelve en las ediciones Evaluation, Developer y Enterprise).<br /><br /> 4 = Express (se devuelve para Express, Express with Tools y Express con Advanced Services).<br /><br /> 5 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<br /><br /> 6 = [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> 8 = [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]<br /><br /> 9 = Azure SQL Edge (se devuelve para todas las ediciones de Azure SQL Edge)<br /><br /> 11 = Grupo de SQL sin servidor de Azure Synapse<br /><br /> Tipo de datos base: **int**|  
|FilestreamConfiguredLevel|Nivel configurado de acceso de FILESTREAM. Para más información, vea [filestream access level (opción de configuración del servidor)](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).<br /><br /> Tipo de datos base: **int**|  
|FilestreamEffectiveLevel|Nivel efectivo de acceso de FILESTREAM. Este valor puede ser diferente de FilestreamConfiguredLevel si el nivel ha cambiado y queda pendiente un reinicio de la instancia o del equipo. Para más información, vea [filestream access level (opción de configuración del servidor)](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).<br /><br /> Tipo de datos base: **int**|  
|FilestreamShareName|Nombre del recurso compartido usado por FILESTREAM.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **nvarchar(128)**| 
|HadrManagerStatus|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Indica si el administrador de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ha iniciado.<br /><br /> 0 = No iniciado, pendiente de comunicación.<br /><br /> 1 = Iniciado y en ejecución.<br /><br /> 2 = No iniciado y con error.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|InstanceDefaultBackupPath|**Se aplica a**: [!INCLUDE[ssSQL2019](../../includes/sssql19-md.md)] y posterior.<br /><br /> Nombre de la ruta de acceso predeterminada a los archivos de copia de seguridad de la instancia.|  
|InstanceDefaultDataPath|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta la versión actual en actualizaciones a partir de finales de 2015.<br /><br /> Nombre de la ruta de acceso predeterminada a los archivos de datos de instancia.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|InstanceDefaultLogPath|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta la versión actual en actualizaciones a partir de finales de 2015.<br /><br /> Nombre de la ruta de acceso predeterminada a los archivos de registro de instancia.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|InstanceName|Nombre de la instancia a la que está conectado el usuario.<br /><br /> Devuelve NULL si el nombre de la instancia es la instancia predeterminada o si es una entrada no válida o un error.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|IsAdvancedAnalyticsInstalled|Devuelve 1 si se ha instalado la característica Análisis avanzado durante la instalación; 0 si no se instaló Análisis avanzado.<br /><br /> Tipo de datos base: **int**|  
|IsBigDataCluster| Introducido en [!INCLUDE[ssSQL2019](../../includes/sssql19-md.md)] a partir de CU4.<br /><br />Devuelve 1 si la instancia es un clúster de macrodatos de SQL Server; en caso contrario, devuelve 0.<br /><br /> Tipo de datos base: **int**|  
|IsClustered|La instancia del servidor está configurada en un clúster de conmutación por error.<br /><br /> 1 = Clúster.<br /><br /> 0 = No clúster.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|IsFullTextInstalled|Los componentes de indización de texto completo y semántica están instalados en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Los componentes de indización de texto completo y semántica están instalados.<br /><br /> 0 = Los componentes de indización de texto completo y semántica no están instalados.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|IsHadrEnabled|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] está habilitado en esta instancia del servidor.<br /><br /> 0 = La característica [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] está deshabilitada.<br /><br /> 1 = La característica [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] está habilitada.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**<br /><br /> Para que las réplicas de disponibilidad se creen y se ejecuten en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] debe estar habilitado en la instancia del servidor. Para más información, vea [Habilitar y deshabilitar grupos de disponibilidad de AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).<br /><br /> **Nota:** La propiedad IsHadrEnabled pertenece solamente a [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Otras características de alta disponibilidad o de recuperación ante desastres, como la creación de reflejo de la base de datos o el trasvase de registros, no se ven afectadas por esta propiedad de servidor.|  
|IsIntegratedSecurityOnly|El servidor está en modo de seguridad integrada.<br /><br /> 1= Seguridad integrada (Autenticación de Windows)<br /><br /> 0 = Seguridad no integrada. (Tanto la autenticación de Windows como la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|IsLocalDB|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> El servidor es una instancia de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|IsPolyBaseInstalled|**Se aplica a**: [!INCLUDE[ssSQL2016](../../includes/sssql16-md.md)].<br /><br /> Devuelve un valor que indica si la instancia del servidor tiene instalada la característica PolyBase.<br /><br /> 0 = PolyBase no está instalada.<br /><br /> 1 = PolyBase está instalada.<br /><br /> Tipo de datos base: **int**|  
|IsSingleUser|El servidor está en modo de usuario único.<br /><br /> 1 = Usuario único.<br /><br /> 0 = Usuario no único.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|IsTempDbMetadataMemoryOptimized|**Válido para** : [!INCLUDE[ssSQL2019](../../includes/sssql19-md.md)] y versiones posteriores.<br /><br />Devuelve 1 si tempdb se ha habilitado para usar tablas optimizadas para memoria para metadatos y 0 si tempdb usa tablas normales basadas en disco para los metadatos. Para obtener más información, consulte [tempdb Database](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata).<br /><br /> Tipo de datos base: **int**|  
|IsXTPSupported|**Se aplica a**: SQL Server ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores), [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<br /><br /> El servidor admite OLTP en memoria.<br /><br /> 1= El servidor admite OLTP en memoria.<br /><br /> 0= El servidor no admite OLTP en memoria.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|LCID|Identificador de configuración regional (LCID) de Windows de la intercalación.<br /><br /> Tipo de datos base: **int**|  
|LicenseType|Sin usar. El producto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no conserva ni mantiene la información sobre la licencia. Siempre devuelve DISABLED.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|MachineName|Nombre del equipo con Windows en el que se está ejecutando la instancia del servidor.<br /><br /> Para una instancia en clúster, una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en el Servicio de Cluster Server de Microsoft, devuelve el nombre del servidor virtual.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|NumLicenses|Sin usar. El producto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no conserva ni mantiene la información sobre la licencia. Siempre devuelve NULL.<br /><br /> Tipo de datos base: **int**|  
|ProcessID|Identificador de proceso del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. ProcessID es útil para identificar a qué Sqlservr.exe pertenece esta instancia.<br /><br /> NULL = La entrada no es válida, es un error o no es aplicable.<br /><br /> Tipo de datos base: **int**|  
|ProductBuild|**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a partir de octubre de 2015.<br /><br /> Número de compilación.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ProductBuildType|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta la versión actual en actualizaciones a partir de finales de 2015.<br /><br /> Tipo de compilación de la compilación actual.<br /><br /> Devuelve una de las siguientes opciones:<br /><br /> OD = versión a petición de un cliente específico.<br /><br /> GDR = versión de distribución general publicada en Windows Update.<br /><br /> NULL = no aplicable.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ProductLevel|Nivel de la versión de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Devuelve una de las siguientes opciones:<br /><br /> 'RTM' = Versión comercial original<br /><br /> 'SP *n*' = versión de Service Pack<br /><br /> 'CTP *n*', = versión de Community Technology Preview<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ProductMajorVersion|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta la versión actual en actualizaciones a partir de finales de 2015.<br /><br /> Versión principal.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ProductMinorVersion|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta la versión actual en actualizaciones a partir de finales de 2015.<br /><br /> Versión secundaria.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ProductUpdateLevel|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta la versión actual en actualizaciones a partir de finales de 2015.<br /><br /> Nivel de actualización de la compilación actual. CU indica una actualización acumulativa.<br /><br /> Devuelve una de las siguientes opciones:<br /><br /> CU *n* = Actualización acumulativa<br /><br /> NULL = no aplicable.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ProductUpdateReference|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta la versión actual en actualizaciones a partir de finales de 2015.<br /><br /> Artículo de Knowledge Base para esa versión.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ProductVersion|Versión de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el formato '*major.minor.build.revision*'.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ResourceLastUpdateDateTime|Devuelve la fecha y hora de la última actualización de la base de datos de recursos.<br /><br /> Tipo de datos base: **datetime**|  
|ResourceVersion|Devuelve la versión de la base de datos de recursos.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|nombreDeServidor|La información del servidor Windows y de la instancia asociada con una instancia especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = La entrada no es válida o es un error.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|SqlCharSet|Id. del juego de caracteres de SQL a partir del Id. de intercalación.<br /><br /> Tipo de datos base: **tinyint**|  
|SqlCharSetName|Juego de caracteres de SQL a partir de la intercalación.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|SqlSortOrder|Id. de criterio de ordenación de SQL a partir de la intercalación.<br /><br /> Tipo de datos base: **tinyint**|  
|SqlSortOrderName|Nombre de criterio de ordenación de SQL a partir de la intercalación.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
  
## <a name="return-types"></a>Tipos de valor devuelto  

**sql_variant**
  
## <a name="remarks"></a>Observaciones  
  
### <a name="servername-property"></a>Propiedad ServerName

La propiedad `ServerName` de la función `SERVERPROPERTY` y [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) devuelven información parecida. La propiedad `ServerName` proporciona el nombre de la instancia y el servidor Windows que forman la instancia de servidor única. [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) proporciona el nombre del servidor local configurado actualmente.  
  
La propiedad `ServerName` y [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) devuelven la misma información si no se cambió el nombre del servidor predeterminado durante la instalación. El nombre del servidor local se puede configurar ejecutando lo siguiente:  
  
```sql
EXEC sp_dropserver 'current_server_name';  
GO  
EXEC sp_addserver 'new_server_name', 'local';  
GO  
```  
  
Si durante la instalación se seleccionó un nombre para el servidor local distinto del predeterminado, [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) devuelve el nombre nuevo.  
  
### <a name="version-properties"></a>Propiedades de la versión

La función `SERVERPROPERTY` devuelve propiedades sueltas relacionadas con la información de la versión, mientras que la función [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md) combina la salida en una cadena. Si la aplicación requiere cadenas con las propiedades por separado, puede usar la función `SERVERPROPERTY` para devolverlas en lugar de analizar los resultados de [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md).  

## <a name="permissions"></a>Permisos

Todos los usuarios pueden consultar las propiedades del servidor.
  
## <a name="examples"></a>Ejemplos

En este ejemplo se usa la función `SERVERPROPERTY` en una instrucción `SELECT` para devolver información sobre la instancia actual de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
  
```sql
SELECT  
  SERVERPROPERTY('MachineName') AS ComputerName,
  SERVERPROPERTY('ServerName') AS InstanceName,  
  SERVERPROPERTY('Edition') AS Edition,
  SERVERPROPERTY('ProductVersion') AS ProductVersion,  
  SERVERPROPERTY('ProductLevel') AS ProductLevel;  
GO  
```  
  
## <a name="see-also"></a>Consulte también

[Ediciones y componentes de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)  

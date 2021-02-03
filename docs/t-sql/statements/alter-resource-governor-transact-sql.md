---
description: ALTER RESOURCE GOVERNOR (Transact-SQL)
title: ALTER RESOURCE GOVERNOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER RESOURCE GOVERNOR
- ALTER_RESOURCE_GOVERNOR_TSQL
- ALTER RESOURCE GOVERNOR RECONFIGURE
- ALTER_RESOURCE_GOVERNOR_RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE GOVERNOR
- RECONFIGURE, ALTER RESOURCE GOVERNOR
ms.assetid: 442c54bf-a0a6-4108-ad20-db910ffa6e3c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cd2d0ecbf0dde509c57afb8991b4cd2676eaf82b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204069"
---
# <a name="alter-resource-governor-transact-sql"></a>ALTER RESOURCE GOVERNOR (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esta instrucción se usa para realizar las siguientes acciones de Resource Governor en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   Aplicar los cambios de configuración especificados al emitir instrucciones CREATE|ALTER|DROP WORKLOAD GROUP o CREATE|ALTER|DROP RESOURCE POOL o CREATE|ALTER|DROP EXTERNAL RESOURCE POOL.  
  
-   Habilitar o deshabilitar el regulador de recursos.  
  
-   Configurar la clasificación de las solicitudes entrantes.  
  
-   Restablece las estadísticas de los grupos de cargas de trabajo y los grupos de recursos.  
  
-   Establece la cantidad máxima de operaciones de E/S por volumen de disco.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER RESOURCE GOVERNOR   
      { DISABLE | RECONFIGURE }  
    | WITH ( CLASSIFIER_FUNCTION = { schema_name.function_name | NULL } )  
    | RESET STATISTICS  
    | WITH ( MAX_OUTSTANDING_IO_PER_VOLUME = value )   
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 DISABLE  
 Deshabilita el regulador de recursos. La deshabilitación de Resource Governor tiene las siguientes consecuencias:  
  
-   No se ejecuta la función clasificadora.  
  
-   Todas las conexiones nuevas se clasifican automáticamente en el grupo predeterminado.  
  
-   Las solicitudes iniciadas por el sistema se clasifican en el grupo de cargas de trabajo interno.  
  
-   Se restablecen los valores predeterminados de todas las configuraciones existentes del grupo de cargas de trabajo y el grupo de recursos de servidor. En este caso, no se desencadena ningún evento cuando se alcanzan los límites.  
  
-   La supervisión normal del sistema no resulta afectada.  
  
-   Se puede cambiar la configuración, pero los cambios no surten efecto hasta que se habilita Resource Governor.  
  
-   Al reiniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el regulador de recursos no cargará su configuración, sino que tendrá únicamente los grupos de cargas de trabajo y los grupos de recursos de servidor predeterminados e internos.  
  
 RECONFIGURE  
 Si el regulador de recursos no está habilitado, RECONFIGURE lo habilita. La habilitación de Resource Governor tiene las siguientes consecuencias:  
  
-   La función clasificadora se ejecuta para las nuevas conexiones, de forma que se puede asignar su carga de trabajo a los grupos de cargas de trabajo.  
  
-   Se respetan y se aplican los límites de los recursos especificados en la configuración del regulador de recursos.  
  
-   Las solicitudes existentes antes de la habilitación de Resource Governor se ven afectadas por cualquier cambio realizado en la configuración en el momento de deshabilitar Resource Governor.  
  
 Mientras se ejecuta Resource Governor, RECONFIGURE aplica los cambios de configuración solicitados al ejecutar las instrucciones CREATE|ALTER|DROP WORKLOAD GROUP o CREATE|ALTER|DROP RESOURCE POOL o CREATE|ALTER|DROP EXTERNAL RESOURCE POOL.  
  
> [!IMPORTANT]  
>  Debe enviarse ALTER RESOURCE GOVERNOR RECONFIGURE para que surtan efecto los cambios en la configuración.  
  
 CLASSIFIER_FUNCTION = { _schema_name_ **.** _function_name_ | NULL }  
 Registra la función clasificadora especificada por *schema_name.function_name*. Esta función clasifica cada sesión nueva y asigna las solicitudes de sesión y las consultas para un grupo de cargas de trabajo. Si se usa NULL, las nuevas sesiones se asignan automáticamente al grupo de cargas de trabajo predeterminado.  
  
 RESET STATISTICS  
 Restablece las estadísticas en todos los grupos de cargas de trabajo y los grupos de recursos. Para obtener más información, vea [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md) y [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
 MAX_OUTSTANDING_IO_PER_VOLUME = *value*  
 **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
 Establece la cantidad máxima de operaciones de E/S en cola por volumen de disco. Estas operaciones de E/S pueden ser lecturas o escrituras de cualquier tamaño.  El valor máximo de MAX_OUTSTANDING_IO_PER_VOLUME es 100. No es un porcentaje. Este valor está diseñado para optimizar la gobernanza de recursos de E/S según las características de E/S de un volumen de disco. Se recomienda experimentar con distintos valores y plantearse el uso de una herramienta de calibración como IOMeter, [DiskSpd](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) o SQLIO (en desuso) para identificar el valor máximo del subsistema de almacenamiento. Este valor proporciona una comprobación de seguridad de nivel de sistema que permite a SQL Server cumplir el mínimo de IOPS para los grupos de recursos aunque otros grupos tengan el valor de MAX_IOPS_PER_VOLUME establecido en ilimitado. Para obtener más información sobre MAX_IOPS_PER_VOLUME, vea [CREATE RESOURCE POOL](../../t-sql/statements/create-resource-pool-transact-sql.md).  
  
## <a name="remarks"></a>Comentarios  
 ALTER RESOURCE GOVERNOR DISABLE, ALTER RESOURCE GOVENOR RECONFIGURE y ALTER RESOURCE GOVERNOR RESET STATISTICS no se pueden usar en una transacción de usuario.  
  
 El parámetro RECONFIGURE forma parte de la sintaxis de Resource Governor y no debe confundirse con [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md), que es una instrucción DDL independiente.  
  
 Se recomienda familiarizarse con los estados del regulador de recursos antes de ejecutar las instrucciones de DDL. Para obtener más información, vea [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-starting-the-resource-governor"></a>A. Iniciar el regulador de recursos  
 La primera vez que se instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el regulador de recursos está deshabilitado. En el ejemplo siguiente, se inicia regulador de recursos. Después de ejecutar la instrucción, el regulador de recursos se ejecuta y puede usar los grupos de recursos de servidor y los grupos de cargas de trabajo predefinidos.  
  
```sql  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="b-assigning-new-sessions-to-the-default-group"></a>B. Asignar nuevas sesiones al grupo predeterminado  
 En el ejemplo siguiente, se asignan todas las sesiones nuevas al grupo de cargas de trabajo predeterminado; para ello, se quitan las funciones clasificadoras existentes de la configuración del regulador de recursos. Cuando no se designa ninguna función como función clasificadora, todas las sesiones nuevas se asignan al grupo de cargas de trabajo predeterminado. Este cambio solo se aplica a las sesiones nuevas. Las sesiones existentes no resultan afectadas.  
  
```sql  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION = NULL);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="c-creating-and-registering-a-classifier-function"></a>C. Crear y registrar una función clasificadora  
 En el ejemplo siguiente, se crea una función clasificadora denominada `dbo.rgclassifier_v1`. La función clasifica cada nueva sesión basándose en el nombre de usuario o en el nombre de aplicación y asigna las solicitudes de sesión y las consultas a un grupo de cargas de trabajo concreto. Las sesiones que no se corresponden con los nombres de usuario o de aplicación especificados se asignan al grupo de cargas de trabajo predeterminado. A continuación, se registra la función clasificadora y se aplica el cambio de configuración.  
  
```sql  
-- Store the classifier function in the master database.  
USE master;  
GO  
SET ANSI_NULLS ON;  
GO  
SET QUOTED_IDENTIFIER ON;  
GO  
CREATE FUNCTION dbo.rgclassifier_v1() RETURNS sysname   
WITH SCHEMABINDING  
AS  
BEGIN  
-- Declare the variable to hold the value returned in sysname.  
    DECLARE @grp_name AS sysname  
-- If the user login is 'sa', map the connection to the groupAdmin  
-- workload group.   
    IF (SUSER_NAME() = 'sa')  
        SET @grp_name = 'groupAdmin'  
-- Use application information to map the connection to the groupAdhoc  
-- workload group.  
    ELSE IF (APP_NAME() LIKE '%MANAGEMENT STUDIO%')  
        OR (APP_NAME() LIKE '%QUERY ANALYZER%')  
            SET @grp_name = 'groupAdhoc'  
-- If the application is for reporting, map the connection to  
-- the groupReports workload group.  
    ELSE IF (APP_NAME() LIKE '%REPORT SERVER%')  
        SET @grp_name = 'groupReports'  
-- If the connection does not map to any of the previous groups,  
-- put the connection into the default workload group.  
    ELSE  
        SET @grp_name = 'default'  
    RETURN @grp_name  
END;  
GO  
-- Register the classifier user-defined function and update the   
-- the in-memory configuration.  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION=dbo.rgclassifier_v1);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="d-resetting-statistics"></a>D. Restablecer las estadísticas  
 En el ejemplo siguiente, se restablecen las estadísticas de todos los grupos de cargas de trabajo y los grupos de recursos.  
  
```sql 
ALTER RESOURCE GOVERNOR RESET STATISTICS;  
```  
  
### <a name="e-setting-the-max_outstanding_io_per_volume-option"></a>E. Establecer la opción MAX_OUTSTANDING_IO_PER_VOLUME  
 En el ejemplo siguiente se establece la opción MAX_OUTSTANDING_IO_PER_VOLUME en 20.  
  
```sql  
ALTER RESOURCE GOVERNOR  
WITH (MAX_OUTSTANDING_IO_PER_VOLUME = 20);   
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)  
  
  

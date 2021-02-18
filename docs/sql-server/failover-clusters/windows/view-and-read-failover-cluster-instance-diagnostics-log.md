---
title: Visualización y lectura del registro de diagnósticos de la instancia de clúster de conmutación por error
description: Obtenga información sobre cómo ver y leer el registro de diagnóstico en ejecución generado por una instancia de clúster de conmutación por error de SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 68074bd5-be9d-4487-a320-5b51ef8e2b2d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0957ffc760046bfd9e3e10d018f373d94950080a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346162"
---
# <a name="view-and-read-failover-cluster-instance-diagnostics-log"></a>Ver y leer el registro de diagnósticos de la instancia de clúster de conmutación por error
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Todos los errores críticos y eventos de advertencia de la DLL de recursos de SQL Server se escriben en el registro de eventos de Windows. El procedimiento almacenado del sistema [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) captura un registro en ejecución de la información de diagnóstico específica de SQL Server y lo escribe en los archivos de registro de diagnósticos de clústeres de conmutación por error de SQL Server (también conocidos como registros *SQLDIAG*).  
  
-   **Antes de empezar:**  [Recomendaciones](#Recommendations), [Seguridad](#Security)  
  
-   **Para ver el registro de diagnósticos mediante:**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
-   **Para configurar el registro de diagnósticos mediante:** [Transact-SQL](#TsqlConfigure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
 De forma predeterminada, las instancias de SQLDIAG se almacenan en una carpeta LOG local del directorio de la instancia de SQL Server, por ejemplo, "C\Archivos de programa\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\LOG" del nodo propietario de la instancia de clúster de conmutación por error (FCI) Always On. El tamaño de cada archivo de registro de SQLDIAG se fija en 100 MB. Diez archivos de registro se almacenan en el equipo antes de que se reciclen para los nuevos registros.  
  
 Los registros usan el formato de archivo extendido de eventos. La función del sistema **sys.fn_xe_file_target_read_file** se puede usar para leer los archivos creados por eventos extendidos. Se devuelve un evento, en formato XML, por cada fila. Consulte la vista del sistema para analizar los datos XML como conjunto de resultados. Para obtener más información, vea [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 El permiso VIEW SERVER STATE es necesario para ejecutar **fn_xe_file_target_read_file**.  
  
 Abra SQL Server Management Studio como administrador.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para ver los archivos de registro de diagnóstico:**  
  
1.  En el menú **Archivo** , seleccione **Abrir**, **Archivo** y elija el archivo de registro de diagnóstico que desea ver.  
  
2.  Los eventos aparecen como filas en el panel derecho y, de forma predeterminada, las dos únicas columnas mostradas son **name** y **timestamp** .  
  
     De este modo también se activa el menú **ExtendedEvents** .  
  
3.  Para ver más columnas, vaya el menú **ExtendedEvents** y seleccione **Elegir columnas**.  
  
     Se abre un cuadro de diálogo con columnas disponibles que permiten seleccionar las columnas de la presentación.  
  
4.  Puede filtrar y ordenar los datos de evento mediante el menú **ExtendedEvents** y la selección de la opción **Filtrar** .  
  
##  <a name="view-diagnostic-log-files-with-transact-sql"></a><a name="TsqlProcedure"></a> Vista de archivos de registro de diagnóstico con Transact-SQL  
 **Para ver los archivos de registro de diagnóstico:**  
  
 Para ver todos los elementos en el archivo de registro SQLDIAG, use la siguiente consulta:  
  
```  
SELECT  
xml_data.value('(event/@name)[1]','varchar(max)') AS 'Name'  
,xml_data.value('(event/@package)[1]','varchar(max)') AS 'Package'  
,xml_data.value('(event/@timestamp)[1]','datetime') AS 'Time'  
,xml_data.value('(event/data[@name=''state'']/value)[1]','int') AS 'State'  
,xml_data.value('(event/data[@name=''state_desc'']/text)[1]','varchar(max)') AS 'State Description'  
,xml_data.value('(event/data[@name=''failure_condition_level'']/value)[1]','int') AS 'Failure Conditions'  
,xml_data.value('(event/data[@name=''node_name'']/value)[1]','varchar(max)') AS 'Node_Name'  
,xml_data.value('(event/data[@name=''instancename'']/value)[1]','varchar(max)') AS 'Instance Name'  
,xml_data.value('(event/data[@name=''creation time'']/value)[1]','datetime') AS 'Creation Time'  
,xml_data.value('(event/data[@name=''component'']/value)[1]','varchar(max)') AS 'Component'  
,xml_data.value('(event/data[@name=''data'']/value)[1]','varchar(max)') AS 'Data'  
,xml_data.value('(event/data[@name=''info'']/value)[1]','varchar(max)') AS 'Info'  
FROM  
 ( SELECT object_name AS 'event'  
  ,CONVERT(xml,event_data) AS 'xml_data'  
  FROM sys.fn_xe_file_target_read_file('C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log\SQLNODE1_MSSQLSERVER_SQLDIAG_0_129936003752530000.xel',NULL,NULL,NULL)   
)   
AS XEventData  
ORDER BY Time;  
  
```  
  
> [!NOTE]  
>  Puede filtrar los resultados para los componentes específicos o con estado la cláusula WHERE.  
  
##  <a name="configure-diagnostic-log-properties-with-transact-sql"></a><a name="TsqlConfigure"></a> Configuración de propiedades del registro de diagnóstico con Transact-SQL  
 **Para configurar las propiedades del registro de diagnóstico:**  
  
> [!NOTE]  
>  Para ver un ejemplo de este procedimiento, vea [Ejemplo (Transact-SQL)](#TsqlExample)más adelante en esta sección.  
  
 Con la instrucción de lenguaje de definición de datos (DDL) **ALTER SERVER CONFIGURATION**, puede iniciar o detener el registro de los datos de diagnóstico capturados por el procedimiento [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) y establecer parámetros de configuración del registro de SQLDIAG como el recuento de sustitución incremental del archivo de registro, el tamaño del archivo de registro y la ubicación del archivo. Para obtener información detallada sobre la sintaxis, vea [Setting diagnostic log options](../../../t-sql/statements/alter-server-configuration-transact-sql.md#Diagnostic).  
  
###  <a name="examples-transact-sql"></a><a name="ConfigTsqlExample"></a> Ejemplos (Transact-SQL)  
  
####  <a name="setting-diagnostic-log-options"></a><a name="TsqlExample"></a> Setting diagnostic log options  
 En los ejemplos de esta sección se muestra cómo establecer los valores para la opción de registro de diagnóstico.  
  
##### <a name="a-starting-diagnostic-logging"></a>A. Iniciar el registro de diagnóstico  
 En el ejemplo siguiente se inicia el registro de los datos de diagnóstico.  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
##### <a name="b-stopping-diagnostic-logging"></a>B. Detener el registro de diagnóstico  
 En el ejemplo siguiente se detiene el registro de los datos de diagnóstico.  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
##### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. Especificar la ubicación de los registros de diagnóstico  
 En el ejemplo siguiente se establece la ubicación de los registros de diagnóstico en la ruta de acceso de archivo especificada.  
  
```  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
##### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D. Especificar el tamaño máximo de cada registro de diagnóstico  
 En el ejemplo siguiente se establece el tamaño máximo de cada registro de diagnóstico en 10 megabytes.  
  
```  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
  

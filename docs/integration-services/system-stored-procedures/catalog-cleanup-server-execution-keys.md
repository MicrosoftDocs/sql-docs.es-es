---
description: catalog.cleanup_server_execution_keys
title: catalog.cleanup_server_execution_keys | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: a79f1006-54e8-4cbf-96f8-5ed143ebb830
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 02872a9142ecb9b010846925f87d9b852a4d4d0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340423"
---
# <a name="catalogcleanup_server_execution_keys"></a>catalog.cleanup_server_execution_keys 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita certificados y claves simétricas de la base de datos de SSISDB.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql
catalog.cleanup_server_execution_keys [ @cleanup_flag = ] cleanup_flag ,  
[ @delete_batch_size = ] delete_batch_size  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @cleanup_flag = ] *cleanup_flag*  
 Indica si los certificados de nivel de ejecución (1) o de nivel de proyecto (2) y las claves simétricas deben quitarse.  
  
 Use el nivel de ejecución (1) solo cuando SERVER_OPERATION_ENCRYPTION_LEVEL esté establecido en PER_EXECUTION (1).  
  
 Use el nivel de proyecto (2) solo cuando SERVER_OPERATION_ENCRYPTION_LEVEL esté establecido en PER_EXECUTION (2). Los certificados y las claves simétricas se quitan solo para los proyectos que se han eliminado y cuyos registros de operaciones se han limpiado.  
  
 [ @delete_batch_size = ] *delete_batch_size*  
 El número de claves y certificados que se van a quitar. El valor predeterminado es 1000.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 si es correcto y 1 si es erróneo.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Ninguno.  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos de lectura y ejecución en el proyecto y, si es aplicable, permisos de lectura en el entorno al que se hace referencia.  
  
-   Pertenencia al rol de base de datos **ssis_admin**.  
  
-   Pertenencia al rol del servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 Este procedimiento almacenado genera errores en los escenarios siguientes:  
  
-   Hay una o más operaciones activas en la base de datos SSISDB.  
  
-   La base de datos SSISDB no está en modo de usuario único.  
  
## <a name="remarks"></a>Observaciones  
 SQL Server 2012 Service Pack 2 agregó la propiedad SERVER_OPERATION_ENCRYPTION_LEVEL a la tabla **internal.catalog_properties**. Esta propiedad tiene dos valores posibles:  
  
-   **PER_EXECUTION (1)**: el certificado y la clave simétrica que se usan para proteger los parámetros de ejecución confidenciales y los registros de ejecución se crean en cada ejecución. Este es el valor predeterminado. Es posible que se encuentre con problemas de rendimiento (interbloqueos, trabajos de mantenimiento con errores, etc.) en un entorno de producción porque los certificados o las claves se generan en cada ejecución. Sin embargo, esta configuración proporciona un mayor nivel de seguridad que el otro valor (2).  
  
-   **PER_PROJECT (2)**: el certificado y la clave simétrica que se usan para proteger parámetros confidenciales se crean en cada proyecto. Ello le proporciona un rendimiento mayor que el que ofrece el nivel PER_EXECUTION, debido a que la clave y el certificado se generan una vez por proyecto, en lugar de crearse para cada ejecución.  
  
 Tiene que ejecutar el procedimiento almacenado [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md) para poder cambiar SERVER_OPERATION_ENCRYPTION_LEVEL de 1 a 2 o de 2 a 1. Debe hacer lo siguiente antes de ejecutar este procedimiento almacenado:  
  
1.  Asegúrese de que el valor de la propiedad OPERATION_CLEANUP_ENABLED esté establecido en TRUE en la tabla [catalog.catalog_properties &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
2.  Establezca la base de datos de Integration Services (SSISDB) en modo de usuario único. En SQL Server Management Studio, inicie el cuadro de diálogo Propiedades de la base de datos para SSISDB, vaya a la pestaña Opciones y establezca la propiedad Restringir acceso en modo de usuario único (SINGLE_USER). Después de ejecutar el procedimiento almacenado cleanup_server_log, vuelva a establecer el valor de propiedad original.  
  
3.  Ejecute el procedimiento almacenado [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md).  
  
4.  A continuación, cambie el valor de la propiedad SERVER_OPERATION_ENCRYPTION_LEVEL en la tabla [catalog.catalog_properties &#40;base de datos de SSISDB &#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
5.  Ejecute el procedimiento almacenado [catalog.cleanup_server_execution_keys](../../integration-services/system-stored-procedures/catalog-cleanup-server-execution-keys.md) para borrar las claves de certificados de la base de datos SSISDB. Quitar certificados y claves de la base de datos SSISDB puede llevar bastante tiempo, por lo que debe realizar este proceso periódicamente durante horas de poca actividad.  
  
     Puede especificar el ámbito o el nivel (ejecución o proyecto) y el número de claves que va a eliminar. El tamaño predeterminado del lote para la eliminación es 1000. Cuando el nivel se establece en 2, solo se eliminan las claves y certificados si se han eliminado los proyectos asociados.  
  
 Para obtener más información, vea el artículo de Knowledge Base siguiente. [FIX: Performance issues when you use SSISDB as your deployment store in SQL Server 2012](https://support.microsoft.com/kb/2972285) (REVISIÓN: Problemas de rendimiento cuando usa SSISDB como almacén de implementación en SQL Server 2012)  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se llama al procedimiento almacenado cleanup_server_execution_keys.  
  
```sql  
USE [SSISDB]  
GO  
  
DECLARE@return_value int  
  
EXEC@return_value = [internal].[cleanup_server_execution_keys]  
@cleanup_flag = 1,  
@delete_batch_size = 500  
  
SELECT'Return Value' = @return_value  
  
GO  
```  
  
  

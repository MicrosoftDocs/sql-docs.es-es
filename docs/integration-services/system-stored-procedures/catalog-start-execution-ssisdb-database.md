---
description: catalog.start_execution (base de datos de SSISDB)
title: catalog.start_execution (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: f8663ff3-aa98-4dd8-b850-b21efada0b87
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e98650614583eab2b40e6433721a7cf2d73b1a6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342172"
---
# <a name="catalogstart_execution-ssisdb-database"></a>catalog.start_execution (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inicia una instancia de ejecución en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.start_execution [ @execution_id = ] execution_id [, [ @retry_count = ] retry_count]  
```  
  
## <a name="arguments"></a>Argumentos  
 [@execution_id =] *execution_id*  
 Identificador único de la instancia de ejecución. El parámetro *execution_id* es de tipo **bigint**.
 
 [@retry_count =] *retry_count*  
 Es el número de reintentos si se produce un error en la ejecución. Solo tendrá efecto si la ejecución está en modo de escalabilidad horizontal. Este parámetro es opcional. Si no se especifica, su valor se establece en 0. El parámetro *retry_count* es **int**.
  
## <a name="remarks"></a>Observaciones  
 Una ejecución se usa para especificar los valores de parámetro que va a usar un paquete durante una instancia única de ejecución del paquete. Puede ocurrir que, después de crear una instancia de ejecución y antes de que se inicie, el proyecto correspondiente se implemente de nuevo. En este caso, la instancia de ejecución hará referencia a un proyecto obsoleto. Esta referencia no válida hace que el procedimiento almacenado genere un error.  
  
> [!NOTE]  
>  Las ejecuciones solo pueden iniciarse una vez. Para iniciar una instancia de ejecución, debe tener el estado creado (el valor de `1` en la columna de **estado** de la vista [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md)).  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se llama a catalog.create_execution para crear una instancia de ejecución para el paquete Child1.dtsx. Project1 de Integration Services contiene el paquete. En el ejemplo se llama a catalog.set_execution_parameter_value para establecer valores para los parámetros Parameter1, Parameter2 y LOGGING_LEVEL. En el ejemplo se llama a catalog.start_execution para iniciar una instancia de ejecución.  
  
```sql
Declare @execution_id bigint  
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Child1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'TestDeply4', @project_name=N'Integration Services Project1', @use32bitruntime=False, @reference_id=Null  
Select @execution_id  
DECLARE @var0 sql_variant = N'Child1.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter1', @parameter_value=@var0  
DECLARE @var1 sql_variant = N'Child2.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter2', @parameter_value=@var1  
DECLARE @var2 smallint = 1  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var2  
EXEC [SSISDB].[catalog].[start_execution] @execution_id  
GO  
```  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Los permisos READ y MODIFY de la instancia de ejecución, los permisos READ y EXECUTE del proyecto y, si procede, los permisos READ del entorno al que se hace referencia  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El usuario no tiene los permisos adecuados.  
  
-   El identificador de ejecución no es válido  
  
-   La ejecución se ha iniciado previamente o se ha completado ya; las ejecuciones pueden iniciarse una sola vez  
  
-   La referencia de entorno asociado al proyecto no es válida  
  
-   No se han establecido los valores de parámetro necesarios  
  
-   La versión del proyecto asociada a la instancia de ejecución está obsoleta; solo se puede ejecutar la versión más actual de un proyecto  
  
  

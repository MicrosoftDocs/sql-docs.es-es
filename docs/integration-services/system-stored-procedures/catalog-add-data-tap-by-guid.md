---
description: catalog.add_data_tap_by_guid
title: catalog.add_data_tap_by_guid | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ed9d7fa3-61a1-4e21-ba43-1ead7dfc74eb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 969f6975d2f73cfea80582694a3b0370a00f7264
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354909"
---
# <a name="catalogadd_data_tap_by_guid"></a>catalog.add_data_tap_by_guid 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega una derivación de datos a una ruta de acceso de flujo de datos específica de un flujo de datos de paquete, para una instancia de la ejecución.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.add_data_tap_by_guid [ @execution_id = ] execution_id  
, [ @dataflow_task_guid = ] dataflow_task_guid   
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @execution_id = ] *execution_id*  
 Identificador de ejecución correspondiente a la ejecución que contiene el paquete. El parámetro *execution_id* es de tipo **bigint**.  
  
 [ @dataflow_task_guid = ] *dataflow_task_guid*  
 El identificador del flujo de tarea de datos del paquete que contiene la ruta de flujo de datos que se va a derivar. El parámetro *dataflow_task_guid* es de tipo **uniqueidentifier**.  
  
 [ @dataflow_path_id_string = ] *dataflow_path_id_string*  
 Cadena de identificación de la ruta de flujo de datos. Una ruta de acceso conecta dos componentes de flujo de datos. La propiedad **IdentificationString** de la ruta de acceso especifica la cadena.  
  
 Para buscar la cadena de identificación, en [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], haga clic con el botón derecho en la ruta de acceso entre dos componentes de flujo de datos y, a continuación, haga clic en **Propiedades**. La propiedad **IdentificationString** aparece en la ventana **Propiedades**.  
  
 El parámetro *dataflow_path_id_string* es de tipo **nvarchar(4000)** .  
  
 [ @data_filename = ] *data_filename*  
 Nombre del archivo que almacena los datos derivados. Si la tarea Flujo de datos se ejecuta en un bucle Foreach o en un contenedor de bucles For, los datos derivados se almacenan en archivos diferentes para cada iteración del bucle. Cada archivo tiene como prefijo un número que corresponde a una iteración. Los archivos de derivación de datos se escriben en la carpeta " *\<SQL Server installation folder>* \130\DTS\\". El parámetro *data_filename* es de tipo **nvarchar(4000)** .  
  
 [ @max_rows = ] max_rows  
 Número de filas que se capturan durante la derivación de datos. Si no se especifica este valor, se capturan todas las filas. El parámetro max_rows es de tipo **int**.  
  
 [ @data_tap_id = ] *data_tap_id*  
 Identificador de la derivación de datos. El parámetro *data_tap_id* es de tipo **bigint**.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente, se crea una derivación de datos en la ruta de flujo de datos, `Paths[SRC DimDCVentor.OLE DB Source Output]`, en la tarea de flujo de datos `{D978A2E4-E05D-4374-9B05-50178A8817E8}`. Los datos derivados se almacenan en el archivo DCVendorOutput.csv.  
  
```sql
exec catalog.add_data_tap_by_guid   @execution_id,   
'{D978A2E4-E05D-4374-9B05-50178A8817E8}',   
'Paths[SRC DimDCVentor.OLE DB Source Output]',   
'D:\demos\datafiles\DCVendorOutput.csv'  
```  
  
## <a name="remarks"></a>Observaciones  
 Para agregar derivaciones de datos, la instancia de la ejecución debe estar en el estado creado (un valor 1 en la columna **status** de la vista [catalog.operations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)). El valor de estado cambia cuando se ejecuta la ejecución. Puede crear una ejecución mediante una llamada a [catalog.create_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
 A continuación se indican algunas consideraciones sobre el procedimiento almacenado add_data_tap_by_guid.  
  
-   Al agregar una derivación de datos, esta no se valida antes de que se ejecute el paquete.  
  
-   Se recomienda que limite el número de filas que se capturan durante la derivación de datos para evitar que se generen archivos de datos grandes. Si el equipo en el que se ejecuta el procedimiento almacenado se queda sin espacio para almacenar los archivos de datos, se detiene la ejecución del procedimiento almacenado.  
  
-   La ejecución del procedimiento almacenado add_data_tap_by_guid afecta al rendimiento del paquete. Se recomienda que ejecute el procedimiento almacenado solo para solucionar problemas de datos.  
  
-   Para tener acceso al archivo que almacena los datos derivados, debe tener permisos de administrador en el equipo en el que se ejecuta el procedimiento almacenado o debe ser el usuario que inició la ejecución que contiene el paquete con la derivación de datos.  
  
## <a name="return-codes"></a>Códigos de retorno  
 0 (correcto)  
  
 Cuando se produce un error en el procedimiento almacenado, se genera un error.  
  
## <a name="result-set"></a>Tipo de cursor  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos MODIFY en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la lista siguiente se describen las condiciones que hacen que el procedimiento almacenado genere un error.  
  
-   El usuario no tiene permisos MODIFY.  
  
-   La derivación de datos para el componente especificado, en el paquete indicado, ya se ha agregado.  
  
-   El valor especificado para el número de filas para capturar no es válido.  
  
## <a name="requirements"></a>Requisitos  
  
## <a name="see-also"></a>Consulte también  
 [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md)  
  
  

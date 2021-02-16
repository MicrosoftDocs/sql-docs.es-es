---
description: catalog.add_data_tap
title: catalog.add_data_tap | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a25ebcc7-535e-4619-adf6-4e2b5a62ba37
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8d188b361b32627ffac604a9b69e14f54fa26355
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339203"
---
# <a name="catalogadd_data_tap"></a>catalog.add_data_tap 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega una derivación de datos en el resultado de un componente de un flujo de datos de paquete, para una instancia de la ejecución.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.add_data_tap [ @execution_id = ] execution_id  
, [ @task_package_path = ] task_package_path  
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id OUTPUT  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @execution_id = ] *execution_id*  
 Identificador de ejecución correspondiente a la ejecución que contiene el paquete. El parámetro *execution_id* es de tipo **bigint**.  
  
 [ @task_package_path = ] *task_package_path*  
 Ruta de acceso del paquete para la tarea Flujo de datos. La propiedad **PackagePath** para la tarea de flujo de datos especifica la ruta de acceso. La ruta de acceso distingue mayúsculas de minúsculas. Para buscar la ruta de acceso del paquete, en [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], haga clic con el botón derecho en la tarea Flujo de datos y, a continuación, haga clic en **Propiedades**. La propiedad **PackagePath** aparece en la ventana **Propiedades**.  
  
 El parámetro *task_package_path* es de tipo **nvarchar(max)** .  
  
 [ @dataflow_path_id_string = ] *dataflow_path_id_string*  
 Cadena de identificación de la ruta de flujo de datos. Una ruta de acceso conecta dos componentes de flujo de datos. La propiedad **IdentificationString** de la ruta de acceso especifica la cadena.  
  
 Para buscar la cadena de identificación, en [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], haga clic con el botón derecho en la ruta de acceso entre dos componentes de flujo de datos y, a continuación, haga clic en **Propiedades**. La propiedad **IdentificationString** aparece en la ventana **Propiedades**.  
  
 El parámetro *dataflow_path_id_string* es de tipo **nvarchar(4000)** .  
  
 [ @data_filename = ] *data_filename*  
 Nombre del archivo que almacena los datos derivados. Si la tarea Flujo de datos se ejecuta en un bucle Foreach o en un contenedor de bucles For, los datos derivados se almacenan en archivos diferentes para cada iteración del bucle. Cada archivo tiene como prefijo un número que corresponde a una iteración.  
  
 De forma predeterminada, el archivo se almacena en la carpeta \<*drive*>:\Archivos de programa\Microsoft SQL Server\130\DTS\DataDumps.  
  
 El parámetro *data_filename* es de tipo **nvarchar(4000)** .  
  
 [ @max_rows = ] *max_rows*  
 Número de filas que se capturan durante la derivación de datos. Si no se especifica este valor, se capturan todas las filas. El parámetro *max_rows* es de tipo **int**.  
  
 [ @data_tap_id = ] *data_tap_id*  
 Devuelve el identificador de la derivación de datos. El parámetro *data_tap_id* es de tipo **bigint**.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente, se crea una derivación de datos en la ruta de flujo de datos, `'Paths[OLE DB Source.OLE DB Source Output]`, en la tarea de flujo de datos, `\Package\Data Flow Task`. Los datos derivados se almacenan en el archivo `output0.txt`, en la carpeta DataDumps (\<*drive*>:\Archivos de programa\Microsoft SQL Server\130\DTS\DataDumps).  
  
```sql
Declare @execution_id bigint  
Exec SSISDB.Catalog.create_execution @folder_name='Packages',@project_name='SSISPackages', @package_name='Package.dtsx',@reference_id=Null, @use32bitruntime=False, @execution_id=@execution_id OUTPUT  
  
Exec SSISDB.Catalog.set_execution_parameter_value @execution_id,50, 'LOGGING_LEVEL', 0  
  
Exec SSISDB.Catalog.add_data_tap @execution_id, @task_package_path='\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[OLE DB Source.OLE DB Source Output]', @data_filename = 'output0.txt'  
  
Exec SSISDB.Catalog.start_execution @execution_id  
```  
  
## <a name="remarks"></a>Observaciones  
 Para agregar derivaciones de datos, la instancia de la ejecución debe estar en el estado creado (un valor 1 en la columna **status** de la vista [catalog.operations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)). El valor de estado cambia cuando se ejecuta la ejecución. Puede crear una ejecución mediante una llamada a [catalog.create_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
 A continuación se indican algunas consideraciones sobre el procedimiento almacenado add_data_tap.  
  
-   Si una ejecución contiene un paquete primario y uno o varios paquetes secundarios, necesita agregar una derivación de datos para cada paquete para el que desee derivar datos.  
  
-   Si un paquete contiene más de una tarea Flujo de datos con el mismo nombre, task_package_path identifica de forma única la tarea Flujo de datos que contiene la salida del componente derivado.  
  
-   Al agregar una derivación de datos, esta no se valida antes de que se ejecute el paquete.  
  
-   Se recomienda que limite el número de filas que se capturan durante la derivación de datos para evitar que se generen archivos de datos grandes. Si el equipo en el que se ejecuta el procedimiento almacenado se queda sin espacio de almacenamiento para los archivos de datos, el paquete deja de ejecutarse y se escribe un mensaje de error en el registro.  
  
-   La ejecución del procedimiento almacenado add_data_tap afecta al rendimiento del paquete. Se recomienda que ejecute el procedimiento almacenado solo para solucionar problemas de datos.  
  
-   Para tener acceso al archivo que almacena los datos derivados, debe ser administrador en el equipo en el que se ejecuta el procedimiento almacenado. También debe ser el usuario que inició la ejecución que contiene el paquete con la derivación de datos.  
  
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
  
## <a name="external-resources"></a>Recursos externos  
 Entrada de blog, [SSIS 2012: A Peek to Data Taps](https://go.microsoft.com/fwlink/?LinkId=239983) (SSIS 2012: información general sobre las derivaciones de datos), en rafael-salas.com.  
  
## <a name="see-also"></a>Consulte también  
 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)  
  
  

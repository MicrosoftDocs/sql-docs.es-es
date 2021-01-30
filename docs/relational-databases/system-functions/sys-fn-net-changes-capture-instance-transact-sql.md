---
description: '&lt;capture_instance sys.fn_net_changes_ &gt; (TRANSACT-SQL)'
title: sys.fn_net_changes_ &lt; capture_instance &gt; (TRANSACT-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_net_changes_TSQL
- fn_net_changes_TSQL
- fn_net_changes
- sys.fn_net_changes
dev_langs:
- TSQL
helpviewer_keywords:
- fn_net_changes_<capture_instance>
- sys.fn_net_changes_<capture_instance>
ms.assetid: 342fa030-9fd9-4b74-ae4d-49f6038a5073
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fdaa7d3a2f741ba480443a997fe561c4aa5c3a50
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201905"
---
# <a name="sysfn_net_changes_ltcapture_instancegt-transact-sql"></a>&lt;capture_instance sys.fn_net_changes_ &gt; (TRANSACT-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contenedores para las funciones de consulta **net Changes** . El procedimiento almacenado sys.sp_cdc_generate_wrapper_function genera los scripts necesarios para crear estas funciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_net_changes_<capture_instance> ('start_time', 'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all with mask  
  | all with merge  
}  
```  
  
## <a name="arguments"></a>Argumentos  
 *start_time*  
 Valor **DateTime** que representa el extremo inferior del intervalo de entradas de la tabla de cambios que se va a incluir en el conjunto de resultados.  
  
 Solo se incluyen en el conjunto de resultados las filas de la tabla de cambios CDC. <capture_instance>_CT que tienen un tiempo de confirmación asociado estrictamente mayor que *start_time* .  
  
 Si se proporciona un valor NULL para este argumento, el extremo inferior del intervalo de consulta se corresponderá con el extremo inferior del intervalo válido de la instancia de captura.  
  
 *end_time*  
 Valor **DateTime** que representa el extremo superior del intervalo de entradas de la tabla de cambios que se va a incluir en el conjunto de resultados.  
  
 Este parámetro puede tomar uno de dos significados, según el valor elegido para @closed_high_end_point cuando se llama a sys.sp_cdc_generate_wrapper_function para generar el script con el fin de crear la función de contenedor:  
  
-   @closed_high_end_point = 1  
  
     Solo se incluyen en el conjunto de resultados las filas de la tabla de cambios CDC. <capture_instance>_CT que tengan un valor en \_ \_ $Start _lsn y un tiempo de confirmación correspondiente menor o igual que **start_time** .  
  
-   @closed_high_end_point = 0  
  
     Solo se incluyen en el conjunto de resultados las filas de la tabla de cambios CDC. <capture_instance>_CT que tengan un valor en \_ \_ $Start _lsn y un tiempo de confirmación correspondiente estrictamente inferior a **start_time** .  
  
 Si se proporciona un valor NULL para este argumento, el extremo superior del intervalo de consulta se corresponderá con el extremo superior del intervalo válido de la instancia de captura.  
  
 *<row_filter_option>* :: = {ALL | ALL with Mask | All with Merge}  
 Una opción que rige el contenido de las columnas de metadatos y las filas devueltas en el conjunto de resultados. Puede ser una de las siguientes opciones:  
  
 all  
 Devuelve el contenido final de una fila cambiada en las columnas de contenido y la operación que se necesita para aplicar la fila en la columna de metadatos __CDC_OPERATION.  
  
 all with mask  
 Devuelve el contenido final de todas las filas cambiadas en las columnas de contenido y la operación necesaria para aplicar cada fila en la columna de metadatos __CDC_OPERATION. Si se especificó una lista de marcas de actualización al generar el script para crear la función contenedora, esta opción es necesaria para rellenar la máscara de actualización.  
  
 todos con combinación  
 Devuelve el contenido final de todas las filas cambiadas en las columnas de contenido.  
  
 La columna __CDC_OPERATION contendrá uno de los dos valores siguientes:  
  
-   D, si la fila se debe eliminar  
  
-   M, si la fila se debe insertar o actualizar.  
  
 La lógica para determinar si se requiere una operación de inserción o actualización para aplicar un cambio al destino agrega complejidad a la consulta. Utilice esta opción para mejorar el rendimiento cuando no sea necesario diferenciar entre operaciones de inserción y actualización. Este enfoque funciona mejor en entornos de destino en los que las operaciones de mezcla están disponibles directamente, por ejemplo en un entorno de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de columna|Descripción|  
|-----------------|-----------------|-----------------|  
|\<columns from @column_list>|**varía**|Las columnas que se identifican en el argumento **column_list** en el sp_cdc_generate_wrapper_function cuando se llama a para generar el script con el fin de crear el contenedor. Si *column_list* es null, todas las columnas de origen sometidas a seguimiento aparecerán en el conjunto de resultados.|  
|__CDC_OPERATION|**nvarchar (2)**|Código de operación que indica la operación necesaria para aplicar la fila al entorno de destino. La operación variará en función del valor del argumento *row_filter_option* que se proporciona en la llamada siguiente:<br /><br /> *row_filter_option* = ' All ', ' All with Mask '<br /><br /> 'D' - operación de eliminación<br /><br /> 'I' - operación de inserción<br /><br /> 'UN' - operación de actualización<br /><br /> *row_filter_option* = ' All with Merge '<br /><br /> 'D' - operación de eliminación<br /><br /> 'M' - operación de inserción o de actualización|  
|\<columns from @update_flag_list>|**bit**|Marca de bits que se denomina anexando _uflag al nombre de columna. La marca solo toma un valor no nulo cuando *row_filter_option* **= ' All with mask '** y \_ _CDC_OPERATION **= ' un '**. Se establece en 1 si la columna correspondiente se modificó dentro de la ventana de consulta. De lo contrario, es 0.|  
  
## <a name="remarks"></a>Observaciones  
 La fn_net_changes_<capture_instance función> actúa como contenedor de la CDC.fn_cdc_get_net_changes_<capture_instance> consulta. El procedimiento almacenado sys.sp_cdc_generate_wrapper se utiliza para crear el script del contenedor.  
  
 Las funciones contenedoras no se crean automáticamente. Para crear funciones contenedoras hay que seguir dos pasos:  
  
1.  Ejecutar el procedimiento almacenado para generar el script que permite crear el contenedor.  
  
2.  Ejecutar el script para crear realmente la función contenedora.  
  
 Las funciones contenedoras permiten a los usuarios consultar de forma sistemática los cambios que se produjeron dentro de un intervalo delimitado por valores de **fecha y hora** en lugar de los valores de LSN. Las funciones de contenedor realizan todas las conversiones necesarias entre los valores **DateTime** proporcionados y los valores de LSN necesarios internamente como argumentos para las funciones de consulta. Cuando las funciones de contenedor se utilizan en serie para procesar un flujo de datos modificados, garantizan que no se pierden ni se repiten datos siempre que se siga la Convención siguiente: el @end_time valor del intervalo asociado a una llamada se proporciona como el @start_time valor para el intervalo asociado a la llamada subsiguiente.  
  
 Si se usa el parámetro @closed_high_end_point al crear el script, se pueden generar contenedores para admitir un límite superior cerrado o un límite superior abierto en la ventana de consulta especificada. Es decir, puede decidir si las entradas que tienen un tiempo de confirmación igual al límite superior del intervalo de extracción se incluirán en el intervalo. De forma predeterminada, se incluye el límite superior.  
  
 El conjunto de resultados devuelto por la función de contenedor **net Changes** devuelve solo las columnas sometidas a seguimiento que estaban en @column_list cuando se generó el contenedor. Si @column_list es NULL, se devuelven todas las columnas de origen sometidas a seguimiento. Las columnas de origen van seguidas de una columna de operación, __CDC_OPERATION, que es una columna de uno o dos caracteres que identifica la operación.  
  
 A continuación, las marcas de bits se anexan al conjunto de resultados para cada columna que se identifica en el parámetro @update_flag_list . En el caso del contenedor de **cambios netos** , los marcadores de bits siempre serán NULL si el @row_filter_option que se usa en la llamada a la función contenedora es ' All ' o ' All with Merge '. Si @row_filter_option se establece en ' All with Mask ' y __CDC_OPERATION es ' d ' o ' I ', el valor de la marca también será null. Si \_ _CDC_OPERATION es ' un ', la marca se establecerá en 1 o 0, dependiendo de si la operación de actualización de **red** provocó un cambio en la columna.  
  
 La plantilla de configuración de captura de datos modificados ' Create CDC wrapper TVF for Schema ' muestra cómo usar el sp_cdc_generate_wrapper_function procedimiento almacenado para obtener scripts de creación para todas las funciones de contenedor para las funciones de consulta definidas de un esquema. A continuación, la plantilla crea esos scripts. Para obtener más información acerca de las plantillas, vea [Explorador de plantillas](../../ssms/template/template-explorer.md).  
  
## <a name="see-also"></a>Consulte también  
 [sys.sp_cdc_generate_wrapper_function &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)  
  
  

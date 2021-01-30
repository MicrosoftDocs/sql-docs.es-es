---
description: '&lt;capture_instance sys.fn_all_changes_ &gt; (TRANSACT-SQL)'
title: sys.fn_all_changes_ &lt; capture_instance &gt; (TRANSACT-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_all_changes
- sys.fn_all_changes
- fn_all_changes_TSQL
- sys.fn_all_changes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_all_changes_<capture_instance>
- sys.fn_all_changes_<capture_instance>
ms.assetid: 564fae96-b88c-4f22-9338-26ec168ba6f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a4736042dc8770660f076d3865df4033ef3233d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194486"
---
# <a name="sysfn_all_changes_ltcapture_instancegt-transact-sql"></a>&lt;capture_instance sys.fn_all_changes_ &gt; (TRANSACT-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contenedores para las funciones de consulta **All Changes** . El procedimiento almacenado sys.sp_cdc_generate_wrapper_function genera los scripts necesarios para crear estas funciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_all_changes_<capture_instance> ('start_time' ,'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all update old  
}  
```  
  
## <a name="arguments"></a>Argumentos  
 *start_time*  
 Valor **DateTime** que representa el extremo inferior del intervalo de entradas de la tabla de cambios que se va a incluir en el conjunto de resultados.  
  
 Solo se incluyen en el conjunto de resultados las filas de la tabla de cambios CDC. <capture_instance>_CT que tienen un tiempo de confirmación asociado superior a *start_time* .  
  
 Si se proporciona un valor NULL para este argumento, el extremo inferior del intervalo de consulta se corresponderá con el extremo inferior del intervalo válido de la instancia de captura.  
  
 *end_time*  
 Valor **DateTime** que representa el extremo superior del intervalo de entradas de la tabla de cambios que se va a incluir en el conjunto de resultados.  
  
 Este parámetro puede tomar uno de dos significados posibles según el valor elegido para @closed_high_end_point cuando se llama a sys.sp_cdc_generate_wrapper_function para generar el script de creación para la función de contenedor:  
  
-   @closed_high_end_point = 1  
  
     Solo se incluyen en el conjunto de resultados las filas de la tabla de cambios CDC. <capture_instance>_CT que tienen un tiempo de confirmación asociado menor o igual que end_time.  
  
-   @closed_high_end_point = 0  
  
     Solo se incluyen en el conjunto de resultados las filas de la tabla de cambios cdc.capture_instance_CT que tienen un tiempo de confirmación asociado estrictamente inferior a end_time.  
  
 Si se proporciona un valor NULL para este argumento, el extremo superior del intervalo de consulta se corresponderá con el extremo superior del intervalo válido de la instancia de captura.  
  
 <row_filter_option>:: = {ALL | ALL Update Old}  
 Opción que rige el contenido de las columnas de metadatos y las filas devueltas en el conjunto de resultados.  
  
 Puede ser una de las siguientes opciones:  
  
 all  
 Devuelve todos los cambios dentro del intervalo LSN especificado. Para los cambios que se producen debido a una operación de actualización, esta opción devuelve solo la fila que contiene los valores nuevos una vez aplicada la actualización.  
  
 all update old  
 Devuelve todos los cambios dentro del intervalo LSN especificado. Para los cambios que se producen debido a una operación de actualización, esta opción devuelve las filas que contienen los valores de columna antes y después de la actualización.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de columna|Descripción|  
|-----------------|-----------------|-----------------|  
|__CDC_STARTLSN|**binary(10)**|LSN de confirmación de la transacción que se asocia al cambio. Todos los cambios que se confirman en la misma transacción comparten el mismo LSN de confirmación.|  
|__CDC_SEQVAL|**binary(10)**|Valor de secuencia que se usa para ordenar los cambios de fila en una transacción.|  
|\<columns from @column_list>|**varía**|Columnas que se identifican en el argumento *column_list* para sp_cdc_generate_wrapper_function cuando se llama para generar el script que crea la función de contenedor.|  
|__CDC_OPERATION|**nvarchar (2)**|Código de operación que indica qué operación hay que aplicar a la fila en el entorno de destino. Variará en función del valor del argumento *row_filter_option* proporcionado en la llamada:<br /><br /> *row_filter_option* = ' All '<br /><br /> 'D' - operación de eliminación<br /><br /> 'I' - operación de inserción<br /><br /> 'UN' - valores nuevos de la operación de actualización<br /><br /> *row_filter_option* = ' All Update Old '<br /><br /> 'D' - operación de eliminación<br /><br /> 'I' - operación de inserción<br /><br /> 'UN' - valores nuevos de la operación de actualización<br /><br /> 'UO' - valores anteriores de la operación de actualización|  
|\<columns from @update_flag_list>|**bit**|Un indicador de bits se denomina anexando _uflag al nombre de la columna. La marca siempre se establece en NULL cuando \_ _CDC_OPERATION es ' d ', ' I ', de ' uo '. Cuando \_ _CDC_OPERATION es ' un ', se establece en 1 si la actualización generó un cambio en la columna correspondiente. De lo contrario, es 0.|  
  
## <a name="remarks"></a>Observaciones  
 La fn_all_changes_<capture_instance función> actúa como contenedor de la CDC.fn_cdc_get_all_changes_<capture_instance> consulta. El procedimiento almacenado sys.sp_cdc_generate_wrapper se utiliza para generar el script que crea el contenedor.  
  
 Las funciones contenedoras no se crean automáticamente. Para crear funciones contenedoras hay que seguir dos pasos:  
  
1.  Ejecutar el procedimiento almacenado para generar el script que permite crear el contenedor.  
  
2.  Ejecutar el script para crear realmente la función contenedora.  

 Las funciones contenedoras permiten a los usuarios consultar de forma sistemática los cambios que se produjeron dentro de un intervalo delimitado por valores de **fecha y hora** en lugar de los valores de LSN. Las funciones de contenedor realizan todas las conversiones necesarias entre los valores **DateTime** proporcionados y los valores de LSN necesarios internamente como argumentos para las funciones de consulta. Cuando las funciones de contenedor se utilizan en serie para procesar un flujo de datos modificados, garantizan que no se pierden ni se repiten datos siempre que se siga la Convención siguiente: el @end_time valor del intervalo asociado a una llamada se proporciona como el @start_time valor para el intervalo asociado a la llamada subsiguiente.  
  
 Si se usa el parámetro @closed_high_end_point al crear el script, se pueden generar contenedores para admitir un límite superior cerrado o un límite superior abierto en la ventana de consulta especificada. Es decir, puede decidir si las entradas que tienen un tiempo de confirmación igual al límite superior del intervalo de extracción se incluirán en el intervalo. De forma predeterminada, se incluye el límite superior.  
  
 El conjunto de resultados devuelto por la función de contenedor **All Changes** devuelve las columnas _ _ $ start_lsn y \_ \_ $seqval de la tabla de cambios como columnas \_ _CDC_STARTLSN y \_ _CDC_SEQVAL, respectivamente. Se sigue con las columnas sometidas a seguimiento que aparecían en el parámetro *\@ column_list* cuando se generó el contenedor. Si *\@ COLUMN_LIST* es null, se devuelven todas las columnas de origen sometidas a seguimiento. Las columnas de origen van seguidas de una columna de operación, \_ _CDC_OPERATION, que es una columna de uno o dos caracteres que identifica la operación.  
  
 A continuación, las marcas de bits se anexan al conjunto de resultados para cada columna que se identifica en el parámetro @update_flag_list. Para el contenedor **All Changes** , las marcas de bits siempre serán NULL si __CDC_OPERATION es ' d ', ' I ' o ' uo '. Si \_ _CDC_OPERATION es ' un ', la marca se establecerá en 1 o 0, dependiendo de si la operación de actualización provocó un cambio en la columna.  
  
 La plantilla de configuración de captura de datos modificados ' Create CDC wrapper TVF for Schema ' muestra cómo usar el sp_cdc_generate_wrapper_function procedimiento almacenado para obtener scripts de creación para todas las funciones de contenedor para las funciones de consulta definidas de un esquema. A continuación, la plantilla crea esos scripts. Para obtener más información acerca de las plantillas, vea [Explorador de plantillas](../../ssms/template/template-explorer.md).  
  
## <a name="see-also"></a>Consulte también  
 [sys.sp_cdc_generate_wrapper_function &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  

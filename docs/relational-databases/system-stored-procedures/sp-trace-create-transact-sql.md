---
description: sp_trace_create (Transact-SQL)
title: sp_trace_create (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_create_TSQL
- sp_trace_create
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_create
ms.assetid: f3a43597-4c5a-4520-bcab-becdbbf81d2e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c63a724ad0b82a1a11145cc559ac52fc2d6f9cc3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200622"
---
# <a name="sp_trace_create-transact-sql"></a>sp_trace_create (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una definición de seguimiento. El nuevo seguimiento estará en estado de detención.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use eventos extendidos en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_trace_create [ @traceid = ] trace_id OUTPUT   
          , [ @options = ] option_value   
          , [ @tracefile = ] 'trace_file'   
     [ , [ @maxfilesize = ] max_file_size ]  
     [ , [ @stoptime = ] 'stop_time' ]  
     [ , [ @filecount = ] 'max_rollover_files' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @traceid = ] trace_id` Es el número asignado por [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al nuevo seguimiento. Se pasará por alto cualquier entrada proporcionada por el usuario. *trace_id* es de **tipo int** y su valor predeterminado es NULL. El usuario emplea el valor *trace_id* para identificar, modificar y controlar el seguimiento definido por este procedimiento almacenado.  
  
`[ @options = ] option_value` Especifica las opciones establecidas para el seguimiento. *option_value* es de **tipo int** y no tiene ningún valor predeterminado. Los usuarios pueden elegir una combinación de estas opciones especificando el valor de la suma de las opciones seleccionadas. Por ejemplo, para activar las opciones TRACE_FILE_ROLLOVER y SHUTDOWN_ON_ERROR, especifique **6** para *option_value*.  
  
 En la tabla siguiente se muestran las opciones, las descripciones y sus valores.  
  
|Nombre de la opción|Valor de la opción|Descripción|  
|-----------------|------------------|-----------------|  
|TRACE_FILE_ROLLOVER|**2**|Especifica que cuando se alcanza el *max_file_size* , el archivo de seguimiento actual se cierra y se crea un archivo nuevo. Todos los nuevos registros se escribirán en el archivo nuevo. El archivo nuevo tendrá el mismo nombre que el archivo anterior, pero se agregará un entero para indicar su secuencia. Por ejemplo, si el archivo de seguimiento original se llama FILENAME.TRC, siguiente archivo de seguimiento se llamará FILENAME_1.TRC, el siguiente archivo de seguimiento se llamará FILENAME_2.TRC, y así sucesivamente.<br /><br /> A medida que se creen más archivos de seguimiento de sustitución incremental, el valor entero agregado al nombre del archivo aumentará secuencialmente.<br /><br /> SQL Server usa el valor predeterminado de *max_file_size* (5 MB) si se especifica esta opción sin especificar un valor para *max_file_size*.|  
|SHUTDOWN_ON_ERROR|**4**|Especifica que, si por cualquier razón, no se puede escribir el seguimiento en el archivo, SQL Server se cerrará. Esta opción es muy útil cuando se realizan seguimientos de auditoría de seguridad.|  
|TRACE_PRODUCE_BLACKBOX|**8**|Especifica que el servidor guardará un registro de los últimos 5 MB de información de seguimiento producidos por el servidor. TRACE_PRODUCE_BLACKBOX es incompatible con las demás opciones.|  
  
`[ @tracefile = ] 'trace_file'` Especifica la ubicación y el nombre de archivo en el que se escribirá el seguimiento. *trace_file* es **nvarchar (245)** y no tiene ningún valor predeterminado. *trace_file* puede ser un directorio local (como n ' C:\MSSQL\Trace\trace.TRC ') o una UNC de un recurso compartido o una ruta de acceso (n ' \\ \\ *ServerName* \\ *nombreDeRecursoCompartido* \\ *Directory*\trace.TRC ').  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anexará una extensión **. TRC** a todos los nombres de archivo de seguimiento. Si se especifican la opción TRACE_FILE_ROLLOVER y un *max_file_size* , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea un nuevo archivo de seguimiento cuando el archivo de seguimiento original crece hasta su tamaño máximo. El nuevo archivo tiene el mismo nombre que el archivo original, pero se anexa _ *n* para indicar su secuencia, comenzando por **1**. Por ejemplo, si el primer archivo de seguimiento se denomina **filename. TRC**, el segundo archivo de seguimiento se denomina **filename_1. TRC**.  
  
 Si usa la opción TRACE_FILE_ROLLOVER, no es recomendable que emplee caracteres de subrayado en el nombre de archivo de seguimiento original. Si usa caracteres de subrayado, puede producirse el siguiente comportamiento:  
  
-   [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] no carga automáticamente ni le pide que cargue los archivos de sustitución incremental (si alguna de estas opciones de sustitución incremental de archivos está configurada).  
  
-   La función fn_trace_gettable no carga los archivos de sustitución incremental (cuando se especifica mediante el argumento *number_files* ), donde el nombre de archivo original termina con un carácter de subrayado y un valor numérico. (Esto no se aplica al carácter de subrayado y al número que se anexan automáticamente cuando un archivo realiza la sustitución incremental).  
  
> [!NOTE]  
>  Para solucionar estos comportamientos, puede cambiar el nombre de los archivos de seguimiento y quitar los caracteres de subrayado del nombre de archivo original. Por ejemplo, si el archivo original se denomina **my_trace. TRC** y el archivo de sustitución incremental se llama **my_trace_1. TRC**, puede cambiar el nombre de los archivos a mis **seguimiento. TRC** y **mytrace_1. TRC** antes de abrir los archivos en [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
 no se puede especificar *trace_file* cuando se usa la opción TRACE_PRODUCE_BLACKBOX.  
  
`[ @maxfilesize = ] max_file_size` Especifica el tamaño máximo en megabytes (MB) que puede alcanzar un archivo de seguimiento. *max_file_size* es de tipo **BIGINT** y su valor predeterminado es **5**.  
  
 Si este parámetro se especifica sin la opción TRACE_FILE_ROLLOVER, el seguimiento detiene la grabación en el archivo cuando el espacio en disco utilizado supera la cantidad especificada por *max_file_size*.  
  
`[ @stoptime = ] 'stop_time'` Especifica la fecha y la hora en que se detendrá el seguimiento. *stop_time* es de **tipo DateTime** y su valor predeterminado es NULL. Si es NULL, el seguimiento se ejecuta hasta que se detiene de forma manual o bien hasta que se cierra el servidor.  
  
 Si se especifican *stop_time* y *max_file_size* , y no se especifica TRACE_FILE_ROLLOVER, se alcanza el valor de la parte superior del seguimiento cuando se alcanza la hora de detención especificada o el tamaño máximo de archivo. Si se especifican *stop_time*, *max_file_size* y TRACE_FILE_ROLLOVER, el seguimiento se detiene en la hora de detención especificada, suponiendo que el seguimiento no llena la unidad.  
  
`[ @filecount = ] 'max_rollover_files'` Especifica el número máximo de archivos de seguimiento que se van a mantener con el mismo nombre de archivo base. *MAX_ROLLOVER_FILES* es de **tipo int** y es mayor que uno. Este parámetro solo es válido si se especifica la opción TRACE_FILE_ROLLOVER. Cuando se especifica *MAX_ROLLOVER_FILES* , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta mantener menos de *MAX_ROLLOVER_FILES* archivos de seguimiento eliminando el archivo de seguimiento más antiguo antes de abrir un nuevo archivo de seguimiento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hace un seguimiento de la edad de los archivos de seguimiento agregando un número al nombre del archivo base.  
  
 Por ejemplo, cuando el parámetro *trace_file* se especifica como "c:\mytrace", un archivo con el nombre "c:\ mytrace_123. TRC" es más antiguo que un archivo con el nombre "c:\ mytrace_124. TRC". Si *MAX_ROLLOVER_FILES* está establecido en 2, SQL Server elimina el archivo "c:\ mytrace_123. TRC" antes de crear el archivo de seguimiento "c:\ mytrace_125. TRC".  
  
 Observe que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo intenta eliminar cada archivo una vez, y no puede eliminar un archivo que esté siendo utilizado en otro proceso. Por lo tanto, si otra aplicación está utilizando archivos de seguimiento mientras se ejecuta el seguimiento, puede que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deje estos archivos de seguimiento en el sistema de archivos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 En la tabla siguiente se describen los valores del código que los usuarios pueden obtener después de completar el procedimiento almacenado.  
  
|Código devuelto|Descripción|  
|-----------------|-----------------|  
|0|Ningún error.|  
|1|Error desconocido.|  
|10|Opciones no válidas. Se devuelve cuando las opciones especificadas no son compatibles.|  
|12|Archivo no creado.|  
|13|Memoria insuficiente Se devuelve cuando no hay memoria suficiente para realizar la acción especificada.|  
|14|Hora de detención no válida. Se devuelve cuando ya se ha alcanzado la hora de detención especificada.|  
|15|Parámetros no válidos. Se devuelve cuando el usuario ha proporcionado parámetros no compatibles.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_trace_create** es un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procedimiento almacenado que realiza muchas de las acciones ejecutadas previamente por **xp_trace_ \* *_ procedimientos almacenados extendidos disponibles en versiones anteriores de SQL Server. Use _* sp_trace_create** en lugar de:  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_setqueuecreateinfo**  
  
-   **xp_trace_setqueuedestination**  
  
 **sp_trace_create** solo crea una definición de seguimiento. Este procedimiento almacenado no se puede utilizar para iniciar o cambiar un seguimiento.  
  
 Los parámetros de todos los procedimientos almacenados de seguimiento de SQL (**sp_trace_xx**) tienen un tipo estricto. Si no se llama a estos parámetros con los tipos de datos de parámetros de entrada correctos, según se especifica en la descripción del argumento, el procedimiento almacenado devolverá un error.  
  
 Por **sp_trace_create**, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio debe tener permiso de escritura en la carpeta de archivos de seguimiento. Si la cuenta del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no es administrador en el equipo donde se encuentra el archivo de seguimiento, debe conceder explícitamente permiso de escritura a la cuenta del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Puede cargar automáticamente el archivo de seguimiento creado con **sp_trace_create** en una tabla mediante la función del sistema **fn_trace_gettable** . Para obtener información sobre cómo usar esta función del sistema, vea [sys.fn_trace_gettable &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md).  
  
 Para obtener un ejemplo de cómo usar los procedimientos almacenados de seguimiento, vea [Crear un seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
 **TRACE_PRODUCE_BLACKBOX** tiene las siguientes características:  
  
-   Es un seguimiento de sustitución incremental. El *file_count* predeterminado es 2, pero el usuario puede invalidarlo con la opción *FileCount* .  
  
-   La *FILE_SIZE* predeterminada como con otros seguimientos es de 5 MB y se puede cambiar.  
  
-   No se puede especificar ningún nombre de archivo. El archivo se guardará como: **N '%SQLDIR%\MSSQL\DATA\blackbox.TRC '**  
  
-   Solo los eventos siguientes y sus columnas están contenidos en el seguimiento:  
  
    -   **RPC starting**  
  
    -   **Batch starting**  
  
    -   **Exception**  
  
    -   **Atención**  
  
-   Los eventos o columnas no se pueden agregar o quitar de este seguimiento.  
  
-   No se pueden especificar filtros para este seguimiento.  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener permiso ALTER TRACE.  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_generateevent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [Seguimiento de SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  

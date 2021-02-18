---
title: dta, utilidad
description: La utilidad DTA es una versión de línea de comandos del Asistente para la optimización de motor de base de datos que permite usar la funcionalidad en aplicaciones y scripts.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- physical design structures [SQL Server]
- command prompt utilities [SQL Server], dta
- dta utility
- tuning databases [SQL Server], Database Engine Tuning Advisor
- workloads [SQL Server], analyzing
- dta utility, about dta utility
- performance [SQL Server], Database Engine Tuning Advisor
- Database Engine Tuning Advisor [SQL Server], command prompt
- optimizing databases [SQL Server]
ms.assetid: a0b210ce-9b58-4709-80cb-9363b68a1f5a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/09/2017
ms.openlocfilehash: b05279df08153efab4257a74a8389ea33b38b966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338742"
---
# <a name="dta-utility"></a>dta, utilidad

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La utilidad **dta** es la versión del símbolo del sistema del Asistente para la optimización de motor de base de datos. La utilidad **dta** está diseñada para permitir usar la funcionalidad del Asistente para la optimización de motor de base de datos en aplicaciones y scripts.  

Al igual que el Asistente para la optimización de motor de base de datos, la utilidad **dta** analiza una carga de trabajo y recomienda estructuras de diseño físico para mejorar el rendimiento del servidor para esa carga de trabajo. La carga de trabajo puede ser una caché del plan, un archivo de seguimiento de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , una tabla o un script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Las estructuras de diseño físico incluyen índices, vistas indizadas y particiones. Después de analizar una carga de trabajo, la utilidad **dta** genera una recomendación para el diseño físico de bases de datos y puede generar el script necesario para implementar la recomendación. Las cargas de trabajo se pueden especificar desde el símbolo del sistema con el argumento **-if** o **-it** . También puede especificar un archivo de entrada XML desde el símbolo del sistema con el argumento **-ix** . En ese caso, la carga de trabajo se especifica en el archivo de entrada XML.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
dta  
[ -? ] |  
[  
      [ -S server_name[ \instance ] ]  
      { { -U login_id [-P password ] } | -E  }  
      { -D database_name [ ,...n ] }  
      [ -d database_name ]   
      [ -Tl table_list | -Tf table_list_file ]  
      { -if workload_file | -it workload_trace_table_name  |   
        -ip | -iq }  
      { -ssession_name | -IDsession_ID }  
      [ -F ]  
      [ -of output_script_file_name ]  
      [ -or output_xml_report_file_name ]  
      [ -ox output_XML_file_name ]  
      [ -rl analysis_report_list [ ,...n ] ]  
      [ -ix input_XML_file_name ]  
      [ -A time_for_tuning_in_minutes ]  
      [ -n number_of_events ]
      [ -I time_window_in_hours ]  
      [ -m minimum_improvement ]  
      [ -fa physical_design_structures_to_add ]  
      [ -fi filtered_indexes]  
      [ -fc columnstore_indexes]  
      [ -fp partitioning_strategy ]  
      [ -fk keep_existing_option ]  
      [ -fx drop_only_mode ]  
      [ -B storage_size ]  
      [ -c max_key_columns_in_index ]  
      [ -C max_columns_in_index ]  
      [ -e | -e tuning_log_name ]  
      [ -N online_option]  
      [ -q ]  
      [ -u ]  
      [ -x ]  
      [ -a ]  
]  
```  
  
## <a name="arguments"></a>Argumentos  
 **-?**  
 Muestra información de uso.  
  
 **-A** _time_for_tuning_in_minutes_  
 Especifica el límite de tiempo de optimización en minutos. **dta** usa el tiempo especificado para optimizar la carga de trabajo y generar un script con los cambios de diseño físico recomendados. De manera predeterminada, **dta** asume un tiempo de optimización de 8 horas. Al especificar 0, se permite un tiempo de optimización ilimitado. **dta** puede acabar de optimizar toda la carga de trabajo antes de que pase el límite de tiempo. No obstante, para asegurarse de que se optimiza toda la carga de trabajo, se recomienda especificar un tiempo de optimización ilimitado (-A 0).  
  
 **-a**  
 Optimiza la carga de trabajo y aplica la recomendación sin preguntarle.  
  
 **-B** _storage_size_  
 Especifica el espacio máximo en megabytes que la partición y el índice recomendados pueden usar. Cuando se optimizan varias bases de datos, se tienen en cuenta las recomendaciones para todas las bases de datos sobre el cálculo del espacio. De manera predeterminada, **dta** supone que existe el menor de los siguientes tamaños de almacenamiento:  
  
-   Tres veces el tamaño actual de los datos sin procesar, lo que incluye el tamaño total de los montones e índices clúster de las tablas de la base de datos.  
  
-   El espacio disponible en todas las unidades de disco adjuntas más el tamaño de los datos sin procesar.  
  
 El tamaño de almacenamiento predeterminado no incluye los índices no clúster ni las vistas indizadas.  
  
 **-C** _max_columns_in_index_  
 Especifica el número máximo de columnas en los índices propuestos por **dta** . El valor máximo es 1024. De forma predeterminada, este argumento tiene asignado el valor 16 .  
  
 **-c** _max_key_columns_in_index_  
 Especifica el número máximo de columnas de clave en los índices propuestos por **dta** . El valor predeterminado es 16, que es el valor máximo permitido. **dta** también tiene en cuenta la creación de índices con columnas incluidas. Los índices recomendados con columnas incluidas pueden superar el número de columnas especificado en este argumento.  
  
 **-D** _database_name_  
 Especifica el nombre de cada base de datos que se va a optimizar. La primera base de datos es la base de datos predeterminada. Puede especificar varias bases de datos separando los nombres de la base de datos con comas, por ejemplo:  
  
```  
dta -D database_name1, database_name2...  
```  
  
 De forma alternativa, puede especificar varias bases de datos con el argumento **-D** para cada nombre de base de datos, por ejemplo:  
  
```  
dta -D database_name1 -D database_name2... n  
```  
  
 El argumento **-D** es obligatorio. Si el argumento **-D** no se ha especificado, **dta** se conecta inicialmente con la base de datos que se ha especificado con la primera cláusula `USE database_name` en la carga de trabajo. Si no hay ninguna cláusula explícita `USE database_name` en la carga de trabajo, debe usar el argumento **-d** .  
  
 Por ejemplo, si tiene una carga de trabajo que no contiene ninguna cláusula explícita `USE database_name` y usa el siguiente comando **dta** no se generará ninguna recomendación:  
  
```  
dta -D db_name1, db_name2...  
```  
  
 Pero si usa la misma carga de trabajo y utiliza el siguiente comando **dta** con el argumento **-d** , se generará una recomendación:  
  
```  
dta -D db_name1, db_name2 -d db_name1  
```  
  
 **-d** _database_name_  
 Especifica la primera base de datos a la que se conecta **dta** cuando optimiza una carga de trabajo. Solo se puede especificar una base de datos para este argumento. Por ejemplo:  
  
```  
dta -d AdventureWorks2012 ...  
```  
  
 Si se especifican varios nombres de bases de datos, **dta** devuelve un error. El argumento **-d** es opcional.  
  
 Si usa un archivo de entrada XML, puede especificar la primera base de datos a la que se conecta **dta** mediante el elemento **DatabaseToConnect** que se encuentra debajo del elemento **TuningOptions** . Para obtener más información, vea [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).  
  
 Si solo optimiza una base de datos, el argumento **-d** proporciona una funcionalidad similar a la del argumento **-d** de la utilidad **sqlcmd** , pero no ejecuta la instrucción USE *database_name* . Para obtener más información, consulte [sqlcmd Utility](../../tools/sqlcmd-utility.md).  
  
 **-E**  
 Utiliza una conexión de confianza en lugar de solicitar una contraseña. Debe usarse el argumento **-E** o el argumento **-U** , que especifica un identificador de inicio de sesión.  
  
 **-e** _tuning_log_name_  
 Especifica el nombre de la tabla o del archivo en el que **dta** registra los eventos que no puede optimizar. La tabla se crea en el servidor en el que se realiza la optimización.  
  
 Si se usa una tabla, especifique su nombre en el formato *[database_name].[owner_name].table_name*. En la siguiente tabla se muestran los valores predeterminados para cada parámetro:  
  
|Parámetro|Valor predeterminado|Detalles|  
|---------------|-------------------|-------------|  
|*database_name*|*database_name* especificado con la opción **–D**||  
|*owner_name*|**dbo**|*owner_name* debe ser **dbo**. Si se especifica otro valor, la ejecución de **dta** es errónea y devuelve un error.|  
|*table_name*|None||  
  
 Si se usa un archivo, especifique .xml como extensión. Por ejemplo, TuningLog.xml.  
  
> [!NOTE]  
>  La utilidad **dta** no elimina el contenido de las tablas de registro de optimización especificadas por el usuario si se elimina la sesión. Cuando optimice cargas de trabajo de gran tamaño, se recomienda especificar una tabla para el registro de optimización. Puesto que la optimización de cargas de trabajo de gran tamaño puede producir registros de optimización grandes, las sesiones se pueden eliminar mucho más rápidamente cuando se usa una tabla.  
  
 **-F**  
 Permite que **dta** sobrescriba un archivo de salida existente. Si ya existe un archivo de salida con el mismo nombre y no se especifica **-F** , **dta** devuelve un error. Puede usar **-F** con **-of**, **-or** o **-ox**.  
  
 **-fa** _physical_design_structures_to_add_  
 Especifica los tipos de estructuras de diseño físico que **dta** debe incluir en la recomendación. En la tabla siguiente se muestran y describen los valores que se pueden especificar para este argumento. Cuando no se especifica ningún valor, **dta** usa el parámetro **-fa IDX** predeterminado.  
  
|Value|Descripción|  
|-----------|-----------------|  
|IDX_IV|Índices y vistas indizadas.|  
|IDX|Solo índices.|  
|IV|Solo vistas indizadas.|  
|NCL_IDX|Solo índices no clúster.|  
  
 **-fi**  
 Especifica que los índices filtrados se consideren para las nuevas recomendaciones. Para obtener más información, consulte [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
**-fc**  
 Especifica que se deben tener en cuenta los índices de almacén de columnas para nuevas recomendaciones. DTA considerará los índices de almacén de columnas clúster y no clúster. Para obtener más información, vea    
[Recomendaciones de índice de almacén de columnas en el Asistente para la optimización de motor de base de datos (DTA)](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md).
 ||  
|-|  
|**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  

  
 **-fk** _keep_existing_option_  
 Especifica las estructuras de diseño físico existentes que **dta** debe conservar cuando genere su recomendación. En la tabla siguiente se muestran y describen los valores que se pueden especificar para este argumento:  
  
|Value|Descripción|  
|-----------|-----------------|  
|Ninguno|Ninguna estructura existente|  
|ALL|Todas las estructuras existentes|  
|ALIGNED|Todas las estructuras alineadas de partición.|  
|CL_IDX|Todos los clúster en las tablas|  
|IDX|Todos los índices clúster y no clúster de las tablas|  
  
 **-fp** _partitioning_strategy_  
 Especifica si se deben crear particiones de las nuevas estructuras de diseño físico (índices y vistas indexadas) que **dta** propone y cómo se deben crear esas particiones. En la tabla siguiente se muestran y describen los valores que se pueden especificar para este argumento:  
  
|Value|Descripción|  
|-----------|-----------------|  
|Ninguno|No crear particiones|  
|FULL|Particiones completas (para mejorar el rendimiento)|  
|ALIGNED|Solo particiones alineadas (para mejorar la capacidad de administración)|  
  
 ALIGNED significa que, en la recomendación generada por **dta** , cada índice propuesto se divide exactamente igual que la tabla subyacente para la que se ha definido el índice. Los índices no clúster de una vista indizada se alinean con la vista indizada. Solo se puede especificar un valor para este argumento. El valor predeterminado es **-fp NONE**.  
  
 **-fx** _drop_only_mode_  
 Especifica que **dta** solo tiene en cuenta la eliminación de estructuras de diseño físicas existentes. No se tienen en cuenta las nuevas estructuras de diseño físico. Cuando se especifica esta opción, **dta** evalúa la utilidad de las estructuras de diseño físico existentes y recomienda la eliminación de las estructuras que se usan en contadas ocasiones. Este argumento no necesita valores. No puede usarse con los argumentos **-fa**, **-fp** o **-fk ALL** .  
  
 **-ID** _session_ID_  
 Especifica un identificador numérico para la sesión de optimización. Si no se especifica, **dta** genera un número de identificación. Puede usar este identificador para ver la información de las sesiones de optimización existentes. Si no especifica un valor para **-ID**, debe especificar un nombre de sesión con **-s**.  
  
 **-ip**  
 Especifica que la memoria caché del plan se usará como carga de trabajo. Se analizan los primeros 1.000 eventos de la memoria caché del plan para las bases de datos seleccionadas explícitamente. Este valor se puede cambiar mediante la opción **- n**.  

**-iq**  
 Especifica que el Almacén de consultas se usará como carga de trabajo. Se analizan los primeros 1000 eventos del Almacén de consultas para las bases de datos seleccionadas explícitamente. Este valor se puede cambiar mediante la opción **- n**.  Para obtener más información, vea [Almacén de consultas](../../relational-databases/performance/how-query-store-collects-data.md) y [Optimización de la base de datos mediante carga de trabajo del Almacén de consultas](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).
 ||  
|-|  
|**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
     
 **-if** _workload_file_  
 Especifica el nombre y la ruta del archivo de carga de trabajo que se desea usar como entrada para la optimización. El archivo debe estar en uno de estos formatos: .trc (archivo de seguimiento de SQL Server Profiler). .sql (archivo de SQL) o .log (archivo de seguimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Debe especificarse un archivo de carga de trabajo o una tabla de carga de trabajo.  
  
 **-it** _workload_trace_table_name_  
 Especifica el nombre de una tabla que contiene el seguimiento de carga de trabajo para la optimización. El nombre se debe especificar en el formato [*database_name*] **.** [*owner_name*] **.** _table_name_.  
  
 En la tabla siguiente se muestran los valores predeterminados para cada parámetro:  
  
|Parámetro|Valor predeterminado|  
|---------------|-------------------|  
|*database_name*|*database_name* especificado con la opción **-D**.|  
|*owner_name*|**dbo**.|  
|*table_name*|Ninguno.|  
  
> [!NOTE]  
>  *owner_name* debe ser **dbo**. Si se especifica cualquier otro valor, la ejecución de **dta** no será correcta y se devolverá un error. Tenga en cuenta también que debe especificarse una tabla de carga de trabajo o un archivo de carga de trabajo.  
  
 **-ix** _input_XML_file_name_  
 Especifica el nombre del archivo XML que contiene la información de entrada de **dta** . Debe ser un documento XML válido conforme a DTASchema.xsd. Los argumentos en conflicto especificados en el símbolo del sistema para las opciones de optimización invalidan el valor correspondiente en este archivo XML. La única excepción se produce cuando se usa una configuración especificada por el usuario en el modo de evaluación del archivo de entrada XML. Por ejemplo, si se especifica una configuración en el elemento **Configuration** del archivo de entrada XML y el elemento **EvaluateConfiguration** también se especifica como una de las opciones de optimización, las opciones de optimización especificadas en el archivo de entrada XML anulan las opciones de optimización especificadas desde el símbolo del sistema.  

 **-k** _totalMáximoÍndices_  
 Número máximo de índices en la recomendación.  

 **-K** _totalMáximoÍndices_  
 Número máximo de índices por tabla.

 **-m** _minimum_improvement_  
 Especifica el porcentaje mínimo de mejora que debe satisfacer la configuración recomendada.  
  
 **-N** _online_option_  
 Especifica si las estructuras de diseño físico se crean en línea. En la tabla siguiente se muestran y describen los valores que pueden especificarse para este argumento:  
  
|Value|Descripción|  
|-----------|-----------------|  
|Apagado|No se pueden crear en línea las estructuras recomendadas de diseño físico.|  
|ACTIVAR|Se pueden crear en línea todas las estructuras recomendadas de diseño físico.|  
|MIXED|Siempre que es posible, el Asistente para la optimización de motor de base de datos intenta recomendar las estructuras de diseño físico que se pueden crear en línea.|  
  
 Si los índices se crean en línea, se anexa ONLINE = ON a la definición del objeto.  
  
 **-n** _number_of_events_  
 Especifica el número de los eventos de la carga de trabajo que **dta** debe optimizar. Si se especifica este argumento y la carga de trabajo es un archivo de seguimiento que contiene información de duración, **dta** optimiza los eventos en orden de duración decreciente. Este argumento es útil para comparar dos configuraciones de estructuras de diseño físico. Para comparar dos configuraciones, especifique el mismo número de eventos que se optimizarán para ambas configuraciones y después especifique un tiempo de optimización ilimitado para las dos de la siguiente manera:  
  
```  
dta -n number_of_events -A 0  
```  
  
 En este caso es importante especificar un tiempo de optimización ilimitado (`-A 0`). De lo contrario, el Asistente para la optimización de motor de base de datos supone que el tiempo de optimización es de 8 horas de forma predeterminada.
 
 **-I** _time_window_in_hours_   
   Especifica el período de tiempo (en horas) en el que se debe haber ejecutado una consulta para que DTA tenga en cuenta la optimización cuando se usa la opción **-iq** (carga de trabajo del Almacén de consultas). 
```  
dta -iq -I 48  
```  
En este caso, DTA usará el Almacén de consultas como el origen de la carga de trabajo y solo tenga en cuenta las consultas que se han ejecutado con las últimas 48 horas.  
  ||  
|-|  
|**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  


  
 **-of** _output_script_file_name_  
 Especifica que **dta** escribe la recomendación como un script [!INCLUDE[tsql](../../includes/tsql-md.md)] en el nombre de archivo y el destino especificados.  
  
 Puede usar **-F** con esta opción. Asegúrese de que el nombre de archivo es exclusivo, especialmente si también usa **-or** y **-ox**.  
  
 **-or** _output_xml_report_file_name_  
 Especifica que **dta** escribe la recomendación en un informe de salida en XML. Si se proporciona un nombre de archivo, las recomendaciones se escriben en ese destino. De lo contrario, **dta** usa el nombre de sesión para generar el nombre de archivo y lo escribe en el directorio actual.  
  
 Puede usar **-F** con esta opción. Asegúrese de que el nombre de archivo es exclusivo, especialmente si también usa **-of** y **-ox**.  
  
 **-ox** _output_XML_file_name_  
 Especifica que **dta** escribe la recomendación como un archivo XML en el nombre de archivo y el destino especificados. Asegúrese de que el Asistente para la optimización de motor de base de datos tiene permiso para escribir en el directorio de destino.  
  
 Puede usar **-F** con esta opción. Asegúrese de que el nombre de archivo es exclusivo, especialmente si también usa **-of** y **-or**.  
  
 **-P** _password_  
 Especifica la contraseña para el identificador de inicio de sesión. Si no se usa esta opción, **dta** solicitará una contraseña.  
  
 **-q**  
 Establece el modo silencioso. No se escribe ninguna información en la consola, ni siquiera información de progreso o de encabezado.  
  
 **-rl** _analysis_report_list_  
 Especifica la lista de informes de análisis que se generarán. En la tabla siguiente se muestran los valores que se pueden especificar para este argumento:  
  
|Value|Informe|  
|-----------|------------|  
|ALL|Todos los informes de análisis|  
|STMT_COST|Informe de costo de instrucciones|  
|EVT_FREQ|Informe de frecuencia de eventos|  
|STMT_DET|Informe de detalles de instrucciones|  
|CUR_STMT_IDX|Informe relacional de instrucciones e índices (configuración actual)|  
|REC_STMT_IDX|Informe relacional de instrucciones e índices (configuración recomendada)|  
|STMT_COSTRANGE|Informe de intervalo de costo de instrucciones|  
|CUR_IDX_USAGE|Informe de uso de índices (configuración actual)|  
|REC_IDX_USAGE|Informe de uso de índices (configuración recomendada)|  
|CUR_IDX_DET|Informe de detalles de índices (configuración actual)|  
|REC_IDX_DET|Informe de detalles de índices (configuración recomendada)|  
|VIW_TAB|Informe relacional de vistas y tablas|  
|WKLD_ANL|Informe de análisis de carga de trabajo|  
|DB_ACCESS|Informe de acceso a bases de datos|  
|TAB_ACCESS|Informe de acceso a tablas|  
|COL_ACCESS|Informe de acceso a columnas|  
  
 Para especificar varios informes, separe los valores con comas, por ejemplo:  
  
```  
... -rl EVT_FREQ, VIW_TAB, WKLD_ANL ...  
```  
  
 **-S** _server_name_[ *\instance*]  
 Especifica el nombre del equipo y la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que se va a conectar. Si no se especifica ningún valor de *server_name* , **dta** se conecta con la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo local. Esta opción es necesaria cuando se conecta con una instancia con nombre o cuando se ejecuta **dta** desde un equipo remoto de la red.  
  
 **-s** _session_name_  
 Especifica el nombre de la sesión de optimización. Es necesario si no se especifica **-ID** .  
  
 **-Tf** _table_list_file_  
 Especifica el nombre de un archivo que contiene una lista de tablas para optimizar. Cada tabla enumerada en el archivo debe empezar en una nueva línea. Los nombres de tabla deben tener nombres en tres partes, por ejemplo, **AdventureWorks2012.HumanResources.Department**. Opcionalmente, para invocar la característica de escala de tablas, el nombre de una tabla existente puede ir seguido de un número que indica el número previsto de filas de la tabla. El Asistente para la optimización de motor de base de datos toma en consideración el número proyectado de filas a la vez que optimiza o evalúa instrucciones de la carga de trabajo que hacen referencia a esas tablas. Tenga en cuenta que puede haber uno o más espacios entre el número de *number_of_rows* y *table_name*.  
  
 Este es el formato de archivo de *table_list_file*:  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 Este argumento es una alternativa a especificar una lista de tablas en el símbolo del sistema ( **-Tl**). No use un archivo de lista de tabla ( **-Tf**) si usa **-Tl**. Si se usan ambos argumentos, **dta** devuelve un error.  
  
 Si se omiten los argumentos **-Tf** y **-Tl** , todas las tablas de usuario de las bases de datos especificadas se tienen en cuenta para la optimización.  
  
 **-Tl** _table_list_  
 Especifica en el símbolo del sistema una lista de tablas que se optimizarán. Use una coma entre los nombres de tabla para separarlos. Si solo se especifica una base de datos con el argumento **-D** , no es necesario calificar los nombres de tabla con un nombre de base de datos. De lo contrario, se requiere el nombre completo en el formato *database_name.schema_name.table_name* para cada tabla.  
  
 Este argumento es una alternativa al uso de un archivo de lista de tablas ( **-Tf**). Si se usa tanto **-Tl** como **-Tf** , **dta** devuelve un error.  
  
 **-U** _login_id_  
 Especifica el identificador de inicio de sesión para conectar con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **-u**  
 Inicia la interfaz de usuario gráfica del Asistente para la optimización de motor de base de datos. Todos los parámetros se tratan como la configuración inicial para la interfaz de usuario.  
  
 **-x**  
 Inicia la sesión de optimización y se cierra.  
  
## <a name="remarks"></a>Observaciones  
 Pulse Ctrl+C una vez para detener la sesión de optimización y generar recomendaciones basadas en el análisis que **dta** ha completado hasta este punto. Se le solicitará que decida si desea o no generar recomendaciones. Presione CTRL+C de nuevo para detener la sesión de optimización sin generar recomendaciones.  
  
## <a name="examples"></a>Ejemplos  
 **A. Optimizar una carga de trabajo que incluye índices y vistas indizadas en su recomendación**  
  
 En este ejemplo se usa una conexión segura (`-E`) para conectar con la base de datos **tpcd1G** en MyServer para analizar una carga de trabajo y crear recomendaciones. Escribe la salida en un archivo de script denominado script.sql. Si script.sql ya existe, **dta** sobrescribirá el archivo porque se ha especificado el argumento `-F` . La sesión de optimización se ejecuta durante un período de tiempo ilimitado para garantizar un completo análisis de la carga de trabajo (`-A 0`). La recomendación debe proporcionar una mejora mínima del 5% (`-m 5`). **dta** debe incluir índices y vistas indexadas en su recomendación final (`-fa IDX_IV`).  
  
```  
dta -S MyServer -E -D tpcd1G -if tpcd_22.sql -F -of script.sql -A 0 -m 5 -fa IDX_IV  
```  
  
 **B. Limitar la utilización del disco**  
  
 Este ejemplo limita el tamaño total de la base de datos, que incluye los datos sin procesar y los índices adicionales, hasta 3 gigabytes (GB) (`-B 3000`) y dirige la salida a d:\result_dir\script1.sql. Se ejecuta durante 1 hora como máximo (`-A 60`).  
  
```  
dta -D tpcd1G -if tpcd_22.sql -B 3000 -of "d:\result_dir\script1.sql" -A 60  
```  
  
 **C. Limitar el número de consultas optimizadas**  
  
 Este ejemplo limita el número de consultas leídas desde el archivo orders_wkld.sql hasta un máximo de 10 (`-n 10`) y se ejecuta durante 15 minutos (`-A 15`), lo que se dé primero. Para asegurarse de que se optimizan las 10 consultas, especifique un tiempo de optimización ilimitado con `-A 0`. Si el tiempo es importante, especifique un límite de tiempo adecuado estableciendo el número de minutos que está disponible para optimizar con el argumento `-A` como se muestra en este ejemplo.  
  
```  
dta -D orders -if orders_wkld.sql -of script.sql -A 15 -n 10  
```  
  
 **D. Optimizar tablas específicas enumeradas en un archivo**  

 En este ejemplo se muestra el uso de *table_list_file* (el argumento **-Tf** ). El contenido del archivo table_list.txt es el siguiente:  

```
AdventureWorks2012.Sales.Customer  100000  
AdventureWorks2012.Sales.Store  
AdventureWorks2012.Production.Product  2000000  
```

El contenido de table_list.txt especifica que:  

- Solo deben optimizarse las tablas **Customer**, **Store** y **Product** de la base de datos.  
  
- Se asume que el número de filas de las tablas **Customer** y **Product** es 100.000 y 2.000.000, respectivamente.  
  
- Se supone que el número de filas de **Store** es igual al número actual de filas de la tabla.  

    Tenga en cuenta que puede haber uno o más espacios entre el número de filas y el nombre de tabla que lo precede en *table_list_file*.  
    
    El tiempo de optimización es de 2 horas (`-A 120`) y la salida se escribe en un archivo XML (`-ox XMLTune.xml`).  

``` 
dta -D pubs -if pubs_wkld.sql -ox XMLTune.xml -A 120 -Tf table_list.txt  
``` 

## <a name="see-also"></a>Consulte también

- [Referencia de la utilidad del símbolo del sistema &#40;motor de base de datos&#41;](../../tools/command-prompt-utility-reference-database-engine.md)
- [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)
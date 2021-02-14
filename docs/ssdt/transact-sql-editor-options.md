---
title: Opciones del Editor de Transact-SQL
description: Familiarícese con las opciones del Editor de Transact-SQL. Obtenga información sobre las propiedades de ejecución de consultas y las de resultados de consulta, y vea cómo ajustar los valores.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlExecutionAdvancedSettingsOption
- sql.data.tools.SqlExecutionAnsiSettingsDlg
- sql.data.tools.SqlResultToTextSettingsDlg
- sql.data.tools.SqlExecutionGeneralSettingsDlg
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.GENERAL
- sql.data.tools.unittesting.tsqleditor
- sql.data.tools.SqlResultsToGridSettingsDlg
- sql.data.tools.queryresults.executequeryfailure
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.EDITOR_TAB_AND_STATUS_BAR
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_TEXT
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ANSI
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_GRID
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.ONLINE_EDITING
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ADVANCED
ms.assetid: fa9a250f-7feb-433e-91bd-a09779d74c8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 28bb933ec92a6f96045a1f316c2757fbf3a9d564
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082438"
---
# <a name="transact-sql-editor-options"></a>Opciones del Editor de Transact-SQL

Este tema contiene información sobre algunas de las opciones del Editor de Transact-SQL. Para establecer estas opciones, navegue al cuadro de diálogo **Opción** a través del menú **Herramientas\Opciones**.  
  
[Ejecución de la consulta](#QueryExecution)  
  
[Resultados de la consulta](#QueryResults)  
  
## <a name="query-execution"></a><a name="QueryExecution"></a>Ejecución de la consulta  
  
|Propiedad|Descripción|  
|------------|---------------|  
|**SET ROWCOUNT**|El valor predeterminado 0 indica que SQL Server esperará a que se reciban todos los resultados. Especifique un valor mayor que 0 si desea que SQL Server detenga la consulta después de obtener el número de filas especificado. Para desactivar esta opción (de modo que se devuelvan todas las filas), especifique SET ROWCOUNT 0.|  
|**SET TEXTSIZE**|El valor predeterminado, 2.147.483.647 bytes, indica que SQL Server proporcionará un campo de datos completo hasta el límite de los campos de datos text, ntext, nvarchar(max) y varchar(max). No afecta al tipo de datos XML. Especifique un número menor para limitar los resultados en caso de que los valores sean elevados. Las columnas que superen el número especificado se truncarán.|  
|**Tiempo de espera de ejecución**|Indica el número de segundos de espera antes de cancelar la consulta. El valor 0 indica una espera infinita o que no hay tiempo de espera.|  
|**De manera predeterminada, abrir nuevas consultas en modo SQLCMD**|Active esta casilla para abrir nuevas consultas en modo SQLCMD. Esta casilla solo se muestra cuando el cuadro de diálogo se abre desde el menú Herramientas.<br /><br />Cuando seleccione esta opción, tenga en cuenta las siguientes limitaciones:<br /><br />- IntelliSense se desactiva en el Editor de consultas del Motor de base de datos.<br />- Debido a que el Editor de consultas no se ejecuta desde la línea de comandos, no podrá pasar parámetros de línea de comandos, tales como variables.<br />- Dado que el Editor de consultas no puede responder a comandos del sistema operativo, debe tener cuidado de no ejecutar instrucciones interactivas.|  
|**SET NOCOUNT**|Evita que se devuelva el mensaje que indica el número de filas afectadas por una instrucción Transact-SQL como parte de los resultados. Para obtener más información, vea [SET NOCOUNT](../t-sql/statements/set-nocount-transact-sql.md).|  
|**SET NOEXEC**|Cuando es **ON**, indica a Microsoft® SQL Server™ que debe compilar cada lote de instrucciones Transact-SQL pero no debe ejecutarlas. Cuando es **OFF**, indica a Microsoft® SQL Server™ que ejecute todos los lotes después de la compilación. Para más información, consulte [SET NOEXEC](../t-sql/statements/set-noexec-transact-sql.md).|  
|**SET PARSEONLY**|Comprueba la sintaxis de cada instrucción Transact-SQL y devuelve los mensajes de error sin compilar ni ejecutar la instrucción. Para obtener más información, vea [SET PARSEONLY](../t-sql/statements/set-parseonly-transact-sql.md).|  
|**SET CONCAT_NULL_YIELDS_NULL**|Determina si los resultados de la concatenación se tratan como valor NULL o como cadena vacía. Para más información, consulte [SET CONCAT_NULL_YIELDS_NULL](../t-sql/statements/set-concat-null-yields-null-transact-sql.md).|  
|**SET ARITHABORT**|Cancela una consulta cuando se produce un error de desbordamiento o división por cero durante su ejecución. Para más información, consulte [SET ARITHABORT](https://msdn.microsoft.com/library/aa259212(v=SQL.80).aspx).|  
|**SET SHOWPLAN_TEXT**|Hace que Microsoft® SQL Server™ no ejecute instrucciones Transact-SQL. En su lugar, SQL Server devuelve información detallada sobre el modo en que se ejecutan las instrucciones. Para más información, consulte [SET SHOWPLAN_TEXT](../t-sql/statements/set-showplan-text-transact-sql.md).|  
|**SET STATISTICS TIME**|Muestra el número de milisegundos necesarios para analizar, compilar y ejecutar cada instrucción.|  
|**SET STATISTICS IO**|Hace que Microsoft® SQL Server™ muestre información relacionada con la cantidad de actividad de disco generada por las instrucciones Transact-SQL.|  
|**SET TRANSACTION ISOLATION LEVEL**|Controla el comportamiento del bloqueo predeterminado de las transacciones para todas las instrucciones de Microsoft® SQL Server™ **SELECT** emitidas por una conexión. Para obtener información, vea  [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).|  
|**SET LOCK_TIMEOUT**|Especifica el número de milisegundos que una instrucción espera a que se libere un bloqueo. Para más información, consulte [SET LOCK_TIMEOUT](../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|**SET QUERY_GOVERNOR_COST_LIMIT**|Invalida el valor configurado actualmente para la conexión actual. Para más información, consulte [SET QUERY_GOVERNOR_COST_LIMIT](../t-sql/statements/set-query-governor-cost-limit-transact-sql.md).|  
|**SET ANSI_DEFAULTS**|Controla un grupo de opciones de Microsoft® SQL Server™ que especifican de forma colectiva parte del comportamiento estándar de SQL-92. Para más información, consulte [SET ANSI_DEFAULTS](../t-sql/statements/set-ansi-defaults-transact-sql.md).|  
|**SET QUOTED_IDENTIFIER**|Hace que Microsoft® SQL Server™ siga las reglas de SQL-92 en cuanto a la comilla delimitadora de identificadores y cadenas literales. Los identificadores delimitados por comillas dobles pueden ser palabras clave reservadas de Transact-SQL o pueden contener caracteres no admitidos normalmente por las reglas de sintaxis de Transact-SQL para los identificadores. Para más información, consulte [SET QUOTED_IDENTIFIER](../t-sql/statements/set-quoted-identifier-transact-sql.md).|  
|**SET ANSI_NULL_DFLT_ON**|Modifica el comportamiento de la sesión para invalidar la nulabilidad predeterminada de las columnas nuevas cuando la opción ANSI null default de la base de datos es false. Para más información, consulte [SET ANSI_NULL_DFLT_ON](../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).|  
|**SET IMPLICIT_TRANSACTIONS**|Cuando es **ON**, establece la conexión en el modo de transacción implícita. Cuando es **OFF**, regresa la conexión al modo de transacción con confirmación automática. Para más información, consulte [SET IMPLICIT_TRANSACTIONS](../t-sql/statements/set-implicit-transactions-transact-sql.md).|  
|**SET CURSOR_CLOSE_ON_COMMIT**|Controla si un cursor se cierra o no cuando se confirma una instrucción. Para más información, consulte [SET CURSOR_CLOSE_ON_COMMIT](../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).|  
|**SET ANSI_PADDING**|Controla el modo en que la columna almacena valores más cortos que el tamaño que tiene definido y cómo almacena valores con espacios en blanco a la derecha en datos de tipo **char**, **varchar**, **binary** y **varbinary** . Para más información, consulte [SET ANSI_PADDING](../t-sql/statements/set-ansi-padding-transact-sql.md).|  
|**SET ANSI_WARNINGS**|Especifica el comportamiento estándar de SQL-92 para varias condiciones de error. Para más información, consulte [SET ANSI_WARNINGS](../t-sql/statements/set-ansi-warnings-transact-sql.md).|  
|**SET ANSI_NULLS**|Especifica el comportamiento conforme a SQL-92 para los operadores de comparación Igual que ( **=** ) y Distinto de ( **<>** ) cuando se usan con valores NULL. Para más información, consulte [SET ANSI_NULLS](../t-sql/statements/set-ansi-nulls-transact-sql.md).|  
  
## <a name="query-results"></a><a name="QueryResults"></a>Resultados de la consulta  
  
|Propiedad|Descripción|  
|------------|---------------|  
|**Incluir la consulta en el conjunto de resultados**|Devuelve el texto de la consulta como parte del conjunto de resultados.|  
|**Incluir encabezados de columna al copiar o guardar los resultados**|Incluye los encabezados de columna (títulos) cuando los resultados se copian en el portapapeles o se guardan en un archivo. Si desea que los resultados guardados o copiados incluyan solo los datos y no los encabezados de columna, desactive esta casilla.|  
|**Descartar resultados tras la ejecución**|Si descarta los resultados de la consulta después de que la pantalla los reciba, liberará memoria.|  
|**Mostrar resultados en otra pestaña**|Muestra el conjunto de resultados en una nueva ventana de documento, en lugar de mostrarlos en la parte inferior de la ventana del documento de consulta.|  
|**Cambiar a la pestaña de resultados tras ejecutar la consulta**|Establece el foco de la pantalla automáticamente en el conjunto de resultados.|  
|**Número máximo de caracteres recuperados**|Datos no XML:<br /><br />Especifique un número entre 1 y 65535 para definir el número máximo de caracteres que aparecerán en cada celda. **Nota:** La especificación de un número alto puede provocar que los datos del conjunto de resultados aparezcan truncados. El número máximo de caracteres que se muestra en cada celda depende del tamaño de la fuente. Cuando se devuelven conjuntos de resultados grandes, un valor elevado en esta casilla puede provocar que SQL Server Management Studio no disponga de suficiente memoria y el rendimiento del sistema se vea afectado.<br /><br />Datos XML:<br /><br />Seleccione 1 MB, 2 MB o 5 MB. Seleccione Ilimitados para recuperar todos los caracteres.|  
|**Formato de salida**|De manera predeterminada, la salida se muestra en columnas que se han creado rellenando de espacios los resultados. Otras de las opciones son el uso de comas, tabulaciones o espacios para separar las columnas. Seleccione el cuadro de diálogo **Delimitador personalizado** para especificar un carácter delimitador diferente en el cuadro **Delimitador personalizado** .|  
|**Delimitador personalizado**|Especifique el carácter que desee para separar las columnas. Esta opción solo se encuentra disponible si está activada la casilla **Delimitador personalizado** en el cuadro **Formato de salida** .|  
|**Incluir encabezados de columna en el conjunto de resultados**|Desactive esta casilla si no desea que cada columna tenga una etiqueta con un título de columna.|  
|**Desplazarse a medida que se reciben resultados**|Active esta casilla para mantener el foco de visualización en los registros devueltos más recientemente en la parte inferior. Desactive esta casilla para mantener el foco de visualización en las primeras filas recibidas.|  
|**Alinear a la derecha los valores numéricos**|Active esta casilla para alinear los valores numéricos a la derecha de la columna. Esta opción puede ser de ayuda a la hora de revisar números con un número fijo de posiciones decimales.|  
|**Descartar resultados tras la ejecución**|Si descarta los resultados de la consulta después de que la pantalla los reciba, liberará memoria.|  
|**Mostrar resultados en otra pestaña**|Active esta casilla para mostrar el conjunto de resultados en una nueva ventana de documento, en lugar de en la parte inferior de la ventana de documento de la consulta.|  
|**Cambiar a la pestaña de resultados tras ejecutar la consulta**|Haga clic en esta opción para establecer automáticamente el foco de pantalla en el conjunto de resultados.|  
|**Número máximo de caracteres mostrados en cada columna**|El valor predeterminado es 256. Aumente este valor para poder mostrar conjuntos de resultados mayores sin truncamientos.|  
|**Valores predeterminados**|Restablece todos los valores de esta página a los valores predeterminados originales.|  

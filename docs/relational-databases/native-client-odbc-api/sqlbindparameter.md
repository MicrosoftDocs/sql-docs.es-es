---
description: SQLBindParameter
title: SQLBindParameter | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLBindParameter function
ms.assetid: c302c87a-e7f4-4d2b-a0a7-de42210174ac
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e676580ced0122e01286a0c6142cf63edcdf6e0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469526"
---
# <a name="sqlbindparameter"></a>SQLBindParameter
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLBindParameter** puede eliminar la carga de conversión de datos cuando se usa para proporcionar datos para el controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, produciendo ganancias de rendimiento significativas para los componentes de servidor y cliente de las aplicaciones. Entre otras ventajas se incluyen la pérdida reducida de precisión al insertar o actualizar los tipos de datos numéricos aproximados.  
  
> [!NOTE]  
>  Al insertar los datos de tipo **char** y **wchar** en una columna de imagen, se usa el tamaño de los datos pasados, a diferencia del tamaño de los datos después de la conversión a un formato binario.  
  
 Si el controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client encuentra un error en un elemento de matriz único de una matriz de parámetros, el controlador continúa ejecutando la instrucción para los elementos de matriz restantes. Si la aplicación ha enlazado una matriz de elementos de estado de parámetro para la instrucción, las filas de parámetros que generan errores se pueden determinar de la matriz.  
  
 Al usar el controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, especifique SQL_PARAM_INPUT al enlazar parámetros de entrada. Especifique únicamente SQL_PARAM_OUTPUT o SQL_PARAM_INPUT_OUTPUT al enlazar parámetros de procedimientos almacenados definidos con la palabra clave OUTPUT.  
  
 [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md) no es confiable con el controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client si un elemento de matriz de parámetros enlazados produce un error en la ejecución de la instrucción. El atributo de instrucción ODBC SQL_ATTR_PARAMS_PROCESSED_PTR notifica el número de filas procesadas antes de que se produzca el error. A continuación, la aplicación puede atravesar su matriz de estado de parámetro para detectar el número de instrucciones ejecutado correctamente, si es necesario.  
  
## <a name="binding-parameters-for-sql-character-types"></a>Enlazar parámetros para tipos de caracteres SQL  
 Si el tipo de datos SQL que se pasa es un tipo de caracteres, *ColumnSize* es el tamaño en caracteres (no en bytes). Si la longitud de la cadena de datos en bytes es mayor de 8000, *ColumnSize* debería establecerse en **SQL_SS_LENGTH_UNLIMITED**, lo que indica que no hay límite en el tamaño del tipo SQL.  
  
 Por ejemplo, si el tipo de datos SQL es **SQL_WVARCHAR**, *ColumnSize* no debe ser mayor que 4000. Si la longitud real de los datos es mayor que 4000, *ColumnSize* debe establecerse en **SQL_SS_LENGTH_UNLIMITED** para que el controlador use **nvarchar(max)** .  
  
## <a name="sqlbindparameter-and-table-valued-parameters"></a>SQLBindParameter y parámetros con valores de tabla  
 Al igual que otros tipos de parámetros, los parámetros con valores de tabla están enlazados por SQLBindParameter.  
  
 Una vez enlazado un parámetro con valores de tabla, también se enlazan sus columnas. Para enlazar las columnas, llame a [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) para establecer SQL_SOPT_SS_PARAM_FOCUS en el ordinal del parámetro con valores de tabla. A continuación, llame a SQLBindParameter para cada columna del parámetro con valores de tabla. Para volver a los enlaces de parámetro de nivel superior, establezca SQL_SOPT_SS_PARAM_FOCUS en 0.  
  
 Para obtener información sobre la asignación de parámetros a campos descriptores para parámetros con valores de tabla, vea [enlazar y transferencia de datos de parámetros de Table-Valued y valores de columna](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md).  
  
 Para obtener más información sobre los parámetros con valores de tabla, vea [parámetros con valores de tabla &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlbindparameter-support-for-enhanced-date-and-time-features"></a>SQLBindParameter admite las características mejoradas de fecha y hora  
 Los valores de parámetro de los tipos de fecha y hora se convierten como se describe en [conversiones de C a SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md). Observe que los parámetros de tipo **time** y **datetimeoffset** deben tener *ValueType* especificado como **SQL_C_DEFAULT** o **SQL_C_BINARY** si se usan sus estructuras correspondientes (**SQL_SS_TIME2_STRUCT** y **SQL_SS_TIMESTAMPOFFSET_STRUCT**).  
  
 Para obtener más información, vea [mejoras de fecha y hora &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlbindparameter-support-for-large-clr-udts"></a>Compatibilidad con SQLBindParameter para los UDT CLR grandes  
 **SQLBindParameter** admite tipos definidos por el usuario (UDT) CLR grandes. Para obtener más información, vea [tipos de User-Defined CLR grandes &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Consulte también  
 [Detalles de implementación de la API de ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [Función SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md)  
  

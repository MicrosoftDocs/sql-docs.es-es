---
description: Conversiones del tipo de datos de fecha y hora de C a SQL
title: Conversiones de C a SQL | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- conversions [ODBC], C to SQL
ms.assetid: 7ac098db-9147-4883-8da9-a58ab24a0d31
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a65be08afc1a8570c64f7ffabddcb6c2d2d51b1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438558"
---
# <a name="datetime-data-type-conversions-from-c-to-sql"></a>Conversiones del tipo de datos de fecha y hora de C a SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  En este tema se enumeran los problemas que se deben tener en cuenta al convertir tipos de C a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos de fecha y hora.  
  
 Las conversiones descritas en la tabla siguiente se aplican a conversiones realizadas en el cliente. En los casos en los que el cliente especifica una precisión de fracciones de segundo para un parámetro que difiere de la definida en el servidor, la conversión del cliente podría realizarse correctamente, pero el servidor devolverá un error cuando se llame a **SQLExecute** o **SQLExecuteDirect** . En concreto, ODBC trata cualquier truncamiento de fracciones de segundo como un error, mientras que el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamiento es redondear; por ejemplo, el redondeo se produce cuando se pasa de **datetime2 (6)** a **datetime2 (2)**. Los valores de las columnas datetime se redondean a la fracción 1/300 de segundo, mientras que para las columnas smalldatetime, el servidor establece los segundos en cero.  
  
|   | SQL_TYPE_DATE | SQL_TYPE_TIME | SQL_SS_TIME2 | SQL_TYPE_TIMESTAMP | SQL_SS_TIMESTAMPOFFSET | SQL_CHAR | SQL_WCHAR |
| - | ------------- | ------------- | ------------ | ------------------ | --------------------- | -------- | --------- |
| **SQL_C_DATE** |1|-|-|1,6|1,5,6|1,13|1,13|  
| **SQL_C_TIME** |-|1|1|1,7|1, 5, 7|1,13|1,13|  
| **SQL_C_SS_TIME2** |-|1,3|1,10|1,7|1, 5, 7|1,13|1,13|  
| **SQL_C_BINARY (SQL_SS_TIME2_STRUCT)** |N/D|N/D|1,10,11|N/D|N/D|N/D|N/D|  
| **SQL_C_TYPE_TIMESTAMP** |1,2|1,3,4|1,4,10|1,10|1,5,10|1,13|1,13|  
| **SQL_C_SS_TIMESTAMPOFFSET** |1,2,8|1,3,4,8|1,4,8,10|1,8,10|1,10|1,13|1,13|  
| **SQL_C_BINARY(SQL_SS_TIMESTAMPOFFSET_STRUCT)** |N/D|N/D|N/D|N/D|1,10,11|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (date)** |9|9|9|9,6|9,5,6|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (time2)** |9|9, 3|9,10|9,7,10|9,5,7,10|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (datetime)** |9,2|9, 3, 4|9,4,10|9,10|9,5,10|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (datetimeoffset)** |9,2,8|9, 3, 4, 8|9,4,8,10|9,8,10|9,10|N/D|N/D|  
| **SQL_C_BINARY(SQL_DATE_STRUCT)** |1,11|N/D|N/D|N/D|N/D|N/D|N/D|  
| **SQL_C_BINARY(SQL_TIME_STRUCT)** |N/D|N/D|N/D|N/D|N/D|N/D|N/D|  
| **SQL_C_BINARY(SQL_TIMESTAMP_STRUCT)** |N/D|N/D|N/D|N/D|N/D|N/D|N/D|  
  
## <a name="key-to-symbols"></a>Clave de los símbolos  
  
-   **-**: No se admite la conversión. Se genera un registro de diagnóstico con SQLSTATE 07006 y el mensaje "Infracción del atributo de tipo de datos restringido".  
  
-   **1**: si los datos proporcionados no son válidos, se genera un registro de diagnóstico con SQLSTATE 22007 y el mensaje "formato de fecha y hora no válido".  
  
-   **2**: los campos de hora deben ser cero o se genera un registro de diagnóstico con SQLSTATE 22008 y el mensaje "truncamiento fraccionario".  
  
-   **3**: las fracciones de segundo deben ser cero o se genera un registro de diagnóstico con SQLSTATE 22008 y el mensaje "truncamiento fraccionario".  
  
-   **4**: se omite el componente de fecha.  
  
-   **5**: la zona horaria se establece en la configuración de zona horaria del cliente.  
  
-   **6**: la hora se establece en cero.  
  
-   **7**: la fecha se establece en la fecha actual.  
  
-   **8**: la hora se convierte de la zona horaria del cliente a UTC. Si se produce un error durante esta conversión, se genera un registro de diagnóstico con SQLSTATE 22008 y el mensaje "Desbordamiento del campo DateTime".  
  
-   **9**: la cadena se analiza y se convierte en un valor Date, DateTime, DateTimeOffset o Time, dependiendo del primer carácter de puntuación encontrado y de la presencia de los componentes restantes. A continuación, la cadena se convierte al tipo de destino, siguiendo las reglas de la tabla anterior para el tipo de origen detectado por este proceso. Si se detecta un error mientras se analizan los datos, se genera un registro de diagnóstico con SQLSTATE 22018 y el mensaje "Valor de carácter no válido para especificación cast". Para los parámetros datetime y smalldatetime, si el año está fuera del intervalo admitido por estos tipos, se genera un registro de diagnóstico con SQLSTATE 22007 y el mensaje "Formato de fecha y hora no válido".  
  
     Para datetimeoffset, el valor debe estar comprendido dentro del intervalo después de la conversión a UTC, aunque no se haya solicitado ninguna conversión a UTC. Esto se debe a que la secuencia de datos tabulares (TDS) y el servidor siempre normalizan la hora en valores datetimeoffset para UTC, de modo que el cliente tenga que comprobar que los componentes de hora están dentro del intervalo admitido después de la conversión a UTC. Si el valor no está dentro del intervalo UTC admitido, se genera un registro de diagnóstico con SQLSTATE 22007 y el mensaje "Formato de fecha y hora no válido".  
  
-   **10**: si se produce un truncamiento con pérdida de datos, se genera un registro de diagnóstico con SQLSTATE 22008 y el mensaje "formato de hora no válido". Este error también se produce si el valor está fuera del intervalo que puede representarse mediante el intervalo UTC utilizado por el servidor.  
  
-   **11**: Si la longitud de bytes de los datos no es igual al tamaño de la estructura requerida por el tipo SQL, se genera un registro de diagnóstico con SQLSTATE 22003 y el mensaje "valor numérico fuera del intervalo".  
  
-   **12**: Si la longitud de bytes de los datos es 4 u 8, los datos se envían al servidor en formato smalldatetime o DateTime TDS sin formato. Si la longitud de bytes de los datos coincide exactamente con el tamaño de SQL_TIMESTAMP_STRUCT, los datos se convierten al formato TDS para datetime2.  
  
-   **13**: si se produce un truncamiento con pérdida de datos, se genera un registro de diagnóstico con SQLSTATE 22001 y el mensaje "datos de cadena, truncados a la derecha".  
  
     El número de dígitos de fracciones de segundo (la escala) se determina a partir del tamaño de la columna de destino de acuerdo con la tabla siguiente:  
  
    |   | Escala supuesta | Escala supuesta |
    | - | ------------- | ------------- |
    | **Tipo** | 0 | 1.. 9 |  
    |**SQL_C_TYPE_TIMESTAMP** |19|21..29|  
  
     Sin embargo, para SQL_C_TYPE_TIMESTAMP, si las fracciones de segundo se pueden representar con tres dígitos sin perder datos y el tamaño de columna es 23 o superior, se generan exactamente tres dígitos de fracciones de segundo. Este comportamiento asegura la compatibilidad con versiones anteriores para aplicaciones desarrolladas utilizando controladores ODBC anteriores.  
  
     Si el tamaño de las columnas es mayor que el intervalo de la tabla, se presupone una escala de 9. Esta conversión debe permitir hasta nueve dígitos de fracciones de segundo, el máximo permitido por ODBC.  
  
     Un tamaño de columna igual a cero implica un tamaño ilimitado de los tipos de caracteres de longitud variable en ODBC (9 dígitos, a menos que sea aplicable la regla de 3 dígitos para SQL_C_TYPE_TIMESTAMP). Especificar un tamaño de columna igual a cero con un tipo de caracteres de longitud fija es un error.  
  
-   **N/A**: [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] se mantiene el comportamiento existente y el anterior.  
  
## <a name="see-also"></a>Consulte también  
 [Mejoras de fecha y hora &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
  
  

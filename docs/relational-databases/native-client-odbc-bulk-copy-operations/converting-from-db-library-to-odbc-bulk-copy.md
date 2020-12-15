---
description: Convertir un programa de copia masiva de DB-Library a ODBC
title: Convertir de DB-Library a copia masiva de ODBC | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- converting DB-Library to ODBC bulk copy
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC], DB-Library bulk copy
- ODBC, bulk copy operations
- DB-Library bulk copy
ms.assetid: 0bc15bdb-f19f-4537-ac6c-f249f42cf07f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 488f3f1583a58431c5606fade81e9eb3f56bd076
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465036"
---
# <a name="converting-from-db-library-to-odbc-bulk-copy"></a>Convertir un programa de copia masiva de DB-Library a ODBC
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Convertir un programa de copia masiva de DB-Library en ODBC es fácil porque las funciones de copia masiva que admite el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controlador ODBC de Native Client son similares a las funciones de copia masiva DB-Library, con las siguientes excepciones:  
  
-   Las aplicaciones DB-Library pasan un puntero a una estructura DBPROCESS como primer parámetro de las funciones de copia masiva. En las aplicaciones ODBC, el puntero a DBPROCESS se reemplaza por un identificador de conexión ODBC.  
  
-   DB-Library aplicaciones llaman a **BCP_SETL** antes de conectarse para habilitar las operaciones de copia masiva en DBPROCESS. En su lugar, las aplicaciones ODBC llaman a [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) antes de conectarse para habilitar las operaciones masivas en un identificador de conexión:  
  
    ```  
    SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP,  
        (void *)SQL_BCP_ON, SQL_IS_INTEGER);  
    ```  
  
-   El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controlador ODBC de Native Client no admite DB-Library los controladores de mensajes y errores; debe llamar a **SQLGetDiagRec** para obtener los errores y mensajes generados por las funciones de copia masiva de ODBC. Las versiones ODBC de las funciones de copia masiva devuelven los códigos de retorno de copia masiva estándar, es decir, SUCCEED o FAILED, no códigos de retorno de estilo ODBC, como SQL_SUCCESS o SQL_ERROR.  
  
-   Los valores especificados para el DB-Library [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)parámetro *varlen* se interpretan de forma diferente que el parámetro _cbData_ de ODBC **bcp_bind**.  
  
    |Condición indicada|DB-Library valor *varlen*|Valor *cbData* de ODBC|  
    |-------------------------|--------------------------------|-------------------------|  
    |Se suministraron valores NULL|0|-1 (SQL_NULL_DATA)|  
    |Se suministraron datos variables|-1|-10 (SQL_VARLEN_DATA)|  
    |Cadena binaria o carácter de longitud cero|NA|0|  
  
     En DB-Library, un valor *varlen* de-1 indica que se están suministrando datos de longitud variable, que en el *cbData* de ODBC se interpreta para indicar que solo se proporcionan valores NULL. Cambie cualquier DB-Library especificaciones de *varlen* de-1 a SQL_VARLEN_DATA y cualquier especificación de *varlen* de 0 a SQL_NULL_DATA.  
  
-   Los DB-Library **bcp_colfmt**_file_collen_ y ODBC [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)*cbUserData* tienen el mismo problema que los parámetros **bcp_bind**_varlen_ y *cbData* indicados anteriormente. Cambie cualquier DB-Library *file_collen* especificaciones de-1 a SQL_VARLEN_DATA y cualquier especificación de *file_collen* de 0 a SQL_NULL_DATA.  
  
-   El parámetro *iValue* de la función [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) de ODBC es un puntero void. En DB-Library, *iValue* era un entero. Convierta los valores de *iValue* de ODBC a void *.  
  
-   La opción **BCP_CONTROL** BCPMAXERRS especifica el número de filas individuales que pueden tener errores antes de que se produzca un error en una operación de copia masiva. El valor predeterminado de BCPMAXERRS es 0 (error en el primer error) en la versión DB-Library de **bcp_control** y 10 en la versión de ODBC. DB-Library las aplicaciones que dependen del valor predeterminado de 0 para finalizar una operación de copia masiva deben cambiarse para llamar a la **BCP_CONTROL** ODBC para establecer BCPMAXERRS en 0.  
  
-   La función **bcp_control** de ODBC admite las siguientes opciones no admitidas por la versión DB-Library de **bcp_control**:  
  
    -   BCPODBC  
  
         Cuando se establece en TRUE, especifica que los valores **DateTime** y **smalldatetime** guardados en formato de caracteres tendrán el prefijo y el sufijo de la secuencia de escape de la marca de tiempo de ODBC. Esto solo se aplica a las operaciones BCP_OUT.  
  
         Con BCPODBC establecido en FALSE, un valor **DateTime** convertido en una cadena de caracteres se muestra como:  
  
        ```  
        1997-01-01 00:00:00.000  
        ```  
  
         Con BCPODBC establecido en TRUE, el mismo valor **DateTime** se genera como:  
  
        ```  
        {ts '1997-01-01 00:00:00.000' }  
        ```  
  
    -   BCPKEEPIDENTITY  
  
         Cuando se establece en TRUE, especifica que las funciones de copia masiva deben insertar valores de datos suministrados para las columnas con restricciones de identidad. Si no se establece, se generan nuevos valores de identidad para las filas insertadas.  
  
    -   BCPHINTS  
  
         Especifica diversas optimizaciones de copia masiva. Esta opción no puede usarse en la versión 6.5 ni en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
    -   BCPFILECP  
  
         Especifica la página de códigos del archivo de copia masiva.  
  
    -   BCPUNICODEFILE  
  
         Especifica que un archivo de copia masiva en modo de carácter es un archivo Unicode.  
  
-   La función **bcp_colfmt** de ODBC no admite el indicador de *File_Type* de SQLCHAR porque entra en conflicto con la definición de tipo SQLCHAR de ODBC. En su lugar, use SQLCHARACTER para **bcp_colfmt**.  
  
-   En las versiones ODBC de las funciones de copia masiva, el formato para trabajar con valores **DateTime** y **smalldatetime** en cadenas de caracteres es el formato ODBC de AAAA-MM-DD HH: mm: SS. SSS; los valores **smalldatetime** usan el formato ODBC de AAAA-MM-DD HH: mm: SS.  
  
     Las versiones DB-Library de las funciones de copia masiva aceptan valores **DateTime** y **smalldatetime** en cadenas de caracteres mediante varios formatos:  
  
    -   El formato predeterminado es *MMM DD AAAA HH: mmxx* , donde *XX* es AM o PM.  
  
    -   cadenas de caracteres **DateTime** y **smalldatetime** en cualquier formato admitido por la función DB-Library **dbconvert** .  
  
    -   Cuando se activa la casilla **Usar configuración internacional** en la pestaña **Opciones** de DB-Library de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herramienta de red de cliente, las funciones de copia masiva DB-Library también aceptan fechas en el formato de fecha regional definido para la configuración regional del registro del equipo cliente.  
  
     Las funciones de copia masiva DB-Library no aceptan los formatos **DateTime** y **smalldatetime** de ODBC.  
  
     Si el atributo SQL_SOPT_SS_REGIONALIZE de la instrucción está establecido en SQL_RE_ON, las funciones de copia masiva de ODBC aceptan fechas en el formato de fecha regional definido para la configuración regional del Registro del equipo cliente.  
  
-   Al generar valores de **moneda** en formato de caracteres, las funciones de copia masiva de ODBC proporcionan cuatro dígitos de precisión y sin separadores de coma; DB-Library versiones solo proporcionan dos dígitos de precisión e incluyen los separadores de comas.  
  
## <a name="see-also"></a>Consulte también  
 [Realizar operaciones de copia masiva &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)   
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  

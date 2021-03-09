---
title: Instrucciones de programación (controlador ODBC)
description: Las características de programación de Microsoft ODBC Driver for SQL Server en macOS y Linux se basan en ODBC en SQL Server Native Client (ODBC).
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: v-makouz
ms.author: v-daenge
ms.openlocfilehash: 6d602db9c189b6e7fce8b767b60204253ccb7f67
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464401"
---
# <a name="programming-guidelines"></a>Instrucciones de programación

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Las características de programación de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en macOS y Linux se basan en ODBC de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)). [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client se basa en ODBC de los Componentes de Windows Data Access ([Referencia del programador de ODBC](../../../odbc/reference/odbc-programmer-s-reference.md)).  

Una aplicación de ODBC puede utilizar conjuntos de resultados activos múltiples (MARS) y otras [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]características específicas incluyendo `/usr/local/include/msodbcsql.h` después de agregar los encabezados de unixODBC (`sql.h`, `sqlext.h`, `sqltypes.h` y `sqlucode.h`). Luego use los mismos nombres simbólicos para los elementos específicos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que usaría en las aplicaciones de ODBC para Windows.

## <a name="available-features"></a>Características disponibles  
Las siguientes secciones de la documentación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client para ODBC ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)) son válidas cuando se usa el controlador ODBC en macOS y Linux:  

-   [Comunicar con SQL Server (ODBC)](../../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
-   [Compatibilidad con el tiempo de espera de la conexión y consulta](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
-   [Cursores](../../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
-   [Mejoras en los tipos de datos de fecha y hora (ODBC)](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
-   [Ejecutar consultas (ODBC)](../../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
-   [Controlar errores y mensajes](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
-   [Autenticación Kerberos](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
-   [Tipos CLR grandes definidos por el usuario (ODBC)](../../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)  
-   [Realización de transacciones (ODBC) (excepto las transacciones distribuidas)](../../../relational-databases/native-client/odbc/performing-transactions-in-odbc.md)  
-   [Procesar resultados (ODBC)](../../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
-   [Ejecutar procedimientos almacenados](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)
-   [Compatibilidad con columnas dispersas (ODBC)](../../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)
-   [Utilizar el cifrado sin validación](../../../relational-databases/native-client/features/using-encryption-without-validation.md)
-   [Parámetros con valores de tabla](../../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)
-   [UTF-8 y UTF-16 para la API de datos y comandos](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)
-   [Utilizar funciones de catálogo](../../../relational-databases/native-client/odbc/using-catalog-functions.md)  

## <a name="unsupported-features"></a>Características no admitidas

Las siguientes características no se han verificado para funcionar correctamente con esta versión del controlador ODBC en macOS y Linux:

-   Conexión de clúster de conmutación por error
-   [Resolución de IP de red transparente](../using-transparent-network-ip-resolution.md) (antes, versión 17 del controlador ODBC)
-   [Seguimiento de controlador avanzado](/archive/blogs/mattn/enabling-advanced-driver-tracing-for-the-sql-native-client-odbc-drivers)

Las siguientes características no están disponibles en esta versión del controlador ODBC en macOS y Linux: 

-   Transacciones distribuidas (no se admite el atributo SQL_ATTR_ENLIST_IN_DTC)  
-   Creación de reflejo de la base de datos  
-   FILESTREAM  
-   Generación de perfiles de rendimiento del controlador ODBC, que se describe en [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md), y los siguientes atributos de conexión relacionados con el rendimiento:  
    -   SQL_COPT_SS_PERF_DATA  
    -   SQL_COPT_SS_PERF_DATA_LOG  
    -   SQL_COPT_SS_PERF_DATA_LOG_NOW  
    -   SQL_COPT_SS_PERF_QUERY  
    -   SQL_COPT_SS_PERF_QUERY_INTERVAL  
    -   SQL_COPT_SS_PERF_QUERY_LOG  
-   SQLBrowseConnect (antes de la versión 17.2)
-   Los tipos de intervalo de C como SQL_C_INTERVAL_YEAR_TO_MONTH (se trata en [Identificadores y descriptores de tipo de datos](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md))
-   El valor SQL_CUR_USE_ODBC del atributo SQL_ATTR_ODBC_CURSORS de la función SQLSetConnectAttr.

## <a name="character-set-support"></a>Compatibilidad con juegos de caracteres

Para la versión 13 y 13.1 del controlador ODBC, los datos de SQLCHAR deben ser UTF-8. No se admite ninguna otra codificación.

Para la versión 17 del controlador ODBC, se admiten datos de SQLCHAR en una de las siguientes codificaciones o conjuntos de caracteres:

> [!NOTE]  
> Debido a diferencias de `iconv` en `musl` y `glibc`, no se admiten muchas de estas configuraciones regionales en Alpine Linux.
>
> Para más información, consulte el artículo sobre las [diferencias funcionales de glibc](https://wiki.musl-libc.org/functional-differences-from-glibc.html).

|Nombre|Descripción|
|-|-|
|UTF-8|Unicode|
|CP437|MS-DOS Latín EE. UU.|
|CP850|MS-DOS Latín 1|
|CP874|Latín/tailandés|
|CP932|Japonés, Shift-JIS|
|CP936|Chino simplificado, GBK|
|CP949|Coreano, EUC-KR|
|CP950|Chino tradicional, Big5|
|CP1251|Cirílico|
|CP1253|Griego|
|CP1256|Árabe|
|CP1257|Báltico|
|CP1258|Vietnamita|
|ISO-8859-1 / CP1252|Latín-1|
|ISO-8859-2 / CP1250|Latín-2|
|ISO-8859-3|Latín-3|
|ISO-8859-4|Latín-4|
|ISO-8859-5|Latín/cirílico|
|ISO-8859-6|Latín/árabe|
|ISO-8859-7|Latín/griego|
|ISO-8859-8 / CP1255|Hebreo|
|ISO-8859-9 / CP1254|Turco|
|ISO-8859-13|Latín-7|
|ISO-8859-15|Latín-9|

Al establecer la conexión, el controlador detecta la configuración regional actual del proceso en el que se carga. Si utiliza una de las codificaciones anteriores, el controlador utiliza esa codificación para los datos de SQLCHAR (caracteres estrechos). En caso contrario, el valor predeterminado es UTF-8. Dado que todos los procesos empiezan de forma predeterminada en la configuración regional "C" (y esto hace que el valor predeterminado del controlador sea UTF-8), si una aplicación necesita utilizar una de las codificaciones anteriores, debe usar la función **setlocale** para establecer la configuración regional adecuadamente antes de conectarse, ya sea especificando la configuración regional deseada de forma explícita o mediante una cadena vacía, como `setlocale(LC_ALL, "")`, para usar la configuración regional del entorno.

Por lo tanto, en un entorno típico de Linux o macOS, donde la codificación es UTF-8, los usuarios de la versión 17 del controlador ODBC que actualizan desde la versión 13 o 13.1 no observarán ninguna diferencia. Sin embargo, las aplicaciones que usan una codificación que no es UTF-8 en la lista anterior a través de `setlocale()` deben usar esa codificación para los datos hacia y desde el controlador en lugar de UTF-8.

Los datos de SQLWCHAR deben tener la codificación UTF-16LE (Little Endian).

Al enlazar parámetros de entrada con SQLBindParameter, si se especifica un carácter estrecho de tipo SQL como SQL_VARCHAR, el controlador convierte los datos proporcionados en la codificación del cliente a la codificación [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (normalmente, la página de códigos 1252) predeterminada. Para los parámetros de salida, el controlador lleva a cabo la conversión desde la codificación especificada en la información de intercalación asociada con los datos a la codificación del cliente. Sin embargo, es posible que se pierdan datos: los caracteres de la codificación de origen que no se puedan representar en la codificación de destino se convertirán en un signo de interrogación ("?").

Para evitar que se pierdan datos al enlazar parámetros de entrada, especifique un tipo de carácter Unicode SQL, como SQL_NVARCHAR. En este caso, el controlador lleva a cabo la conversión desde la codificación del cliente en UTF-16, con el que se pueden representar todos los caracteres Unicode. Además, la columna de destino o el parámetro en el servidor debe ser también un tipo de Unicode (**nchar**, **nvarchar**, **ntext**) o uno con una intercalación o codificación, que puede representar todos los caracteres de los datos de origen. Para evitar que se pierdan datos con los parámetros de salida, especifique un tipo SQL de Unicode y un tipo C de Unicode (SQL_C_WCHAR), que haga que el controlador devuelva datos como UTF-16, o bien un tipo C estrecho, y asegúrese de que la codificación del cliente puede representar todos los caracteres del origen de datos (esto siempre es posible con UTF-8.)

Para obtener más información sobre las intercalaciones y las codificaciones, vea [Compatibilidad con la intercalación y Unicode](../../../relational-databases/collations/collation-and-unicode-support.md).

Hay algunas diferencias de conversión de codificación entre Windows y varias versiones de la biblioteca de iconv en Linux y macOS. Los datos de texto en la página de códigos 1255 (hebreo) tienen un punto de código (0xCA) que se comporta de forma distinta durante la conversión a Unicode. En Windows, este carácter se convierte en el punto de código UTF-16 de 0x05BA. En macOS y Linux con versiones de libiconv anteriores a 1.15, se convierte en 0x00CA. En equipos Linux con bibliotecas de iconv que no admiten la revisión de 2003 de Big5/CP950 (denominada `BIG5-2003`), los caracteres agregados con esa revisión no se convierten correctamente. En la página de códigos 932 (japonés, Shift-JIS), el resultado de la descodificación de caracteres que no se definen originalmente en el estándar de codificación también difiere. Por ejemplo, el byte 0x80 se convierte en U+0080 en Windows, pero puede llegar a ser U+30FB en Linux y macOS, según la versión de iconv.

En ODBC Driver 13 y 13.1, cuando los caracteres multibyte UTF-8 o UTF-16 suplentes se dividen en búferes de SQLPutData, se producen daños en los datos. Los búferes se utilizan para transmitir datos de SQLPutData sin codificaciones de caracteres parciales. Esta limitación se ha quitado con la versión 17 del controlador ODBC.

## <a name="openssl"></a><a name="bkmk-openssl"></a>OpenSSL
A partir de la versión 17.4, el controlador carga OpenSSL de manera dinámica, lo que permite que se ejecute en sistemas que tienen la versión 1.0 o 1.1 sin necesidad de archivos de controlador independientes. Cuando hay varias versiones de OpenSSL presentes, el controlador intentará cargar la más reciente. El controlador admite actualmente OpenSSL 1.0.x y 1.1.x.

> [!NOTE]  
> Puede producirse un posible conflicto si la aplicación que usa el controlador (o uno de sus componentes) está vinculada a una versión diferente de OpenSSL o si la carga de manera dinámica. Si hay varias versiones de OpenSSL presentes en el sistema y la aplicación las utiliza, se recomienda encarecidamente que se asegure de que no haya un error de coincidencia entre la versión cargada por la aplicación y el controlador, ya que los errores podrían dañar la memoria y, por tanto, no incluirá necesariamente manifiestos de maneras obvias o coherentes.

## <a name="alpine-linux"></a><a name="bkmk-alpine"></a>Alpine Linux
En el momento de redactar este documento, el tamaño predeterminado de la pila en MUSL es 128 K, que es suficiente para la funcionalidad básica del controlador ODBC, pero en función de lo que hace la aplicación no es difícil superar este límite, especialmente cuando se llama al controlador desde varios subprocesos. Se recomienda compilar una aplicación ODBC en Alpine Linux con `-Wl,-z,stack-size=<VALUE IN BYTES>` para aumentar el tamaño de la pila. Como referencia, el tamaño predeterminado de la pila en la mayoría de los sistemas GLIBC es de 2 MB.

## <a name="additional-notes"></a>Notas adicionales  

1.  Puede realizar una conexión de administrador dedicada (DAC) con la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y **host,port**. Un miembro de la función Sysadmin debe detectar primero el puerto DAC. Para descubrir cómo, consulte [Conexión de diagnóstico para administradores de bases de datos](../../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md#dac-port). Por ejemplo, si el puerto DAC fuera 33000, podría conectarse a él con `sqlcmd` de la siguiente forma:  

    ```
    sqlcmd -U <user> -P <pwd> -S <host>,33000
    ```

    > [!NOTE]  
    > Las conexiones DAC deben usar la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
    
2.  El Administrador de controladores unixODBC devuelve el mensaje "Identificador de opción o atributo no válido" en todos los atributos de instrucción cuando se transmiten a través de SQLSetConnectAttr. En Windows, cuando SQLSetConnectAttr recibe un valor de atributo de instrucción, el controlador tiene que establecer ese valor en todas las instrucciones activas que pertenecen al identificador de conexión.  

3.  Al usar el controlador con aplicaciones multiproceso, la validación del identificador de unixODBC puede convertirse en un cuello de botella de rendimiento. En estos escenarios, se puede obtener significativamente más rendimiento compilando unixODBC con la opción `--enable-fastvalidate`. Sin embargo, tenga cuidado de que esto puede hacer que las aplicaciones que pasan identificadores no válidos a las API de ODBC se bloqueen en lugar de devolver errores de `SQL_INVALID_HANDLE`.

## <a name="see-also"></a>Consulte también  
[Preguntas más frecuentes](frequently-asked-questions-faq-for-odbc-linux.yml)

[Problemas conocidos en esta versión del controlador](known-issues-in-this-version-of-the-driver.md)

[Notas de la versión](release-notes-odbc-sql-server-linux-mac.md)

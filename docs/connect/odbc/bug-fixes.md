---
title: Lista de los errores corregidos
description: Esta página contiene una lista de los errores corregidos en cada versión, a partir de Microsoft ODBC Driver 17 for SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-chojas
ms.author: v-jizho2
manager: kenvh
ms.openlocfilehash: 1816f1a8255041d6211931ffc41963acc460e15d
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770482"
---
# <a name="list-of-bugs-fixed"></a>Lista de los errores corregidos

Esta página contiene una lista de los errores corregidos en cada versión, a partir de [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="bug-fixes-in-the-msconame-odbc-driver-1772-for-ssnoversion"></a>Correcciones de errores en [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.7.2 para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Corrección de un problema con los errores de tipo "404 No encontrado" al usar la autenticación de Managed Service Identity
- Corrección de los errores intermitentes de tipo "No se admite el cifrado" en cargas multiproceso altas
- Corrección del bloqueo intermitente en cargas multiproceso altas

### <a name="bug-fixes-in-the-msconame-odbc-driver-177-for-ssnoversion"></a>Correcciones de errores en [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.7 para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Corrección de la codificación de caracteres de las columnas VARIANT en el modo nativo de BCP
- Corrección del valor de SQL_ATTR_PARAMS_PROCESSED_PTR en condiciones específicas
- Corrección de la función SQLDescribeParam en el modo FMTONLY para las instrucciones que contienen comentarios
- Corrección de un problema con la autenticación federada al usar Okta
- Corrección del uso excesivo de memoria en sistemas de varios procesadores
- Corrección de la autenticación de Azure AD para algunas variantes de Azure SQL Database

### <a name="bug-fixes-in-the-msconame-odbc-driver-176-for-ssnoversion"></a>Correcciones de errores en [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.6 para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Corrección del error de ADAL al autenticarse con una cuenta federada (Windows)
- Corrección un problema en el que el controlador deja de responder cuando se produce un tiempo de espera durante una operación de notificación asincrónica
- Corrección del recuento de referencias del controlador tras la actualización en Alpine Linux
- Corrección de la versión de dependencia de libc6 para Ubuntu
- Incorporación de definiciones que faltan a msodbcsql.h de Linux o Mac

### <a name="bug-fixes-in-the-msconame-odbc-driver-17522-for-ssnoversion-alpine-linux-only"></a>Correcciones de errores en el controlador ODBC 17.5.2.2 de [!INCLUDE[msCoName](../../includes/msconame_md.md)] para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (solo Alpine Linux)

- Corrección de un bloqueo al usar Always Encrypted con enclaves seguros en Alpine Linux

### <a name="bug-fixes-in-the-msconame-odbc-driver-1752-for-ssnoversion"></a>Correcciones de errores en el controlador ODBC 17.5.2 de [!INCLUDE[msCoName](../../includes/msconame_md.md)] para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Se ha agregado msodbcsql.h al paquete de Alpine Linux.

### <a name="bug-fixes-in-the-msconame-odbc-driver-175-for-ssnoversion"></a>Correcciones de errores en el Controlador ODBC 17.5 de [!INCLUDE[msCoName](../../includes/msconame_md.md)]para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Corrección del cálculo de hash de metadatos de AKV CMK en Linux/macOS
- Corrección del error al cargar OpenSSL 1.0.0
- Corrección de problemas de conversión al usar las páginas de código de ISO-8859-1 e ISO-8859-2
- Corrección del nombre de la biblioteca interna en macOS para incluir el número de versión
- Corrección del valor de un indicador NULL cuando se utilizan enlaces de indicador y longitud independientes

### <a name="bug-fixes-in-the-msconame-odbc-driver-1742-for-ssnoversion"></a>Correcciones de errores en el Controlador ODBC 17.4.2 de [!INCLUDE[msCoName](../../includes/msconame_md.md)]para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 - Corrección de un problema en el que el identificador de proceso y el nombre de la aplicación no se enviarían correctamente a SQL Server (para el análisis de sys.dm_exec_sessions) (Linux)
 - Se quitó la dependencia redundante de libuuid (Linux)
 - Corrección de un error al enviar datos UTF8 a SQL Server 2019
 - Corrección de un error en el que las configuraciones regionales que finalizan en "@euro" no se detectaban correctamente (Linux)
 - Corrección para los datos XML que se devuelven de manera incorrecta cuando se capturan como parámetro de salida mientras se usa Always Encrypted

### <a name="bug-fixes-in-the-msconame-odbc-driver-174-for-ssnoversion"></a>Correcciones de errores en el Controlador ODBC 17.4 de [!INCLUDE[msCoName](../../includes/msconame_md.md)]para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Corrección de un problema intermitente que hace que el controlador deje de responder cuando se habilitan conjuntos de resultados activos múltiples (MARS)
- Corrección de un problema de resistencia de conexión que hace que el controlador deje de responder cuando se habilita la notificación asincrónica
- Corrección del bloqueo al recuperar registros de diagnóstico para intentos de conexión multiproceso
- Corrección del error de cifrado no compatible tras la reconexión después de llamar a SQLGetInfo() con SQL_USER_NAME y SQL_DATA_SOURCE_READ_ONLY
- Corrección del error de inicialización COM durante la autenticación interactiva de Azure Active Directory
- Corrección de SQLGetData() para datos UTF8 de varios bytes
- Corrección de la recuperación de la longitud de las columnas sql_variant mediante SQLGetData()
- Corrección de la importación de las columnas sql_variant que contienen más de 7992 bytes mediante bcp
- Corrección del envío de la codificación correcta al servidor para datos de caracteres estrechos

### <a name="bug-fixes-in-the-msconame-odbc-driver-173-for-ssnoversion"></a>Correcciones de errores en el Controlador ODBC 17.3 de [!INCLUDE[msCoName](../../includes/msconame_md.md)]para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Se corrigió la fuga de memoria del identificador de eventos de notificación de envío de TCP
- Se corrigió el problema de redefinición de la enumeración _SQL_FILESTREAM_DESIRED_ACCESS en el archivo de encabezado msodbcsql.h
- Se corrigió la falta de una definición relacionada con ACCESS_TOKEN y AUTHENTICATION en el archivo de encabezado msodbcsql.h para Linux

### <a name="bug-fixes-in-the-msconame-odbc-driver-172-for-ssnoversion"></a>Correcciones de errores en el Controlador ODBC 17.2 de [!INCLUDE[msCoName](../../includes/msconame_md.md)]para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Se corrigió un mensaje de error sobre la autenticación de Azure Active Directory
- Se corrigió la detección de codificación cuando las variables de entorno de la configuración regional están establecidas de manera distinta
- Se corrigió un bloqueo al desconectarse con la recuperación de la conexión en curso
- Se corrigió la detección de la ejecución de la conexión
- Se corrigió la detección incorrecta de los sockets cerrados
- Se corrigió una espera infinita al intentar liberar un identificador de instrucción durante la recuperación con errores
- Se corrigió el comportamiento incorrecto de la desinstalación cuando se instalan las versiones 13 y 17 en Windows
- Se corrigió el comportamiento de cifrado en una plataforma Windows anterior (Windows 7, 8 y Server 2012)
- Se corrigió un problema de caché al usar la autenticación ADAL en Windows
- Se corrigió un problema que bloqueaba y sobrescribía los registros de seguimiento en Windows

### <a name="bug-fixes-in-the-msconame-odbc-driver-171-for-ssnoversion"></a>Correcciones de errores en el Controlador ODBC 17.1 de [!INCLUDE[msCoName](../../includes/msconame_md.md)]para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Se corrigió el retraso de 1 segundo al llamar a SQLFreeHandle con MARS habilitado y el atributo de conexión "Encrypt=yes"
- Se corrigió un error de bloqueo 22003 en SQLGetData cuando el tamaño del búfer pasado es más pequeño que los datos que se recuperan (Windows)
- Se corrigieron los mensajes de error de ADAL truncados
- Se corrigió un error poco frecuente en Windows de 32 bits al convertir un número de punto flotante en un entero
- Se corrigió un problema que hacía que la inserción doble en el campo decimal con Always Encrypted devolvería un error de truncamiento de datos
- Se corrigió una advertencia en el instalador de macOS
- Se corrigió el envío de un estado incorrecto a SQL Server durante el intento de recuperación de la sesión cuando tanto la resistencia de la conexión como la agrupación de conexiones están habilitadas, lo que provoca que el servidor deje la sesión

### <a name="bug-fixes-in-the-msconame-odbc-driver-17-for-ssnoversion"></a>Correcciones de errores en el Controlador ODBC 17 de [!INCLUDE[msCoName](../../includes/msconame_md.md)]para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Se corrigió un error en el que, al usar la autenticación de Kerberos, la inserción masiva podía generar el error de acceso denegado
- Se quitó la solución alternativa para un error unixODBC presente en versiones anteriores a 2.3.1 (el controlador ha duplicado los tamaños de ciertos búferes pasados a unixODBC)
- Se ha corregido la falta de respuesta de la resistencia de la conexión (reconexión) al usar ColumnEncryption=Enabled
- Se corrigió el error de creación de DSN donde, al usar la opción "Autenticación interactiva de Active Directory ", la ventana de autenticación de Azure podría dejar de responder (Windows)
- Se corrigió un bloqueo inusual durante el cierre de ODBC cuando la ejecución asincrónica está habilitada (se produjo al borrar el identificador de conexión)
- Se corrigió un problema en el que el controlador de SQL generó un alto consumo de CPU al ejecutar procedimientos almacenados largos
- Se corrigió la incapacidad de recuperar datos en una columna varbinary(max) cifrada sin conversión
- Se corrigió un problema por el que, después de que se capture una columna cifrada varchar(max) NULL con SQLGetData() en un cursor estático, la columna siguiente también queda con valores NULL aunque tenga datos
- Se corrigió un problema con la captura del campo varbinary(max) con Always Encrypted activado
- Se corrigió un problema de setlocale() que no funciona con Always Encrypted
- Se corrigió un problema con SQLDescribeParam () que devolvía un error al llamarlo en un parámetro de procedimiento almacenado de tipo XML con Always Encrypted activado
- Se corrigió un carácter de subrayado de escape que no funcionaba en SQLTables
- Se corrigió un error en el que los datos en hebreo (varchar) se truncan cuando se devuelven como caracteres anchos en Linux
- Se corrigió un problema con la consulta char/varchar con codificación Shift-JIS desde una aplicación UTF-8
- Se corrigió el error en el que, al llamar a SQLGetInfo con el parámetro SQL_DRIVER_NAME, se devolvía un nombre de archivo de estilo Linux en macOS
- Se corrigió un problema en el que cargar datos de caracteres de Windows-1252, con archivos de entrada de más de 32 000 bytes en columnas VARCHAR mediante la utilidad BCP, daría lugar a errores

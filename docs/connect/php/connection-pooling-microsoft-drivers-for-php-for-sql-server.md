---
title: Agrupación de conexiones (controladores de Microsoft para PHP para SQL Server)
description: Obtenga información sobre la agrupación de conexiones al usar los controladores de Microsoft para PHP para SQL Server y cómo puede variar la experiencia en función del sistema operativo.
ms.custom: ''
ms.date: 08/01/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connection pooling support
ms.assetid: 4d9a83d4-08de-43a1-975c-0a94005edc94
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7aa190aca90d72a2d99c260acba85056c850e334
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038495"
---
# <a name="connection-pooling-microsoft-drivers-for-php-for-sql-server"></a>Agrupación de conexiones (controladores de Microsoft para PHP para SQL Server)
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

A continuación, se muestran consideraciones importantes que hay tener cuenta sobre la agrupación de conexiones en los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]:  
  
-   Los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] utilizan la agrupación de conexiones ODBC.  
  
-   De forma predeterminada, la agrupación de conexiones está habilitada en Windows. En Linux y macOS, las conexiones se agrupan solo si la agrupación de conexiones está habilitada para ODBC (consulte [Habilitación y deshabilitación de la agrupación de conexiones](#enablingdisabling-connection-pooling)). Cuando la agrupación de conexiones está habilitada y se conecta a un servidor, el controlador intenta usar una conexión agrupada antes de crear una. Si no encuentra una conexión equivalente en el grupo, se crea una nueva conexión y se agrega al grupo. El controlador determina si las conexiones son equivalentes según una comparación de las cadenas de conexión.  
  
-   Cuando se utiliza una conexión del grupo, se restablece el estado de conexión (solo Windows).  
  
-   Al cerrar la conexión, la conexión vuelve al grupo.  
  
Para obtener más información sobre la agrupación de conexiones, consulte [Agrupación de conexiones de administrador de controladores](../../odbc/reference/develop-app/driver-manager-connection-pooling.md).  
  
## <a name="enablingdisabling-connection-pooling"></a>Habilitación o deshabilitación de la agrupación de conexiones
### <a name="windows"></a>Windows
Puede forzar a que el controlador cree una nueva conexión en lugar de buscar una equivalente en la agrupación de conexiones mediante el establecimiento del valor del atributo *ConnectionPooling* de la cadena de conexión en **false** (o 0).  
  
Si el atributo *ConnectionPooling* se omite de la cadena de conexión, o si se establece en **True** (o 1), el controlador solo crea una nueva conexión en el caso de que no exista una equivalente en la agrupación de conexiones.  

> [!NOTE]  
> Conjuntos de resultados activos múltiples (MARS) está habilitado de forma predeterminada. Cuando tanto MARS como la agrupación están en uso, para que MARS funcione correctamente, el controlador requiere un tiempo más largo para restablecer la conexión en la *primera* consulta, por lo que se omite cualquier tiempo de espera de consulta especificado. Sin embargo, el valor de tiempo de espera de consulta surtirá efecto en las consultas posteriores.
  
Si es necesario, consulte [Procedimiento: Desactivación de los conjuntos de resultados activos múltiples (MARS)](../../connect/php/how-to-disable-multiple-active-resultsets-mars.md). Para obtener información sobre otros atributos de conexión, consulte [Connection Options](../../connect/php/connection-options.md).  

### <a name="linux-and-macos"></a>Linux y macOS
No se puede usar el atributo *ConnectionPooling* para habilitar o deshabilitar la agrupación de conexiones. 

La agrupación de conexiones se puede habilitar o deshabilitar mediante la edición del archivo de configuración odbcinst.ini. Se debe volver a cargar el controlador para que los cambios surtan efecto.

Si se establece `Pooling` en `Yes` y un valor de `CPTimeout` positivo en el archivo odbcinst.ini, se habilita la agrupación de conexiones. 
```
[ODBC]
Pooling=Yes

[ODBC Driver 17 for SQL Server]
CPTimeout=<int value>
```
  
Como mínimo, el archivo odbcinst.ini debe tener un aspecto similar al de este ejemplo:

```
[ODBC]
Pooling=Yes

[ODBC Driver 17 for SQL Server]
Description=Microsoft ODBC Driver 17 for SQL Server
Driver=/opt/microsoft/msodbcsql17/lib64/libmsodbcsql-17.5.so.2.1
UsageCount=1
CPTimeout=120
```

Al establecer `Pooling` en `No` en el archivo odbcinst.ini se obliga al controlador a crear una conexión.
```
[ODBC]
Pooling=No
```

## <a name="remarks"></a>Observaciones
- En Linux o macOS, no se recomienda la agrupación de conexiones con una versión de unixODBC inferior a la 2.3.7. Todas las conexiones se agruparán si está habilitada la agrupación en el archivo odbcinst.ini, lo que significa que la opción de conexión ConnectionPooling no tiene ningún efecto. Para deshabilitar la agrupación, establezca Pooling=No en el archivo odbcinst.ini y vuelva a cargar los controladores. 
  - Puede que unixODBC <= 2.3.4 (Linux y macOS) no devuelva la información de diagnóstico adecuada, como mensajes de error, advertencias y mensajes de información.
  - Por esta razón, es posible que los controladores SQLSRV y PDO_SQLSRV no puedan capturar correctamente datos largos (por ejemplo, XML, binarios) como cadenas. Como solución temporal, los datos largos se pueden capturar como secuencias. Consulte el ejemplo siguiente para obtener más información sobre SQLSRV.

```
<?php
$connectionInfo = array("Database"=>"test", "UID"=>"username", "PWD"=>"password");

$conn1 = sqlsrv_connect("servername", $connectionInfo);

$longSample = str_repeat("a", 8500);
$xml1 = 
'<ParentXMLTag>
  <ChildTag01>'.$longSample.'</ChildTag01>
</ParentXMLTag>';

// Create table and insert xml string into it
sqlsrv_query($conn1, "CREATE TABLE xml_table (field xml)");
sqlsrv_query($conn1, "INSERT into xml_table values ('$xml1')");

// retrieve the inserted xml
$column1 = getColumn($conn1);

// return the connection to the pool
sqlsrv_close($conn1);

// This connection is from the pool
$conn2 = sqlsrv_connect("servername", $connectionInfo);
$column2 = getColumn($conn2);

sqlsrv_query($conn2, "DROP TABLE xml_table");
sqlsrv_close($conn2);

function getColumn($conn)
{
    $tsql = "SELECT * from xml_table";
    $stmt = sqlsrv_query($conn, $tsql);
    sqlsrv_fetch($stmt);
    // This might fail in Linux and macOS
    // $column = sqlsrv_get_field($stmt, 0, SQLSRV_PHPTYPE_STRING(SQLSRV_ENC_CHAR));
    // The workaround is to fetch it as a stream
    $column = sqlsrv_get_field($stmt, 0, SQLSRV_PHPTYPE_STREAM(SQLSRV_ENC_CHAR));
    sqlsrv_free_stmt($stmt);
    return ($column);
}
?>
```


## <a name="see-also"></a>Consulte también  
[Procedimientos: Conexión con la autenticación de Windows](../../connect/php/how-to-connect-using-windows-authentication.md)

[Cómo: Conexión mediante la autenticación de SQL Server](../../connect/php/how-to-connect-using-sql-server-authentication.md)  
  

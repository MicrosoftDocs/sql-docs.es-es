---
title: Resistencia de conexión inactiva
description: Obtenga información sobre qué es la resistencia de conexión inactiva y cómo se comporta en los controladores de Microsoft para PHP para SQL Server.
ms.date: 03/04/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07223e87c82a64fe17cd1b2969b5c93227c70af2
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247488"
---
# <a name="idle-connection-resiliency"></a>Resistencia de conexión inactiva
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Resistencia de la conexión](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) es el principio de que se puede restablecer una conexión inactiva interrumpida, con ciertas restricciones. Si se produce un error en una conexión a Microsoft SQL Server, la resistencia de la conexión permite que el cliente intente restablecer la conexión automáticamente. La resistencia de la conexión es una propiedad del origen de datos; solo SQL Server 2014 y versiones posteriores y Azure SQL Database admiten la resistencia de la conexión.

La resistencia de la conexión se implementa con dos palabras clave de conexión que se pueden agregar a las cadenas de conexión: **ConnectRetryCount** y **ConnectRetryInterval**.

|Palabra clave|Valores|Valor predeterminado|Descripción|
|-|-|-|-|
|**ConnectRetryCount**| Un entero comprendido entre 0 y 255, ambos incluidos|1|El número máximo de intentos de restablecer una conexión interrumpida antes de abandonar. De forma predeterminada, se realiza un solo intento de restablecer una conexión cuando se interrumpe. Un valor de 0 significa que no se intentará ninguna reconexión.|
|**ConnectRetryInterval**| Un entero comprendido entre 1 y 60, ambos incluidos|10| El tiempo, en segundos, entre los intentos de restablecer una conexión. La aplicación intentará volver a conectarse inmediatamente al detectar una conexión interrumpida y, a continuación, esperará **ConnectRetryInterval** segundos antes de volver a intentarlo. Esta palabra clave se omite si **ConnectRetryCount** es igual a 0.

Si el producto de **ConnectRetryCount** multiplicado por **ConnectRetryInterval** es mayor que **LoginTimeout**, el cliente dejará de intentar conectarse una vez que se alcance **LoginTimeout**; de lo contrario, seguirá intentando volver a conectarse hasta que se alcance **ConnectRetryCount**.

#### <a name="remarks"></a>Observaciones

La resistencia de la conexión se aplica cuando la conexión está inactiva. Los errores que se producen durante la ejecución de una transacción, por ejemplo, no desencadenarán intentos de reconexión (se producirá un error en ellos, en contra de lo que cabría esperar). Las siguientes situaciones, conocidas como estados de sesión no recuperable, no desencadenarán intentos de reconexión:

* Tablas temporales
* Cursores globales y locales
* Bloqueos de transacción de nivel de sesión y contexto de transacción
* Bloqueos de aplicaciones
* Contexto de seguridad EXECUTE AS/REVERT
* Identificadores de automatización OLE
* Identificadores XML preparados
* Marcas de seguimiento

## <a name="example"></a>Ejemplo

El siguiente código se conecta a una base de datos y ejecuta una consulta. La conexión se interrumpe al terminar la sesión y se intenta realizar una nueva consulta mediante la conexión interrumpida. En este ejemplo se utiliza la base de datos de ejemplo [AdventureWorks](/previous-versions/sql/sql-server-2008/ms124501(v=sql.100)).

En este ejemplo, especificamos un cursor de búfer antes de interrumpir la conexión. Si no especificamos un cursor de búfer, la conexión no se restablecería, ya que habría un cursor del lado servidor activo y, por tanto, la conexión no estaría inactiva al interrumpirse. Sin embargo, en ese caso llamaríamos a sqlsrv_free_stmt() antes de interrumpir la conexión para desocupar el cursor y la conexión se restablecería correctamente.

```php
<?php
// This function breaks the connection by determining its session ID and killing it.
// A separate connection is used to break the main connection because a session
// cannot kill itself. The sleep() function ensures enough time has passed for KILL
// to finish ending the session.
function BreakConnection( $conn, $conn_break )
{
    $stmt1 = sqlsrv_query( $conn, "SELECT @@SPID" );
    if ( sqlsrv_fetch( $stmt1 ) )
    {
        $spid=sqlsrv_get_field( $stmt1, 0 );
    }

    $stmt2 = sqlsrv_prepare( $conn_break, "KILL ".$spid );
    sqlsrv_execute( $stmt2 );
    sleep(1);
}

// Connect to the local server using Windows authentication and specify
// AdventureWorks as the database in use. Specify values for
// ConnectRetryCount and ConnectRetryInterval as well.
$databaseName = 'AdventureWorks2014';
$serverName = '(local)';
$connectionInfo = array( "Database"=>$databaseName, "ConnectRetryCount"=>10, "ConnectRetryInterval"=>10 );

$conn = sqlsrv_connect( $serverName, $connectionInfo );
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}

// A separate connection that will be used to break the main connection $conn
$conn_break = sqlsrv_connect( $serverName, array( "Database"=>$databaseName) );

// Create a statement to retrieve the contents of a table
$stmt1 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Employee",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt1 === false )
{
     echo "Error in statement 1.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 1 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt1 );
    echo $rowcount." rows in result set.\n";
}

// Now break the connection $conn
BreakConnection( $conn, $conn_break );

// Create another statement. The connection will be reestablished.
$stmt2 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Department",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt2 === false )
{
     echo "Error in statement 2.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 2 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt2 );
    echo $rowcount." rows in result set.\n";
}

sqlsrv_close( $conn );
sqlsrv_close( $conn_break );
?>
```
Resultado esperado:
```
Statement 1 successful.
290 rows in result set.
Statement 2 successful.
16 rows in result set.
```

## <a name="see-also"></a>Consulte también
[Resistencia de conexión en el controlador Windows ODBC](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)

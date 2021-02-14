---
title: PDO::prepare
description: Referencia de API de la función PDO::prepare en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 01/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a8b16fdc-c748-49be-acf2-a6ac7432d16b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5e8a42aa2728ea8136c2889c18f26bafeea36ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039115"
---
# <a name="pdoprepare"></a>PDO::prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prepara una instrucción para su ejecución.

## <a name="syntax"></a>Sintaxis

```
PDOStatement PDO::prepare ( $statement [, array(key_pair)] )
```

#### <a name="parameters"></a>Parámetros
$*instrucción*: una cadena que contiene la instrucción SQL.

*key_pair*: una matriz que contiene un valor y un nombre de atributo. Vea la sección Comentarios para obtener más información.

## <a name="return-value"></a>Valor devuelto
Devuelve un objeto PDOStatement si se ejecuta correctamente. En caso de error, devuelve un objeto PDOException o false, según el valor de `PDO::ATTR_ERRMODE`.

## <a name="remarks"></a>Observaciones
Los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] no evalúan instrucciones preparadas hasta la ejecución.

En la siguiente tabla se incluyen los posibles valores *key_pair*.

|Clave|Descripción|
|-------|---------------|
|PDO::ATTR_CURSOR|Especifica el comportamiento del cursor. El valor predeterminado es `PDO::CURSOR_FWDONLY`, un cursor de avance que no es desplazable. `PDO::CURSOR_SCROLL` es un cursor desplazable.<br /><br />Por ejemplo, `array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY )`.<br /><br />Cuando se establece en `PDO::CURSOR_SCROLL`, puede usar `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` para establecer el tipo de cursor desplazable, que se describe a continuación.<br /><br />Vea [Cursor Types &#40;PDO_SQLSRV Driver&#41; (Tipos de cursor &#40;Controlador PDO_SQLSRV&#41;)](../../connect/php/cursor-types-pdo-sqlsrv-driver.md) para obtener más información sobre los conjuntos de resultados y los cursores del controlador PDO_SQLSRV.|
|PDO::ATTR_EMULATE_PREPARES|De forma predeterminada, este atributo es false, que se puede cambiar por `PDO::ATTR_EMULATE_PREPARES => true`. Vea [Emulate Prepare](#emulate-prepare) para consultar detalles y ejemplos.|
|PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE|Especifica el tipo de cursor desplazable. Es válido solo cuando `PDO::ATTR_CURSOR` está establecido en `PDO::CURSOR_SCROLL`. Vea a continuación los valores que este atributo puede adoptar.|
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|Especifica el número de lugares decimales al dar formato a los valores de moneda obtenidos. Esta opción solo funciona cuando `PDO::SQLSRV_ATTR_FORMAT_DECIMALS` es true. Para más información, vea [Aplicación de formato a cadenas decimales y valores de moneda (controlador PDO_SQLSRV)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|Cuando es True, especifica la ejecución de una consulta directa. False implica la ejecución de una instrucción preparada. Para más información sobre `PDO::SQLSRV_ATTR_DIRECT_QUERY`, vea [Ejecución de la instrucción preparada o directa en el controlador PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_ENCODING|PDO::SQLSRV_ENCODING_UTF8 (valor predeterminado)<br /><br />PDO::SQLSRV_ENCODING_SYSTEM<br /><br />PDO::SQLSRV_ENCODING_BINARY|
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|Especifica si debe recuperar tipos de fecha y hora como objetos [PHP DateTime](http://php.net/manual/en/class.datetime.php). Para más información, vea: [Cómo: Recuperación de los tipos de fecha y hora como objetos de fecha y hora PHP mediante el controlador PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|Controla los resultados numéricos de las columnas en relación con los tipos SQL numéricos. Para obtener más información, consulte [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|Especifica si se agregan ceros iniciales a las cadenas decimales cuando proceda. Si se establece, esta opción habilita la opción `PDO::SQLSRV_ATTR_DECIMAL_PLACES` para aplicar formato a los tipos de divisa. Para más información, vea [Aplicación de formato a cadenas decimales y valores de moneda (controlador PDO_SQLSRV)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|Para obtener más información, consulte [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|

Cuando se usa `PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL`, puede usar `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` para especificar el tipo de cursor. Por ejemplo, pase la matriz siguiente a PDO::prepare para establecer un cursor dinámico:
```
array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_DYNAMIC));
```
En la tabla siguiente se muestran los valores posibles para `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE`. Para más información sobre los cursores desplazables, vea [Tipos de cursor &#40;controlador PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

|Value|Descripción|
|---------|---------------|
|PDO::SQLSRV_CURSOR_BUFFERED|Crea un cursor estático del lado cliente (en búfer), que almacena en búfer el conjunto de resultados en memoria en el equipo cliente.|
|PDO::SQLSRV_CURSOR_DYNAMIC|Crea un cursor dinámico de servidor (no almacenado en búfer), que le permite acceder a las filas en cualquier orden y reflejará los cambios de la base de datos.|
|PDO::SQLSRV_CURSOR_KEYSET|Crea un cursor de conjunto de claves de servidor. Los cursores de conjunto de claves no actualizan el recuento de filas si se elimina una fila de la tabla (las filas eliminadas se devuelven sin valores).|
|PDO::SQLSRV_CURSOR_STATIC|Crea un cursor estático de servidor, que le permite acceder a las filas en cualquier orden, pero no reflejará los cambios de la base de datos.<br /><br />`PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL` implica `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_STATIC`.|

Puede cerrar un objeto PDOStatement mediante una llamada a `unset`:
```
unset($stmt);
```

## <a name="forward-only-example"></a>Ejemplo de solo avance
En este ejemplo se muestra cómo usar PDO::prepare con marcadores de parámetros y un cursor de sólo avance.

```
<?php
$database = "Test";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$col1 = 'a';
$col2 = 'b';

$query = "insert into Table1(col1, col2) values(?, ?)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( $col1, $col2 ) );
print $stmt->rowCount();
echo "\n";

$query = "insert into Table1(col1, col2) values(:col1, :col2)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( ':col1' => $col1, ':col2' => $col2 ) );
print $stmt->rowCount();

unset($stmt);
?>
```

## <a name="static-cursor-example"></a>Ejemplo de cursor estático
En este ejemplo se muestra cómo usar PDO::prepare con un cursor estático de servidor. Para ver un ejemplo en el que se muestre un cursor del lado cliente, vea [Tipos de cursor &#40;controlador PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

```
<?php
$database = "AdventureWorks";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$query = "select * from Person.ContactType";
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));
$stmt->execute();

echo "\n";

while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){
   print "$row[Name]\n";
}
echo "\n..\n";

$row = $stmt->fetch( PDO::FETCH_BOTH, PDO::FETCH_ORI_FIRST );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_ASSOC, PDO::FETCH_ORI_REL, 1 );
print "$row[Name]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_NEXT );
print "$row[1]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_PRIOR );
print "$row[1]..\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_ABS, 0 );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_LAST );
print_r($row);
?>
```

## <a name="targeted-example"></a>Ejemplo de destino
En los dos fragmentos de código siguientes se muestra cómo usar PDO::prepare con los datos destinados a las columnas CHAR/VARCHAR. Dado que la codificación predeterminada de PDO::prepare es UTF-8, el usuario puede usar la opción `PDO::SQLSRV_ENCODING_SYSTEM` para evitar conversiones implícitas.

**Opción 1**
```
$options = array(PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_SYSTEM);
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue',
  $options
);

$statement->bindValue(':myVarcharValue', 'my data', PDO::PARAM_STR);
```

**Opción 2**
```
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue'
);
$p = 'my data';
$statement->bindParam(':myVarcharValue', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_SYSTEM);
```

<a name="emulate-prepare" />

## <a name="prepare-example"></a>Ejemplo de preparación

En este ejemplo se muestra cómo usar PDO::prepare con `PDO::ATTR_EMULATE_PREPARES` establecido en true.

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$database = "tempdb";
$conn = new PDO("sqlsrv:server = $serverName; Database = $database", $username, $password);

$pdo_options = array();
$pdo_options[PDO::ATTR_EMULATE_PREPARES] = true;
$pdo_options[PDO::SQLSRV_ATTR_ENCODING] = PDO::SQLSRV_ENCODING_UTF8;

$stmt = $conn->prepare("CREATE TABLE TEST([id] [int] IDENTITY(1,1) NOT NULL,
                                          [name] nvarchar(max))",
                                          $pdo_options);
$stmt->execute();

$prefix = '가각';
$name = '가각ácasa';
$name2 = '가각sample2';

$stmt = $conn->prepare("INSERT INTO TEST(name) VALUES(:p0)", $pdo_options);
$stmt->execute(['p0' => $name]);
unset($stmt);

$stmt = $conn->prepare("SELECT * FROM TEST WHERE NAME LIKE :p0", $pdo_options);
$stmt->execute(['p0' => "$prefix%"]);
foreach ($stmt as $row) {
    echo "\n" . 'FOUND: ' . $row['name'];
}

unset($stmt);
unset($conn);
?>
```

El controlador PDO_SQLSRV reemplaza internamente todos los marcadores de posición con los parámetros que están enlazados mediante [PDOStatement::bindParam()](../../connect/php/pdostatement-bindparam.md). Por tanto, se envía al servidor una cadena de consulta SQL sin ningún marcador de posición. Considere este ejemplo:

```
$statement = $PDO->prepare("INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)");
$statement->bindParam(:cus_name, "Cardinal");
$statement->bindParam(:con_name, "Tom B. Erichsen");
$statement->execute();
```

Con `PDO::ATTR_EMULATE_PREPARES` establecido en false (el caso predeterminado), los datos enviados a la base de datos son:

```
"INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)"
Information on :cus_name parameter
Information on :con_name parameter
```

El servidor ejecuta la consulta con su característica de consulta con parámetros para enlazar parámetros. Por otro lado, con `PDO::ATTR_EMULATE_PREPARES` establecido en true, la consulta enviada al servidor es básicamente:

```
"INSERT into Customers (CustomerName, ContactName) VALUES ('Cardinal', 'Tom B. Erichsen')"
```

Establecer `PDO::ATTR_EMULATE_PREPARES` en true puede omitir algunas restricciones en SQL Server. Por ejemplo, SQL Server no admite parámetros con nombre o posicionales en algunas cláusulas de Transact-SQL. Además, SQL Server tiene un límite de 2100 parámetros de enlace.

> [!NOTE]
> Con emulate prepares establecido en true, la seguridad de las consultas con parámetros no está en vigor. Por lo tanto, la aplicación debe asegurarse de que los datos enlazados a los parámetros no contengan código Transact-SQL malintencionado.

### <a name="encoding"></a>Encoding

Si el usuario desea enlazar parámetros con distintas codificaciones (por ejemplo, UTF-8 o binario), el usuario debe especificar claramente la codificación en el script PHP.

El controlador PDO_SQLSRV comprueba primero la codificación especificada en `PDO::bindParam()` (por ejemplo, `$statement->bindParam(:cus_name, "Cardinal", PDO::PARAM_STR, 10, PDO::SQLSRV_ENCODING_UTF8)`).

Si no se encuentra, el controlador comprueba si cualquier codificación está establecida en `PDO::prepare()` o `PDOStatement::setAttribute()`. En caso contrario, el controlador utilizará la codificación especificada en `PDO::__construct()` o `PDO::setAttribute()`.

Además, a partir de la versión 5.8.0, cuando se usa PDO::prepare con `PDO::ATTR_EMULATE_PREPARES` establecido en true, el usuario puede usar [los tipos de cadenas extendidas introducidos en PHP 7.2](https://wiki.php.net/rfc/extended-string-types-for-pdo) para asegurarse de que se usa el prefijo de `N`. En los fragmentos de código siguientes se muestran varias alternativas.

> [!NOTE]
> De forma predeterminada, emulate prepares está establecido en false, en cuyo caso se omitirán las constantes de cadena PDO extendidas.

**Uso de la opción de controlador PDO::SQLSRV_ENCODING_UTF8 al enlazar**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_UTF8);
$stmt->execute();
```

**Uso del atributo PDO::SQLSRV_ATTR_ENCODING**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true, PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_UTF8);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

**Usar la constante PDO PDO::PARAM_STR_NATL**
```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR | PDO::PARAM_STR_NATL);
$stmt->execute();
```

**Establecer el tipo de parámetro de cadena predeterminado PDO::PARAM_STR_NATL**
```
$conn->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

### <a name="limitations"></a>Limitaciones

Como puede ver, el enlace lo realiza el controlador internamente. Se envía una consulta válida al servidor para su ejecución sin ningún parámetro. En comparación con el caso normal, se producen algunas limitaciones cuando no se usa la característica de consulta con parámetros.

- No funciona para los parámetros que se enlazan como `PDO::PARAM_INPUT_OUTPUT`.
    - Cuando el usuario especifica `PDO::PARAM_INPUT_OUTPUT` en `PDO::bindParam()`, se produce una excepción de PDO.
- No funciona para los parámetros que se enlazan como parámetros de salida.
    - Cuando el usuario crea una instrucción preparada con marcadores de posición que están destinados a los parámetros de salida (es decir, con un signo igual inmediatamente después de un marcador de posición, como `SELECT ? = COUNT(*) FROM Table1`), se produce una excepción PDO.
    - Cuando una instrucción preparada invoca un procedimiento almacenado con un marcador de posición como el argumento para un parámetro de salida, no se lanza ninguna excepción porque el controlador no puede detectar el parámetro de salida. Sin embargo, la variable que el usuario proporciona para el parámetro de salida permanecerá sin cambios.
- Los marcadores de posición duplicados para un parámetro codificado como binario no funcionarán.

## <a name="see-also"></a>Consulte también
[Clase PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)


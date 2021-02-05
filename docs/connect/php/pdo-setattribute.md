---
title: PDO::setAttribute
description: Referencia de API de la función PDO::setAttribute en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 56f9ee96-e1d2-46cc-b137-38f06a251863
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ac1b72ea564df077ae7edb0f771becd9b76cda7b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195237"
---
# <a name="pdosetattribute"></a>PDO::setAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Establece un atributo PDO predefinido o un controlador personalizado.  
  
## <a name="syntax"></a>Sintaxis  
  
```
bool PDO::setAttribute ( $attribute, $value );  
```  
  
#### <a name="parameters"></a>Parámetros  
*$attribute*: el atributo que se establecerá. Consulte la sección Comentarios para obtener la lista de atributos admitidos.  
  
*$value*: valor (tipo mixto).  
  
## <a name="return-value"></a>Valor devuelto  
Se devuelve True si la operación se realiza correctamente; de lo contrario, False.  
  
## <a name="remarks"></a>Observaciones  
  
|Atributo|Procesado mediante|Valores admitidos|Descripción|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|Especifica la mayúsculas en los nombres de columna.<br /><br />PDO::CASE_LOWER aplica minúsculas a los nombres de columna.<br /><br />PDO::CASE_NATURAL (el valor predeterminado) muestra los nombres de columna con el mismo uso de mayúsculas y minúsculas que aparece en la base de datos.<br /><br />PDO::CASE_UPPER aplica mayúsculas a los nombres de columna.<br /><br />Este atributo también puede establecerse utilizando PDO::setAttribute.|  
|PDO::ATTR_DEFAULT_FETCH_MODE|PDO|Consulte la documentación de PDO.|Consulte la documentación de PDO.|  
|PDO::ATTR_DEFAULT_STR_PARAM|PDO|PDO::PARAM_STR_CHAR<br /><br />PDO::PARAM_STR_NATL|Para más información, consulte los ejemplos en [PDO::quote](../../connect/php/pdo-quote.md).|
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|Especifica cómo el controlador informa de errores.<br /><br />PDO::ERRMODE_SILENT (el valor predeterminado) establece la información y los códigos de los errores.<br /><br />PDO::ERRMODE_WARNING genera una advertencia E_WARNING.<br /><br />PDO::ERRMODE_EXCEPTION genera una excepción.<br /><br />Este atributo también puede establecerse utilizando PDO::setAttribute.|  
|PDO::ATTR_ORACLE_NULLS|PDO|Consulte la documentación de PDO.|Especifica cómo se deben devolver los valores Null.<br /><br />PDO::NULL_NATURAL no realiza ninguna conversión.<br /><br />PDO::NULL_EMPTY_STRING convierte el valor de una cadena vacía en Null.<br /><br />PDO::NULL_TO_STRING convierte el valor de una cadena vacía en Null.|  
|PDO::ATTR_STATEMENT_CLASS|PDO|Consulte la documentación de PDO.|Establece que la clase de la instrucción proporcionada por el usuario se obtenga de PDOStatement.<br /><br />Se requiere `array(string classname, array(mixed constructor_args))`.<br /><br />Para obtener más información, vea la documentación de PDO.|  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|true o false|Convierte los valores numéricos en cadenas al recuperar los datos.|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|1 para el límite de memoria PHP.|Establece el tamaño de búfer que contiene el conjunto de resultados de un cursor de cliente.<br /><br />El valor predeterminado es 10 240 KB, si no se especifica en el archivo php.ini.<br /><br />No se permiten ni el cero ni los números negativos.<br /><br />Para obtener más información sobre las consultas que crean un cursor de cliente, vea [Tipos de cursor &#40;controlador PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Un entero comprendido entre 0 y 4, ambos incluidos|Especifica el número de lugares decimales al dar formato a los valores de moneda obtenidos.<br /><br />Se omitirá cualquier entero negativo o un valor mayor que 4.<br /><br />Esta opción solo funciona si PDO::SQLSRV_ATTR_FORMAT_DECIMALS es true.<br /><br />Esta opción también se puede establecer en el nivel de instrucción. Si es así, entonces, la opción a nivel de instrucción anula esta.<br /><br />Para más información, vea [Aplicación de formato a cadenas decimales y valores de moneda (controlador PDO_SQLSRV)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true o false|Especifica la ejecución de la consulta directa o preparada. Para obtener más información, vea [Direct Statement Execution and Prepared Statement Execution in the PDO_SQLSRV Driver (Ejecución de la instrucción preparada o directa en el controlador PDO_SQLSRV)](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM.|Establece el juego de caracteres que utiliza el controlador para comunicarse con el servidor.<br /><br />No se admite PDO::SQLSRV_ENCODING_BINARY.<br /><br />El valor predeterminado es PDO::SQLSRV_ENCODING_UTF8.|  
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true o false|Especifica si debe recuperar tipos de fecha y hora como objetos [PHP DateTime](http://php.net/manual/en/class.datetime.php). Si el valor se deja como false, el comportamiento predeterminado es devolverlos como cadenas.<br /><br />Esta opción también se puede establecer en el nivel de instrucción. Si es así, entonces, la opción a nivel de instrucción anula esta.<br /><br />Para más información, vea: [Cómo: Recuperación de los tipos de fecha y hora como objetos de fecha y hora PHP mediante el controlador PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true o false|Controla los resultados numéricos de las columnas con los tipos SQL numéricos (bit, integer, smallint, tinyint, float o real).<br /><br />Cuando la marca de la opción de conexión ATTR_STRINGIFY_FETCHES está activada, el valor devuelto es una cadena incluso cuando SQLSRV_ATTR_FETCHES_NUMERIC_TYPE está activado.<br /><br />Cuando el tipo devuelto de PDO en la columna de enlace es PDO_PARAM_INT, el valor devuelto de una columna de enteros es un valor int aunque SQLSRV_ATTR_FETCHES_NUMERIC_TYPE esté desactivado.|  
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true o false|Especifica si se agregan ceros iniciales a las cadenas decimales cuando proceda. Si se establece, esta opción habilita la opción de PDO::SQLSRV_ATTR_DECIMAL_PLACES para aplicar formato a los tipos de divisa. Si el valor se deja como false, se usa el comportamiento predeterminado de devolver la precisión exacta y omitir los ceros iniciales de los valores menores que 1.<br /><br />Esta opción también se puede establecer en el nivel de instrucción. Si es así, entonces, la opción a nivel de instrucción anula esta.<br /><br />Para más información, vea [Aplicación de formato a cadenas decimales y valores de moneda (controlador PDO_SQLSRV)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Entero|Establece el tiempo de espera de consulta en segundos.<br /><br />El valor predeterminado es 0, lo que significa que el controlador esperará indefinidamente para obtener los resultados.<br /><br />No se permiten números negativos.|  
  
PDO procesa algunos de los atributos predefinidos y requiere que el controlador procese otros. Todos los atributos personalizados y opciones de conexión se procesan mediante el controlador. Según la configuración de PDO::ATTR_ERRMODE, se notifican un atributo, una opción de conexión o un valor no compatibles.  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo se muestra cómo establecer el atributo PDO::ATTR_ERRMODE.  
  
```  
<?php  
   $database = "AdventureWorks";  
   $conn = new PDO( "sqlsrv:server=(local) ; Database = $database", "", "");  
  
   $attributes1 = array( "ERRMODE" );  
   foreach ( $attributes1 as $val ) {  
      echo "PDO::ATTR_$val: ";  
      var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
   }  
  
   $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
  
   $attributes1 = array( "ERRMODE" );  
   foreach ( $attributes1 as $val ) {  
      echo "PDO::ATTR_$val: ";  
      var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
   }  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Clase PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  

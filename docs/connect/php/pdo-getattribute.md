---
title: PDO::getAttribute
description: Referencia de API de la función PDO::getAttribute en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c81833ea-8b8a-459d-8f24-920098da994d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ba4e6b81216933ea4367bb4b37feeb61dd646c88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187612"
---
# <a name="pdogetattribute"></a>PDO::getAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera el valor de un atributo de controlador o un PDO predefinido.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
mixed PDO::getAttribute ( $attribute )  
```  
  
#### <a name="parameters"></a>Parámetros  
*$attribute*: uno de los atributos admitidos. Consulte la sección Comentarios para obtener la lista de atributos admitidos.  
  
## <a name="return-value"></a>Valor devuelto  
Si se ejecuta correctamente, devuelve el valor de una opción de conexión, el atributo PDO predefinido o el atributo de controlador personalizado. En caso de error, se devuelve el valor Null.  
  
## <a name="remarks"></a>Observaciones  
En la tabla siguiente se incluye la lista de atributos admitidos.  
  
|Atributo|Procesado mediante|Valores admitidos|Descripción|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|Especifica el uso de mayúsculas y minúsculas en los nombres de columna. PDO::CASE_LOWER obliga a que los nombres de columna usen minúsculas, PDO::CASE_NATURAL deja el nombre de columna que devuelve la base de datos y PDO::CASE_UPPER fuerza que los nombres de columna estén en mayúsculas.<br /><br />El valor predeterminado es PDO::CASE_NATURAL.<br /><br />Este atributo también puede establecerse utilizando PDO::setAttribute.|  
|PDO::ATTR_CLIENT_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Matriz de cadenas|Describe las versiones del controlador y las bibliotecas relacionadas. Devuelve una matriz con los elementos siguientes: la versión de ODBC (*MajorVer*.*MinorVer*), el nombre y la versión de la DLL de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client y la versión de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] (*MajorVer*.*MinorVer*.*BuildNumber*.*Revision*).|  
|PDO::ATTR_DEFAULT_STR_PARAM|PDO|PDO::PARAM_STR_CHAR<br /><br />PDO::PARAM_STR_NATL|Si no se establece en PDO::PARAM_STR_CHAR, se devuelve PDO::PARAM_STR_NATL.|
|PDO::ATTR_DRIVER_NAME|PDO|String|Siempre devuelve "sqlsrv".|  
|PDO::ATTR_DRIVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|String|Indica la versión de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] (*MajorVer*.*MinorVer*.*BuildNumber*.*Revision*).|  
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|Especifica cómo debe controlar los errores el controlador.<br /><br />PDO::ERRMODE_SILENT (el valor predeterminado) establece la información y los códigos de los errores.<br /><br />PDO::ERRMODE_WARNING genera una advertencia E_WARNING.<br /><br />PDO::ERRMODE_EXCEPTION produce una excepción.<br /><br />Este atributo también puede establecerse utilizando PDO::setAttribute.|  
|PDO::ATTR_ORACLE_NULLS|PDO|Consulte la documentación de PDO.|Consulte la documentación de PDO.|  
|PDO::ATTR_SERVER_INFO|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Matriz de 3 elementos|Devuelve la base de datos actual, la versión de SQL Server y la instancia de SQL Server.|  
|PDO::ATTR_SERVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|String|Indica la versión de SQL Server (*Major*.*Minor*.*BuildNumber*).|  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|Consultar la documentación de PDO|Consulte la documentación de PDO.|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|1 para el límite de memoria PHP.|Configura el tamaño de búfer que contiene el conjunto de resultados de un cursor de cliente.<br /><br />El valor predeterminado es 10 240 KB (10 MB).<br /><br />Para obtener más información sobre los cursores de cliente, vea [Cursor Types &#40;SQLSRV Driver&#41;](../../connect/php/cursor-types-sqlsrv-driver.md) (Tipos de cursor &#40;Controlador SQLSRV&#41;).|  
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true<br /><br />false|Especifica la ejecución de la consulta directa o preparada. Para obtener más información, vea [Direct Statement Execution and Prepared Statement Execution in the PDO_SQLSRV Driver (Ejecución de la instrucción preparada o directa en el controlador PDO_SQLSRV)](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM|Especifica el juego de caracteres que utiliza el controlador para comunicarse con el servidor.<br /><br />El valor predeterminado es PDO::SQLSRV_ENCODING_UTF8.|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true o false|Controla los resultados numéricos de las columnas con los tipos SQL numéricos (bit, integer, smallint, tinyint, float o real).<br /><br />Cuando la marca de la opción de conexión ATTR_STRINGIFY_FETCHES está activada, el valor devuelto es una cadena, incluso cuando SQLSRV_ATTR_FETCHES_NUMERIC_TYPE está activado.<br /><br />Cuando el tipo devuelto de PDO en la columna de enlace es PDO_PARAM_INT, el valor devuelto de una columna de enteros es un valor int aunque SQLSRV_ATTR_FETCHES_NUMERIC_TYPE esté desactivado.|  
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|Entero|Establece el tiempo de espera de consulta en segundos.<br /><br />El valor predeterminado es 0, lo que significa que el controlador esperará indefinidamente para obtener los resultados.<br /><br />No se permiten números negativos.|  

  
PDO procesa algunos de los atributos predefinidos, aunque requiere que el controlador controle otros. Todas las opciones de conexión y los atributos personalizados se administran mediante el controlador; las opciones de conexión o los atributos no admitidos devuelven el valor NULL.  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo se muestra el valor del atributo PDO::ATTR_ERRMODE antes y después de cambiar su valor.  
  
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
  
// An example using PDO::ATTR_CLIENT_VERSION  
print_r($conn->getAttribute( PDO::ATTR_CLIENT_VERSION ));  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Clase PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  

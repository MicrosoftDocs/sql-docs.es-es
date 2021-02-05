---
title: PDO::__construct
description: Referencia de API de la función PDO::__construct en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 3ee53aff-6fe4-44cd-a15b-51770c98c712
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 27155dbbae9967895412628b4cf82da381c97f8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202062"
---
# <a name="pdo__construct"></a>PDO::__construct
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crear una conexión a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
PDO::__construct($dsn [,$username [,$password [,$driver_options ]]] )  
```  
  
#### <a name="parameters"></a>Parámetros  
*$dsn*: una cadena que contiene el nombre de prefijo (siempre `sqlsrv`), dos puntos y la palabra clave Server. Por ejemplo, `"sqlsrv:server=(local)"`. También puede especificar otras palabras clave de conexión. Consulte [Connection Options](../../connect/php/connection-options.md) para obtener una descripción de la palabra clave Server y del resto de las palabras clave de conexión. *$dsn* está entre comillas, por lo que no se debe entrecomillar cada palabra clave de conexión.  
  
*$username*: Opcional. Una cadena que contiene el nombre del usuario. Para establecer la conexión utilizando la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], especifique el id. de inicio de sesión. Para establecer la conexión utilizando la autenticación de Windows, especifique `""`.  
  
*$password*: opcional. Una cadena que contiene la contraseña del usuario. Para establecer la conexión utilizando la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], especifique la contraseña. Para establecer la conexión utilizando la autenticación de Windows, especifique `""`.  
  
*$driver_options*: Opcional. Puede especificar atributos del administrador de controladores de PDO y atributos de controladores específicos de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]: PDO::SQLSRV_ATTR_ENCODING, PDO::SQLSRV_ATTR_DIRECT_QUERY. Los atributos no válidos no generan excepciones, solo lo harán cuando se especifican con [PDO::setAttribute](../../connect/php/pdo-setattribute.md).  
  
## <a name="return-value"></a>Valor devuelto  
Devuelve un objeto de PDO. Si se produce un error, devuelve un objeto PDOException.  
  
## <a name="exceptions"></a>Excepciones  
PDOException  
  
## <a name="remarks"></a>Observaciones  
Puede cerrar un objeto de conexión estableciendo el valor de la instancia en Null.  
  
Después de una conexión, PDO::errorCode muestra 01000 en lugar de 00000.  
  
Si, por cualquier motivo, se produce un error en PDO::__construct, se genera una excepción, aunque PDO::ATTR_ERRMODE esté establecido en PDO::ERRMODE_SILENT.  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example-with-database"></a>Ejemplo con base de datos  
En este ejemplo se muestra cómo conectarse a un servidor mediante la autenticación de Windows y especificar una base de datos.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:Server=(local) ; Database = AdventureWorks ", "", "", array(PDO::SQLSRV_ATTR_DIRECT_QUERY => true));   
  
   $query = 'SELECT * FROM Person.ContactType';   
   $stmt = $c->query( $query );   
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ) {   
      print_r( $row );   
   }  
   $c = null;   
?>  
```  
  
## <a name="example-without-database"></a>Ejemplo sin base de datos  
En este ejemplo se muestra cómo conectarse a un servidor especificando la base de datos más adelante.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:server=(local)");  
  
   $c->exec( "USE AdventureWorks");  
   $query = 'SELECT * FROM Person.ContactType';  
   $stmt = $c->query( $query );  
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
      print_r( $row );  
   }  
   $c = null;  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Clase PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  

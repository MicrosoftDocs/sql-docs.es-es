---
title: sqlsrv_connect
description: Crea un recurso de conexión y abre una conexión mediante el controlador sql_srv para PHP. De forma predeterminada, se intentará realizar la conexión mediante la autenticación de Windows.
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_connect
apitype: NA
helpviewer_keywords:
- connecting to the server
- API Reference, sqlsrv_connect
- connection pooling support
- sqlsrv_connect
ms.assetid: 37836b49-258e-45ce-9549-b8bd85d6952d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ea1fdf0513e8ba793425154020cb4dfc837a1045
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195075"
---
# <a name="sqlsrv_connect"></a>sqlsrv_connect
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crea un recurso de conexión y abre una conexión. De forma predeterminada, se intentará realizar la conexión mediante la autenticación de Windows.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlsrv_connect( string $serverName [, array $connectionInfo])  
```  
  
#### <a name="parameters"></a>Parámetros  
*$serverName*: una cadena que especifica el nombre del servidor con el que se está estableciendo una conexión. Se puede incluir un nombre de instancia (por ejemplo, "miServidor\nombreDeInstancia") o el número de puerto (por ejemplo, "miServidor, 1521") como parte de esta cadena. Para obtener una descripción completa de las opciones disponibles en este parámetro, vea la palabra clave Server en la sección Palabras clave de la cadena de conexión del controlador ODBC del artículo [Usar palabras clave de cadena de conexión con SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
A partir de la versión 3.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], también puede especificar una instancia de LocalDB con `"(localdb)\instancename"`. Para más información, consulte [Compatibilidad con LOcalDB](php-driver-for-sql-server-support-for-localdb.md).  
  
También, a partir de la versión 3.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], puede especificar un nombre de red virtual para conectarse a un grupo de disponibilidad AlwaysOn. Para más información sobre la compatibilidad con [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] para [!INCLUDE[ssHADR](../../includes/sshadr_md.md)], consulte [Compatibilidad con alta disponibilidad y recuperación ante desastres](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).  
  
*$connectionInfo* [OPTIONAL]: una **matriz** asociativa que contiene atributos de conexión (por ejemplo, **array**("Database" => "AdventureWorks")). Consulte [Connection Options](connection-options.md) para obtener una lista de las claves admitidas para la matriz.  
  
## <a name="return-value"></a>Valor devuelto  
Un recurso de conexión PHP. Si no se crea y abre correctamente una conexión, se devuelve **False** .  
  
## <a name="remarks"></a>Observaciones  
Si los valores de las claves de *UID* y *PWD* no se especifican en el parámetro opcional *$connectionInfo* , se tratará de realizar la conexión usando la autenticación de Windows. Para más información sobre la conexión al servidor, consulte [Procedimiento: Conexión mediante la autenticación de Windows](how-to-connect-using-windows-authentication.md) y [Procedimiento: Conexión mediante la autenticación de SQL Server](how-to-connect-using-sql-server-authentication.md).  
  
## <a name="example"></a>Ejemplo  
En el ejemplo siguiente se crea y abre una conexión mediante la autenticación de Windows. En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://www.codeplex.com/SqlServerSamples) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/*  
Connect to the local server using Windows Authentication and specify  
the AdventureWorks database as the database in use. To connect using  
SQL Server Authentication, set values for the "UID" and "PWD"  
 attributes in the $connectionInfo parameter. For example:  
$connectionInfo = array("UID" => $uid, "PWD" => $pwd, "Database"=>"AdventureWorks");  
*/  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
if( $conn )  
{  
     echo "Connection established.\n";  
}  
else  
{  
     echo "Connection could not be established.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
//-----------------------------------------------  
// Perform operations with connection.  
//-----------------------------------------------  
  
/* Close the connection. */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Referencia de API del controlador SQLSRV](sqlsrv-driver-api-reference.md)

[Conexión al servidor](connecting-to-the-server.md)

[Sobre los ejemplos de código de la documentación](about-code-examples-in-the-documentation.md)  
  

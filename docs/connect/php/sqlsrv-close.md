---
description: sqlsrv_close
title: sqlsrv_close | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_close
apitype: NA
helpviewer_keywords:
- sqlsrv_close
- API Reference, sqlsrv_close
ms.assetid: 6ac6209c-a134-4f8f-b88b-8eefaa1cbc7f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 16dfb43ebc8a35ef1a3e6016092f67e328d1eda5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190717"
---
# <a name="sqlsrv_close"></a>sqlsrv_close
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cierra la conexión especificada y libera los recursos asociados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlsrv_close( resource $conn )  
```  
  
#### <a name="parameters"></a>Parámetros  
*$conn*: la conexión que se va a cerrar.  
  
## <a name="return-value"></a>Valor devuelto  
Se devuelve el valor booleano **True** , salvo que se llame a la función con un parámetro no válido. En este caso, se devolverá **False** .  
  
> [!NOTE]  
> **Null** es un parámetro válido para esta función. Esto permite que se llame varias veces a la función en un script. Por ejemplo, si cierra una conexión en una condición de error y la vuelve a cerrar al final del script, la segunda llamada a **sqlsrv_close** devuelve **true** debido a que la primera llamada a **sqlsrv_close** (en la condición de error) establece el recurso de conexión en **null**.  
  
## <a name="example"></a>Ejemplo  
En el ejemplo siguiente se cierra una conexión. En el ejemplo se da por hecho que SQL Server está instalado en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$conn = sqlsrv_connect( $serverName);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
//-----------------------------------------------  
// Perform operations with connection here.  
//-----------------------------------------------  
  
/* Close the connection. */  
sqlsrv_close( $conn);  
echo "Connection closed.\n";  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Sobre los ejemplos de código de la documentación](../../connect/php/about-code-examples-in-the-documentation.md)  
  

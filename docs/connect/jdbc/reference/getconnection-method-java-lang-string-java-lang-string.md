---
description: Método getConnection (java.lang.String, java.lang.String)
title: Método getConnection (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getConnection (java.lang.String, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 78db89d6-a8a0-4116-8885-548e627220ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bdb2c566b6cbd89ac01c9dc5b990268055a3fe09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163293"
---
# <a name="getconnection-method-javalangstring-javalangstring"></a>Método getConnection (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Intenta establecer una conexión con el origen de datos que representa este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) mediante el nombre de usuario y contraseña especificados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Connection getConnection(java.lang.String username,  
                                         java.lang.String password)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *username*  
  
 Objeto **String** que contiene el nombre del usuario.  
  
 *password*  
  
 Objeto **String** que contiene la contraseña.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 java.sql.SQLException  
  
## <a name="remarks"></a>Observaciones  
 El método getConnection especifica este método getConnection en la interfaz javax.sql.DataSource.  
  
 Si se llama al método getConnection con un nombre de usuario o contraseña que no sean NULL, se reemplazarán las propiedades de contraseña y nombre de usuario que se establecen en la clase SQLServerDataSource al inicializar el objeto SQLServerConnection. Por ejemplo, si el autor de llamada ha llamado a [setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md) y a [setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md) en el origen de datos y, a continuación, llama a getConnection y proporciona un nombre de usuario o contraseña que no sean NULL, el nombre de usuario y contraseña que se hayan establecido con setUser y setPassword se reemplazarán con el nombre de usuario y la contraseña que se hayan pasado en getConnection.  
  
> [!NOTE]  
>  El nombre de usuario y la contraseña que se establecen dentro de la dirección URL mediante una llamada al método [setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md), en este caso, no se verán modificados.  
  
## <a name="see-also"></a>Consulte también  
 [Método getConnection &#40;SQLServerDataSource&#41;](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)   
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  

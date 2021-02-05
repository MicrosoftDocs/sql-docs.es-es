---
description: Método setObject (int, java.lang.Object)
title: Método setObject (int, java.lang.Object) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setObject (int, java.lang.Object)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 61f19faa-3006-4a1c-974c-55951e3b3000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d21949a1d33107cd5f10a9352aa6282a8ad2da79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178764"
---
# <a name="setobject-method-int-javalangobject"></a>Método setObject (int, java.lang.Object)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el valor del parámetro designado utilizando el objeto determinado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setObject(int index,  
                            java.lang.Object obj)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *índice*  
  
 Valor **int** que indica el número de parámetro.  
  
 *obj*  
  
 Objeto.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setObject especifica este método setObject en la interfaz java.sql.PreparedStatement.  
  
 Antes de llamar a este método setObject, la aplicación podría establecer el parámetro especificado con uno de los siguientes métodos:  
  
-   Los métodos set\<Type> de la clase SQLServerPreparedStatement o la clase SQLServerCallableStatement  
  
-   Los métodos setNull de la clase SQLServerPreparedStatement o la clase SQLServerCallableStatement  
  
-   El método [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) de la clase SQLServerCallableStatement  
  
 En tal caso, el tipo del parámetro se establece automáticamente. Si la aplicación llama a este método setObject con un valor de obj NULL, el controlador supone que el tipo del parámetro se establece a través del método que se llamó anteriormente.  
  
 Si el valor de obj es NULL y no se puede averiguar información de tipo alguna, este método setObject convierte el parámetro especificado en CHAR antes de enviarlo a la base de datos.  
  
 Desde el controlador JDBC 3.0 de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el comportamiento de este método queda modificado por la propiedad de conexión **sendTimeAsDatetime** ([Establecimiento de las propiedades de conexión](../../../connect/jdbc/setting-the-connection-properties.md)) y [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Para más información, vea [Configurar el modo en que los valores java.sql.Time se envían al servidor](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Consulte también  
 [Método setObject &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)   
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Clase SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  

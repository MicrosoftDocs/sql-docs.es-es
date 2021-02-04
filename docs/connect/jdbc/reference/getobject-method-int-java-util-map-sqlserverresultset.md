---
description: Método getObject (int, java.util.Map) (SQLServerResultSet)
title: Método getObject (int, java.util.Map) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getObject (int, java.util.Map)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: df85a514-ab43-4bf6-98dd-f7f37fad1850
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bfc8688546099bcdc334af9cdd62e6b86aeab1ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175408"
---
# <a name="getobject-method-int-javautilmap-sqlserverresultset"></a>Método getObject (int, java.util.Map) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Obtiene el valor del índice de columna designado en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto en el lenguaje de programación Java mediante el objeto Map especificado.  
  
> [!NOTE]  
>  El [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] no admite este método actualmente. Al utilizar este método, siempre se devolverá la asignación predeterminada.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.Object getObject(int i,  
                                  java.util.Map map)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *i*  
  
 Valor **int** que indica el índice de la columna.  
  
 *map*  
  
 Un objeto Map.  
  
## <a name="return-value"></a>Valor devuelto  
 Valor **Object**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getObject especifica este método getObject en la interfaz java.sql.ResultSet.  
  
 Este método devolverá el valor de la columna determinada como un objeto de Java. El tipo del objeto de Java será el tipo de objeto de Java predeterminado que corresponde al tipo SQL de la columna, tras la asignación para los tipos integrados que se indica en las especificaciones de JDBC. Si el valor es NULL de SQL, el controlador devuelve un NULL de Java.  
  
 Este método también se puede utilizar para leer los tipos de datos abstractos específicos de la base de datos. En la API de JDBC 2.0, el comportamiento del método getObject se extiende para materializar datos de tipos de SQL definidos por el usuario. Cuando una columna contiene un valor estructurado o distinto, el comportamiento de este método es como si se llamara a `getObject(columnIndex, this.getStatement().getConnection().getTypeMap())`.  
  
 A partir del controlador JDBC 3.0 de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   Un valor de tipo date se devolverá como un objeto java.sql.Date.  
  
-   Un valor de tipo time se devolverá como un objeto java.sql.Time.  
  
-   Un valor de tipo datetime2 se devolverá como un objeto java.sql.Timestamp.  
  
-   Un valor de tipo datetimeoffset se devolverá como un objeto microsoft.sql.DateTimeOffset.  
  
## <a name="see-also"></a>Consulte también  
 [Método getObject &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  

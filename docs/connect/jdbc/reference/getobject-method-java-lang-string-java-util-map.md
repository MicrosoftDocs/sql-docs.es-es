---
description: Método getObject (java.lang.String, java.util.Map)
title: Método getObject (java.lang.String, java.util.Map) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getObject (java.lang.String, Java.util.Map)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e174eb81-d569-479e-a171-365cd6d44b6a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 86dbc3181b6635e4be010d5f4e0c886e20dd4c23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162619"
---
# <a name="getobject-method-javalangstring-javautilmap"></a>Método getObject (java.lang.String, java.util.Map)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del parámetro designado como un objeto en el lenguaje de programación Java según el nombre de parámetro y usando el objeto Map determinado.  
  
> [!NOTE]  
>  El [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] no admite este método actualmente. Al utilizar este método, siempre se devolverá la asignación predeterminada.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.Object getObject(java.lang.String sCol,  
                                  java.util.Map m)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sCol*  
  
 Objeto **String** que contiene el nombre del parámetro.  
  
 *m*  
  
 Un objeto Map.  
  
## <a name="return-value"></a>Valor devuelto  
 Valor **Object**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getObject especifica este método getObject en la interfaz java.sql.CallableStatement.  
  
 Este método devolverá el valor de la columna determinada como un objeto de Java. El tipo del objeto de Java será el tipo de objeto de Java predeterminado que corresponde al tipo SQL de la columna, tras la asignación para los tipos integrados que se indica en las especificaciones de JDBC. Si el valor es NULL de SQL, el controlador devuelve un NULL de Java.  
  
 Este método también se puede utilizar para leer los tipos de datos abstractos específicos de la base de datos. En la API de JDBC 2.0, el comportamiento del método getObject se extiende para materializar datos de tipos de SQL definidos por el usuario. Cuando una columna contiene un valor estructurado o distinto, el comportamiento de este método es como si se llamara a `getObject(columnIndex, this.getStatement().getConnection().getTypeMap())`.  
  
 A partir del controlador JDBC 3.0 de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   Se devolverá un valor de tipo **date** como un objeto java.sql.Date.  
  
-   Se devolverá un valor de tipo **time** como un objeto java.sql.Time.  
  
-   Se devolverá un valor de tipo **datetime2** como un objeto java.sql.Timestamp.  
  
-   Se devolverá un valor de tipo **datetimeoffset** como un objeto microsoft.sql.DateTimeOffset.  
  
## <a name="see-also"></a>Consulte también  
 [Método getObject &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  

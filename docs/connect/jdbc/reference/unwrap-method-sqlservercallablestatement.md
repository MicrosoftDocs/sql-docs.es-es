---
description: Método unwrap (SQLServerCallableStatement)
title: Método unwrap (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: cbbf2728-b8c8-4c35-875a-6e967c8285dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d73dbbec2500ba50a91a13dd1ec171c207096425
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190322"
---
# <a name="unwrap-method-sqlservercallablestatement"></a>Método unwrap (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve un objeto que implementa la interfaz especificada para permitir el acceso a los métodos específicos de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *iface*  
  
 Clase de tipo **T** que define una interfaz.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto que implementa la interfaz especificada.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md) queda definido por la interfaz java.sql.Wrapper, que se incorporó en las especificaciones de JDBC 4.0.  
  
 Es posible que las aplicaciones necesiten acceso a las extensiones para la API de JDBC que sean específicas de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. El método unwrap admite la desencapsulación para las clases públicas que extiende este objeto, si las clases tienen extensiones de proveedor.  
  
 [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) implementa [ISQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md), que se extiende a partir de [ISQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Cuando se llama a este método, el objeto desencapsula las siguientes clases: [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) y [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
 Para más información, consulte [Contenedores e interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
 En el siguiente ejemplo de código se muestra cómo utilizar los métodos isWrapperFor y unwrap para comprobar las extensiones del controlador e invocar a los métodos específicos del proveedor, como [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) y [getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md).  
  
```  
public static void executeStoredProcedure(Connection con) {  
   try {  
    CallableStatement cstmt =   
       con.prepareCall("{call dbo.stored_proc_name(?, ?)}");  
  
    // The recommended way to access the JDBC   
    // Driver-specific methods is to use the JDBC 4.0 Wrapper   
    // functionality.   
    // The following code statements demonstrates how to use the   
    // isWrapperFor and unwrap methods  
    // to access the driver-specific response buffering methods.  
  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerCallableStatement.class)) {  
     // The CallableStatement object can unwrap to   
     // SQLServerCallableStatement.  
     SQLServerCallableStatement SQLcstmt =   
     cstmt.unwrap(  
        com.microsoft.sqlserver.jdbc.SQLServerCallableStatement.class);  
     SQLcstmt.setResponseBuffering("adaptive");  
     System.out.println("Response buffering mode has been set to " +  
         SQLcstmt.getResponseBuffering());  
     }  
  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement.class)) {  
      // The CallableStatement object can unwrap to   
      // SQLServerPreparedStatement.                    
      SQLServerPreparedStatement SQLpstmt =   
       cstmt.unwrap(  
       com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement.class);  
      SQLpstmt.setResponseBuffering("adaptive");  
      System.out.println("Response buffering mode has been set to " +  
          SQLpstmt.getResponseBuffering());  
    }  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerStatement.class)) {  
  
      // The CallableStatement object can unwrap to SQLServerStatement.   
      SQLServerStatement SQLstmt =   
        cstmt.unwrap(  
        com.microsoft.sqlserver.jdbc.SQLServerStatement.class);  
      SQLstmt.setResponseBuffering("adaptive");  
      System.out.println("Response buffering mode has been set to " +  
      SQLstmt.getResponseBuffering());  
    }  
  }  
  catch (Exception e) {  
     e.printStackTrace();  
  }  
}   
```  
  
## <a name="see-also"></a>Consulte también  
 [Método isWrapperFor &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  

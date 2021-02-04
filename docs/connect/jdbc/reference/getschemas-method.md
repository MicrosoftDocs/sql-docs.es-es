---
description: Método getSchemas ()
title: Método getSchemas () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getSchemas
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: adba0ee6-ff6d-4215-b646-62c735be3fe9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a20265dfd8080e75aae0ab5b293ae2e5651cd571
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162371"
---
# <a name="getschemas-method-"></a>Método getSchemas ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera los nombres de esquema que están disponibles en la base de datos actual.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.ResultSet getSchemas()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getSchemas especifica este método getSchemas en la interfaz java.sql.DatabaseMetaData.  
  
 El conjunto de resultados devuelto por el método getSchemas contiene la siguiente información:  
  
|Nombre|Tipo|Descripción|  
|----------|----------|-----------------|  
|TABLE_SCHEM|**String**|Nombre del esquema.|  
|TABLE_CATALOG|**String**|Nombre del catálogo para el esquema.|  
  
 Los resultados se ordenan según TABLE_CATALOG y después, según TABLE_SCHEM. En cada fila TABLE_SCHEM es la primera columna y TABLE_CATALOG la segunda.  
  
> [!NOTE]  
>  Para más información sobre los datos que devuelve el método getSchemas, vea "sys.schemas (Transact-SQL)" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se muestra cómo utilizar el método getSchemas para devolver información sobre el catálogo y sus nombres de esquema asociados en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cuando el argumento de conexión especifica la base de datos que se va a utilizar.  
  
```  
public static void executeGetSchemas(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getSchemas();  
      ResultSetMetaData rsmd = rs.getMetaData();  
  
      // Display the result set data.  
      int cols = rsmd.getColumnCount();  
      while(rs.next()) {  
         for (int i = 1; i <= cols; i++) {  
            System.out.println(rs.getString(i));  
         }  
      }  
      rs.close();  
   }   
  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  

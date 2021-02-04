---
description: Método getCatalogs (SQLServerDatabaseMetaData)
title: Método getCatalogs (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getCatalogs
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7f8bd0f1-f340-4bb9-b559-0a6176124033
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58ed8fc72dfb7d1abc4f6fc2c77b6c9325408dae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165558"
---
# <a name="getcatalogs-method-sqlserverdatabasemetadata"></a>Método getCatalogs (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera los nombres del catálogo que están disponibles en el servidor conectado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.ResultSet getCatalogs()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getCatalogs especifica este método getCatalogs en la interfaz java.sql.DatabaseMetaData.  
  
> [!NOTE]  
>  En Azure SQL Database, debe conectarse a la base de datos maestra para llamar a **SQLServerDatabaseMetaData.getCatalogs**. SQL Database no admite la devolución del conjunto entero de catálogos desde una base de datos de usuario. **SQLServerDatabaseMetaData.getCatalogs** usa la vista sys.databases para obtener los catálogos. Consulte la explicación de los permisos en [sys.database_usage (Azure SQL Database)](../../../relational-databases/system-catalog-views/sys-database-usage-azure-sql-database.md) para comprender el comportamiento de **SQLServerDatabaseMetaData.getCatalogs** en SQL. En Azure SQL Database, debe conectarse a la base de datos maestra para llamar a **SQLServerDatabaseMetaData.getCatalogs**. SQL Database no admite la devolución del conjunto entero de catálogos desde una base de datos de usuario. **SQLServerDatabaseMetaData.getCatalogs** usa la vista sys.databases para obtener los catálogos. Consulte la explicación de los permisos en [sys.database_usage (Azure SQL Database)](../../../relational-databases/system-catalog-views/sys-database-usage-azure-sql-database.md) para entender el comportamiento de **SQLServerDatabaseMetaData.getCatalogs** en SQL Database.                      .  
  
 El conjunto de resultados devuelto por el método getCatalogs contendrá la siguiente información:  
  
|Nombre|Tipo|Descripción|  
|----------|----------|-----------------|  
|TABLE_CAT|**String**|Nombre del catálogo, lo cual incluye las bases de datos del sistema de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se muestra cómo utilizar el método getCatalogs para devolver los nombres de todas las bases de datos que contiene [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], incluso las bases de datos del sistema.  
  
```  
public static void executeGetCatalogs(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getCatalogs();  
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
  
  

---
description: Método getTypeInfo (SQLServerDatabaseMetaData)
title: Método getTypeInfo (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getTypeInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 23208f01-c1bf-4235-b29c-9051d3df59a3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43145d029b4095ab05cd47526deface99dc81eb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155647"
---
# <a name="gettypeinfo-method-sqlserverdatabasemetadata"></a>Método getTypeInfo (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una descripción de todos los tipos SQL estándar que se admiten en la base de datos actual.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.ResultSet getTypeInfo()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getTypeInfo especifica este método getTypeInfo en la interfaz java.sql.DatabaseMetaData.  
  
 El conjunto de resultados devuelto por el método getTypeInfo contendrá la siguiente información:  
  
|Nombre|Tipo|Descripción|  
|----------|----------|-----------------|  
|TYPE_NAME|**String**|El nombre del tipo de datos.|  
|DATA_TYPE|**short**|Tipo de datos SQL de java.sql.Types.|  
|PRECISION|**int**|Número total de dígitos significativos.|  
|LITERAL_PREFIX|**String**|Carácter o caracteres utilizados antes de una constante.|  
|LITERAL_SUFFIX|**String**|Carácter o caracteres utilizados para terminar una constante.|  
|CREATE_PARAMS|**String**|Descripción de los parámetros de creación para el tipo de datos.|  
|NULLABLE|**short**|Indica si la columna puede contener un valor NULL. Puede ser uno de los siguientes valores:<br /><br /> typeNoNulls (0)<br /><br /> typeNullable (1)<br /><br /> typeNullableUnknown (2)|  
|CASE_SENSITIVE|**boolean**|Indica si el tipo de datos distingue mayúsculas de minúsculas. "**true**" si el tipo distingue mayúsculas de minúsculas; de lo contrario, es "**false**".|  
|BUSCABLE|**short**|Indica si la columna se puede utilizar en una cláusula WHERE de SQL. Puede ser uno de los siguientes valores:<br /><br /> typePredNone (0)<br /><br /> typePredChar (1)<br /><br /> typePredBasic (2)<br /><br /> typeSeachable (3)|  
|UNSIGNED_ATTRIBUTE|**boolean**|Indica el signo del tipo de datos. "**true**" si el tipo no tiene signo; de lo contrario, es "**false**".|  
|FIXED_PREC_SCALE|**boolean**|Indica que el tipo de datos puede ser un valor de moneda. "**true**" si el tipo de datos es de tipo moneda; de lo contrario, es "**false**".|  
|AUTO_INCREMENT|**boolean**|Indica que el tipo de datos se puede incrementarse automáticamente. "**true**" si el tipo se puede incrementar automáticamente; de lo contrario, es "**false**".|  
|LOCAL_TYPE_NAME|**String**|Nombre localizado del tipo de datos.|  
|MINIMUM_SCALE|**short**|Número máximo de dígitos a la derecha del signo decimal.|  
|MAXIMUM_SCALE|**short**|Número mínimo de dígitos a la derecha del signo decimal.|  
|SQL_DATA_TYPE|**int**|El controlador JDBC no lo admite.|  
|SQL_DATETIME_SUB|**int**|El controlador JDBC no lo admite.|  
|NUM_PREC_RADIX|**int**|El número de bits o dígitos para calcular el número máximo que puede tener una columna.|  
|INTERVAL_PRECISION|**smallint**|Valor de la precisión del principio del intervalo.|  
|USERTYPE|**smallint**|Valor **usertype** de la tabla **systypes**. Para obtener más información, vea los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
> [!NOTE]  
>  Para más información sobre los datos que devuelve el método getTypeInfo, vea "sp_datatype_info (Transact-SQL)" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se muestra cómo utilizar el método getTypeInfo para devolver información sobre los tipos de datos que se utilizan en una base de datos de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] (o versiones posteriores).  
  
```  
public static void executeGetTypeInfo(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getTypeInfo();  
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
  
  

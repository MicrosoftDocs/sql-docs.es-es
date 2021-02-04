---
description: Método execute (java.lang.String, java.lang.String)
title: Método execute (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.execute (java.lang.String.java.lang.String[])
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9451c7c2-4c0d-4d1e-9b42-a26ff28e3f6a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4103e4a10aff5f16c68cfc584d31a1e6600e723
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168479"
---
# <a name="execute-method-javalangstring-javalangstring"></a>Método execute (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ejecuta la instrucción SQL especificada, que puede devolver varios resultados, e indica al [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] que las claves que se han generado automáticamente y están presentes en la matriz dada deben estar disponibles para su recuperación.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final boolean execute(java.lang.String sql,  
                             java.lang.String[] columnNames)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sql*  
  
 Un valor **String** que contiene una instrucción SQL.  
  
 *columnNames*  
  
 Una matriz de cadenas que indica que los nombres de columna de las claves que se generaron automáticamente deben estar disponibles.  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si el primer resultado es un conjunto de resultados. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método execute especifica este método execute en la interfaz java.sql.Statement.  
  
## <a name="see-also"></a>Consulte también  
 [Método execute &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md)   
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  

---
description: Método close (SQLServerResultSet)
title: Método close (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.close
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8f3adf5b-874e-4cf2-b4ef-672dda42d77a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0a6f5e311333da6c16015acd2fe6ab007204b8d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168600"
---
# <a name="close-method-sqlserverresultset"></a>Método close (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Libera inmediatamente la base de datos de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) y los recursos de JDBC en lugar de esperar a que esto se realice cuando se cierre de forma automática.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void close()  
```  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método close especifica este método close en la interfaz java.sql.ResultSet.  
  
 Un objeto SQLServerResultSet se cierra automáticamente por el objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) que lo generó cuando ese objeto SQLServerStatement se cierra, se vuelve a utilizar o se utiliza para recuperar el siguiente resultado en un flujo de varios resultados. También se cierra un objeto SQLServerResultSet automáticamente cuando se recopila como elemento no utilizado.  
  
 Cuando se ejecuta una instrucción que genera un conjunto de resultados grande de solo avance y solo lectura, es posible que únicamente esté interesado en algún conjunto inicial de filas en el conjunto de resultados que se ha devuelto. En este caso, la aplicación podría llamar al método [cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md) del objeto de instrucción asociado antes de cerrar el conjunto de resultados con el fin de minimizar el tiempo de procesamiento necesario para descartar las filas innecesarias restantes. Recomendamos comparar los inconvenientes entre el tiempo de procesamiento que se ahorraría y el tiempo y el viaje de ida y vuelta al servidor que se necesita para cancelar la ejecución a la hora de decidir si utilizar o no esta técnica.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  

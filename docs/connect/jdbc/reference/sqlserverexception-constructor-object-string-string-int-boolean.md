---
description: Constructor SQLServerException (java.lang.Object, java.lang.String, java.lang.String, int, boolean)
title: Constructor SQLServerException (java.lang.Object, java.lang.String, java.lang.String, int, boolean) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 738f5c040a06fb5b11fc6cf69cf76624304a9ef9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176628"
---
# <a name="sqlserverexception-constructor-javalangobject-javalangstring-javalangstring-int-boolean"></a>Constructor SQLServerException (java.lang.Object, java.lang.String, java.lang.String, int, boolean)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Inicializa una nueva instancia de la clase [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md) cuando se proporciona un **objeto**, un objeto de **cadena**, un objeto de **cadena**, un **entero** y un **valor booleano**.

## <a name="syntax"></a>Sintaxis  
  
```  

public SQLServerException(java.lang.Object obj,
            java.lang.String errText,
            java.lang.String errState,
            int errNum,
            boolean bStack)

            
```  
  
#### <a name="parameters"></a>Parámetros  
 *obj*  
  
 Búfer de E/S que generó la excepción.

 *errText*  
  
 Cadena que contiene el texto de error.
  
 *sqlState*  
  
 Objeto de enumeración que contiene el estado de SQL.
 
 *errNum*  
  
 Un entero que contiene el código de error para la excepción.
 
 *bStack*  
  
 Un valor booleano que indica si se debe generar el seguimiento de la pila.
  
## <a name="see-also"></a>Consulte también  
 [Constructores SQLServerException](../../../connect/jdbc/reference/sqlserverexception-constructors.md)   
 [Miembros SQLServerException](../../../connect/jdbc/reference/sqlserverexception-members.md)   
 [Clase SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
  

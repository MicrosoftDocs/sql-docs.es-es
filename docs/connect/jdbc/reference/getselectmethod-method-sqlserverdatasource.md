---
description: Método getSelectMethod (SQLServerDataSource)
title: Método getSelectMethod (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getSelectMethod
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b6255d2e-0028-474a-afa8-553ef092243e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b7f60926fdf69a27c5e78d2817084cfe1500f91b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175044"
---
# <a name="getselectmethod-method-sqlserverdatasource"></a>Método getSelectMethod (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve el tipo de cursor predeterminado que se utiliza para todos los conjuntos de resultados creados mediante este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getSelectMethod()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **String** que contiene el tipo de cursor predeterminado.  
  
## <a name="remarks"></a>Observaciones  
 La propiedad selectMethod especifica el tipo de cursor predeterminado que se utiliza para un conjunto de resultados. Esta propiedad es útil cuando se estén abordando grandes conjuntos de resultados y no se quiera almacenar el conjunto de resultados entero en la memoria del lado cliente. Si se establece la propiedad en "cursor", podrá crear un cursor en el lado del servidor que puede capturar fragmentos más pequeños de cada vez. Si no se establece la propiedad selectMethod, el método getSelectMethod devuelve el valor predeterminado "direct".  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  

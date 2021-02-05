---
description: Método isWrapperFor (SQLServerStatement)
title: Método isWrapperFor (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 53f3291f-d43a-476b-a656-d86168dacf6c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b2cb6fe67d9a2ec5cbb0c56c5e1d1c3573533ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177125"
---
# <a name="iswrapperfor-method-sqlserverstatement"></a>Método isWrapperFor (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica si este objeto de instrucción es un contenedor para la interfaz especificada.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean isWrapperFor(Class iface)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *iface*  
  
 Una **clase** que define una interfaz.  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si este objeto implementa la interfaz o encapsula un objeto que implementa la interfaz. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método [isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md) y el método [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md) se definen a través de la interfaz java.sql.Wrapper, que se incorpora en JDBC 4.0.  
  
 Si este método devuelve el valor true, la llamada a [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md) con el mismo argumento se realizará correctamente.  
  
 Para ver un código de ejemplo, consulte [Actualización de ejemplo de datos grandes](../../../connect/jdbc/updating-large-data-sample.md).  
  
 Para más información, consulte [Contenedores e interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Consulte también  
 [Método unwrap &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md)   
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  

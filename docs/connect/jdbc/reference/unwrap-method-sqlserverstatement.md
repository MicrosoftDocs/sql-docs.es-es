---
description: Método unwrap (SQLServerStatement)
title: Método unwrap (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ce680176-ef04-4e44-bb6c-ec50bd06e7e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 633d24d6d90166e361f21f197da27e1d27c0eee1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160902"
---
# <a name="unwrap-method-sqlserverstatement"></a>Método unwrap (SQLServerStatement)
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
 El método [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md) queda definido por la interfaz java.sql.Wrapper, que se incorporó en las especificaciones de JDBC 4.0.  
  
 Es posible que las aplicaciones necesiten acceso a las extensiones para la API de JDBC que sean específicas de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. El método unwrap admite la desencapsulación para las clases públicas que extiende este objeto, si las clases tienen extensiones de proveedor.  
  
 Cuando se llama a este método, el objeto desencapsula la clase [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
 Para obtener código de ejemplo, consulte [Actualización de ejemplo de datos grandes](../../../connect/jdbc/updating-large-data-sample.md) o [Método unwrap &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md).  
  
 Para más información, consulte [Contenedores e interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Consulte también  
 [Método isWrapperFor &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md)   
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  

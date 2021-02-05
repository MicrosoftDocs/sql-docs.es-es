---
description: Clase SQLServerStatement
title: Clase SQLServerStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ec24963c-8b51-4838-91e9-1fbfa2347451
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b969c9ce8073ebfbd65e207af82c004984b04857
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180002"
---
# <a name="sqlserverstatement-class"></a>Clase SQLServerStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Representa la implementación básica de la funcionalidad de la instrucción de JDBC.  
  
 **Paquete:** com.microsoft.sqlserver.jdbc  
  
 **Implementa:** [ISQLServerStatement](../../../connect/jdbc/reference/isqlserverstatement-interface.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public class SQLServerStatement  
```  
  
## <a name="remarks"></a>Observaciones  
 La clase SQLServerStatement también proporciona varios métodos de implementación de la clase base para la instrucción preparada e instrucciones invocables de JDBC. El rol básico de la clase SQLServerStatement es ejecutar las instrucciones SQL y, a continuación, devolver los conteos de actualizaciones y conjuntos de resultados a la aplicación del usuario.  
  
 Esta clase admite la desencapsulación en la clase SQLServerStatement, la interfaz ISQLServerStatement y la interfaz java.sql.Statement. Para más información, consulte [Contenedores e interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Referencia de API del controlador JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  

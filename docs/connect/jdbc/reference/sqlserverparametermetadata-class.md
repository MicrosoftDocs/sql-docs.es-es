---
description: Clase SQLServerParameterMetaData
title: Clase SQLServerParameterMetaData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 546290e0-9411-4a2b-aa36-61251e70e9cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cac82c9b917f4357f35f8e5bff2ea661f0a50ee3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178134"
---
# <a name="sqlserverparametermetadata-class"></a>Clase SQLServerParameterMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Representa los metadatos para los parámetros de instrucción preparados.  
  
 **Paquete:** com.microsoft.sqlserver.jdbc  
  
 **Extiende:** java.lang.Object  
  
 **Implementa:** java.sql.ParameterMetaData  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public class SQLServerParameterMetaData  
```  
  
## <a name="remarks"></a>Observaciones  
 Para recuperar los metadatos de parámetro, las instrucciones preparadas se ejecutan con SET FMT ONLY. Las instrucciones invocables llaman a sp_sproc_columns para recuperar nombres y metadatos para los parámetros de procedimiento.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-members.md)   
 [Referencia de API del controlador JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  

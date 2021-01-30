---
description: Función SQLSetConnectOption
title: Función SQLSetConnectOption | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConnectOption
helpviewer_keywords:
- SQLSetConnectOption function [ODBC]
ms.assetid: 8cd2c2a2-25c8-4aff-951c-b593bbfc90ad
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f07cd0cb70c83e1ea2eb7bc1d9a7fa9e583f1ce8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192414"
---
# <a name="sqlsetconnectoption-function"></a>Función SQLSetConnectOption
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: desusado  
  
 **Resumen**  
 En ODBC 3 *. x*, la función **SQLSetConnectOption** de ODBC 2,0 se ha reemplazado por **SQLSetConnectAttr**. Para obtener más información, consulte [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
> [!NOTE]
>  Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando una aplicación ODBC 2 *. x* está trabajando con un controlador ODBC 3 *. x* , vea [asignar funciones en desuso](../../../odbc/reference/appendixes/mapping-deprecated-functions.md).  
  
## <a name="remarks"></a>Observaciones  
 Consulte la [información de ODBC 64](../../../odbc/reference/odbc-64-bit-information.md)bits, si la aplicación se ejecutará en un sistema operativo de 64 bits.  
  
> [!NOTE]  
>  **SQLSetConnectOption** no admite el SQL_ASYNC_DBC_FUNCTION_ENABLE atributo introducido en ODBC 3,8. Las aplicaciones que usan la operación asincrónica en el identificador de conexión deben usar **SQLSetConnectAttr**.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)

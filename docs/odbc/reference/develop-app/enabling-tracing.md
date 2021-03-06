---
description: La habilitación del seguimiento
title: Habilitando el seguimiento | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- tracing options [ODBC], enabling
ms.assetid: 48e318bd-2487-4708-a698-ea01f36a45e9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: af9a78989a80124fb9a7f475edf17022e7942bde
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88482988"
---
# <a name="enabling-tracing"></a>La habilitación del seguimiento
El seguimiento se puede habilitar de las tres maneras siguientes:  
  
-   Establezca **las palabras clave Trace y de** **seguimiento** en la entrada del registro Odbc.ini. Esto habilita o deshabilita el seguimiento cuando se llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV. Estas opciones se establecen en la pestaña seguimiento del cuadro de diálogo Administrador de orígenes de datos ODBC que se muestra durante la configuración del origen de datos. Para obtener más información, vea [entradas del registro para orígenes de datos](../../../odbc/reference/install/registry-entries-for-data-sources.md).  
  
-   Llame a **SQLSetConnectAttr** para establecer el atributo de conexión SQL_ATTR_TRACE en SQL_OPT_TRACE_ON. Esto habilita o deshabilita el seguimiento mientras dure la conexión. Para obtener más información, consulte la descripción de la función [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md) .  
  
-   Use **ODBCSharedTraceFlag** para activar o desactivar el seguimiento de forma dinámica. (Para obtener más información, vea el tema siguiente, [seguimiento dinámico](../../../odbc/reference/develop-app/dynamic-tracing.md)).

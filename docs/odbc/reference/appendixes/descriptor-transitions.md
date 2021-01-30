---
description: Transiciones de descriptor
title: Transiciones de descriptor | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC], descriptor
- transitioning states [ODBC], descriptor
- descriptor transitions [ODBC]
ms.assetid: 0cf24fe6-5e3c-45fa-81b8-4f52ddf8501d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d312b372a0817ae79ad07bdbf149f90ba0c71939
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194890"
---
# <a name="descriptor-transitions"></a>Transiciones de descriptor
Los descriptores ODBC tienen los tres Estados siguientes.  
  
|Estado|Descripción|  
|-----------|-----------------|  
|D0|Descriptor sin asignar|  
|D1i|Descriptor asignado implícitamente|  
|D1e|Descriptor asignado explícitamente|  
  
 En las tablas siguientes se muestra cómo afecta cada función ODBC al estado del descriptor.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|D0<br /><br /> Sin asignar|D1i<br /><br /> Implícita|D1e<br /><br /> Explícita|  
|------------------------|----------------------|----------------------|  
|D1i [1]|--|--|  
|D1e [2]|--|--|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DESC *HandleType* .  
  
## <a name="sqlcopydesc"></a>SQLCopyDesc  
  
|D0<br /><br /> Sin asignar|D1i<br /><br /> Implícita|D1e<br /><br /> Explícita|  
|------------------------|----------------------|----------------------|  
|ADMITIR|--|--|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|D0<br /><br /> Sin asignar|D1i<br /><br /> Implícita|D1e<br /><br /> Explícita|  
|------------------------|----------------------|----------------------|  
|--[1]|D0|--|  
|ADMITIR dos|(HY017)|D0|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DESC *HandleType* .  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField y SQLGetDescRec  
  
|D0<br /><br /> Sin asignar|D1i<br /><br /> Implícita|D1e<br /><br /> Explícita|  
|------------------------|----------------------|----------------------|  
|ADMITIR|--|--|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField y SQLSetDescRec  
  
|D0<br /><br /> Sin asignar|D1i<br /><br /> Implícita|D1e<br /><br /> Explícita|  
|------------------------|----------------------|----------------------|  
|ADMITIR dimensional|--|--|  
  
 [1] esta fila muestra las transiciones cuando *DescriptorHandle* era el identificador de ARD, APD o IPD, o (para **SQLSetDescField**) cuando *DescriptorHandle* era el identificador de un IRD y *FieldIdentifier* se SQL_DESC_ARRAY_STATUS_PTR o SQL_DESC_ROWS_PROCESSED_PTR.  
  
## <a name="all-other-odbc-functions"></a>Todas las demás funciones ODBC  
  
|D0<br /><br /> Sin asignar|D1i<br /><br /> Implícita|D1e<br /><br /> Explícita|  
|------------------------|----------------------|----------------------|  
|--|--|--|

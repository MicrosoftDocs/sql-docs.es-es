---
description: Asignación de SQLAllocConnect
title: Asignación de SQLAllocConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocConnect function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLAllocConnect
ms.assetid: ac89dd1f-c565-47cc-8fa3-6fa5f80b5d63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fd1659e4faaee713a2b9d942deb53fe00ea367c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202982"
---
# <a name="sqlallocconnect-mapping"></a>Asignación de SQLAllocConnect
Cuando una aplicación llama a **SQLAllocConnect** a través de ODBC 3. *x* , la llamada a **SQLAllocConnect**(*HENV*, *phdbc*) se asigna a **SQLAllocHandle** de la siguiente manera:  
  
1.  El administrador de controladores asigna una conexión y la devuelve a la aplicación.  
  
2.  Cuando la aplicación establece una conexión, el administrador de controladores llama a  
  
    ```  
    SQLAllocHandle(SQL_HANDLE_DBC, InputHandle, OutputHandlePtr)  
    ```  
  
     en el controlador con *InputHandle* establecido en *HENV* y *OutputHandlePtr* establecido en *phdbc*.

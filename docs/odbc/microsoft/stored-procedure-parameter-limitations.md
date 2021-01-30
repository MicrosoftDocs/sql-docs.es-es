---
description: Limitaciones de parámetro de procedimiento almacenado
title: Limitaciones de los parámetros de procedimientos almacenados | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], stored procedures
ms.assetid: 8b804bcf-4cce-4e6f-aa45-00bab9ef9921
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c92ff03bf5cf8899706966169c9b3c770f64f57c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188962"
---
# <a name="stored-procedure-parameter-limitations"></a>Limitaciones de parámetro de procedimiento almacenado
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. En su lugar, utilice el controlador ODBC proporcionado por Oracle.  
  
 Cuando se ejecutan procedimientos almacenados de Oracle que usan 10 o más parámetros de salida, se producirá un error en la llamada al procedimiento almacenado, lo que provocará un error de infracción de acceso o de Objetos de datos ActiveX (ADO). Esto puede ocurrir cuando se usa el controlador ODBC de Microsoft para Oracle con las versiones 8.0.4.0.0 y 8.0.4.0.4 del software cliente de Oracle.  
  
 Para corregir el problema, el software de cliente de Oracle debe actualizarse a la versión 8.0.4.2.0 o posterior. Póngase en contacto con Oracle Corporation para obtener más información acerca de las [revisiones](../../odbc/microsoft/oracle-software-patches.md).  
  
> [!NOTE]  
>  Este problema no se produce con la versión temprana de la versión 8.0.3.0.0 del software cliente de Oracle.

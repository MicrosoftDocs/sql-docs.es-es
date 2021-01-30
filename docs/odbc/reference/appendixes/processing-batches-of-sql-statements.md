---
description: Procesamiento de lotes de instrucciones SQL
title: Procesar lotes de instrucciones SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], batches
- ODBC cursor library [ODBC], statement processing
- cursor library [ODBC], batches
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
- SQL statements [ODBC], batches
- batches [ODBC], processing batches of SQL statements
ms.assetid: 04b93ef9-11de-47a3-8bd8-ba963c42f182
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2c70dc742b170f608246eff1bbb12234a26414b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209488"
---
# <a name="processing-batches-of-sql-statements"></a>Procesamiento de lotes de instrucciones SQL
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 La biblioteca de cursores no admite lotes de instrucciones SQL, incluidas las instrucciones SQL para las que el atributo de instrucción SQL_ATTR_PARAMSET_SIZE es mayor que 1. Si una aplicación envía un lote de instrucciones SQL a la biblioteca de cursores, los resultados son indefinidos.

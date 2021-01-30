---
description: SQLSetEnvAttr y la biblioteca de cursores
title: SQLSetEnvAttr y la biblioteca de cursores | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC], Cursor Library
ms.assetid: 59cc8eae-09ae-4796-869a-c5806488ae83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 791933e7c4edd263e282437b06fb79fca65fca65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202641"
---
# <a name="sqlsetenvattr-and-the-cursor-library"></a>SQLSetEnvAttr y la biblioteca de cursores
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLSetEnvAttr** con la biblioteca de cursores. Para obtener información general sobre **SQLSetEnvAttr**, consulte la [función SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md).  
  
 La biblioteca de cursores no se ve afectada por la configuración del atributo de entorno SQL_ATTR_ODBC_VERSION, independientemente de la versión de la aplicación o de la versión del controlador.

---
description: Cargar por ordinal
title: Cargando por ordinal | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], loading by ordinal
- compatibility [ODBC], loading by ordinal
- loading by ordinal [ODBC]
ms.assetid: 337d90ab-68eb-4940-a2f3-f7d5693ee766
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ffb2516a79144a5ae79b21e6621882056b1f69ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184373"
---
# <a name="loading-by-ordinal"></a>Cargar por ordinal
En ODBC *2. x*, se puede realizar la carga por ordinal para mejorar el rendimiento del proceso de conexión. Un controlador ODBC *2. x* exporta una función ficticia con el ordinal 199; Cuando el administrador de controladores lo detecta, resuelve las direcciones de las funciones ODBC por ordinal, no por nombre. Esta funcionalidad todavía se admite para los controladores ODBC *2. x* , pero no se admite para los controladores ODBC *3. x* .

---
description: 'Apéndice G: Directrices de controlador para la compatibilidad con versiones anteriores'
title: 'Apéndice G: instrucciones del controlador para la compatibilidad con versiones anteriores | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC drivers [ODBC], backward compatibility
- backward compatibility [ODBC], drivers
- compatibility [ODBC], drivers
ms.assetid: 911cd335-f2c0-4d03-9739-1078308a678a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 337640ec943833dc333483fe2256fb8204c92e9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212518"
---
# <a name="appendix-g-driver-guidelines-for-backward-compatibility"></a>Apéndice G: Directrices de controlador para la compatibilidad con versiones anteriores
Este apéndice proporciona información para los escritores de controladores que trabajan en ODBC 3. Controladores *x* que necesitan admitir ODBC 2. aplicaciones *x* . Para obtener más información sobre la compatibilidad con versiones anteriores, vea [compatibilidad con versiones anteriores y cumplimiento de estándares](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).  
  
 Esta sección contiene los temas siguientes.  
  
-   [Cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores para los controladores ODBC 3. x](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) : las nuevas características son características que existen en ODBC 3. *x* y no en ODBC 2. *x*. ODBC 3. por lo general, los controladores *x* no tienen que preocuparse por la compatibilidad con las nuevas características porque ODBC 2. las aplicaciones *x* nunca las usan. La única excepción a esto son las características relacionadas con **SQLFetch**, **SQLFetchScroll**, **SQLSetPos** y **SQLExtendedFetch**; para obtener más información, vea, más adelante en este apéndice.  
  
-   [Asignación de funciones desusadas](../../../odbc/reference/appendixes/mapping-deprecated-functions.md) : las características duplicadas son características que se implementan de forma diferente en ODBC 3. *x* y ODBC 2. *x*. ODBC 3. los controladores *x* no tienen que preocuparse por la compatibilidad con versiones anteriores, ya que el administrador de controladores siempre asigna ODBC 2. *x* características de ODBC 3. *x* características de al llamar a un ODBC 3. controlador *x* . Por lo tanto, ODBC 3. el controlador *x* solo ve ODBC 3. *x* características de. Para obtener más información acerca de estas asignaciones, vea, más adelante en este apéndice.  
  
-   [Cambios de comportamiento y controladores ODBC 3. x](../../../odbc/reference/appendixes/behavioral-changes-and-odbc-3-x-drivers.md) : los cambios de comportamiento son características que se administran de forma diferente en ODBC 3. *x* y ODBC 2. *x*. ODBC 3. los controladores *x* deben preocuparse de los cambios de comportamiento y actuar en respuesta al atributo de entorno SQL_ATTR_ODBC_VERSION establecido por la aplicación.

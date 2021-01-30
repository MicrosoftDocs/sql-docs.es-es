---
description: Cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores
title: Cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [ODBC]
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: d9d271f6-d2d9-49b9-a365-4909ca06caae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 32e21d7f066432d2ccd6cced3c326dfe35c76867
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212481"
---
# <a name="block-cursors-scrollable-cursors-and-backward-compatibility"></a>Cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores
La existencia de **SQLFetchScroll** y **SQLExtendedFetch** representa la primera división clara en ODBC entre la interfaz de programación de aplicaciones (API), que es el conjunto de funciones a las que llama la aplicación y la interfaz del proveedor de servicios (SPI), que es el conjunto de funciones que implementa el controlador. Esta división es necesaria para que ODBC *3. x*, que utiliza **SQLFetchScroll**, se alinee con los estándares y también sea compatible con ODBC *2. x*, que utiliza **SQLExtendedFetch**.  
  
 La API de ODBC *3. x* , que es el conjunto de funciones a las que llama la aplicación, incluye **SQLFetchScroll** y atributos de instrucción relacionados. El SPI de ODBC *3. x* , que es el conjunto de funciones que implementa el controlador, incluye **SQLFetchScroll**, **SQLExtendedFetch** y atributos de instrucción relacionados. Dado que ODBC no aplica formalmente esta división entre la API y el SPI, es posible que las aplicaciones de ODBC *3. x* llamen a **SQLExtendedFetch** y a los atributos de instrucción relacionados. Sin embargo, no hay ninguna razón para que la aplicación ODBC *3. x* lo haga. Para obtener más información sobre las API y el SPI, consulte la introducción a la [arquitectura de ODBC](../../../odbc/reference/odbc-architecture.md).  
  
 Para obtener información acerca de las funciones y los atributos de instrucción que una aplicación ODBC *3. x* debe utilizar con cursores de bloque y desplazables, consulte [cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores para aplicaciones ODBC 3. x](../../../odbc/reference/develop-app/block-cursors-scrollable-backward-compatibility-odbc-3-x-applications.md).  
  
 Esta sección contiene los temas siguientes.  
  
-   [Lo que hace el Administrador de controladores](../../../odbc/reference/appendixes/what-the-driver-manager-does.md)  
  
-   [Lo que hace el controlador](../../../odbc/reference/appendixes/what-the-driver-does.md)

---
description: Compatibilidad internacional (controlador ODBC de Visual FoxPro)
title: Compatibilidad Internacional (controlador ODBC de Visual FoxPro) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- double-byte character sets [ODBC]
- FoxPro ODBC driver [ODBC], international support
- DBCS [ODBC]
- international support [ODBC]
- sort order [ODBC]
- collating sequences [ODBC]
- Visual FoxPro ODBC driver [ODBC], international support
ms.assetid: cd3fab32-13f1-4a86-abc4-5e18667669fc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 575eb2361b5869f924cf2b8e5721121182b684db
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88449467"
---
# <a name="international-support-visual-foxpro-odbc-driver"></a>Compatibilidad internacional (controlador ODBC de Visual FoxPro)
El controlador ODBC de Microsoft Visual FoxPro admite:  
  
-   Juegos de caracteres de doble byte (DBCS)  
  
-   Varias secuencias de intercalación  
  
 Una secuencia de intercalación define el criterio de *ordenación* de los datos almacenados en una tabla o base de datos de Visual FoxPro. De forma predeterminada, el controlador está configurado para usar las secuencias de intercalación que admiten la versión de idioma del sistema operativo.  
  
 Para obtener una lista de las secuencias de intercalación admitidas, vea [set COLLATE](../../odbc/microsoft/set-collate-command.md).  
  
## <a name="locale"></a>locale  
 Conjunto de información que corresponde a un idioma y país o región determinados. Una configuración regional indica una configuración específica, como separadores decimales, formatos de fecha y hora y orden de clasificación de caracteres.  
  
## <a name="sort-order"></a>criterio de ordenación  
 Los criterios de ordenación incorporan las reglas de ordenación de diferentes *configuraciones regionales*, lo que le permite ordenar los datos de esos lenguajes correctamente. En Visual FoxPro, el criterio de ordenación actual determina los resultados de las comparaciones de expresiones de caracteres y el orden en que los registros aparecen en las tablas indizadas o ordenadas.

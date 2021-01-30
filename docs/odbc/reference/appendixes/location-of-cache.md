---
description: Ubicación de caché
title: Ubicación de la caché | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 240d6162-4da6-4b1f-96c7-f379f4ecb16f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b500aa9b7955c91c6ca39b3f7f830b2022dc4a57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184364"
---
# <a name="location-of-cache"></a>Ubicación de caché
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 La biblioteca de cursores almacena en caché los datos en memoria y en Windows® archivos temporales. Esto limita el tamaño del conjunto de resultados que la biblioteca de cursores puede controlar solo en el espacio disponible en disco. Un archivo temporal se utiliza cuando los datos que se van a almacenar en caché cruzan el límite del segmento si se insertan al final de la memoria caché de la biblioteca de cursores. En su lugar, se agregan los datos que se van a almacenar en caché en lugar del bloque de datos guardado por última vez en la memoria caché. El bloque de datos guardado por última vez se guarda en un archivo temporal. Si la biblioteca de cursores finaliza de forma anómala, como cuando se produce un error en la alimentación, puede dejar archivos temporales de Windows en el disco. Se denominan ~ CTT *nnnn*. tmp y se crean en el directorio actual.  
  
> [!NOTE]  
>  Si la biblioteca de cursores de Microsoft® WindowsNT®/windows2000 intenta almacenar en caché los datos de un archivo temporal en el directorio actual mientras la aplicación se ejecuta desde un recurso compartido de solo lectura o un disco compacto (por ejemplo, un ejemplo biblioteca MFC (Microsoft Foundation Class)), se devolverá SQLSTATE HY000 (general Error-Unable para crear un búfer de archivos).

---
description: Comando BLOCKSIZE Set
title: Comando SET blocksize | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set blocksize command [ODBC]
ms.assetid: 0c11580f-37f5-4a8e-99be-9fb9c44bb433
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f1777b62d279db78154383a3f328591776c0a5c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208660"
---
# <a name="set-blocksize-command"></a>Comando BLOCKSIZE Set
Especifica cómo se asigna espacio en disco para el almacenamiento de campos de memorando.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET BLOCKSIZE TO nBytes  
```  
  
## <a name="arguments"></a>Argumentos  
 *nBytes*  
 Especifica el tamaño de bloque en el que se asigna espacio en disco para los campos de memorando. Si *nBytes* es 0, el espacio en disco se asigna en bytes únicos (bloques de 1 bytes). Si *nBytes* es un entero entre 1 y 32, el espacio en disco se asigna en bloques de *nBytes* bytes multiplicado por 512. Si *nBytes* es mayor que 32, el espacio en disco se asigna en bloques de *nBytes* bytes. Si especifica un valor de tamaño de bloque superior a 32, puede ahorrar bastante espacio en disco.  
  
## <a name="remarks"></a>Observaciones  
 El valor predeterminado para SET blocksize es 64. Para restablecer el tamaño de bloque a un valor diferente una vez creado el archivo, establézcalo en un valor nuevo y, a continuación, use copiar para crear una nueva tabla. La nueva tabla tiene el tamaño de bloque especificado.

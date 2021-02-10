---
description: Cursor y las características de bloqueo
title: Características de cursor y bloqueo | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- locks [ADO], characteristics
- adOpenDynamic [ADO]
- cursors [ADO], characteristics
ms.assetid: 459c29cb-4230-42bf-8cc2-f3132ccc7aba
author: rothja
ms.author: jroth
ms.openlocfilehash: cecbdd12fd98e6f58d07cfe30c6c0b8a044ed15e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037695"
---
# <a name="cursor-and-lock-characteristics"></a>Cursor y las características de bloqueo
Aunque las características de un cursor dependen de las capacidades del proveedor, las siguientes ventajas y desventajas suelen aplicarse a los distintos tipos de cursores y bloqueos.  
  
|Cursor o tipo de bloqueo|Ventajas|Inconvenientes|  
|-------------------------|----------------|-------------------|  
|**adOpenForwardOnly**|-Requisitos de recursos insuficientes|-No se puede desplazar hacia atrás<br />-Sin simultaneidad de datos|  
|**adOpenStatic**|-Desplazable|-Sin simultaneidad de datos|  
|**adOpenKeyset**|-Parte de la simultaneidad de datos<br />-Desplazable|-Requisitos de recursos más altos<br />-No disponible en escenario desconectado|  
|**adOpenDynamic**|-Simultaneidad de datos alta<br />-Desplazable|-Requisitos de recursos más altos<br />-No disponible en escenario desconectado|  
|**adLockReadOnly**|-Requisitos de recursos insuficientes<br />-Muy escalable|-Los datos no se pueden actualizar a través del cursor|  
|**adLockBatchOptimistic**|-Actualizaciones por lotes<br />-Permite escenarios desconectados<br />-Otros usuarios que pueden tener acceso a los datos|-Los datos pueden ser modificados por varios usuarios a la vez.|  
|**adLockPessimistic**|-Otros usuarios no pueden cambiar los datos mientras están bloqueados|-Impide que otros usuarios tengan acceso a los datos mientras están bloqueados|  
|**adLockOptimistic**|-Otros usuarios que pueden tener acceso a los datos|-Los datos pueden ser modificados por varios usuarios a la vez.|

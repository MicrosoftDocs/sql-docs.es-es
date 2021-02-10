---
description: MemberTypeEnum
title: MemberTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- MemberTypeEnum
helpviewer_keywords:
- MemberTypeEnum enumeration [ADO MD]
ms.assetid: 5d8132c0-7ca2-4f86-8336-1b34213869ad
author: rothja
ms.author: jroth
ms.openlocfilehash: b83747edabc12f2f84d728a456255513ee1f0f66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051086"
---
# <a name="membertypeenum"></a>MemberTypeEnum
Especifica el valor para la propiedad [Type](./type-property-ado-md.md) de un objeto [miembro](./member-object-ado-md.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adMemberAll**|4|Indica que el objeto **miembro** representa todos los miembros del nivel.|  
|**adMemberFormula**|3|Indica que el objeto **miembro** se calcula mediante una expresión de fórmula.|  
|**adMemberMeasure**|2|Indica que el objeto **miembro** pertenece a la dimensión Measures y representa un atributo cuantitativo.|  
|**adMemberRegular**|1|Predeterminada. Indica que el objeto **miembro** representa una instancia de una entidad comercial.|  
|**adMemberUnknown**|0|No se puede determinar el tipo del miembro.|
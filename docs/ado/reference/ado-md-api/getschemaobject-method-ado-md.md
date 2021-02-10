---
description: Método GetSchemaObject (ADO MD)
title: Método GetSchemaObject (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- GetSchemaObject
- Cellset::GetSchemaObject
helpviewer_keywords:
- GetSchemaObject method [ADO MD]
ms.assetid: 36b754b4-6b17-4dd1-a925-bca46938b7c4
author: rothja
ms.author: jroth
ms.openlocfilehash: 96b0a2b6b8fc0a8d87c51c68701a64c971255ec8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054630"
---
# <a name="getschemaobject-method-ado-md"></a>Método GetSchemaObject (ADO MD)
Recupera un objeto de esquema ADO MD ([dimensión](./dimension-object-ado-md.md), [jerarquía](./hierarchy-object-ado-md.md), [nivel](./level-object-ado-md.md)o [miembro](./member-object-ado-md.md)) por su [UniqueName](./uniquename-property-ado-md.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Set object = CubeDef.GetSchemaObject (ObjType, UniqueName)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *ObjType*  
 Un valor [SchemaObjectTypeEnum](./schemaobjecttypeenum.md) que especifica el tipo de objeto de esquema (dimensión, jerarquía, nivel o miembro) que se va a recuperar.  
  
 *UniqueName*  
 **Cadena** que especifica el valor de la propiedad **UniqueName** del objeto que se va a recuperar.  
  
## <a name="remarks"></a>Observaciones  
 **GetSchemaObject** recupera objetos usando sus nombres únicos, tal como se especifica en la propiedad **UniqueName** . No es necesario conocer los nombres de los objetos primarios y no es necesario rellenar las colecciones primarias para recuperar un objeto de esquema.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto CubeDef (ADO MD)](./cubedef-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto CubeDef (ADO MD)](./cubedef-object-ado-md.md)
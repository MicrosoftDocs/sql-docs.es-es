---
description: SaveOptionsEnum
title: SaveOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SaveOptionsEnum
helpviewer_keywords:
- SaveOptionsEnum enumeration [ADO]
ms.assetid: 59339100-6e29-48d1-aea3-6873796d186b
author: rothja
ms.author: jroth
ms.openlocfilehash: db94b409a799a24d0c74dfec5a3d388c55df20e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040675"
---
# <a name="saveoptionsenum"></a>SaveOptionsEnum
Especifica si se debe crear o sobrescribir un archivo al guardar desde un objeto de [flujo](./stream-object-ado.md) . Los valores pueden ser **adSaveCreateNotExist** o **adSaveCreateOverWrite**.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adSaveCreateNotExist**|1|Predeterminada. Crea un nuevo archivo si el archivo especificado por el parámetro *filename* todavía no existe.|  
|**adSaveCreateOverWrite**|2|Sobrescribe el archivo con los datos del objeto de **secuencia** abierto actualmente, si el archivo especificado por el parámetro *filename* ya existe. Si el archivo especificado por el parámetro *filename* no existe, se crea un archivo nuevo.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Estas constantes no tienen equivalentes de ADO/WFC.  
  
## <a name="applies-to"></a>Se aplica a  
 [Método SaveToFile](./savetofile-method.md)
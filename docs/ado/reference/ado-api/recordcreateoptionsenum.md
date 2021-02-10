---
description: RecordCreateOptionsEnum
title: RecordCreateOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordCreateOptionsEnum
helpviewer_keywords:
- RecordCreateOptionsEnum enumeration [ADO]
ms.assetid: 6d746670-0850-4065-9cd4-168dea1d3ea9
author: rothja
ms.author: jroth
ms.openlocfilehash: 942f11dfa25a92f6914c868ffcdeb65c434e490c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051856"
---
# <a name="recordcreateoptionsenum"></a>RecordCreateOptionsEnum
Especifica si se debe abrir un **registro** existente o crear un nuevo **registro** para el método [Open](./open-method-ado-record.md) del objeto [Record](./record-object-ado.md) . Los valores se pueden combinar con un operador AND.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adCreateCollection**|0x2000|Crea un nuevo **registro** en el nodo especificado por el parámetro *source* , en lugar de abrir un **registro** existente. Si el origen apunta a un nodo existente, se produce un error en tiempo de ejecución, a menos que **adCreateCollection** se combine con **adOpenIfExists** o **adCreateOverwrite**.|  
|**adCreateNonCollection**|0|Crea un nuevo **registro** de tipo [adSimpleRecord](./recordtypeenum.md).|  
|**adCreateOverwrite**|0x4000000|Modifica las marcas de creación **adCreateCollection**, **adCreateNonCollection** y **adCreateStructDoc**. Cuando se usa o con este valor y uno de los valores de la marca de creación, si la dirección URL de origen apunta a un nodo o **registro** existente, se sobrescribe el **registro** existente y se crea uno nuevo en su lugar. Este valor no se puede usar junto con **adOpenIfExists**.|  
|**adCreateStructDoc**|0x80000000|Crea un nuevo **registro** de tipo [adStructDoc](./recordtypeenum.md), en lugar de abrir un **registro** existente.|  
|**adFailIfNotExists**|-1|Predeterminada. Da como resultado un error en tiempo de ejecución si el *origen* apunta a un nodo no existente.|  
|**adOpenIfExists**|0x2000000|Modifica las marcas de creación **adCreateCollection**, **adCreateNonCollection** y **adCreateStructDoc**. Cuando se usa o con este valor y uno de los valores de la marca de creación, si la dirección URL de origen apunta a un objeto de **registro** o nodo existente, el proveedor debe abrir el **registro** existente en lugar de crear uno nuevo. Este valor no se puede usar junto con **adCreateOverwrite**.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Estas constantes no tienen equivalentes de ADO/WFC.  
  
## <a name="applies-to"></a>Se aplica a  
 [Open (método) (registro de ADO)](./open-method-ado-record.md)
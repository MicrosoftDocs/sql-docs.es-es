---
description: Propiedad de conjunto de caracteres (ADO)
title: Propiedad charset (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Charset
helpviewer_keywords:
- Charset property [ADO]
ms.assetid: e42507cb-9b46-4ce4-8191-2948eaf14ca2
author: rothja
ms.author: jroth
ms.openlocfilehash: e796c9a079bda856aaf6c1e25e19e1fe6d4d21b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027123"
---
# <a name="charset-property-ado"></a>Propiedad de conjunto de caracteres (ADO)
Indica el juego de caracteres en el que se debe traducir el contenido de una [secuencia](./stream-object-ado.md) de texto para el almacenamiento en el búfer interno del objeto de **secuencia** .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** que especifica el juego de caracteres al que se traducirá el contenido de la **secuencia** . El valor predeterminado es **Unicode**. Los valores permitidos son cadenas típicas que se pasan a través de la interfaz como nombres de juegos de caracteres de Internet (por ejemplo, "ISO-8859-1", "Windows-1252", etc.). Para obtener una lista de los nombres de los juegos de caracteres que conoce un sistema, vea las subclaves de HKEY_CLASSES_ROOT\MIME\Database\Charset en el registro de Windows.  
  
## <a name="remarks"></a>Observaciones  
 En un objeto de **flujo** de texto, los datos de texto se almacenan en el juego de caracteres especificado por la propiedad **CharSet** . El valor predeterminado es Unicode. La propiedad **CharSet** se usa para convertir los datos que entran en la **secuencia** o salen de la **secuencia**. Por ejemplo, si la **secuencia** contiene datos ISO-8859-1 y esos datos se copian en un BSTR, el objeto de **secuencia** convertirá los datos a Unicode. Lo contrario también es cierto.  
  
 En el caso de una **secuencia** abierta, la [posición](./position-property-ado.md) actual debe estar al principio de la **secuencia** (0) para poder establecer **CharSet**.  
  
 **CharSet** solo se utiliza con objetos de **flujo** de texto (el [tipo](./type-property-ado-stream.md) es **adTypeText**). Esta propiedad se omite si el **tipo** es **adTypeBinary**.  
  
 Para obtener un ejemplo de código, vea [Step 4: rellenar el cuadro de texto details](../../guide/data/step-4-populate-the-details-text-box.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)
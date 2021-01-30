---
description: Método ReadText
title: Método READTEXT | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_ReadText
- _Stream::ReadText
helpviewer_keywords:
- ReadText method [ADO]
ms.assetid: be5a409e-cf87-4859-9ea5-713401755a77
author: rothja
ms.author: jroth
ms.openlocfilehash: e0d88fba7389c40442fccf006afc03020004d928
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166754"
---
# <a name="readtext-method"></a>Método ReadText
Lee el número especificado de caracteres de un objeto de [flujo](./stream-object-ado.md) de texto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
String = Stream.ReadText ( NumChars)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *NumChars*  
 Opcional. Valor de **tipo Long** que especifica el número de caracteres que se van a leer del archivo, o un valor [StreamReadEnum](./streamreadenum.md) . El valor predeterminado es **adReadAll**.  
  
## <a name="return-value"></a>Valor devuelto  
 El método **READTEXT** Lee un número especificado de caracteres, una línea completa o toda la secuencia de un objeto de **secuencia** y devuelve la cadena resultante.  
  
## <a name="remarks"></a>Observaciones  
 Si *NumChar* es mayor que el número de caracteres que quedan en la secuencia, solo se devuelven los caracteres restantes. La lectura de la cadena no se rellena para que coincida con la longitud especificada por *NumChar*. Si no quedan caracteres para leer, se devuelve una variante cuyo valor es NULL. **READTEXT** no se puede usar para leer hacia atrás.  
  
> [!NOTE]
>  El método **READTEXT** se usa con secuencias de texto (el [tipo](./type-property-ado-stream.md) es **adTypeText**). En el caso de secuencias binarias (el **tipo** es **adTypeBinary**), utilice [Read](./read-method.md).  
  
 Las consultas que generan una gran cantidad de datos XML que se devuelven a través del método **READTEXT** del objeto de secuencia de objetos de datos ActiveX (ADO) pueden tardar mucho tiempo en ejecutarse; Si esto se hace en un componente de COM+ que se invoca desde una página ASP, la sesión del usuario puede agotar el tiempo de espera. ADO convierte los datos de objetos de secuencia de la codificación UTF-8 a Unicode; la reasignación de memoria frecuente implicada en la conversión de una gran cantidad de datos a la vez es bastante lenta. Para resolverlo, realice llamadas repetidas al método **READTEXT** del objeto de comando de ADO y especifique un número menor de caracteres. Las pruebas han demostrado que un valor equivalente a 128K (131.072) es óptimo. El tiempo de respuesta disminuye a medida que se reduce este valor. Para obtener más información, vea el artículo 280067 de Knowledge base, "PRB: recuperar documentos XML de gran tamaño de SQL Server 2000 mediante el método READTEXT de ADO Stream Object puede ser lento", en Microsoft Knowledge base en https://support.microsoft.com .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método Read](./read-method.md)
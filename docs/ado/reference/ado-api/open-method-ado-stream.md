---
description: Open (método) (Stream de ADO)
title: Open (método) (secuencia de ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_Open
- _Stream::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: d26f48fb-904e-4932-a245-3b4332ca1600
author: rothja
ms.author: jroth
ms.openlocfilehash: 9151af754b245acc04dae70c120f01e37ac587cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166976"
---
# <a name="open-method-ado-stream"></a>Open (método) (Stream de ADO)
Abre un objeto de [flujo](./stream-object-ado.md) para manipular flujos de datos binarios o de texto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Stream.Open Source, Mode , OpenOptions, UserName, Password  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Origen*  
 Opcional. Valor **Variant** que especifica el origen de datos para la **secuencia**. *Source* puede contener una cadena de dirección URL absoluta que apunta a un nodo existente en una estructura de árbol conocida, como un sistema de archivos o de correo electrónico. Se debe especificar una dirección URL mediante la palabra clave URL ("URL =*Scheme*:// / *carpeta* del servidor"). Como alternativa, *source* puede contener una referencia a un objeto [Record](./record-object-ado.md) ya abierto, que abre la secuencia predeterminada asociada al **registro**. Si no se especifica *source* , se crea una instancia de un **flujo** y se abre, y se asocia sin ningún origen subyacente de forma predeterminada. Para obtener más información sobre los esquemas de dirección URL y sus proveedores asociados, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
 *Modo*  
 Opcional. Un valor [ConnectModeEnum](./connectmodeenum.md) que especifica el modo de acceso para la **secuencia** resultante (por ejemplo, de lectura/escritura o de solo lectura). El valor predeterminado es **adModeUnknown**. Vea la propiedad [mode](./mode-property-ado.md) para obtener más información sobre los modos de acceso. Si no se especifica *mode* , lo hereda el objeto de origen. Por ejemplo, si el **registro** de origen se abre en modo de solo lectura, el **flujo** también se abrirá de forma predeterminada en modo de solo lectura.  
  
 *OpenOptions*  
 Opcional. Valor de [StreamOpenOptionsEnum](./streamopenoptionsenum.md) . El valor predeterminado es **adOpenStreamUnspecified**.  
  
 *UserName*  
 Opcional. Valor de **cadena** que contiene la identificación del usuario que, si es necesario, tiene acceso al objeto de **secuencia** .  
  
 *Contraseña*  
 Opcional. Valor de **cadena** que contiene la contraseña que, si se necesita, tiene acceso al objeto de **secuencia** .  
  
## <a name="remarks"></a>Observaciones  
 Cuando se pasa un objeto de **registro** como parámetro de origen, no se usan los parámetros *userid* y *password* porque el acceso al objeto **Record** ya está disponible. De forma similar, el [modo](./mode-property-ado.md) del objeto de **registro** se transfiere al objeto de **secuencia** . Cuando no se especifica *source* , el **flujo** abierto no contiene datos y [su tamaño](./size-property-ado-stream.md) es cero (0). Para evitar la pérdida de datos que se escriben en esta **secuencia** cuando la **secuencia** está cerrada, guarde la **secuencia** con los métodos [CopyTo](./copyto-method-ado.md) o [SaveToFile](./savetofile-method.md) , o guárdela en otra ubicación de memoria.  
  
 Un valor *OpenOptions* de **adOpenStreamFromRecord** identifica el contenido del parámetro de *origen* para que sea un objeto **Record** ya abierto. El comportamiento predeterminado es tratar el *origen* como una dirección URL que apunta directamente a un nodo en una estructura de árbol, como un archivo. Se abre el flujo predeterminado asociado a ese nodo.  
  
 Mientras la **secuencia** no está abierta, es posible leer todas las propiedades de solo lectura de la **secuencia**. Si un **flujo** se abre de forma asincrónica, todas las operaciones subsiguientes (distintas de la comprobación del [Estado](./state-property-ado.md) y otras propiedades de solo lectura) se bloquean hasta que se completa la operación de **apertura** .  
  
 Además de las opciones descritas anteriormente, si no se especifica *source*, puede crear una instancia de un objeto **Stream** en la memoria sin asociarla a un origen subyacente. Puede agregar datos a la secuencia de forma dinámica escribiendo datos binarios o de texto en la **secuencia** con [Write](./write-method.md) o [WRITETEXT](./writetext-method.md), o cargando datos de un archivo con [LoadFromFile](./loadfromfile-method-ado.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Open (método) (conexión de ADO)](./open-method-ado-connection.md)   
 [Open (método) (record de ADO)](./open-method-ado-record.md)   
 [Open (método) (conjunto de registros ADO)](./open-method-ado-recordset.md)   
 [OpenSchema (método)](./openschema-method.md)   
 [Método SaveToFile](./savetofile-method.md)
---
description: Método MoveRecord (ADO)
title: MoveRecord (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::MoveRecord
- _Record::raw_MoveRecord
helpviewer_keywords:
- MoveRecord method [ADO]
ms.assetid: 6d2807b0-b861-4583-bcaf-fb0b82e0f2d0
author: rothja
ms.author: jroth
ms.openlocfilehash: d55a2a2aacc281271789d2c456ea5bdc8de44499
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167082"
---
# <a name="moverecord-method-ado"></a>Método MoveRecord (ADO)
Mueve la entidad representada por un [registro](./record-object-ado.md) a otra ubicación.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Record.MoveRecord (Source, Destination, UserName, Password, Options, Async)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Origen*  
 Opcional. Valor de **cadena** que contiene una dirección URL que identifica el **registro** que se va a desplace. Si se omite *source* o especifica una cadena vacía, se mueve el objeto representado por este **registro** . Por ejemplo, si el **registro** representa un archivo, el contenido del archivo se mueve a la ubicación especificada por *Destination*.  
  
 *Destino*  
 Opcional. Valor de **cadena** que contiene una dirección URL que especifica la ubicación donde se va a desplace el *origen* .  
  
 *UserName*  
 Opcional. Valor de **cadena** que contiene el identificador de usuario que, si es necesario, autoriza el acceso al *destino*.  
  
 *Contraseña*  
 Opcional. Una **cadena** que contiene la contraseña que, si es necesario, comprueba el *nombre de usuario*.  
  
 *Opciones*  
 Opcional. Un valor de [MoveRecordOptionsEnum](./moverecordoptionsenum.md) cuyo valor predeterminado es **adMoveUnspecified**. Especifica el comportamiento de este método.  
  
 *Asincrónico*  
 Opcional. Valor **booleano** que, cuando **es true**, especifica que esta operación debe ser asincrónica.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **String**. Normalmente, se devuelve el valor de *Destination* . Sin embargo, el valor exacto devuelto depende del proveedor.  
  
## <a name="remarks"></a>Observaciones  
 Los valores de *origen* y *destino* no deben ser idénticos; de lo contrario, se produce un error en tiempo de ejecución. Al menos los nombres de servidor, ruta de acceso y recurso deben ser diferentes.  
  
 En el caso de los archivos que se movieron mediante el proveedor de publicación en Internet, este método actualiza todos los vínculos de hipertexto de los archivos que se mueven, a *menos que se* especifique lo contrario Este método produce un error si el *destino* identifica un objeto existente (por ejemplo, un archivo o un directorio), a menos que se especifique **adMoveOverWrite** .  
  
> [!NOTE]
>  Use la opción **adMoveOverWrite** con prudencia. Por ejemplo, si se especifica esta opción cuando se mueve un archivo a un directorio, se eliminará el directorio y se reemplazará por el archivo.  
  
 Algunos atributos del objeto de **registro** , como la propiedad [ParentURL](./parenturl-property-ado.md) , no se actualizarán una vez completada esta operación. Actualice las propiedades del objeto **Record** cerrando el **registro** y, a continuación, vuelva a abrirlo con la dirección URL de la ubicación donde se ha despasado el archivo o directorio.  
  
 Si este **registro** se obtuvo de un [conjunto de registros](./recordset-object-ado.md), la nueva ubicación del archivo o directorio que se ha descargado no se reflejará inmediatamente en el **conjunto de registros**. Actualice el **conjunto de registros** . para ello, cierre y vuelva a abrirlo.  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Record (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Move (método) (ADO)](./move-method-ado.md)   
 [Métodos MoveFirst, MoveLast, MoveNext y MovePrevious (ADO)](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveFirst, MoveLast, MoveNext y MovePrevious métodos (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)
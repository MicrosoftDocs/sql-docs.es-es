---
description: Método CopyRecord (ADO)
title: CopyRecord (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_CopyRecord
- _Record::CopyRecord
helpviewer_keywords:
- CopyRecord method [ADO]
ms.assetid: b9bcf272-3c74-479f-95dd-0229a32e98fc
author: rothja
ms.author: jroth
ms.openlocfilehash: d2f6cd1b8bb40b4c62ad2c75fef63fd641201e9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171406"
---
# <a name="copyrecord-method-ado"></a>Método CopyRecord (ADO)
Copia una entidad representada por un [registro](./record-object-ado.md) en otra ubicación.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Record.CopyRecord (Source, Destination, UserName, Password, Options, Async)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Origen*  
 Opcional. Valor de **cadena** que contiene una dirección URL que especifica la entidad que se va a copiar (por ejemplo, un archivo o un directorio). Si se omite *source* o especifica una cadena vacía, se copiará el archivo o directorio representado por el [registro](./record-object-ado.md) actual.  
  
 *Destino*  
 Opcional. Valor de **cadena** que contiene una dirección URL que especifica la ubicación donde se copiará el *origen* .  
  
 *UserName*  
 Opcional. Valor de **cadena** que contiene el identificador de usuario que, si es necesario, autoriza el acceso al *destino*.  
  
 *Contraseña*  
 Opcional. Valor de **cadena** que contiene la contraseña que, si es necesario, comprueba el *nombre de usuario*.  
  
 *Opciones*  
 Opcional. Un valor de [CopyRecordOptionsEnum](./copyrecordoptionsenum.md) que tiene un valor predeterminado de **adCopyUnspecified**. Especifica el comportamiento de este método.  
  
 *Asincrónico*  
 Opcional. Valor **booleano** que, cuando **es true**, especifica que esta operación debe ser asincrónica.  
  
## <a name="return-value"></a>Valor devuelto  
 Valor de **cadena** que normalmente devuelve el valor de *Destination*. Sin embargo, el valor exacto devuelto depende del proveedor.  
  
## <a name="remarks"></a>Observaciones  
 Los valores de *origen* y *destino* no deben ser idénticos; de lo contrario, se produce un error en tiempo de ejecución. Al menos uno de los nombres de servidor, ruta de acceso o recurso debe ser diferente.  
  
 Todos los elementos secundarios (por ejemplo, subdirectorios) de *source* se copian de forma recursiva, a menos que se especifique **adCopyNonRecursive** . En una operación recursiva, *Destination* no debe ser un subdirectorio de *source*; de lo contrario, la operación no se completará.  
  
 Este método produce un error si el *destino* identifica una entidad existente (por ejemplo, un archivo o un directorio), a menos que se especifique **adCopyOverWrite** .  
  
> [!IMPORTANT]
>  Use la opción **adCopyOverWrite** con prudencia. Por ejemplo, si se especifica esta opción al copiar un archivo en un directorio, se *eliminará* el directorio y se reemplazará por el archivo.  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Record (ADO)](./record-object-ado.md)
---
description: Método GetRows (ADO)
title: GetRows (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::GetRows
- Recordset15::raw_GetRows
helpviewer_keywords:
- Getrows method [ADO]
ms.assetid: 14b92860-4171-47d9-a413-dd60dd6a8880
author: rothja
ms.author: jroth
ms.openlocfilehash: df59c3d464d1c19a3b98e8611e2db2983db2a833
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170995"
---
# <a name="getrows-method-ado"></a>Método GetRows (ADO)
Recupera varios registros de un objeto de [conjunto de registros](./recordset-object-ado.md) en una matriz.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
array = recordset.GetRows(Rows, Start, Fields )  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve una **variante** cuyo valor es una matriz bidimensional.  
  
#### <a name="parameters"></a>Parámetros  
 *Filas*  
 Opcional. Valor de [GetRowsOptionEnum](./getrowsoptionenum.md) que indica el número de registros que se van a recuperar. El valor predeterminado es **adGetRowsRest**.  
  
 *Iniciar*  
 Opcional. Un valor de **cadena** o una **variante** que se evalúa como el marcador del registro del que debe comenzar la operación de **GetRows** . También puede usar un valor de [BookmarkEnum](./bookmarkenum.md) .  
  
 *Fields*  
 Opcional. **Variante** que representa un nombre de campo único o una posición ordinal, o una matriz de nombres de campo o números de posición ordinal. ADO solo devuelve los datos de estos campos.  
  
## <a name="remarks"></a>Observaciones  
 Utilice el método **GetRows** para copiar registros de un **conjunto de registros** en una matriz bidimensional. El primer subíndice identifica el campo y el segundo identifica el número de registro. La variable de *matriz* se dimensiona automáticamente con el tamaño correcto cuando el método **GetRows** devuelve los datos.  
  
 Si no especifica un valor para el argumento *Rows* , el método **GetRows** recupera automáticamente todos los registros del objeto de conjunto de **registros** . Si solicita más registros de los que están disponibles, **GetRows** solo devuelve el número de registros disponibles.  
  
 Si el objeto de **conjunto de registros** admite marcadores, puede especificar en qué registro el método **GetRows** debe empezar a recuperar datos pasando el valor de la propiedad [Bookmark](./bookmark-property-ado.md) del registro en el argumento *Start* .  
  
 Si desea restringir los campos que devuelve la llamada **GetRows** , puede pasar un nombre o número de campo único o una matriz de nombres de campo/números en el argumento *campos* .  
  
 Después de llamar a **GetRows**, el registro unread siguiente se convierte en el registro actual, o la propiedad [EOF](./bof-eof-properties-ado.md) se establece en **true** si no hay más registros.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método GetRows (VB)](./getrows-method-example-vb.md)   
 [Ejemplo del método GetRows (VC ++)](./getrows-method-example-vc.md)
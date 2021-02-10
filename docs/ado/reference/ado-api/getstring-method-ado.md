---
description: GetString (método) (ADO)
title: GetString (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::raw_GetString
- Recordset20::GetString
helpviewer_keywords:
- GetString method [ADO]
ms.assetid: 92452940-b2a7-456e-94fc-3780c71da33c
author: rothja
ms.author: jroth
ms.openlocfilehash: b1cfaa8b75f7e8d1dfb5fc0cecf7e5d99634960a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020838"
---
# <a name="getstring-method-ado"></a>GetString (método) (ADO)
Devuelve el [conjunto de registros](./recordset-object-ado.md) como una cadena.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Variant = recordset.GetString(StringFormat, NumRows, ColumnDelimiter, RowDelimiter, NullExpr)  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve el **conjunto de registros** como una **variante** con valores de cadena (BSTR).  
  
#### <a name="parameters"></a>Parámetros  
 *StringFormat*  
 Valor [StringFormatEnum](./stringformatenum.md) que especifica cómo se debe convertir el **conjunto de registros** en una cadena. Los parámetros *RowDelimiter*, *ColumnDelimiter* y *NullExpr* solo se usan con un *StringFormat* de **adClipString**.  
  
 *NumRows*  
 Opcional. Número de filas que se van a convertir en el **conjunto de registros**. Si *numRows* no se especifica, o si es mayor que el número total de filas del conjunto de **registros**, se convertirán todas las filas del **conjunto de registros** .  
  
 *ColumnDelimiter*  
 Opcional. Un delimitador utilizado entre las columnas, si se especifica; de lo contrario, el carácter de TABULAción.  
  
 *RowDelimiter*  
 Opcional. Un delimitador utilizado entre las filas, si se especifica; de lo contrario, el carácter de retorno de carro.  
  
 *NullExpr*  
 Opcional. Expresión usada en lugar de un valor null, si se especifica; de lo contrario, es la cadena vacía.  
  
## <a name="remarks"></a>Observaciones  
 Los datos de fila, pero no los datos de esquema, se guardan en la cadena. Por lo tanto, no se puede volver a abrir un **conjunto de registros** mediante esta cadena.  
  
 Este método es equivalente al método **GetClipString** de RDO.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método GetString (VB)](./getstring-method-example-vb.md)
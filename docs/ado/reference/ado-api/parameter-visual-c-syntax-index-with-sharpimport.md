---
description: 'Parámetro (Visual C++ Índice de sintaxis con #import)'
title: 'Parameter (Visual C++ Índice de sintaxis con #import) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- 'Parameter collection [ADO], Visual C++ syntax index with #import'
ms.assetid: 6b43cf70-9695-47b0-9e68-f36898859b6b
author: rothja
ms.author: jroth
ms.openlocfilehash: ced00c5855fd961d7db67aacf16ddd86a46acac9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166896"
---
# <a name="parameter-visual-c-syntax-index-with-import"></a>Parámetro (Visual C++ Índice de sintaxis con #import)
## <a name="methods"></a>Métodos  
  
```  
HRESULT AppendChunk( const _variant_t & Val );  
```  
  
## <a name="properties"></a>Propiedades  
  
```  
long GetAttributes( );  
void PutAttributes( long plParmAttribs );  
__declspec(property(get=GetAttributes,put=PutAttributes)) long  
    Attributes;  
  
enum ParameterDirectionEnum GetDirection( );  
void PutDirection( enum ParameterDirectionEnum plParmDirection );  
__declspec(property(get=GetDirection,put=PutDirection)) enum  
    ParameterDirectionEnum Direction;  
  
_bstr_t GetName( );  
void PutName( _bstr_t pbstr );  
__declspec(property(get=GetName,put=PutName)) _bstr_t Name;  
  
unsigned char GetNumericScale( );  
void PutNumericScale( unsigned char pbScale );  
__declspec(property(get=GetNumericScale,put=PutNumericScale)) unsigned  
    char NumericScale;  
  
unsigned char GetPrecision( );  
void PutPrecision( unsigned char pbPrecision );  
__declspec(property(get=GetPrecision,put=PutPrecision)) unsigned char  
    Precision;  
  
long GetSize( );  
void PutSize( long pl );  
__declspec(property(get=GetSize,put=PutSize)) long Size;  
  
enum DataTypeEnum GetType( );  
void PutType( enum DataTypeEnum psDataType );  
__declspec(property(get=GetType,put=PutType)) enum DataTypeEnum Type;  
  
_variant_t GetValue( );  
void PutValue( const _variant_t & pvar );  
__declspec(property(get=GetValue,put=PutValue)) _variant_t Value;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Parameter](./parameter-object.md)
---
description: 'Colecciones (Visual C++ Índice de sintaxis con #import)'
title: 'Colecciones (Visual C++ Índice de sintaxis con #import) | Microsoft Docs'
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
- 'syntax indexes [ADO], ADO for Visual C++ syntax with #import'
- 'collections [ADO], ADO for Visual C++ syntax with #import'
- 'ADO for Visual C++ syntax with #import [ADO]'
- '#import [ADO]'
ms.assetid: 36fbca8e-1884-44b5-806b-d15e30f42fe6
author: rothja
ms.author: jroth
ms.openlocfilehash: 1e29b092ea7c11dc9729986c342c96b2022fbf58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164680"
---
# <a name="collections-visual-c-syntax-index-with-import"></a>Colecciones (Visual C++ Índice de sintaxis con #import)
Resulta útil saber que las colecciones heredan determinados métodos y propiedades comunes.  
  
 Todas las colecciones heredan la propiedad **Count** y el método **Refresh** y todas las colecciones agregan la propiedad **Item** . La colección de **errores** agrega el método **Clear** . La colección **Parameters** hereda los métodos **Append** y **Delete** , mientras que la colección **Fields** agrega los métodos **Append**, **Delete** y **Update** .  
  
## <a name="properties-collection"></a>Colección de propiedades  
  
### <a name="methods"></a>Métodos  
  
```  
HRESULT Refresh( );  
```  
  
### <a name="properties"></a>Propiedades  
  
```  
long GetCount( ); __declspec(property(get=GetCount)) long Count;  
PropertyPtr GetItem( const _variant_t & Index ); __declspec(property(get=GetItem)) PropertyPtr Item[];  
```  
  
## <a name="errors-collection"></a>Colección de errores  
  
### <a name="methods"></a>Métodos  
  
```  
HRESULT Clear( );  
HRESULT Refresh( );  
```  
  
### <a name="properties"></a>Propiedades  
  
```  
long GetCount( ); __declspec(property(get=GetCount)) long Count;  
PropertyPtr GetItem( const _variant_t & Index ); __declspec(property(get=GetItem)) PropertyPtr Item[];  
```  
  
## <a name="parameters-collection"></a>Colección Parameters  
  
### <a name="methods"></a>Métodos  
  
```  
HRESULT Append( IDispatch * Object );  
HRESULT Delete( const _variant_t & Index );  
HRESULT Refresh( );  
```  
  
### <a name="properties"></a>Propiedades  
  
```  
long GetCount( ); __declspec(property(get=GetCount)) long Count;  
PropertyPtr GetItem( const _variant_t & Index ); __declspec(property(get=GetItem)) PropertyPtr Item[];  
```  
  
## <a name="fields-collection"></a>Colección Fields  
  
### <a name="methods"></a>Métodos  
  
```  
HRESULT Append( _bstr_t Name, enum DataTypeEnum Type, long DefinedSize, enum FieldAttributeEnum Attrib, const _variant_t & FieldValue = vtMissing );  
HRESULT Delete( const _variant_t & Index );  
HRESULT Refresh( );  
HRESULT Update( );  
```  
  
### <a name="properties"></a>Propiedades  
  
```  
long GetCount( ); __declspec(property(get=GetCount)) long Count;  
PropertyPtr GetItem( const _variant_t & Index ); __declspec(property(get=GetItem)) PropertyPtr Item[];  
```  
  
## <a name="see-also"></a>Consulte también  
 [Colección de errores (ADO)](./errors-collection-ado.md)   
 [Fields (colección) (ADO)](./fields-collection-ado.md)   
 [Parameters (colección) (ADO)](./parameters-collection-ado.md)   
 [Colección de propiedades (ADO)](./properties-collection-ado.md)
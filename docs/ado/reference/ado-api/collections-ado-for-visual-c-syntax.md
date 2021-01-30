---
description: Colecciones (ADO para la sintaxis Visual C++)
title: Colecciones (ADO para la sintaxis de Visual C++) | Microsoft Docs
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
- ADO for Visual C++ syntax [ADO]
- syntax indexes [ADO], ADO for Visual C++ syntax
- collections [ADO], ADO for Visual C++ syntax
ms.assetid: 6a0109a0-f2d9-4f7c-8e1e-42763f9acaea
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cda61fc0d1d206a75059263c5450c813a9e5ddb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164689"
---
# <a name="collections-ado-for-visual-c-syntax"></a>Colecciones (ADO para la sintaxis Visual C++)
## <a name="parameters"></a>Parámetros  
  
### <a name="methods"></a>Métodos  
  
```  
Append(IDispatch *Object);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 Para obtener más información, vea  
  
-   [Append (método) (ADO)](./append-method-ado.md)  
  
-   [Método Delete (colección de parámetros de ADO)](./delete-method-ado-parameters-collection.md)  
  
-   [Actualizar (método, ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propiedades  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, _ADOParameter **ppvObject);  
```  
  
 Para obtener más información, vea  
  
-   [Count (propiedad, ADO)](./count-property-ado.md)  
  
-   [Propiedad Item (ADO)](./item-property-ado.md)  
  
## <a name="fields"></a>Campos  
  
### <a name="methods"></a>Métodos  
  
```  
Append(BSTR bstrName, DataTypeEnum Type, long DefinedSize, FieldAttributeEnum Attrib);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 Para obtener más información, vea  
  
-   [Append (método) (ADO)](./append-method-ado.md)  
  
-   [Método Delete (colección de parámetros de ADO)](./delete-method-ado-parameters-collection.md)  
  
-   [Actualizar (método, ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propiedades  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOField **ppvObject);  
```  
  
 Para obtener más información, vea  
  
-   [Count (propiedad, ADO)](./count-property-ado.md)  
  
-   [Propiedad Item (ADO)](./item-property-ado.md)  
  
## <a name="errors"></a>Errors  
  
### <a name="methods"></a>Métodos  
  
```  
Clear(void);  
Refresh(void);  
```  
  
 Para obtener más información, vea  
  
-   [Clear (método) (ADO)](./clear-method-ado.md)  
  
-   [Actualizar (método, ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propiedades  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOError **ppvObject);  
```  
  
 Para obtener más información, vea  
  
-   [Count (propiedad, ADO)](./count-property-ado.md)  
  
-   [Propiedad Item (ADO)](./item-property-ado.md)  
  
## <a name="properties"></a>Propiedades  
  
### <a name="methods"></a>Métodos  
  
```  
Refresh(void);  
```  
  
 Para obtener más información, vea  
  
-   [Actualizar (método, ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propiedades  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOProperty **ppvObject);  
```  
  
 Para obtener más información, vea  
  
-   [Count (propiedad, ADO)](./count-property-ado.md)  
  
-   [Propiedad Item (ADO)](./item-property-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Colección de errores (ADO)](./errors-collection-ado.md)   
 [Fields (colección) (ADO)](./fields-collection-ado.md)   
 [Parameters (colección) (ADO)](./parameters-collection-ado.md)   
 [Colección de propiedades (ADO)](./properties-collection-ado.md)
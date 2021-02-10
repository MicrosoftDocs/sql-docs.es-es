---
description: Espacios de nombres
title: Espacios de nombres | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- namespaces in ADO
ms.assetid: efff5569-db52-451d-a039-2e74870534da
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a7657e9758ebaa3507a35aef65d6a077e4dcf35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032677"
---
# <a name="namespaces"></a>Espacios de nombres
El formato de persistencia de XML en ADO utiliza los cuatro espacios de nombres siguientes.  
  
## <a name="remarks"></a>Observaciones  
 El formato de persistencia de XML en ADO utiliza los cuatro espacios de nombres siguientes.  
  
|Prefijo|Descripción|  
|------------|-----------------|  
|s|Hace referencia al espacio de nombres "XML-Data" que contiene los elementos y atributos que definen el esquema del conjunto de registros actual.|  
|dt|Hace referencia a la especificación de definiciones de tipo de datos.|  
|rs|Hace referencia al espacio de nombres que contiene elementos y atributos específicos de las propiedades y atributos del conjunto de registros de ADO.|  
|z|Hace referencia al esquema del conjunto de filas actual.|  
  
 Un cliente no debe agregar sus propias etiquetas a estos espacios de nombres, tal y como se define en la especificación. Por ejemplo, un cliente no debe definir un espacio de nombres como "urn: schemas-microsoft-com: Rowset" y, a continuación, escribir algo como "RS: MyOwnTag". Para obtener más información sobre los espacios de nombres, vea la [Recomendación sobre espacios de nombres del W3C en XML](http://www.w3.org/TR/REC-xml-names/).  
  
> [!IMPORTANT]
>  El identificador de la etiqueta de esquema debe ser "RowsetSchema" y el espacio de nombres usado para hacer referencia al esquema del conjunto de filas actual debe apuntar a "#RowsetSchema".  
  
 Tenga en cuenta que el prefijo del espacio de nombres (la parte entre los dos puntos y el signo igual) es arbitrario.  
  
```  
xmlns:rs="urn:schemas-microsoft-com:rowset"  
```  
  
 El usuario puede definir que sea cualquier nombre, siempre que este nombre se utilice de forma coherente en todo el documento XML. ADO siempre escribe "s", "RS", "DT" y "z", pero estos nombres de prefijo no están codificados de forma rígida en el componente de carga.  
  
## <a name="see-also"></a>Consulte también  
 [Almacenar registros en formato XML](./persisting-records-in-xml-format.md)
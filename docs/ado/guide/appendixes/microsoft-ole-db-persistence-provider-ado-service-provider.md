---
description: Proveedor de persistencia de Microsoft OLE DB (proveedor de servicios ADO)
title: Proveedor de persistencia de Microsoft OLE DB (proveedor de servicios ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB persistence provider
- persistence provider [ADO]
- OLE DB persistence provider [ADO]
ms.assetid: e75ef0dc-2016-4fcc-8918-23311c0d4e02
author: rothja
ms.author: jroth
ms.openlocfilehash: 4ed23adb7922a18f53245f21b087414403ae61d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029369"
---
# <a name="microsoft-ole-db-persistence-provider-overview"></a>Introducción al proveedor de persistencia de Microsoft OLE DB
El proveedor de persistencia de Microsoft OLE DB permite guardar un objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) en un archivo y, posteriormente, restaurar ese objeto de **conjunto de registros** desde el archivo. Se conserva la información de esquema, los datos y los cambios pendientes.

 Puede guardar el **conjunto de registros** en el formato de la tabla de datos avanzada (ADTG) de propiedad o en el formato Open lenguaje de marcado extensible (XML).

## <a name="provider-keyword"></a>Palabra clave Provider
 Para invocar este proveedor, especifique la palabra clave y el valor siguientes en la cadena de conexión.

```vb
"Provider=MSPersist"
```

## <a name="errors"></a>Errors
 Los siguientes errores emitidos por este proveedor se pueden detectar en la aplicación.

|Constante|Descripción|
|--------------|-----------------|
|E_BADSTREAM|El archivo abierto no tiene un formato válido (es decir, el formato no es ADTG ni XML).|
|E_CANTPERSISTROWSET|El objeto de **conjunto de registros** guardado tiene características que impiden que se almacene.|

## <a name="remarks"></a>Observaciones
 El proveedor de persistencia de Microsoft OLE DB no expone propiedades dinámicas.

 Actualmente, solo se pueden guardar los objetos de **conjunto de registros** jerárquicos con parámetros.

 Para obtener más información sobre el almacenamiento persistente de objetos de **conjunto de registros** , vea persistencia de [conjuntos de registros](../data/more-about-recordset-persistence.md).

 Cuando se usa una secuencia para abrir un **conjunto de registros,** no debería haber ningún parámetro especificado que no sea el parámetro de *origen* del método **abierto** .

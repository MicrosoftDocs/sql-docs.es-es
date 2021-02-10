---
description: Objetos ADOX
title: Objetos ADOX | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- objects [ADOX]
- ADOX, objects
ms.assetid: 3f5287e9-f62c-40c4-bb59-985102be956e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2234203a70bfbc4fd031b85e4f1f66327d58d62b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050606"
---
# <a name="adox-objects"></a>Objetos ADOX
## <a name="adox-object-summary"></a>Resumen de objetos ADOX  
  
|Object|Descripción|  
|------------|-----------------|  
|[Catálogo](./catalog-object-adox.md)|Contiene colecciones que describen el catálogo de esquema de un origen de datos.|  
|[Columna](./column-object-adox.md)|Representa una columna de una tabla, un índice o una clave.|  
|[Grupo](./group-object-adox.md)|Representa una cuenta de grupo que tiene permisos de acceso en una base de datos protegida.|  
|[Index](./index-object-adox.md)|Representa un índice de una tabla de base de datos.|  
|[Key](./key-object-adox.md)|Representa un campo de clave principal, externa o única de una tabla de base de datos.|  
|[Procedimiento](./procedure-object-adox.md)|Representa un procedimiento almacenado.|  
|[Table](./table-object-adox.md)|Representa una tabla de base de datos, que incluye columnas, índices y claves.|  
|[User](./user-object-adox.md)|Representa una cuenta de usuario que tiene permisos de acceso en una base de datos protegida.|  
|[Ver](./view-object-adox.md)|Representa un conjunto filtrado de registros o una tabla virtual.|  
  
 Las relaciones entre estos objetos se ilustran en el [modelo de objetos ADOX](./adox-object-model.md).  
  
 Cada objeto puede estar contenido en su colección correspondiente. Por ejemplo, un objeto **TABLE** puede estar contenido en una colección [tables](./tables-collection-adox.md) . Para obtener más información, vea [colecciones de ADOX](./adox-collections.md) o un tema específico de la colección.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ADOX](./adox-object-model.md)   
 [Colecciones de ADOX](./adox-collections.md)   
 [Modelo de objetos ADOX](./adox-object-model.md)   
 [Extensiones de ADO para lenguaje de definición de datos y seguridad (ADOX)](../../guide/extensions/ado-extensions-for-data-definition-language-and-security-adox.md)
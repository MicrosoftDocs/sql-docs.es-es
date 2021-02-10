---
description: Objeto Procedure (ADOX)
title: Procedure (objeto, ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Procedure
helpviewer_keywords:
- Procedure object [ADOX]
ms.assetid: 927bcf3e-32f5-4a80-98d3-600779f0732e
author: rothja
ms.author: jroth
ms.openlocfilehash: fec4b21cf4aa619fba72f5d62fff7f6e34506de5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053880"
---
# <a name="procedure-object-adox"></a>Objeto Procedure (ADOX)
Representa un procedimiento almacenado. Cuando se usa junto con el objeto de [comando](../ado-api/command-object-ado.md) de ADO, el objeto de **procedimiento** se puede utilizar para agregar, eliminar o modificar procedimientos almacenados.  
  
## <a name="remarks"></a>Observaciones  
 El objeto de **procedimiento** permite crear un procedimiento almacenado sin necesidad de conocer o utilizar la sintaxis de "Create procedure" del proveedor.  
  
 Con las propiedades de un objeto de **procedimiento** , puede:  
  
-   Identifique el procedimiento con la propiedad [Name](./name-property-adox.md) .  
  
-   Especifique el objeto **Command** de ADO que se puede utilizar para crear o ejecutar el procedimiento con la propiedad [Command](./command-property-adox.md) .  
  
-   Devuelve información de fecha con las propiedades [DateCreated](./datecreated-property-adox.md) y [DateModified](./datemodified-property-adox.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Procedure](./procedure-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedades Command y CommandText (VB)](./command-and-commandtext-properties-example-vb.md)   
 [Ejemplo de propiedad de comando, colección de parámetros (VB)](./parameters-collection-command-property-example-vb.md)   
 [Ejemplo de método Append de procedimientos (VB)](./procedures-append-method-example-vb.md)   
 [Ejemplo de método Delete de procedimientos (VB)](./procedures-delete-method-example-vb.md)   
 [Colección de procedimientos (ADOX)](./procedures-collection-adox.md)
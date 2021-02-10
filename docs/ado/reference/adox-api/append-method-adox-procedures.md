---
description: Append (método) (procedimientos ADOX)
title: Append (método) (procedimientos ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Procedures::Append
- Procedures::raw_Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 38e3492c-c1e1-42e3-a71a-befdc90204db
author: rothja
ms.author: jroth
ms.openlocfilehash: babc7e08a399643781ec2293f2dd9f214e3ad48c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050506"
---
# <a name="append-method-adox-procedures"></a>Append (método) (procedimientos ADOX)
Agrega un nuevo objeto de [procedimiento](./procedure-object-adox.md) a la colección [Procedures](./procedures-collection-adox.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Procedures.Append Name, Command  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Nombre*  
 Valor de **cadena** que especifica el nombre del procedimiento que se va a crear y anexar.  
  
 *Comando*  
 Objeto de [comando](../ado-api/command-object-ado.md) ADO que representa el procedimiento que se va a crear y anexar.  
  
## <a name="remarks"></a>Observaciones  
 Crea un nuevo procedimiento en el origen de datos con el nombre y los atributos especificados en el objeto de **comando** .  
  
 Si el texto del comando que especifica el usuario representa una vista en lugar de un procedimiento, el comportamiento depende del proveedor que se está usando. Se producirá un error en **Append** si el proveedor no admite comandos persistentes.  
  
> [!NOTE]
>  Al usar el proveedor de OLE DB para Microsoft Jet, el método **Append** de la colección **Procedures** le permitirá especificar una **vista** en lugar de un **procedimiento** en el parámetro de *comando* . La **vista** se agregará al origen de datos y se agregará a la colección **Procedures** . Después de **anexar**, si se actualizan las colecciones **Procedures** y **views** , la **vista** ya no estará en la colección **Procedures** y aparecerá en la colección **views** .  
  
## <a name="applies-to"></a>Se aplica a  
 [Colección de procedimientos (ADOX)](./procedures-collection-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de método Append de procedimientos (VB)](./procedures-append-method-example-vb.md)   
 [Append (método) (columnas ADOX)](./append-method-adox-columns.md)   
 [Append (método) (grupos ADOX)](./append-method-adox-groups.md)   
 [Append (método) (índices ADOX)](./append-method-adox-indexes.md)   
 [Append (método) (claves ADOX)](./append-method-adox-keys.md)   
 [Append (método) (tablas ADOX)](./append-method-adox-tables.md)   
 [Append (método) (usuarios ADOX)](./append-method-adox-users.md)   
 [Append (método) (vistas ADOX)](./append-method-adox-views.md)
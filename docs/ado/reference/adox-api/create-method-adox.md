---
description: Create (método, ADOX)
title: Create (método) (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Catalog::raw_Create
- _Catalog::Create
helpviewer_keywords:
- Create method [ADOX]
ms.assetid: 64f5c21c-b581-42d8-bdc7-c4f1bebaf105
author: rothja
ms.author: jroth
ms.openlocfilehash: b44be7497ca6952dd88d6f18ac0b42a6c989db36
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172209"
---
# <a name="create-method-adox"></a>Create (método, ADOX)
Crea un nuevo catálogo.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Catalog.Create ConnectString  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Property*  
 Valor de **cadena** que se usa para conectar con el origen de datos.  
  
## <a name="remarks"></a>Observaciones  
 El método **Create** crea y abre una nueva [conexión](../ado-api/connection-object-ado.md) ado con el origen de datos especificado en *connectstring*. Si se realiza correctamente, el nuevo objeto de **conexión** se asigna a la propiedad [ActiveConnection](./activeconnection-property-adox.md) .  
  
 Se producirá un error si el proveedor no admite la creación de nuevos catálogos.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método Create (VB)](./create-method-example-vb.md)   
 [ActiveConnection (propiedad, ADOX)](./activeconnection-property-adox.md)
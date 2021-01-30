---
description: ChangePassword (método, ADOX)
title: ChangePassword (método, ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _User25::raw_ChangePassword
- _User25::ChangePassword
helpviewer_keywords:
- ChangePassword method [ADOX]
ms.assetid: d187fbc6-5fac-4abb-803d-bf344dcf0302
author: rothja
ms.author: jroth
ms.openlocfilehash: 4347cb856b1fcb4d76ef6133024335404e32ad5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164286"
---
# <a name="changepassword-method-adox"></a>ChangePassword (método, ADOX)
Cambia la contraseña de una cuenta de [usuario](./user-object-adox.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
User.ChangePassword OldPassword, NewPassword  
```  
  
#### <a name="parameters"></a>Parámetros  
 *OldPassword*  
 Valor de **cadena** que especifica la contraseña existente del usuario. Si el usuario no tiene actualmente una contraseña, use una cadena vacía ("") para *OldPassword*.  
  
 *NewPassword*  
 Valor de **cadena** que especifica la nueva contraseña.  
  
## <a name="remarks"></a>Observaciones  
 Por motivos de seguridad, se debe especificar la contraseña anterior además de la nueva contraseña.  
  
 Se producirá un error si el proveedor no admite la administración de las propiedades de confianza.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto User (ADOX)](./user-object-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de métodos Append y ChangePassword de grupos y usuarios (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)
---
description: Hacer referencia a las bibliotecas de ADO en una aplicación de Visual C++
title: Hacer referencia a las bibliotecas de ADO en una aplicación Visual C++ | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- libraries [ADO]
- referencing libraries in a Visual C++ application[ADO]
- ADO, libraries
ms.assetid: d3ea12ec-bca8-48c3-af57-ce14576108c9
author: rothja
ms.author: jroth
ms.openlocfilehash: 596e11a51a093be30fab7d9d3b273d6605b48b75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032077"
---
# <a name="referencing-the-ado-libraries-in-a-visual-c-application"></a>Hacer referencia a las bibliotecas de ADO en una aplicación de Visual C++
Para usar la versión más reciente de ADO en una aplicación Visual C++, utilice la siguiente `#import` Directiva:  
  
```cpp
#import "msado15.dll" \  
    no_namespace rename("EOF", "EndOfFile")  
```  
  
 Para usar ADO MD o ADOX, debe importar *msadomd.dll* o *msadox.dll* mediante la sintaxis anterior.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 Para usar cualquier versión anterior de ADO, reemplace *msado15.dll* anterior por una de las siguientes bibliotecas de tipos.  
  
-   Biblioteca de tipos *msado27. tlb*, ADO 2,7  
  
-   Biblioteca de tipos *msado26. tlb*, ADO 2,6  
  
-   Biblioteca de tipos *msado25. tlb*, ADO 2,5  
  
-   Biblioteca de tipos *msado21. tlb*, ADO 2,1  
  
-   Biblioteca de tipos *msado20. tlb*, ADO 2,0

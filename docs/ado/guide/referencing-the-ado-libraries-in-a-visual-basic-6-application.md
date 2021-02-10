---
description: Hacer referencia a las bibliotecas de ADO en una aplicación de Visual Basic 6
title: Hacer referencia a las bibliotecas de ADO en una aplicación Visual Basic 6 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- libraries [ADO]
- referencing libraries in a Visual Basic application[ADO]
- ADO, libraries
ms.assetid: cfd37a82-aad2-41cd-8d13-1566c43d95f0
author: rothja
ms.author: jroth
ms.openlocfilehash: 431a58609ff26ace443b012bc2835e5ba89736db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036615"
---
# <a name="referencing-the-ado-libraries-in-a-visual-basic-6-application"></a>Hacer referencia a las bibliotecas de ADO en una aplicación de Visual Basic 6
Para importar las bibliotecas de ADO en una aplicación de Microsoft Visual Basic 6, debe establecer una referencia en el proyecto Visual Basic.  
  
### <a name="to-set-a-reference-to-the-ado-libraries-in-a-visual-basic-project"></a>Para establecer una referencia a las bibliotecas de ADO en un proyecto de Visual Basic  
  
1.  Cree una nueva o abra un proyecto de Visual Basic existente.  
  
2.  Haga clic en el elemento de menú **proyecto** y, a continuación, seleccione **referencias...** en el panel de menú desplegable.  
  
3.  En **referencias disponibles**, active la casilla de la **biblioteca de Microsoft objetos de datos ActiveX *n. n***, donde **_n. n_*_ representa el número de versión más reciente. El campo _* Location** siguiente debe identificar su elección como *$installDir\msado15.dll*, donde *$installDir* representa la ruta de acceso del directorio en el que se ha instalado la biblioteca de ADO.  
  
4.  Si piensa usar ADO MD, repita el paso 3 para seleccionar **biblioteca de Microsoft objetos de datos ActiveX (multidimensional) *n. n***. El campo **Ubicación** debe identificar esta opción como *$installDir\msadomd.dll*.  
  
5.  Si tiene previsto usar ADOX, repita el paso 3 para seleccionar **Microsoft ADO ext. *n. n* para DDL y Security**. El campo **Ubicación** debe identificar esta opción como *$installDir\msadox.dll*.  
  
6.  Haga clic en **Aceptar** para finalizar la configuración de las referencias.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 La instalación de ADO también copia las siguientes bibliotecas de tipos de versiones anteriores:  
  
-   Biblioteca de tipos *msado27. tlb*, ADO 2,7  
  
-   Biblioteca de tipos *msado26. tlb*, ADO 2,6  
  
-   Biblioteca de tipos *msado25. tlb*, ADO 2,5  
  
-   Biblioteca de tipos *msado21. tlb*, ADO 2,1  
  
-   Biblioteca de tipos *msado20. tlb*, ADO 2,0  
  
 Si su aplicación debe utilizar cualquiera de estas bibliotecas de ADO por motivos de compatibilidad con versiones anteriores, debe importar la versión adecuada de la biblioteca de tipos. Para ello, siga los procedimientos descritos en la sección anterior, reemplazando *msado15.dll* por *msadoXX. tlb*, donde *XX* representa el número de versión que debe importar.

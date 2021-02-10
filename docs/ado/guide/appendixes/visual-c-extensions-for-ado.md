---
description: Extensiones de Visual C++ para ADO
title: Extensiones de Visual C++ para ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- ADO, Visual C++
- Visual C++ [ADO], VC++ extensions for ADO
ms.assetid: 2952ece0-7217-4448-bb09-f6b64f43b7e2
author: rothja
ms.author: jroth
ms.openlocfilehash: 837ecdb45ead79e9e957a6d2381ccfaf1de6fb2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028670"
---
# <a name="visual-c-extensions-for-ado"></a>Extensiones de Visual C++ para ADO
El método preferido para programar ADO con Visual C++ es usar la directiva **#import** , como se describe en [Microsoft Visual C++ programación de ADO](./visual-c-ado-programming.md). Sin embargo, las versiones anteriores de ADO se incluían con un método alternativo de programación mediante Visual C++: las extensiones de Visual C++. En esta sección se documenta esta característica para los usuarios que deben mantener Visual C++ código de extensiones, pero se debe escribir el código de ADO nuevo mediante #**Import**.

 Uno de los trabajos más tediosos Visual C++ los programadores a la hora de recuperar datos con ADO es convertir los datos devueltos como un tipo de datos VARIANT en un tipo de datos de C++ y, a continuación, almacenar los datos convertidos en una clase o estructura. Además de ser engorroso, la recuperación de datos de C++ a través de un tipo de datos VARIANT disminuye el rendimiento.

 ADO proporciona una interfaz que admite la recuperación de datos en tipos de datos nativos de C/C++ sin pasar por una variante y también proporciona macros de preprocesador que simplifican el uso de la interfaz. El resultado es una herramienta flexible que es más fácil de usar y tiene un gran rendimiento.

 Un escenario de cliente de C/C++ común es enlazar un registro de un [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) a una estructura o clase de c/c++ que contiene tipos nativos de c/c++. Al pasar por las variantes, esto implica la escritura de código de conversión de los tipos nativos de C/C++. Las extensiones de Visual C++ para ADO están destinadas a hacer que este escenario sea mucho más fácil para el programador de Visual C++.

 Vea los temas siguientes para obtener más información acerca de las extensiones de Visual C++ para ADO.

-   [Usar extensiones de Visual C++ para ADO](./using-visual-c-extensions.md)

-   [Encabezado de extensiones de Visual C++](./visual-c-extensions-header.md)

-   [Ejemplo de ADO con Visual C++ Extensions](./visual-c-extensions-example.md)

## <a name="see-also"></a>Consulte también
 [Ado para Visual C++ Índice de sintaxis para](../../reference/ado-api/ado-for-visual-c-syntax-index-for-com.md) [las extensiones de Visual C++](./visual-c-extensions-example.md) de com ejemplo que [usa extensiones de Visual C++](./using-visual-c-extensions.md) [Visual C++ encabezado Extensions](./visual-c-extensions-header.md)
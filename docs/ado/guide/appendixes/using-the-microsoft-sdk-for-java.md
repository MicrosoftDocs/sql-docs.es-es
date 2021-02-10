---
description: Mediante el SDK de Microsoft para Java
title: Uso del SDK de Microsoft para Java | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Java (Microsoft SDK for)
- Microsoft SDK for Java [ADO]
ms.assetid: 2d7cb5b5-8307-49dd-b07e-c07069bb1626
author: rothja
ms.author: jroth
ms.openlocfilehash: 13a518a3cbf7c4d9f83da5810e340406147f86f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028835"
---
# <a name="using-the-microsoft-sdk-for-java"></a>Mediante el SDK de Microsoft para Java

> [!IMPORTANT]
> Microsoft no ha dejado de ofrecer compatibilidad con Visual J++ en el 2004 de enero.

Microsoft SDK para Java es el kit para desarrolladores del entorno de Microsoft Internet Explorer. Se proporcionan herramientas, información y ejemplos para ayudarle a desarrollar programas y applets de Java basados en JDK 1,1 y la máquina virtual de Microsoft Win32 (VM de Microsoft). Microsoft SDK para Java no está asociado a Microsoft Visual J++. Para descargar este SDK, haga clic aquí.  
  
 La utilidad Jactivex.exe genera clases a partir de una biblioteca de tipos, pero solo se puede invocar en la línea de comandos. Esta característica no está integrada en el entorno de desarrollo de Visual J++. A diferencia de las clases generadas por el Asistente para la biblioteca de tipos de Java, puede ir a los contenedores de clase creados por el SDK. Esto resulta útil para depurar el modo en que el código utiliza las clases contenedoras de ADO.  
  
 Este mecanismo lee la biblioteca de tipos ADO y genera clases de las que se pueden crear instancias en la aplicación. Genera esas clases en la siguiente ubicación: \\<directorio de Windows \> \Java\trustlib\msado15.  
  
 La creación de una aplicación ADO en Java mediante el SDK de Microsoft para Java es fundamentalmente idéntica, desde la perspectiva del código fuente, hasta el uso del Asistente para la biblioteca de tipos de Java. Para obtener código de ejemplo, vea [contenedores de clase de Java de ADO](./ado-java-class-wrappers.md). La única diferencia real es la forma en que se generan las clases contenedoras en primer lugar, como se muestra en los pasos siguientes.  
  
### <a name="to-create-an-ado-project-with-the-microsoft-sdk-for-java"></a>Para crear un proyecto de ADO con el SDK de Microsoft para Java  
  
1.  Ejecute lo siguiente en un símbolo del sistema. Debe establecer la ruta de acceso para incluir el directorio bin de Microsoft SDK para Java o ejecutar el comando desde esa ubicación. Normalmente, el SDK de Microsoft para Java se instala en la misma ubicación que Visual Studio. Se trata de una única instrucción de comando.  
  
    ```java
    \<path to DevStudio>\<path to Java SDK>\bin\JactiveX.exe /javatlb "C:\program files\common files\system\ado\msado15.dll"  
    ```  
  
2.  Ejecute el siguiente comando para compilar las clases generadas. El modificador/g: t activa la generación de símbolos de depuración para que pueda realizar un seguimiento en. Símbolos de Java. Quítelo para las compilaciones de versión.  
  
    ```java
    jvc /g:t c:\<windows>\Java\trustlib\msado15\*.Java  
    ```  
  
3.  Para usar estos archivos, abra el proyecto en Visual J++. En el menú **proyecto** , elija **Agregar al proyecto**. Seleccione **archivos** y agregue todos los. Archivos de JAVA generados en el directorio trustlib\msado15 para el proyecto.  
  
## <a name="see-also"></a>Consulte también  
 [Contenedores de clase Java de ADO](./ado-java-class-wrappers.md)
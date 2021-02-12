---
title: Instalación y administración de las extensiones de características
description: Obtenga información sobre cómo instalar extensiones de características para aumentar la funcionalidad de SQL Server Data Tools. Vea dónde instalar los distintos tipos de extensiones.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9cdc8cd5-c36f-4bee-a191-87ed457803e7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: c469770ab64401498b35038f7b20e6f245793804
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020748"
---
# <a name="how-to-install-and-manage-feature-extensions"></a>Procedimientos: Instalación y administración de las extensiones de características

Puede agregar reglas para analizar código de base de datos, condiciones para pruebas unitarias de base de datos y colaboradores de compilación e implementación para aumentar la funcionalidad que ofrecen las ediciones de Visual Studio, incluido SQL Server Data Tools. Sin embargo, deberá instalar una extensión de características antes de poder utilizarla, tanto si creó la extensión usted mismo o instaló una extensión creada por otra persona.  
  
La ubicación de instalación de la extensión depende del tipo de extensión y desde dónde tenga previsto utilizarla. En las últimas ediciones de Visual Studio se ha modificado la ubicación de instalación de algunos componentes, del directorio de instalación de SQL Server al directorio de Visual Studio. Esto facilita disponer de versiones diferentes del software ejecutándose en paralelo, pero significa que puede que sea necesario instalar la extensión en varias ubicaciones si desea utilizarla en una versión diferente de SQL Server Data Tools y desde la línea de comandos.  
  
### <a name="installing-extensions-for-use-inside-visual-studio"></a>Instalación de las extensiones para su uso en Visual Studio  
  
|Tipo de extensión|Ubicación de instalación|  
|------------------|--------------------|  
|Condiciones de prueba personalizadas para pruebas unitarias de SQL Server|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\TestConditions|  
|Colaboradores de compilación<br /><br />Colaboradores de implementación<br /><br />Reglas de análisis de código estático|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions|  
  
El <Visual Studio Install Dir> variará según la versión de Visual Studio que se utilice y de su ubicación de instalación. Para Visual Studio 2012, normalmente es C:\Program Files (x86)\\MicrosoftVisual Studio 11.0. Para Visual Studio 2013, normalmente es C:\Program Files (x86)\\MicrosoftVisual Studio 12.0.  
  
Las extensiones se pueden ejecutar como parte de nuestros servicios de línea de comandos:  
  
|Tipo de extensión|Servicio de línea de comandos|Carpeta de instalación|  
|------------------|------------------------|------------------|  
|Condiciones de prueba personalizadas para pruebas unitarias de SQL Server|MSBuild / MSTest puede utilizarse para ejecutar pruebas unitarias desde el Símbolo del sistema para desarrolladores para Visual Studio 2013 y otras herramientas de línea de comandos similares.|Igual que cuando se ejecuta en Visual Studio.|  
|Colaboradores de compilación<br /><br />Colaboradores de implementación|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md), o mediante el uso de destinos de implementación o publicación de MSBuild al crear un proyecto de base de datos.|MSBuild: Igual que cuando se ejecuta en Visual Studio.<br /><br />[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md): si se encuentra dentro del directorio de Visual Studio, igual que antes.<br /><br />Si SqlPackage.exe y otros archivos DLL de DacFx se encuentran fuera de ese directorio, las extensiones deben colocarse en el mismo directorio o en C:\Program Files (x86)\\MicrosoftSQL Server\120\DAC\bin\Extensions.|  
|Reglas de análisis de código estático|MSBuild puede utilizarse para generar el proyecto y ejecutar análisis de código estático.<br /><br />Además puede ejecutar análisis de código con una API CodeAnalysisService desde sus propias aplicaciones. Las reglas de consulta de extensión funcionan en este caso del mismo modo que cuando se utiliza SqlPackage.exe.|Lo mismo es válido para los colaboradores de compilación  y de implementación.|  
  
> [!NOTE]  
> Debe tener permisos de administrador en el equipo para tener acceso a cualquiera de los directorios de instalación de la carpeta Archivos de programa. Si no tiene los permisos adecuados, póngase en contacto con el administrador de red.  
  
**Consideraciones sobre la seguridad**  
  
Antes de instalar una extensión que no ha creado usted, deberá tener en cuenta los siguientes riesgos:  
  
-   El programa de instalación de la extensión podría ser malintencionado y obtener acceso a recursos protegidos según sus permisos de instalación.  
  
-   La propia extensión podría ser malintencionada y obtener el control de recursos protegidos si el usuario que utiliza la extensión tiene permisos suficientes.  
  
Para minimizar los riesgos, instale una extensión únicamente si procede de un origen conocido. Si obtiene la extensión de un origen que no es de confianza, deberá examinar el código fuente de esa extensión y su programa de instalación (si lo tiene) antes de instalarla y utilizarla.  
  
## <a name="to-install-a-custom-feature-extension"></a>Para instalar una extensión de características personalizada  
Copie el ensamblado firmado (.dll) en la carpeta de instalación correcta. Cierre y vuelva a abrir Visual Studio. La extensión deberá estar ahora disponible.  
  
## <a name="see-also"></a>Consulte también  
[Extensión de las características de base de datos](../ssdt/extending-the-database-features.md)  
  

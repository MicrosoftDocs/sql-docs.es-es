---
title: Conversión de proyectos de base de datos de Visual Studio 2010 a proyectos de base de datos de SQL Server
description: Convierta proyectos de base de datos de Visual Studio 2010 en proyectos de SQL Server y cambie su destino a otras plataformas. Vea los objetos que SSDT puede convertir y los que no.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.projectconversion.dialog
- sql.data.tools.ImportDAC
ms.assetid: 7e5acf94-5c46-44c7-9ff5-ca7926f5332a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bf8db10c508ed2cdf5fd2f4bc96c0d1d2508d83d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018385"
---
# <a name="how-to-convert-a-visual-studio-2010-database-projects-to-sql-server-database-projects-and-retarget-to-a-different-platform"></a>Procedimientos: Conversión de proyectos de base de datos de Visual Studio 2010 en proyectos de base de datos de SQL Server y cambio del destino a otra plataforma

En SQL Server Data Tools (SSDT), puede convertir proyectos de bases de datos de SQL Database, CLR y aplicación de capa de datos creados en Visual Studio 2010 en el nuevo proyecto de base de datos de SQL Server. Al hacerlo, puede aprovechar la nueva experiencia de desarrollo de bases de datos que ofrece SSDT, como edición de Transact\-SQL actualizada, y la posibilidad de cambiar el destino del proyecto a Microsoft SQL Server 2012 y SQL Azure con validación del código. El proceso de conversión convierte los objetos (tablas, vistas, procedimientos almacenados, archivos de propiedades o scripts) que tengan un tipo equivalente en SSDT, incluidos sus permisos y archivos de directivas de DAC. Los artefactos que no se puedan convertir aparecen resaltados en un informe de registro de conversión.  
  
En la tabla siguiente se enumeran todos los artefactos de proyecto que SSDT puede o no puede convertir.  
  
|Artefactos de proyecto que se pueden convertir|Artefactos de proyecto que no se pueden convertir|  
|-------------------------------------------|----------------------------------------------|  
|Archivos de proyecto<br /><br />Archivos de proyecto .dbproj (proyectos de base de datos y de servidor de Visual Studio 2010, proyectos de aplicación de capa de datos)<br /><br />Los archivos de proyecto CLR .csproj y .vbproj se pueden convertir, pero se pueden producir pérdidas de datos|Proyectos de prueba unitaria de base de datos<br /><br />Proyectos parciales como elementos .files|  
|Archivos de propiedades<br /><br />Los archivos *.sqldeployment, .sqlsettings y .sqlpolicy se convierten en sus páginas Propiedad de proyecto correspondientes<br /><br />Los archivos .sqlpermissions se convierten en scripts Transact\-SQL|Propiedades del proyecto<br /><br />Server.sqlsettings<br /><br />Variables SQLCMD definidas en archivos .sqlcmd|  
|Los archivos .sql se importan usando su estructura de carpetas existente.|Archivos de extensibilidad.|  
|Scripts anteriores y posteriores a la implementación|Las referencias de base de datos tendrán que volver a establecerse manualmente después de la conversión del proyecto.|  
|Archivos de comparación de esquemas|Archivos de generación de datos.|  
  
### <a name="to-convert-a-project"></a>Para convertir un proyecto  
  
1.  Abra un proyecto de base de datos de SQL Server 2005 o 2008.  
  
2.  Se abrirá automáticamente el asistente **Convertir a proyecto de base de datos de SQL Server**. Seleccione **Convertir en proyecto de base de datos de SQL Server** y haga clic en **Aceptar**. Deje activada la opción predeterminada para hacer copia de seguridad de los archivos existentes.  
  
3.  Se generará automáticamente un informe de conversión en el que se muestran todos los archivos que se han convertido. Para más información sobre el proceso de conversión, haga clic en el signo **+** junto al nombre de archivo del proyecto.  
  
4.  Observe que en el **Explorador de soluciones** se han convertido el archivo de proyecto, los archivos de propiedades y los objetos de esquema.  
  
### <a name="to-change-a-projects-target-platform"></a>Para cambiar la plataforma de destino de un proyecto  
  
1.  Haga clic con el botón derecho en el proyecto recién convertido en el **Explorador de soluciones** y seleccione **Propiedades** para tener acceso al cuadro de diálogo **Configuración del proyecto**.  
  
2.  Seleccione cualquiera de las plataformas admitidas por SSDT en la lista desplegable **Plataforma de destino**.  
  
## <a name="see-also"></a>Consulte también  
[Cómo: Cambiar la plataforma de destino y publicar un proyecto de base de datos](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  

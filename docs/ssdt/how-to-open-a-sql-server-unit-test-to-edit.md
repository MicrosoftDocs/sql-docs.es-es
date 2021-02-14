---
title: Apertura de una prueba unitaria de SQL Server para editarla
description: Obtenga información sobre cómo editar una prueba unitaria de SQL Server para poder agregar funcionalidad o personalizar condiciones. Vea diferentes maneras de abrir el archivo de código fuente de la prueba.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: c6af1b12-54cd-42f9-b2ef-7164f8078323
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0cd06acbd8817c62e728ee054024f570682450fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018175"
---
# <a name="how-to-open-a-sql-server-unit-test-to-edit"></a>Procedimientos: Apertura de una prueba unitaria de SQL Server para editarla

Después de crear una prueba unitaria de SQL Server, puede usar el **Diseñador de pruebas unitarias de SQL Server** para agregar condiciones de prueba e instrucciones Transact\-SQL. Las pruebas creadas con el diseñador generan código de Visual C# o Visual Basic. Este código es lo que se ejecuta para las series de pruebas.  
  
Si está satisfecho con la prueba, puede ejecutarla tal cual está. Si desea agregar más funcionalidad a esta prueba unitaria, puede editar su código. Este código reside en un archivo .cs o .vb en el proyecto de prueba. Para más información, consulte [Archivos de pruebas unitarias de SQL Server](../ssdt/sql-server-unit-test-files.md). También puede personalizar las pruebas si crea nuevas condiciones de prueba. Para más información, vea: [Cómo: Crear condiciones de prueba para el Diseñador de pruebas unitarias de base de datos (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/aa833409(v=vs.100)).  
  
> [!NOTE]  
> Si elimina un método de prueba editando el archivo .cs o .vb, el método de prueba seguirá apareciendo en el **Diseñador de pruebas unitarias de SQL Server**. Este escenario aparece porque el método InitializeComponent de la clase de prueba sigue conteniendo las variables miembro para la prueba. Aunque la prueba aparezca en el diseñador, no puede ejecutarla porque el código ya no está presente. Para regenerar el método de prueba para esta prueba, modifique Transact\-SQL en el editor y, a continuación, guarde el archivo de prueba .cs o .vb, o recompile el proyecto de prueba.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-solution-explorer"></a>Para abrir el archivo de código fuente de una prueba unitaria de SQL Server desde el Explorador de soluciones  
  
-   En el **Explorador de soluciones**, haga clic con el botón derecho en el archivo de código fuente que contiene la prueba unitaria de SQL Server y, a continuación, haga clic en **Ver código**.  
  
    El método de prueba de la prueba unitaria aparecerá en la ventana de edición principal de Visual Studio cuando se abra el archivo.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2010"></a>Para abrir el archivo de código fuente de una prueba unitaria de SQL Server desde la ventana Vista de pruebas (Visual Studio 2010)  
  
1.  Ejecute una prueba unitaria. Para más información, consulte la sección "Ejecutar pruebas unitarias de SQL Server" de [Tutorial: Crear y ejecutar una prueba unitaria de SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  En la ventana Vista de pruebas, haga clic con el botón derecho en la prueba y haga clic en **Abrir prueba**.  
  
    El método de prueba de la prueba unitaria aparecerá en la ventana de edición principal de Visual Studio cuando se abra el archivo.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2012"></a>Para abrir el archivo de código fuente de una prueba unitaria de SQL Server desde la ventana Vista de pruebas (Visual Studio 2012)  
  
1.  Ejecute una prueba unitaria. Para más información, consulte la sección "Ejecutar pruebas unitarias de SQL Server" de [Tutorial: Crear y ejecutar una prueba unitaria de SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  En la ventana Explorador de pruebas, haga clic en el archivo de código fuente de la prueba unitaria.  
  
    El método de prueba de la prueba unitaria aparecerá en la ventana de edición principal de Visual Studio cuando se abra el archivo.  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Comprobar el código de base de datos con pruebas unitarias de SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  

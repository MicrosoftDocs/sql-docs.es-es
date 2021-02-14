---
title: Creación de una prueba unitaria de SQL Server vacía
description: Aprenda a crear pruebas unitarias de SQL Server. Vea cómo usar los mismos scripts TestInitialize y TestCleanup que se utilizan en otras pruebas y cómo usar otros scripts.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.createtest
ms.assetid: b6f3cd5a-3389-42d6-a93f-97b3ddf31b95
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 510d669d780a7f7b6f71f5a84eab87e9b70bf4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018325"
---
# <a name="how-to-create-an-empty-sql-server-unit-test"></a>Procedimientos: Creación de una prueba unitaria de SQL Server vacía

Incluya pruebas unitarias en el proyecto de base de datos para comprobar que los cambios realizados en los objetos de base de datos no interrumpen la funcionalidad existente. En los procedimientos siguientes se explica cómo crear pruebas unitarias de SQL Server para cualquier objeto de base de datos. SQL Server Data Tools incluye compatibilidad adicional para funciones, procedimientos y desencadenadores de base de datos almacenados. Para más información, vea: [Cómo: Creación de pruebas unitarias de SQL Server para funciones, desencadenadores y procedimientos almacenados](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md).  
  
Cuando se crea una prueba unitaria de SQL Server mediante el primer procedimiento, se crea automáticamente un proyecto de prueba si no existe ninguno. Si ya existen proyectos de prueba, tiene la opción de agregar la nueva prueba a uno de esos proyectos o puede crear uno nuevo. Para obtener más información sobre los proyectos de prueba, vea [ Cómo: Crear un proyecto de prueba para las pruebas unitarias de base de datos de SQL Server](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md).  
  
Tiene dos opciones para crear una prueba unitaria de SQL Server:  
  
-   Cree una prueba unitaria de SQL Server en una nueva clase de prueba.  
  
    Todas las pruebas unitarias de SQL Server dentro de una clase de prueba especificada usarán los mismos scripts TestInitialize y TestCleanup. Cree una nueva clase de prueba si desea que la prueba unitaria use scripts TestInitialize y TestCleanup diferentes a los de otras pruebas unitarias. Para más información, consulte [Scripts de pruebas unitarias de SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md).  
  
-   Cree una nueva prueba unitaria de SQL Server en una clase de prueba existente.  
  
    Elija esta opción si la prueba unitaria usará los mismos scripts TestInitialize y TestCleanup que otras pruebas unitarias de la clase.  
  
### <a name="to-create-a-sql-server-unit-test-inside-a-new-test-class"></a>Para crear una prueba unitaria de SQL Server en una nueva clase de prueba  
  
1.  En el menú **Prueba**, haga clic en **Nueva prueba**.  
  
    Aparecerá el cuadro de diálogo **Agregar nueva prueba**.  
  
2.  En **Plantillas**, haga clic en **Prueba unitaria de SQL Server**.  
  
3.  En **Nombre de la prueba**, escriba un nombre para la prueba.  
  
4.  En **Agregar a proyecto de prueba**, seleccione un proyecto de prueba existente al que desee agregar esta prueba. Si no existe ningún proyecto de prueba o si desea crear un nuevo proyecto de prueba, seleccione **Crear un nuevo proyecto de prueba <language>** .  
  
5.  Haga clic en **OK**.  
  
    Si el proyecto de prueba es nuevo, aparecerá el cuadro de diálogo **Nuevo proyecto de prueba**. Proporcione un nombre al proyecto y haga clic en **Aceptar**.  
  
    Si el proyecto de prueba es nuevo o no se ha configurado, aparecerá el cuadro de diálogo **Configuración de prueba de <ProjectName>** . Este cuadro de diálogo permite configurar la siguiente información del proyecto de prueba:  
  
    -   La conexión de base de datos utilizada para ejecutar pruebas.  
  
    -   La conexión de base de datos usada para validar los resultados de pruebas, implementar una base de datos y generar datos.  
  
    -   La implementación automática del proyecto de base de datos y de los cambios de esquema asociados en una configuración de proyecto determinada antes de ejecutar las pruebas unitarias.  
  
    Para más información, vea: [Cómo: Configurar una ejecución de prueba unitaria de SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
6.  Proporcione la información de configuración del proyecto y haga clic en **Aceptar**.  
  
    \- O bien  
  
    Haga clic en **Cancelar** para crear la prueba unitaria sin configurar el proyecto de prueba.  
  
    La prueba en blanco aparece en el **Diseñador de prueba unitaria**. Según el lenguaje especificado para crear el proyecto de prueba, se agregará un archivo de código fuente de Visual Basic o Visual C\# al proyecto de prueba. Este archivo contiene la clase de prueba unitaria de SQL Server que SQL Server Data Tools genera para la prueba unitaria recién creada. Esta clase de prueba puede contener una o más pruebas unitarias que se pueden agregar a través del Diseñador de pruebas unitarias de SQL Server o mediante código como nuevos métodos de prueba de la clase de prueba.  
  
    También puede agregar otras pruebas de la manera siguiente:  
  
    -   Haga clic con el botón derecho en un proyecto de prueba en el **Explorador de soluciones**, seleccione **Agregar**, **Nueva prueba** y **Prueba unitaria de SQL Server**.  
  
    -   En el Explorador de objetos de SQL Server, seleccione Crear pruebas unitarias.  
  
    Cuando se selecciona este archivo en el **Explorador de soluciones**, se muestra en el Diseñador de pruebas unitarias de SQL Server de forma predeterminada. Para ver el código o personalizarlo para agregar más funcionalidad a las pruebas unitarias, seleccione el archivo, haga clic con el botón derecho y elija **Ver código**.  
  
### <a name="to-create-a-sql-server-unit-test-inside-an-existing-test-class"></a>Para crear una prueba unitaria de SQL Server en una clase de prueba existente  
  
1.  Abra una clase de prueba unitaria de SQL Server existente en el **Diseñador de pruebas unitarias de SQL Server**. Puede obtener acceso al **Diseñador de pruebas unitarias de SQL Server** si hace doble clic en el archivo de código fuente de la prueba unitaria en el **Explorador de soluciones**.  
  
2.  Haga clic en el signo más ( **+** ) de la barra de navegación para mostrar el cuadro de diálogo **Especificar un nombre de prueba unitaria**.  
  
3.  Escriba un nombre y haga clic en **Aceptar**.  
  
    La nueva prueba unitaria de SQL Server estará disponible en la lista desplegable de la barra de navegación. También se agrega como nuevo método de prueba en la clase de prueba. Para ver el método de prueba en código, seleccione el archivo de clase, haga clic con el botón derecho y elija **Ver código**. El nombre del archivo actual de la clase de prueba se muestra en la pestaña de la parte superior del **Diseñador de pruebas unitarias de SQL Server**.  
  
Después de configurar el proyecto de prueba y crear la prueba unitaria, los pasos siguientes son:  
  
-   Agregue un script de prueba Transact\-SQL.  
  
-   Definir las acciones anteriores y posteriores a la prueba.  
  
-   Agregar las condiciones de prueba u otra instrucción de aserción para comprobar los resultados del script.  
  
> [!NOTE]  
> La condición de prueba no concluyente es la condición predeterminada agregada a cada prueba. Esta condición de prueba se incluye para indicar que la comprobación de la prueba no se ha implementado. Elimine esta condición de prueba de la prueba después de agregar otras condiciones de prueba. Para más información, vea: [Cómo: Incorporar condiciones de prueba a pruebas unitarias de base de datos](/previous-versions/visualstudio/visual-studio-2010/aa833242(v=vs.100)).  
  
## <a name="see-also"></a>Consulte también  
[Cómo: Ejecutar pruebas unitarias de SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md)  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Crear pruebas unitarias](/previous-versions/visualstudio/visual-studio-2008/ms182523(v=vs.90))  

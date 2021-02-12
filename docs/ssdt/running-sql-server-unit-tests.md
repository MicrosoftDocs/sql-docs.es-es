---
title: Ejecución de pruebas unitarias de SQL Server
description: Familiarícese con las pruebas unitarias de SQL Server. Vea recursos sobre la creación de pruebas y condiciones de prueba personalizadas, la ejecución de pruebas y la interpretación de los resultados.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconfig
ms.assetid: febcc87f-eb18-4c12-ba30-82ef0d49aaa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5983d00806f895d8112ac111312309cf93aa48ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063500"
---
# <a name="running-sql-server-unit-tests"></a>Ejecución de pruebas unitarias de SQL Server

Para mejorar y mantener la calidad del código, puede crear y ejecutar pruebas unitarias de SQL Server que comprueben el comportamiento de cualquier objeto de base de datos y después proteger esas pruebas en el control de versiones. Cuando usted o cualquier miembro del equipo cambie el esquema de la base de datos, ejecute pruebas unitarias de SQL Server y pruebas unitarias de software para comprobar que los cambios no interrumpen la funcionalidad existente. Puede ejecutar pruebas individuales o puede ejecutar grupos de prueba, que se denominan listas de pruebas. Para más información, consulte [Usar listas de pruebas (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182461(v=vs.100)).  
  
## <a name="ways-to-run-sql-server-unit-tests"></a>Maneras de ejecutar las pruebas unitarias de SQL Server  
Puede ejecutar pruebas unitarias de SQL Server de varias maneras, que varían según el software instalado, como se muestra a continuación:  
  
-   Ejecutar pruebas mediante la ventana **Vista de pruebas** de Visual Studio 2010. Para más información, vea: [Cómo: Ejecutar pruebas unitarias de SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md) y [Cómo: Ejecutar pruebas automatizadas desde Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)). Para Visual Studio 2012, vea [Cómo: Ejecutar pruebas automatizadas desde Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Ejecutar pruebas mediante el comando MSTest.exe en un símbolo del sistema. Para más información, consulte [Cómo: Ejecutar pruebas automatizadas desde la línea de comandos usando MSTest (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182487(v=vs.100)) o [Cómo: Ejecutar pruebas automatizadas desde la línea de comandos usando MSTest (Visual Studio 2012)](/previous-versions/ms182487(v=vs.140)).  
  
-   Ejecutar pruebas desde el **Explorador de soluciones** ejecutando un proyecto de prueba. Para más información, consulte [Cómo: Ejecutar pruebas automatizadas desde Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)) o [Cómo: Ejecutar pruebas automatizadas desde Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Volver a ejecutar pruebas desde la ventana **Resultados de las pruebas**. Para más información, vea: [Cómo: Vuelva a ejecutar una prueba (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100)).  
  
-   Ejecutar pruebas individuales o listas de pruebas (Visual Studio 2010) desde la ventana **Editor de listas de pruebas**. Para más información, consulte [Cómo: Ejecutar pruebas automatizadas desde Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)) o [Cómo: Ejecutar pruebas automatizadas desde Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Ejecutar pruebas a medida que se compila un proyecto en Team Foundation Build. Para más información, consulte [Cómo: Configurar y ejecutar pruebas programadas después de compilar la aplicación (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100)) o [Cómo: Configurar y ejecutar pruebas programadas después de compilar la aplicación (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182465(v=vs.110)).  
  
Puede ejecutar las pruebas unitarias de SQL Server en un orden determinado usando una prueba por orden. Para más información, vea: [Cómo: Crear una prueba ordenada (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182631(v=vs.100)) o [Cómo: Crear una prueba ordenada (Visual Studio 2012)](/previous-versions/ms182631(v=vs.140)).  
  
## <a name="interpreting-tests-results"></a>Interpretación de los resultados de pruebas  
Una vez ejecutadas las pruebas, la ventana **Resultados de pruebas** muestra qué pruebas han producido errores y cuáles se han realizado correctamente. Para más información, consulte [Interpretar los resultados de pruebas unitarias de SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md). Para obtener más información sobre cómo diagnosticar un error inesperado, vea [Cómo: Depurar objetos de base de datos](../ssdt/how-to-debug-database-objects.md).  
  
## <a name="topics-in-this-section"></a>Temas de esta sección  
Esta sección contiene los siguientes temas:  
  
-   [Cómo: Depurar objetos de base de datos](../ssdt/how-to-debug-database-objects.md)  
  
-   [Cómo: Ejecutar pruebas unitarias de SQL Server desde Team Foundation Build](../ssdt/how-to-run-sql-server-unit-tests-from-team-foundation-build.md)  
  
-   [Cómo: Ejecutar pruebas unitarias de SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md)  
  
-   [Interpretar los resultados de pruebas unitarias de SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md)  
  
## <a name="related-scenarios"></a>Escenarios relacionados  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
Puede definir pruebas unitarias para comprobar el comportamiento de los objetos de base de datos y asociar cada proyecto de prueba con un plan de generación de datos, una configuración de implementación y una cadena de conexión diferentes.  
  
[Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
Puede crear una condición de prueba personalizada para probar cualquier condición que no pueda comprobar mediante las condiciones de prueba predeterminadas.  
  
## <a name="see-also"></a>Consulte también  
[Comprobar el código de base de datos con pruebas unitarias de SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  

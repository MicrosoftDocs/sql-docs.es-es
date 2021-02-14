---
title: Comprobar el código de base de datos con pruebas unitarias de SQL Server
description: Obtenga información sobre las pruebas unitarias de SQL Server. Vea qué ediciones de Visual Studio proporcionan compatibilidad para las pruebas, y recursos sobre el uso de pruebas para comprobar el código de la base de datos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 003713e2-de6b-4277-a0a8-7d1f2f4ffb39
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 4c05b9e3a3330b395ee3cd8c2cb3da5aa7d50508
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066703"
---
# <a name="verifying-database-code-by-using-sql-server-unit-tests"></a>Comprobar el código de base de datos con pruebas unitarias de SQL Server

Puede usar las pruebas unitarias de SQL Server para establecer un estado de línea base para la base de datos y después para comprobar los cambios subsiguientes que realice en los objetos de base de datos.  
  
Para establecer un estado de línea base para una base de datos, cree un proyecto de prueba y escriba los conjuntos de pruebas de Transact\-SQL que funcionan en los objetos de base de datos. Mediante el uso de estas pruebas puede comprobar en un entorno de desarrollo aislado si esos objetos funcionan como esperaba. Las pruebas unitarias de SQL Server funcionan bien en combinación con el desarrollo de bases de datos sin conexión que usan proyectos de base de datos de SQL Server(consulte [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md) para obtener más información). Una vez que haya establecido una línea base para las pruebas unitarias de SQL Server, puede usar estas pruebas para comprobar que la base de datos funciona correctamente antes de proteger los cambios en el control de versiones.  
  
Puede crear las pruebas que comprueben los cambios en cualquier objeto de base de datos. Además, puede generar automáticamente stubs de código de Transact\-SQL que prueban las funciones de base de datos, los desencadenadores y los procedimientos almacenados.  
  
> [!NOTE]  
> Puede crear y ejecutar las pruebas unitarias de SQL Server sin tener un proyecto de base de datos abierto. Sin embargo, si desea generar automáticamente los scripts de prueba para comprobar objetos de base de datos específicos del proyecto, debe abrir el proyecto de base de datos que contiene objetos que desea probar.  
  
Cuando usted o los miembros del equipo cambien el esquema de la base de datos, puede usar estas pruebas para comprobar si los cambios han interrumpido la funcionalidad existente. Las pruebas unitarias de SQL Server se pueden crear para complementar las pruebas unitarias de software creadas por los desarrolladores de software. Debe completar los conjuntos de pruebas para comprobar el comportamiento general de la aplicación.  
  
Las pruebas unitarias pueden comprobar que los procedimientos tengan éxito cuando se espera que lo tengan y que produzcan errores cuando se espera que los produzcan. Probar que aparecen los errores correspondientes se conoce como prueba negativa.  
  
## <a name="visual-studio-editions-support-for-sql-server-unit-tests"></a>Compatibilidad de las ediciones de Visual Studio con las pruebas unitarias de SQL Server  
La característica de pruebas unitarias de SQL Server, que se agregó en la actualización de diciembre de 2012 de SQL Server Data Tools, le permite crear, modificar y ejecutar pruebas unitarias de SQL Server en Visual Studio 2010 Professional y Visual Studio 2012 Professional y ediciones posteriores.  
  
Para asegurarse de que instala la actualización más reciente de SQL Server Data Tools, acceda al [cuadro de diálogo Buscar actualizaciones](../ssdt/check-for-updates-dialog-box.md).  
  
El shell de SQL Server Data Tools integrado de Visual Studio 2010 y Visual Studio 2012 no admite pruebas unitarias de SQL Server.  
  
## <a name="common-tasks"></a>Tareas comunes  
En la siguiente tabla, puede buscar las descripciones de tareas comunes que admiten este escenario y vínculos a más información acerca de cómo completar correctamente esas tareas.  
  
|Tareas comunes|Contenido adicional|  
|----------------|----------------------|  
|**Obtener experiencia práctica**: puede realizar un tutorial de introducción para familiarizarse con la forma de crear y ejecutar una prueba unitaria simple de SQL Server. En este tutorial se incluye un ejemplo de una prueba unitaria negativa de SQL Server.|[Tutorial: Crear y ejecutar una prueba unitaria de SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**Definir las pruebas unitarias de SQL Server**: debe crear las pruebas unitarias de SQL Server en su propio proyecto. Configure los valores para el proyecto y defina una o varias condiciones de prueba para cada prueba.|[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)<br /><br />[Usar condiciones de prueba en pruebas unitarias de SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)|  
|**Ejecutar pruebas unitarias de SQL Server**: después de definir una o más pruebas, ejecútelas, depure los problemas y examine los resultados.|[Ejecutar pruebas unitarias de SQL Server](../ssdt/running-sql-server-unit-tests.md)|  
|**Administrar grupos de pruebas (Visual Studio 2010)** : podría organizar las pruebas en grupos si se suelen ejecutar al mismo tiempo. Todavía se admiten las listas de pruebas, pero en los grupos de pruebas, debe considerar la posibilidad de usar categorías de prueba en su lugar. Por ejemplo, puede crear una categoría de prueba para las pruebas de los desencadenadores o de todos los objetos de un *esquema* determinado.|[Definición de categorías de prueba para agrupar las pruebas](/previous-versions/visualstudio/visual-studio-2010/dd286595(v=vs.100))<br /><br />[Definición de listas de pruebas para agrupar las pruebas](/previous-versions/visualstudio/visual-studio-2010/dd286584(v=vs.100))|  
|**Proteger los proyectos de prueba y las pruebas en el control de versiones:** después de ejecutar las pruebas y comprobar si funcionan correctamente, debe proteger el proyecto de prueba y todos los archivos asociados al control de versiones para que todos los miembros del equipo puedan ejecutar las pruebas. Al proteger el proyecto de prueba en el control de versiones junto con el proyecto de base de datos de SQL Server, puede restaurar fácilmente versiones compatibles tanto de la base de datos como de las pruebas de base de datos.|[Agregar archivos al control de versiones](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100))<br /><br />[Usar las ventanas Proteger y Cambios pendientes](/previous-versions/visualstudio/visual-studio-2010/ms245462(v=vs.100))|  
|**Definir condiciones de prueba personalizadas**: puede crear condiciones de prueba personalizadas si debe probar el comportamiento que el conjunto predeterminado de condiciones de prueba no cubre. Debe distribuir estas condiciones a todos los miembros del equipo que desea ejecutar las pruebas que usan las nuevas condiciones.|[Escenario: definir las condiciones de prueba personalizadas para pruebas unitarias de SQL Server](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))|  
|**Actualizar pruebas unitarias existentes**: si tiene pruebas unitarias de base de datos que se crearon en una versión anterior de Visual Studio, debe actualizarlas antes de que se compilen y se ejecuten correctamente con esta versión.<br /><br />**NOTA:** Si abre una solución que contiene un proyecto de base de datos y un proyecto de prueba unitaria de base de datos de una versión anterior de Visual Studio, se le preguntará si desea actualizar el proyecto de base de datos. No se le preguntará si desea actualizar los proyectos de prueba unitaria de base de datos, que se deben actualizar manualmente.|[Actualizar un proyecto de prueba anterior que contiene pruebas unitarias de base de datos](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md)|  
|**Extensibilidad**: puede extender SQL Server Data Tools mediante la creación de extensiones de características.|[Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)|  
|**Solucionar problemas**: puede obtener más información acerca de cómo solucionar problemas comunes de las pruebas unitarias de SQL Server.|[Solucionar problemas con las pruebas unitarias de base de datos en SQL Server](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>Escenarios relacionados  
[Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md)  
Las pruebas unitarias de bases de datos resultan especialmente eficaces cuando se usan junto con el desarrollo de proyectos sin conexión mediante proyectos de base de datos de SQL Server.  
  
## <a name="see-also"></a>Consulte también  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  

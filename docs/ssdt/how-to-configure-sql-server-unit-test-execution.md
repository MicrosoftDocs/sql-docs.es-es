---
title: Configuración de una ejecución de prueba unitaria de SQL Server
description: Aprenda a configurar pruebas unitarias de SQL Server. Vea cómo especificar cadenas de conexión y cómo implementar un esquema de base de datos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: e0179429-13ce-4d23-ae27-e6419de0a575
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 569262aece2d8b1212704c0b2ba443c6c63f2b07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018415"
---
# <a name="how-to-configure-sql-server-unit-test-execution"></a>Procedimientos: Configuración de una ejecución de prueba unitaria de SQL Server

Al configurar el proyecto de prueba, puede especificar varios valores que controlan aspectos de la ejecución de las pruebas unitarias de SQL Server. Esta configuración se almacena en el archivo app.config del proyecto de prueba. Si edita este archivo directamente, los nuevos valores aparecerán en el cuadro de diálogo de configuración de prueba.  
  
La solución puede contener varios proyectos de prueba. Cada proyecto de prueba contiene un archivo app.config (es decir, un conjunto de valores de configuración). Como resultado, la solución puede contener diferentes conjuntos de pruebas unitarias (un conjunto para cada proyecto de prueba) que están configurados para ejecutarse de forma diferente.  
  
Estos valores controlan cómo se conecta la prueba a la base de datos que probará y cómo se implementa un esquema desde un proyecto de base de datos en esa base de datos:  
  
-   **Conexiones de base de datos** Este valor se usa para especificar las cadenas de conexión que se utilizan para conectarse a la base de datos que se está probando. Para más información, consulte [Especificar las cadenas de conexión](#SpecifyConnectionStrings).  
  
-   **Esquema de implementación**. Un proyecto de base de datos es una representación sin conexión de la base de datos. El proyecto de base de datos representa la estructura de los objetos de base de datos pero no contiene datos. Después de realizar cambios de esquema en un proyecto de base de datos, puede probarlos en una base de datos actual. En el paso de la implementación del esquema, los objetos de base de datos que desea probar se copian del proyecto de base de datos en el que se ejecutan las pruebas. Para más información sobre la implementación del esquema, consulte [Implementar un esquema de la base de datos](#DeployingDBSchema).  
  
    > [!NOTE]  
    > Las pruebas no se ejecutan en la carpeta de la solución, sino en una carpeta independiente en el disco duro local. Aunque se pueden configurar algunos aspectos de la implementación de prueba, normalmente no es necesario configurarlos para las pruebas unitarias. Para más información acerca de la implementación de prueba, consulte [Ejecutar pruebas](/previous-versions/visualstudio/visual-studio-2010/dd286680(v=vs.100)).  
  
## <a name="specify-connection-strings"></a><a name="SpecifyConnectionStrings"></a>Especificar cadenas de conexión  
  
#### <a name="to-specify-database-connection-strings"></a>Para especificar las cadenas de conexión a la base de datos  
  
1.  Haga clic con el botón derecho en el proyecto de pruebas unitarias en el **Explorador de soluciones** y haga clic en **Configuración de prueba de SQL Server**.  
  
    Se abre el cuadro de diálogo **Configuración de prueba de SQL Server -'<projectname>'** .  
  
2.  En **Conexiones de base de datos**, puede hacer lo siguiente:  
  
    -   Haga clic en la conexión de base de datos en la que desea ejecutar las pruebas unitarias.  
  
    -   Active la casilla **Usar una conexión de datos secundaria para validar pruebas unitarias** y haga clic en una conexión de base de datos en la lista si desea que la ejecución de prueba se valide en una conexión de base de datos diferente.  
  
    -   Haga clic en **Nueva conexión** para agregar una conexión a cada lista. También puede hacer clic en **Editar conexión** para modificar los valores en una conexión existente.  
  
    En este paso se crea la cadena de conexión `ExecutionContext`, que se usa para ejecutar el script de prueba en la prueba unitaria. Si también se especifica una conexión secundaria, también se crea la cadena de conexión de `PrivilegedContext`. Esta conexión se usa para probar las interacciones con la base de datos fuera del script de prueba en la prueba unitaria. Para obtener más información, consulte [Información general acerca de las cadenas de conexión y los permisos](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
3.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Configuración de prueba de SQL Server -'<projectname>'** .  
  
4.  Recompile el proyecto de prueba para aplicar los cambios de configuración.  
  
## <a name="deploy-a-database-schema"></a><a name="DeployingDBSchema"></a>Implementar un esquema de la base de datos  
  
#### <a name="to-deploy-to-a-database-the-schema-of-a-database-project"></a>Para implementar en una base de datos el esquema de un proyecto de base de datos  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto de base de datos y, a continuación, haga clic **Compilar**.  
  
    Al compilar el proyecto de base de datos, se genera un script de Transact\-SQL. Este script, cuando se ejecuta en una base de datos, vuelve a crear la estructura del proyecto de base de datos en ella.  
  
2.  Seleccione el proyecto de prueba que desee configurar.  
  
3.  Haga clic con el botón derecho en el proyecto de pruebas unitarias en el **Explorador de soluciones** y haga clic en **Configuración de prueba de SQL Server**.  
  
    Se abre el cuadro de diálogo **Configuración de prueba de SQL Server -'<projectname>'** .  
  
4.  En **Implementación**, puede hacer lo siguiente:  
  
    -   Active la casilla **Implementar automáticamente el proyecto de base de datos antes de ejecutar pruebas unitarias** para asegurarse de que los cambios de esquema que haya realizado en el proyecto de base de datos se confirmarán antes de ejecutar las pruebas.  
  
    -   En **Proyecto de base de datos**, haga clic en el proyecto de base de datos que desea implementar o haga clic en los puntos suspensivos para buscar otro proyecto. Los archivos de proyecto de base de datos tienen la extensión .dbproj.  
  
    -   Debajo de **Configuración de implementación**, haga clic en la configuración de proyecto en la que desea implementar. Las opciones son **Depuración**, **Predeterminada** o **Liberar**. Sin embargo, si crea una configuración para la prueba unitaria, dicha configuración también aparece como opción.  
  
5.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Configuración de prueba de SQL Server -'<projectname>'** .  
  
    Al inicio de la ejecución de pruebas, se ejecuta el script de Transact\-SQL generado en el paso 1. En esta acción se implementa el esquema en la base de datos de destino.  
  
6.  Recompile el proyecto de prueba unitaria para aplicar los cambios de configuración.  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Comprobar el código de base de datos con pruebas unitarias de SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  

---
title: Actualizar un proyecto de prueba anterior que contiene pruebas unitarias de base de datos
description: Descubra cómo actualizar proyectos de prueba de Visual Studio 2010 que contienen pruebas unitarias de base de datos. Vea cómo usar SQL Server Data Tools con esos proyectos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 42782ff3-e8cf-4c9d-8dac-a95b236edfc4
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2830890ecb1fc9983ce4ccfac61874d4e529f07b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066860"
---
# <a name="upgrade-an-older-test-project-containing-database-unit-tests"></a>Actualizar un proyecto de prueba anterior que contiene pruebas unitarias de base de datos

Puede actualizar un proyecto de prueba anterior, que se creó en Visual Studio 2010 y que contiene pruebas unitarias de base de datos, para que use el nuevo entorno de ejecución y las nuevas herramientas de pruebas unitarias de base de datos de SQL Server Data Tools. Una vez actualizado un proyecto anterior, puede agregar pruebas unitarias de SQL Server al proyecto (consulte [Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)) para obtener más información.  
  
> [!TIP]  
> Si está usando Visual Studio 2010, después de agregar pruebas unitarias de SQL Server a un proyecto de prueba, no debe agregar pruebas unitarias mediante la plantilla anterior de prueba unitaria de base de datos. Si lo hace, necesitará volver a convertir el proyecto para que las pruebas se ejecuten correctamente.  
  
Si tiene un proyecto de base de datos de prueba creado en una versión anterior a Visual Studio 2010, puede usar la información incluida en [Actualización de las pruebas unitarias de base de datos de versiones anteriores de Visual Studio](/previous-versions/visualstudio/visual-studio-2010/dd193412(v=vs.100)) para actualizar el proyecto de base de datos a Visual Studio 2010, antes de actualizar el proyecto a SQL Server Data Tools.  
  
### <a name="initiating-an-upgrade"></a>Iniciar una actualización  
  
-   Puede iniciar la actualización de un proyecto desde el menú contextual de un proyecto de prueba.  
  
    En algunos casos, SQL Server Data Tools mostrará un cuadro de diálogo desde el que puede iniciar la actualización de un proyecto de prueba.  
  
-   La actualización del proyecto quita la referencia de ensamblado al marco de prueba de base de datos anterior y agrega una referencia al nuevo marco y un ensamblado del adaptador. El archivo app.config también se actualiza.  
  
    > [!NOTE]  
    > Si el proyecto de prueba tiene archivos de código DatabaseSetup y SQLDatabaseSetup, la actualización del proyecto a SQL Server Data Tools excluirá el archivo DatabaseSetup de la compilación. Puede quitar el archivo DatabaseSetup si se excluye de la compilación.  
  
-   Después de la conversión, las pruebas unitarias de base de datos existentes creadas con la plantilla anterior usarán tipos del ensamblado del adaptador para obtener acceso al nuevo marco. El uso de un ensamblado del adaptador significa que el procedimiento de actualización no modificó los scripts y el código de prueba. Si agrega al proyecto una prueba unitaria de SQL Server, la nueva prueba hará referencia al nuevo marco directamente y no mediante un adaptador. Puede elegir actualizar manualmente el código existente para que use el nuevo marco por coherencia con las nuevas pruebas, aunque no es necesario hacerlo.  
  
## <a name="see-also"></a>Consulte también  
[Comprobar el código de base de datos con pruebas unitarias de SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  

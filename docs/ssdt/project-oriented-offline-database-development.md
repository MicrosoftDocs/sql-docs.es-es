---
title: Desarrollo de bases de datos sin conexión orientado a proyectos
description: Vea los recursos disponibles sobre tareas de desarrollo de base de datos sin conexión orientadas a proyectos, como la importación de objetos en una base de datos y el uso de objetos de secuencia.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.general
- sql.data.tools.dbprojectwizard.summary
ms.assetid: e61e830d-9fcd-45e7-b7b4-93a42155dd56
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: e36876d1b30b12dbae1dd4c7d3c72230070c9ef8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077065"
---
# <a name="project-oriented-offline-database-development"></a>Desarrollo de bases de datos sin conexión orientado a proyectos

En esta sección se describen las características proporcionadas por SQL Server Data Tools (SSDT) para crear, compilar, depurar y publicar un proyecto de base de datos.  
  
Con SSDT, puede crear un proyecto de base de datos sin conexión e implementar cambios en el esquema agregando, modificando o eliminando definiciones de objetos (representadas por scripts) del proyecto sin una conexión a una instancia de servidor. Todo esto se puede realizar mediante el diseñador de tablas o el Editor de Transact\-SQL. También puede escribir y depurar objetos Transact\-SQL y CLR en el mismo proyecto. Puede usar Comparación de esquemas para asegurarse de que el proyecto está sincronizado con la base de datos de producción y crear instantáneas del proyecto en cada etapa del ciclo de desarrollo con fines de comparación. Mientras se trabaja en proyectos de base de datos en un entorno en equipo, se puede emplear el control de versiones para todos los archivos. Una vez desarrollado, probado y depurado el proyecto de base de datos, puede entregar el proyecto al personal autorizado para su publicación en un entorno de producción.  
  
> [!NOTE]  
> Los temas de procedimientos de esta sección contienen una serie de tareas que se pueden completar en un orden determinado.  
  
## <a name="in-this-section"></a>En esta sección  
  
|Tema|Descripción|  
|---------|---------------|  
|[Importar en un proyecto de base de datos](../ssdt/import-into-a-database-project.md)|Describe la importación de objetos de una base de datos existente, .dacpac o script.|  
|[Cuadro de diálogo Agregar referencia de base de datos](../ssdt/add-database-reference-dialog-box.md)|Describe varias maneras de agregar una referencia de base de datos.|  
|[Cuadro de diálogo Buscar actualizaciones](../ssdt/check-for-updates-dialog-box.md)|Describe cómo SQL Server Data Tools puede buscar actualizaciones de productos.|  
|[Configuración del proyecto de base de datos](../ssdt/database-project-settings.md)|Describe varios valores de configuración de proyectos para controlar facetas de las configuraciones de la compilación y las bases de datos.|  
|[Cómo: Examinar objetos en un proyecto de base de datos de SQL Server](../ssdt/how-to-browse-objects-in-a-sql-server-database-project.md)|El Explorador de objetos de SQL Server de Visual Studio contiene ahora un nodo dedicado Proyectos bajo el cual todos los proyectos de base de datos de SQL Server de la solución se agrupan en una jerarquía estilo SQL Server Management Studio.|  
|[Ventana Operaciones de Data Tools](../ssdt/data-tools-operations-window.md)|Describe la ventana **Operaciones de herramientas de datos** , que muestra el progreso de algunas operaciones y le notifica los errores que puedan producirse.|  
|[Opciones del Editor de Transact-SQL](../ssdt/transact-sql-editor-options.md)|Describe las opciones de Transact\-SQL.|  
|[Cómo: Crear un nuevo proyecto de base de datos](../ssdt/how-to-create-a-new-database-project.md)|Crear un proyecto de base de datos e importar un esquema de la base de datos existente.|  
|[Cómo: Usar Comparación de esquemas para comparar distintas definiciones de base de datos](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)|Comparar los esquemas de una base de datos y un proyecto y sincronizarlos.|  
|[Cómo: Compilar e implementar una base de datos local](../ssdt/how-to-build-and-deploy-to-a-local-database.md)|Use la instancia local de SQL Server a petición, que se activa al depurar un proyecto de base de datos.|  
|[Cómo: Cambiar la plataforma de destino y publicar un proyecto de base de datos](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)|Cambie la plataforma de destino de SQL Server del proyecto a cualquier instancia admitida de SQL Server y valide la sintaxis.|  
|[Cómo: Crear una instantánea de un proyecto](../ssdt/how-to-create-a-snapshot-of-a-project.md)|Crear un proxy de solo lectura del esquema de la base de datos y revertir el proyecto de origen cuando se apliquen cambios no deseados al proyecto.|  
|[Cómo: Usar objetos de Microsoft SQL Server 2012 en un proyecto](../ssdt/how-to-use-microsoft-sql-server-2012-objects-in-your-project.md)|Agregar un nuevo objeto de secuencia a un proyecto.|  
|[Cómo: Trabajar con objetos de base de datos CLR](../ssdt/how-to-work-with-clr-database-objects.md)|Cree y publique objetos CLR en el proyecto de base de datos de SQL Server Data Tools.|  
|[Cómo: Convertir proyectos de base de datos de Visual Studio 2010 en proyectos de base de datos de SQL Server y cambio del destino a otra plataforma](../ssdt/how-to-convert-visual-studio-2010-database-projects-to-ssql-server-projects.md)|Convierta proyectos de bases de datos de SQL Server, objetos CLR y aplicación de capa de datos creados en Visual Studio 2010 al nuevo proyecto de base de datos de SQL Server Data Tools.|  
|[Cómo: Especificar scripts anteriores o posteriores a la implementación](../ssdt/how-to-specify-predeployment-or-postdeployment-scripts.md)|Describe cómo se usan los scripts que se ejecutan antes o después de la implementación de la base de datos.|  
  
## <a name="related-sections"></a>Secciones relacionadas

[Administrar tablas y relaciones y corregir errores](../ssdt/manage-tables-relationships-and-fix-errors.md)

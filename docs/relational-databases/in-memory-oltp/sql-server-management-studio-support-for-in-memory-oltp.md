---
title: Compatibilidad de SSMS con OLTP en memoria
description: Use SQL Server Management Studio para administrar tablas optimizadas para memoria, índices de tablas, procedimientos almacenados compilados de forma nativa y tipos de tabla definidos por el usuario.
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ee847b5f-6a1a-448e-a746-d61a023881ff
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8684764c0386513c0890b10dee8bfa3582c1cf59
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485177"
---
# <a name="sql-server-management-studio-support-for-in-memory-oltp"></a>Compatibilidad de SQL Server Management Studio con OLTP en memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] es un entorno integrado para administrar la infraestructura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] proporciona herramientas para configurar, supervisar y administrar instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, consulte [SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md).  
  
 En las tareas de este tema se explica cómo utilizar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para administrar tablas optimizadas para memoria, índices de tablas optimizadas para memoria, procedimientos almacenados compilados de forma nativa, y tipos de tablas optimizadas para memoria definidos por el usuario.  
  
 Para obtener más información sobre cómo crear tablas optimizadas para memoria mediante programación, vea [Crear una tabla optimizada para memoria y un procedimiento almacenado compilado de forma nativa](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
### <a name="to-create-a-database-with-a-memory-optimized-data-filegroup"></a>Para crear una base de datos con un grupo de archivos de datos optimizados para memoria  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del Motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, a continuación, expándala.  
  
2.  Haga clic con el botón derecho en **Bases de datos** y, después, haga clic en **Nueva base de datos**.  
  
3.  Para agregar un nuevo grupo de archivos de datos optimizados para memoria, haga clic en la página **Grupos de archivos** . En **DATOS OPTIMIZADOS PARA MEMORIA**, haga clic en **Agregar grupo de archivos** y escriba el nombre del grupo de archivos de datos optimizados para memoria.  La columna con la etiqueta **Archivos FILESTREAM** representa el número de contenedores en el grupo de archivos. Los contenedores se agregan en la página **General** .  
  
4.  Para agregar un archivo (contenedor) al grupo de archivos, haga clic en la página **General** . Debajo de **Archivos de la base de datos**, haga clic en **Agregar**. Seleccione el **Tipo de archivo** **Datos de FILESTREAM**, especifique el nombre lógico del contenedor, seleccione el grupo de archivos optimizados para memoria y asegúrese de que **Crecimiento automático/tamaño máximo** se ha establecido en **Ilimitado**.  

     Para obtener más información sobre cómo crear una base de datos con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vea [Crear una base de datos](../../relational-databases/databases/create-a-database.md).  
  
### <a name="to-create-a-memory-optimized-table"></a>Para crear una tabla optimizada para memoria  
  
1.  En el **Explorador de objetos**, haga clic con el botón derecho en el nodo **Tablas** de la base de datos, haga clic en **Nuevo** y, después, haga clic en **Tabla con optimización para memoria**.  
  
     Se mostrará una plantilla para crear tablas optimizadas para memoria.  
  
2.  Para reemplazar los parámetros de plantilla, en el menú **Consulta** , haga clic en **Especificar valores para parámetros de plantilla** .  
  
     Para obtener más información acerca de cómo usar plantillas, vea [Template Explorer](../../ssms/template/template-explorer.md).  
  
3.  En el **Explorador de objetos**, las tablas se ordenarán primero según las tablas basadas en disco, seguidas de las tablas optimizadas para memoria. Use **Detalles del Explorador de objetos** para ver todas las tablas ordenadas por nombre.  
  
### <a name="to-create-a-natively-compiled-stored-procedure"></a>Para crear un procedimiento almacenado compilado de forma nativa  
  
1.  En el **Explorador de objetos**, haga clic con el botón derecho en el nodo **Procedimientos almacenados** de la base de datos, haga clic en **Nuevo** y, luego, haga clic en **Procedimiento almacenado compilado de forma nativa**.  
  
     Se muestra una plantilla para crear procedimientos almacenados compilados de forma nativa.  
  
2.  Para reemplazar los parámetros de plantilla, en el menú **Consulta** , haga clic en **Especificar valores para parámetros de plantilla**.  
  
     Para obtener más información sobre cómo crear un nuevo procedimiento almacenado, vea [Create a Stored Procedure](../../relational-databases/stored-procedures/create-a-stored-procedure.md).  
  
### <a name="to-create-a-user-defined-memory-optimized-table-type"></a>Para crear un tipo de tabla optimizada para memoria definido por el usuario  
  
1.  En el **Explorador de objetos**, expanda el nodo **Tipos** de la base de datos, haga clic con el botón derecho en el nodo **Tipos de tablas definidos por el usuario** , haga clic en **Nuevo** y, después, haga clic en **Nuevo tipo de tabla definido por el usuario optimizado de memoria**.  
  
     Aparece una plantilla para crear el tipo de tabla optimizada para memoria definido por el usuario.  
  
2.  Para reemplazar los parámetros de plantilla, en el menú **Consulta** , haga clic en **Especificar valores para parámetros de plantilla** .  
  
     Para obtener más información sobre cómo crear un procedimiento almacenado, vea [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md).  
  
## <a name="memory-monitoring"></a>Supervisión de la memoria  
  
#### <a name="view-memory-usage-by-memory-optimized-objects-report"></a>Ver el uso de la memoria en el informe de objetos con optimización para memoria  
  
-   En el **Explorador de objetos**, haga clic con el botón derecho en la base de datos, haga clic en **Informes**, haga clic en **Informes estándar** y haga clic en **Uso de memoria por los objetos con optimización para memoria**.  
  
     Este informe proporciona datos detallados acerca de la utilización del espacio de memoria por parte de los objetos optimizados para memoria dentro de la base de datos.  
  
#### <a name="view-properties-for-allocated-and-used-memory-for-a-table-database"></a>Ver las propiedades de la memoria asignada y utilizada para una tabla o una base de datos  
  
1.  Para obtener información sobre el uso de memoria:  
  
    -   En el **Explorador de objetos**, haga clic con el botón derecho en la tabla optimizada para memoria, haga clic en **Propiedades** y, después, haga clic en la página **Almacenamiento** . El valor de la propiedad **Espacio de datos** indica la memoria utilizada por los datos de la tabla. El valor de la propiedad **Espacio de índice** indica la memoria utilizada por los índices de la tabla.  
  
    -   En el **Explorador de objetos**, haga clic con el botón derecho en la base de datos, haga clic en **Propiedades** y, después, haga clic en la página **General** . El valor de la propiedad **Memoria asignada a los objetos optimizados para memoria** indica la memoria asignada a los objetos optimizados para memoria en la base de datos. El valor de la propiedad **Memoria usada por los objetos optimizados para memoria** indica la memoria usada por los objetos optimizados para memoria en la base de datos.  
  
## <a name="supported-features-in-ssmanstudiofull"></a>Características admitidas en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] admite las características y las operaciones compatibles con el motor de base de datos en las bases de datos con un grupo de archivos de datos optimizados para memoria, tablas optimizadas para memoria, índices y procedimientos compilados de forma nativa.  
  
 En los objetos de base de datos, procedimiento almacenado, tipo de tabla definido por el usuario o índice se han actualizado o ampliado las siguientes características de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para que sean compatibles con OLTP en memoria.  
  
-   Explorador de objetos  
  
    -   Menús contextuales  
  
    -   Configuración de filtros  
  
    -   Script como  
  
    -   Tareas  
  
    -   Informes  
  
    -   Propiedades  
  
    -   Tareas de bases de datos:  
  
        -   Adjuntar y separar una base de datos que contiene tablas optimizadas para memoria.  
  
             La interfaz de usuario de **Adjuntar bases de datos** no muestra el grupo de archivos de datos optimizados para memoria. Sin embargo, puede continuar con la operación para adjuntar la base de datos y la base de datos se adjuntará correctamente.  
  
            > [!NOTE]  
            >  Si desea utilizar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para adjuntar una base de datos que tiene un contenedor de grupos de archivos de datos optimizados para memoria, y si el contenedor de grupos de archivos de datos optimizados para memoria de la base de datos se creó en otro equipo, la ubicación del contenedor de grupos de archivos de datos optimizados para memoria debe ser la misma en ambos equipos. Si desea que la ubicación del contenedor de grupos de archivos de datos optimizados para memoria de la base de datos sea diferente en el nuevo equipo, puede utilizar [!INCLUDE[tsql](../../includes/tsql-md.md)] para adjuntar la base de datos. En el ejemplo siguiente, la ubicación del contenedor de grupos de archivos de datos optimizados para memoria en el nuevo equipo es C:\Folder2. Pero cuando se creó el contenedor de grupos de archivos de datos optimizados para memoria en el primer equipo, la ubicación era C:\Folder1.  
            >   
            >  `CREATE DATABASE[imoltp] ON`  
            >   
            >  `(NAME =N'imoltp',FILENAME=N'C:\Folder2\imoltp.mdf'),`  
            >   
            >  `(NAME =N'imoltp_mod1',FILENAME=N'C:\Folder2\imoltp_mod1'),`  
            >   
            >  `(NAME =N'imoltp_log',FILENAME=N'C:\Folder2\imoltp_log.ldf')`  
            >   
            >  `FOR ATTACH`  
            >   
            >  `GO`  
  
        -   Generar scripts.  
  
             En el **Asistente Generar y publicar scripts**, el valor predeterminado de la opción de scripting **Comprobar la existencia de objetos** es FALSE. Si el valor de la opción de scripting **Comprobar la existencia de objetos** se establece en TRUE en la pantalla **Establecer opciones de scripting** del asistente, el script generado contendría "CREATE PROCEDURE <nombre_procedimiento> AS" y "ALTER PROCEDURE <nombre_procedimiento> <definición_procedimiento>". Cuando se ejecute, el script generado devolverá un error porque ALTER PROCEDURE no se admite en los procedimientos almacenados compilados de forma nativa.  
  
             Para cambiar el script generado para cada procedimiento almacenado compilado de forma nativa:  
  
            1.  En "CREATE PROCEDURE <nombre_procedimiento> AS", reemplace "AS" con "<definición_procedimiento>".  
  
            2.  Elimine "ALTER PROCEDURE <nombre_procedimiento> <definición_procedimiento>".  
  
        -   Copiar bases de datos. Para las bases de datos con objetos optimizados para memoria, la creación de la base de datos en el servidor de destino y la transferencia de datos no se ejecutarán dentro de una transacción.  
  
        -   Importe y exporte datos. Use la opción del **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Copiar los datos de una o varias tablas o vistas** . Si la tabla de destino es una tabla optimizada para memoria que no existe en la base de datos de destino:  
  
            1.  En el **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Asistente para importación y exportación**, en la pantalla **Especificar copia o consulta de tabla** , seleccione **Copiar datos de una o varias tablas o vistas**. A continuación, haga clic en **Siguiente**.  
  
            2.  Haga clic en **Editar asignaciones**. Seleccione **Crear tabla de destino** y haga clic en **Editar SQL**. Especifique la sintaxis CREATE TABLE para crear una tabla optimizada para memoria en la base de datos de destino. Haga clic en **Aceptar** y complete los pasos restantes del asistente.  
  
        -   Planes de mantenimiento. Las tareas de mantenimiento para reorganizar y volver a generar el índice no son compatibles con las tablas optimizadas para memoria ni con sus índices. Por tanto, cuando se ejecuta un plan de mantenimiento para volver a generar y reorganizar el índice, se omiten las tablas optimizadas para memoria y sus índices en las bases de datos seleccionadas.  
  
             La tarea de mantenimiento para actualizar estadísticas no es compatible con la realización de un examen de muestra en las tablas optimizadas para memoria ni con sus índices. Por tanto, cuando se ejecuta un plan de mantenimiento para actualizar estadísticas, las estadísticas de las tablas optimizadas para memoria y sus índices siempre se actualizan a **WITH FULLSCAN, NORECOMPUTE**.  
  
-   Panel Detalles del Explorador de objetos  
  
-   Template Explorer  
  
## <a name="unsupported-features-in-ssmanstudiofull"></a>Características no admitidas en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 Para los objetos OLTP en memoria, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] no admite las características y las operaciones que tampoco admite el motor de base de datos.  
  
 Para obtener más información sobre las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admitidas, vea [Características de SQL Server no admitidas para OLTP en memoria](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md).  
  
## <a name="see-also"></a>Consulte también  
 [Compatibilidad de SQL Server con OLTP en memoria](./transact-sql-support-for-in-memory-oltp.md)  
  

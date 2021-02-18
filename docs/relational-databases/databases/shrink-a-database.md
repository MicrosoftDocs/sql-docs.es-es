---
title: Reducir una base de datos | Microsoft Docs
description: Obtenga información sobre cómo reducir una base de datos con un objeto de SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkdatabase.f1
helpviewer_keywords:
- shrinking databases
- databases [SQL Server], shrinking
- decreasing database size
- database shrinking [SQL Server]
- reducing database size
ms.assetid: 83afbf74-fd50-4c39-831c-b1f473a50620
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9e2eced4a7ca9a8e8f443c709d65cc6e2906082a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489499"
---
# <a name="shrink-a-database"></a>Reducir una base de datos
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema, se describe cómo reducir una base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante objetos en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 La reducción de los archivos de datos permite recuperar espacio moviendo páginas de datos del final del archivo a espacio desocupado próximo al principio del archivo. Cuando se crea suficiente espacio libre al final del archivo, las páginas de datos situadas al final del archivo pueden desasignarse y devolverse al sistema de archivos.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para reducir una base de datos, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [reducir una base de datos](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El tamaño de la base de datos no puede ser menor que el tamaño mínimo de la base de datos. El tamaño mínimo es el tamaño especificado cuando se creó la base de datos o el último tamaño establecido explícitamente mediante una operación de modificación del tamaño del archivo, como DBCC SHRINKFILE. Por lo tanto, si se creó una base de datos con un tamaño de 10 MB y ha crecido hasta llegar a 100 MB, solo podrá reducirla hasta un tamaño de 10 MB, aunque se hayan eliminado todos los datos de la base de datos.  
  
-   No se puede reducir una base de datos mientras se está realizando una copia de seguridad de la misma. Asimismo, no se puede realizar una copia de seguridad de una base de datos mientras se está realizando una operación de reducción de ésta.
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Para ver la cantidad actual de espacio disponible (sin asignar) en la base de datos. Para obtener más información, consulte [Mostrar la información del espacio ocupado por los datos y el registro de una base de datos](../../relational-databases/databases/display-data-and-log-space-information-for-a-database.md)  
  
-   Tenga en cuenta la siguiente información cuando vaya a reducir una base de datos:  
  
    -   La reducción es más efectiva después de una operación que cree mucho espacio inutilizado, como por ejemplo una operación para truncar o eliminar tablas.  
  
    -   La mayoría de las bases de datos requieren que haya espacio disponible para realizar las operaciones diarias normales. Si se reduce una base de datos en forma reiterada y su tamaño vuelve a aumentar, esto indica que el espacio que se redujo es necesario para las operaciones normales. En estos casos, no sirve reducir la base de datos reiteradamente.  
  
    -   La reducción no mantiene el estado de fragmentación de los índices de la base de datos y generalmente aumenta la fragmentación hasta cierto punto. Esta es otra razón para no reducir la base de datos reiteradamente.  
  
    -   A menos que tenga un requisito específico, no establezca la opción de base de datos AUTO_SHRINK en ON.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Debe pertenecer al rol fijo de servidor **sysadmin** o al rol fijo de base de datos **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-shrink-a-database"></a>Para reducir una base de datos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]y, a continuación, expándala.  
  
2.  Expanda **Bases de datos** y, después, haga clic con el botón derecho en la base de datos que quiera reducir.  
  
3.  Seleccione **Tareas** y **Reducir**, y haga clic en **Base de datos**.  
  
     **Base de datos**  
     Muestra el nombre de la base de datos seleccionada.  
  
     **Espacio asignado actualmente**  
     Muestra el total de espacio utilizado y no utilizado de la base de datos seleccionada.  
  
     **Espacio disponible**  
     Muestra la suma del espacio disponible en los archivos de datos y de registro de la base de datos seleccionada.  
  
     **Reorganizar archivos antes de liberar espacio no utilizado**  
     Seleccionar esta opción equivale a ejecutar DBCC SHRINKDATABASE y especificar una opción de porcentaje de destino. Desactivar esta opción equivale a ejecutar DBCC SHRINKDATABASE con la opción TRUNCATEONLY. De forma predeterminada, esta opción no está activada cuando se abre el cuadro de diálogo. Si se selecciona se esta opción, el usuario debe especificar una opción de porcentaje de destino.  
  
     **Cantidad máxima de espacio disponible en los archivos tras la reducción**  
     Especifique el porcentaje máximo de espacio disponible que se va a dejar en los archivos de base de datos después de reducirla. Los valores válidos están comprendidos entre 0 y 99.  
  
4.  Haga clic en **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-shrink-a-database"></a>Para reducir una base de datos  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo usa [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md) para reducir el tamaño de los archivos de datos y de registro de la base de datos `UserDB` , y para dejar un `10` por ciento de espacio disponible en la base de datos.  
  
 [!code-sql[DBCC#DBCC_SHRINKDB1](../../relational-databases/databases/codesnippet/tsql/shrink-a-database_1.sql)]  
  
##  <a name="follow-up-after-you-shrink-a-database"></a><a name="FollowUp"></a> Seguimiento: después de reducir una base de datos  
 Los datos que se mueven para reducir un archivo se pueden dispersar en cualquier ubicación disponible en el archivo. Esto produce la fragmentación de índices y puede reducir el rendimiento de las consultas que buscan un intervalo del índice. Para eliminar la fragmentación, considere la posibilidad de volver a generar los índices en el archivo después de la reducción.  
  
## <a name="see-also"></a>Consulte también  
 [Reducir un archivo](../../relational-databases/databases/shrink-a-file.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)   
 [Archivos y grupos de archivos de base de datos](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  

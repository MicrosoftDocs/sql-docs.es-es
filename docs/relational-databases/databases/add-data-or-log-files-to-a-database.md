---
title: Agregar archivos de datos o de registro a una base de datos | Microsoft Docs
description: Obtenga información sobre cómo agregar datos o archivos de registro a una base de datos en SQL Server 2019 mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], files
- adding data files
- adding files
- adding log files
- file additions [SQL Server], steps
- files [SQL Server], adding
- data additions [SQL Server]
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: stevestein
ms.author: sstein
ms.openlocfilehash: e0e411e82a6492fda48cc8d3541dbd272075f994
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813139"
---
# <a name="add-data-or-log-files-to-a-database"></a>Agregar archivos de datos o de registro a una base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo agregar archivos de datos o de registro a una base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para agregar archivos de datos o de registro a una base de datos, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   No se puede agregar o quitar un archivo mientras se está ejecutando una instrucción BACKUP.  
  
-   Para cada base de datos pueden especificarse hasta 32.767 archivos y 32.767 grupos de archivos.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>Para agregar archivos de datos o de registro a una base de datos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, haga clic con el botón derecho en la base de datos de la que quiera agregar los archivos y, después, haga clic en **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades de la base de datos** , seleccione la página **Archivos** .  
  
4.  Para agregar un archivo de datos o de registro de transacciones, haga clic en **Agregar**.  
  
5.  En la cuadrícula **Archivos de la base de datos** , escriba un nombre lógico para el archivo. El nombre debe ser único en la base de datos.  
  
6.  Seleccione el tipo de archivo: de datos o de registro.  
  
7.  En el caso de un archivo de datos, seleccione el grupo de archivos en el que se debe incluir el archivo de la lista, o bien seleccione **\<new filegroup>** para crear un nuevo grupo de archivos. Los archivos de registro de transacciones no pueden formar parte de un grupo de archivos.  
  
8.  Especifique el tamaño inicial del archivo. Defina el mayor tamaño posible para los archivos de datos, según la cantidad de datos máxima prevista para la base datos.  
  
9. Para especificar cómo debe crecer el archivo, haga clic en ( **...** ) en la columna **Crecimiento automático**. Seleccione entre las siguientes opciones:  
  
    1.  Para permitir que el archivo actualmente seleccionado crezca cuando se necesite más espacio para los datos, active la casilla **Habilitar crecimiento automático** y, a continuación, elija una de las opciones siguientes:  
  
    2.  Para especificar que el archivo debe crecer en incrementos fijos, seleccione **En megabytes** y especifique un valor.  
  
    3.  Para especificar que el archivo debe crecer en un porcentaje de su tamaño actual, seleccione **En porcentaje** y especifique un valor.  
  
10. Para especificar el límite de tamaño máximo del archivo, seleccione una de estas opciones:  
  
    1.  Para especificar el tamaño máximo que se debe permitir que alcance el archivo, seleccione **Limitar el crecimiento de los archivos (MB)** y especifique un valor.  
  
    2.  Para que el archivo crezca tanto como sea necesario, seleccione **No limitar el crecimiento de los archivos**.  
  
    3.  Para evitar que el archivo crezca, desactive la casilla **Habilitar crecimiento automático** . El tamaño del archivo no superará el límite especificado en la columna **Tamaño inicial (MB)** .  
  
    > [!NOTE]  
    >  El tamaño máximo de una base de datos está determinado por la cantidad de espacio en disco disponible y los límites de licencia establecidos para la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se utiliza.  
  
11. Especifique la ruta de acceso a la ubicación del archivo. La ruta debe existir antes de agregar el archivo.  
  
    > [!NOTE]  
    >  De forma predeterminada, los datos y los registros de transacciones se colocan en la misma unidad y ruta de acceso para adecuarse a sistemas de un solo disco, pero puede que esto no resulte óptimo para los entornos de producción. Para más información, consulte [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).  
  
12. Haga clic en **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>Para agregar archivos de datos o de registro a una base de datos  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. El ejemplo agrega un grupo de archivos con dos archivos a una base de datos. En el siguiente ejemplo se crea el grupo de archivos `Test1FG1` en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y se agregan dos archivos de 5 MB al grupo de archivos.  
  
 [!code-sql[DatabaseDDL#AlterDatabase2](../../relational-databases/databases/codesnippet/tsql/add-data-or-log-files-to_1.sql)]  
  
 Para obtener más ejemplos, vea [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
## <a name="see-also"></a>Consulte también  
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [Eliminar archivos de datos o de registro de una base de datos](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)   
 [Aumentar el tamaño de una base de datos](../../relational-databases/databases/increase-the-size-of-a-database.md)  
  
  

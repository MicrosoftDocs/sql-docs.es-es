---
description: Actualizar la búsqueda de texto completo
title: Actualizar la búsqueda de texto completo | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], installing
- migrating full-text indexes [SQL Server]
- upgrading Full-Text Search
- installing Full-Text Search
- full-text search [SQL Server], upgrading
ms.assetid: 2fee4691-f2b5-472f-8ccc-fa625b654520
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4f000d04dee158502ee1623031c0a37936995862
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589271"
---
# <a name="upgrade-full-text-search"></a>Actualizar la búsqueda de texto completo

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

SQL Server actualiza la búsqueda de texto completo durante la instalación, o al adjuntar, restaurar o copiar archivos de base de datos y catálogos de texto completo desde una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

##  <a name="upgrade-a-server-instance"></a><a name="Upgrade_Server"></a> Actualizar una instancia de servidor  

Si se trata de una actualización en contexto, se instalan en paralelo una instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y la versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y se migran los datos. Si la versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenía instalada la función de búsqueda de texto completo, automáticamente se instala una nueva versión de dicha función. Para poder realizar la instalación en paralelo, es necesaria la presencia de los componentes siguientes en cada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Separadores de palabras, lematizadores y filtros  
 Ahora, cada instancia usa su propio conjunto de separadores de palabras, lematizadores y filtros en lugar de depender de la versión de estos componentes existente en el sistema operativo. También es más fácil registrar y configurar estos componentes en cada una de las instancias. Para obtener más información, vea [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) y [Configurar y administrar filtros para búsquedas](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
 Host de demonio de filtro  
 Los hosts de demonio de filtro de texto completo son procesos que cargan y controlan de forma segura los componentes extensibles externos que se usan en los índices y en las consultas, como por ejemplo, los separadores de palabras, los lematizadores y los filtros, sin poner en peligro la integridad del motor de búsqueda de texto completo. Una instancia del servidor utiliza un proceso multiproceso para todos los filtros multiproceso y un proceso de un solo subproceso para todos los filtros de un solo subproceso.  
  
> [!NOTE]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] introdujo una cuenta de servicio para el servicio del iniciador del FDHOST (MSSQLFDLauncher). Este servicio propaga la información de la cuenta de servicio a los procesos del host de demonio de filtro de una instancia concreta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información sobre cómo configurar la cuenta de servicio, vea [Establecer la cuenta del servicio para el selector del demonio de filtro completo](../../relational-databases/search/set-the-service-account-for-the-full-text-filter-daemon-launcher.md).  
  
 En [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], cada índice de texto completo reside en un catálogo de texto completo que pertenece a un grupo de archivos, tiene una ruta de acceso física y es tratado como un archivo de base de datos. En [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, un catálogo de texto completo es un concepto lógico o virtual que contiene un grupo de índices de texto completo. Por consiguiente, no se trata a cada nuevo catálogo de texto completo como un archivo de base de datos con una ruta de acceso física. Sin embargo, durante la actualización de cualquier catálogo de texto completo que contiene archivos de datos, se crea un nuevo grupo de archivos en el mismo disco. Esto conserva el comportamiento de E/S del disco antiguo después de la actualización. Si la ruta de acceso raíz existe, todos los índices de texto completo del catálogo se situarán en el nuevo grupo de archivos. Si la ruta de acceso al catálogo de texto completo anterior no es válida, la actualización mantiene el índice de texto completo en el mismo grupo de archivos que la tabla base o, si se trata de una tabla con particiones, en el grupo de archivos principal.  
  
## <a name="full-text-upgrade-options"></a><a name="FT_Upgrade_Options"></a> Opciones de actualización de texto completo  

Durante la actualización de una instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], la interfaz de usuario le permite elegir una de las siguientes opciones de actualización de texto completo.  
  
**Importar**  
 Se importan los catálogos de texto completo. Normalmente, el proceso de importación es significativamente más rápido que el de regeneración. Por ejemplo, si se usa solo una CPU, importar es aproximadamente 10 veces más rápido que volver a generar. Sin embargo, un catálogo de texto completo importado no usa los divisores de palabras nuevos instalados con la última versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para garantizar la coherencia de los resultados de consulta, los catálogos de texto completo se tienen que recompilar.  
  
> [!NOTE]  
>  La recompilación se puede ejecutar en modo de varios subprocesos; además, si hay más de 10 CPU disponibles y permite que el proceso de recompilación las use todas, dicho proceso puede resultar más rápido que el de importación.  
  
 Si un catálogo de texto completo no está disponible, se vuelven a generar los índices de texto completo asociados. Esta opción solo está disponible para bases de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
 Para obtener información sobre el impacto de importar un índice de texto completo, vea "Consideraciones sobre la elección de una opción de actualización de texto completo" más adelante en este tema.  
  
 **Recompilación**  
 Los catálogos de texto completo se vuelven a generar con los separadores de palabras nuevos y mejorados. El proceso de recompilación de los índices puede llevar cierto tiempo y podría ser necesaria una cantidad significativa de CPU y de memoria después de la actualización.  
  
 **Reset**  
 Los catálogos de texto completo se restablecen. Cuando se actualizan desde [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], los catálogos de texto completo se quitan, pero los metadatos de los catálogos de texto completo y los índices de texto completo se conservan. Después de actualizarse, todos los índices de texto completo quedan deshabilitados para el seguimiento de cambios y los rastreos no se inician de forma automática. El catálogo permanecerá vacío hasta que se emita manualmente un rellenado completo después de que se complete la actualización.  
  
##  <a name="considerations-for-choosing-a-full-text-upgrade-option"></a><a name="Choosing_Upgade_Option"></a> Consideraciones sobre la elección de una opción de actualización de texto completo  

Cuando elija la opción de actualización, tenga en cuenta lo siguiente:  
  
- ¿Necesita coherencia en los resultados de consulta?  
  
   [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] instala nuevos separadores de palabras para ser usados en la búsqueda de texto completo y la búsqueda semántica. Los separadores de palabras se usan en el momento de la indización y en el momento de la consulta. Si no recompila los catálogos de texto completo, los resultados de la búsqueda pueden ser incoherentes. Si emite una consulta de texto completo que busca una frase que el separador de palabras de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] separó de forma diferente que el separador de palabras actual, es posible que no se recupere un documento o una fila que contengan la frase. Esto se debe a que las frases indizadas se separaron mediante una lógica diferente de la que está usando la consulta. La solución es volver a rellenar (volver a generar) los catálogos de texto completo con los nuevos separadores de palabras de modo que los comportamientos en el momento de la indización y en el momento de la consulta sean idénticos. Puede elegir la opción Recompilar para realizar esta acción, o puede recompilar manualmente después de elegir la opción Importar.  
  
- ¿Se han generado índices de texto completo en columnas de clave de texto completo cuyo tipo de datos es Integer?  
  
   La regeneración realiza optimizaciones internas que, en algunos casos, mejoran el rendimiento de las consultas del índice de texto completo actualizado. En concreto, si tiene catálogos de texto completo que contienen índices de texto completo para los que la columna de clave de texto completo de la tabla base es un tipo de datos Integer, al volver a generarlos, se logra un rendimiento ideal de las consultas de texto completo después de la actualización. En este caso, es muy recomendable el uso de la opción **Volver a generar** .  
  
   > [!NOTE]  
   > Para los índices de texto completo, se recomienda que la columna que actúa como la clave de texto completo sea un tipo de dato entero. Para obtener más información, vea [Mejorar el rendimiento de los índices de texto completo](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md).  
  
- ¿Cuál es la prioridad para poner en línea la instancia del servidor?  
  
   El proceso de importación o regeneración durante la actualización consume muchos recursos de la CPU, lo que retrasa la actualización y puesta en línea del resto de la instancia del servidor. Si es importante poner en línea lo antes posible la instancia del servidor y desea ejecutar manualmente el rellenado después de la actualización, la opción **Restablecer** resulta adecuada.  
  
## <a name="ensure-consistent-query-results-after-importing-a-full-text-index"></a>Garantizar resultados de consulta coherentes después de importar un índice de texto completo  

Si un catálogo de texto completo se ha importado al actualizar una base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], podrían producirse discrepancias entre la consulta y el contenido del índice de texto completo debido a las diferencias de comportamiento entre los nuevos separadores de palabras y los antiguos. En este caso, para garantizar una coincidencia total entre las consultas y el contenido del índice de texto completo, elija una de las opciones siguientes:  
  
- Recompile el catálogo de texto completo que contiene el índice de texto completo ([ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)*catalog_name* REBUILD)  
  
- Emita un rellenado completo en el índice de texto completo ([ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) ON *table_name* START FULL POPULATION).  
  
 Para obtener más información sobre los separadores de palabras, vea [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
## <a name="upgrade-noise-word-files-to-stoplists"></a>Actualizar archivos de palabras irrelevantes a listas de palabras irrelevantes

Cuando una base de datos se actualiza desde [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], los archivos de palabras irrelevantes de esa versión dejan de usarse. Sin embargo, los archivos de palabras irrelevantes antiguos se almacenan en la carpeta `FTDATA\ FTNoiseThesaurusBak` y puede usarlos posteriormente cuando actualice o genere las correspondientes listas de palabras irrelevantes de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
Después de la actualización a partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]:  
  
- Si nunca agregó, modificó ni eliminó archivos de palabras irrelevantes de la instalación de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], la lista de palabras irrelevantes del sistema será suficiente para cubrir sus necesidades.  
  
- Si modificó los archivos de palabras irrelevantes en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], las modificaciones se perderán durante la actualización. Para volver a crear las modificaciones, debe hacerlo de forma manual en la lista de palabras irrelevantes correspondiente. Para obtener más información, vea [ALTER FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md).  
  
- Si no desea aplicar palabras irrelevantes a sus índices de texto completo (por ejemplo, si eliminó o borró los archivos de palabras irrelevantes de la instalación de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]), debe desactivar la lista de palabras irrelevantes para todos los índices de texto completo actualizados. Ejecute la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] (reemplace *database* por el nombre de la base de datos actualizada y *table* por el nombre de la *tabla*):  
  
    ```  
    Use database;   
    ALTER FULLTEXT INDEX ON table  
       SET STOPLIST OFF;  
    GO  
    ```  
  
     La cláusula STOPLIST OFF quita el filtrado de palabras irrelevantes y desencadena un rellenado de la tabla sin filtrar las palabras consideradas como irrelevantes.  
  
## <a name="backup-and-imported-full-text-catalogs"></a>Copia de seguridad y catálogos de texto completo importados

En los catálogos de texto completo que se vuelven a generar o que se restablecen durante la actualización (y para los nuevos catálogos de texto completo), el catálogo de texto completo es un concepto lógico y no reside en ningún grupo de archivos. Por consiguiente, para hacer una copia de seguridad de un catálogo de texto completo, debe identificar cada grupo de archivos que contenga un índice de texto completo del catálogo y hacer una copia de seguridad de cada uno de ellos, uno por uno. Para obtener más información, vea [Realizar copias de seguridad de los catálogos de texto completo y restaurarlos](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md).  
  
 Para los catálogos de texto completo importados de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], el catálogo de texto completo sigue siendo un archivo de base de datos en su propio grupo de archivos. Se sigue aplicando el proceso de copia de seguridad de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] para los catálogos de texto completo, con la salvedad de que el servicio MSFTESQL no existe en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Para obtener información sobre el proceso de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , vea [Backing Up and Restoring Full-Text Catalogs (Copia de seguridad y restauración de catálogos de texto completo)](/previous-versions/sql/sql-server-2005/ms142511(v=sql.90)) en los Libros en pantalla de SQL Server 2005.  
  
##  <a name="migrating-full-text-indexes-when-upgrading-a-database"></a><a name="Upgrade_Db"></a> Migración de índices de texto completo al actualizar una base de datos

 Los archivos de base de datos y los catálogos de texto completo de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se pueden actualizar a una instancia existente mediante operaciones de anexión o restauración, o con el Asistente para copiar bases de datos. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] los índices de texto completo, si los hubiera, se importan, se restablecen o se vuelven a compilar. La propiedad de servidor **upgrade_option** controla cuál de las opciones de actualización de texto completo usa la instancia del servidor durante estas actualizaciones de bases de datos.  
  
 Después de adjuntar una base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a una instancia más reciente o de restaurarla o copiarla ahí, queda disponible inmediatamente y se actualiza de forma automática. Dependiendo de la cantidad de datos que se indicen, la importación puede requerir varias horas y volver a generar puede requerir hasta diez veces más. Tenga en cuenta también que si la opción de actualización se establece en importar y no hay disponible ningún catálogo de texto completo, se vuelven a generar los índices de texto completo asociados.  
  
 **Para cambiar el comportamiento de la actualización de texto completo en una instancia del servidor**
  
- [!INCLUDE[tsql](../../includes/tsql-md.md)]: Use la acción **upgrade\_option** de [sp\_fulltext\_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
  
- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **:** Use la **Opción de actualización de texto completo** del cuadro de diálogo **Propiedades del servidor** . Para obtener más información, vea [Administrar y supervisar la búsqueda de texto completo para una instancia de servidor](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).  
  
##  <a name="considerations-for-restoring-a-ssversion2005-full-text-catalog"></a><a name="Considerations_for_Restore"></a> Consideraciones sobre la restauración de un catálogo de texto completo de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]
  
Una forma de actualizar los datos de texto completo de una base de datos desde [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] consiste en restaurar una copia de seguridad completa de la base de datos en una instancia más reciente de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
 Al importar un catálogo de texto completo de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , puede hacer una copia de seguridad y restaurar la base de datos y el archivo de catálogo. El comportamiento es el mismo que en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
- La copia de seguridad completa de la base de datos incluirá el catálogo de texto completo. Para hacer referencia al catálogo de texto completo, use su nombre de archivo de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , sysft_+*catalog-name*.  
  
- Si el catálogo de texto completo está sin conexión, se producirá un error en la copia de seguridad.  
  
 Para obtener más información sobre las copias de seguridad y la restauración de catálogos de texto completo de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , vea [Backing Up and Restoring Full-Text Catalogs (Copia de seguridad y restauración de catálogos de texto completo)](./back-up-and-restore-full-text-catalogs-and-indexes.md) y [File Backup and Restore and Full-Text Catalogs (Copia de seguridad y restauración de archivos y catálogos de texto completo)](/previous-versions/sql/sql-server-2008-r2/ms190643(v=sql.105))en los Libros en pantalla de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
 Cuando la base de datos se restaura en una instancia más reciente de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], se crea un archivo de base de datos para el catálogo de texto completo. El nombre predeterminado de este archivo es ftrow_ *catalog-name*.ndf. Por ejemplo, si *catalog-name* es `cat1`, el nombre predeterminado del archivo de base de datos de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] será `ftrow_cat1.ndf`. Pero si el nombre predeterminado ya se usa en el directorio de destino, el nuevo archivo de base de datos se denominará `ftrow_`*catalog-name*`{`*GUID*`}.ndf`, donde *GUID* es el identificador único global del nuevo archivo.  
  
 Después de que se hayan importado los catálogos, los elementos **sys.database_files** y **sys.master_file** se actualizan para eliminar las entradas de catálogo, y la columna **path** de **sys.fulltext_catalogs** se establece en NULL.  
  
 **Para realizar una copia de seguridad de una base de datos**  
  
- [Copias de seguridad completas de bases de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)  
  
- [Copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md) (solo para el modelo de recuperación completa)  
  
 **Para restaurar una copia de seguridad de base de datos**  
  
- [Restauraciones de base de datos completas &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
  
- [Restauraciones de base de datos completas &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)  
  
### <a name="example"></a>Ejemplo

 En el ejemplo siguiente se usa la cláusula MOVE en la instrucción [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) para restaurar una base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] denominada `ftdb1`. La base de datos, el registro y los archivos de catálogo de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] se mueven a las nuevas ubicaciones de la instancia del servidor de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de la manera siguiente:  
  
- El archivo de base de datos, `ftdb1.mdf`, se mueve a `C:\Program Files\Microsoft SQL Server\MSSQL.1MSSQL13.MSSQLSERVER\MSSQL\DATA\ftdb1.mdf`.  
  
- El archivo de registro, `ftdb1_log.ldf`, se mueve a un directorio de registro de la unidad de disco de registro, *log_drive*`:\`*log_directory*`\ftdb1_log.ldf`.  
  
- Los archivos de catálogo que corresponden al catálogo de `sysft_cat90` se mueven a `C:\temp`. Una vez importados los índices de texto completo, estos se sitúan automáticamente en un archivo de base de datos, C:\ftrow_sysft_cat90.ndf, y se eliminan de C:\temp.  
  
```  
RESTORE DATABASE [ftdb1] FROM  DISK = N'C:\temp\ftdb1.bak' WITH  FILE = 1,  
   MOVE N'ftdb1' TO N'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA\ftdb1.mdf',  
    MOVE N'ftdb1_log' TO N'log_drive:\log_directory\ftdb1_log.ldf',  
    MOVE N'sysft_cat90' TO N'C:\temp';  
```  
  
##  <a name="attaching-a-sql-server-2005-database"></a><a name="Attaching_2005_ft_catalogs"></a> Anexión de una base de datos de SQL Server 2005

En [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, un catálogo de texto completo es un concepto lógico que hace referencia a un grupo de índices de texto completo. Un catálogo de texto completo es un objeto virtual y no pertenece a ningún grupo de archivos. Sin embargo, al adjuntar una base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] que contiene archivos de catálogo de texto completo a una instancia de servidor de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] más reciente, los archivos de catálogo se adjuntan desde su ubicación anterior junto con los demás archivos de base de datos, igual que en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
El estado de cada catálogo de texto completo adjunto de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] es el mismo que el que tenía la base de datos cuando se separó de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Si, durante la operación de separación, se suspendió cualquier tarea de rellenado de un índice de texto completo, ésta se reanudará en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], por lo que será posible realizar búsquedas de texto completo en dicho índice de texto completo.  
  
 Si [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] no puede localizar un archivo de catálogo de texto completo o si el archivo de texto completo se mueve durante la operación de separación sin especificar una nueva ubicación, el comportamiento dependerá de la opción de actualización de texto completo seleccionada. Si la opción de actualización de texto completo es **Importar** o **Volver a generar**, se vuelve a generar el catálogo de texto completo adjunto. Si la opción de actualización de texto completo es **Restablecer**, se restablece el catálogo de texto completo adjunto.  
  
 Para obtener más información sobre cómo separar y adjuntar una base de datos, vea [Adjuntar y separar bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md), [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md), [sp_attach_db](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md) y [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md).  
  
## <a name="see-also"></a>Vea también

 [Introducción a la búsqueda de texto completo](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurar y administrar filtros para búsquedas](../../relational-databases/search/configure-and-manage-filters-for-search.md)  

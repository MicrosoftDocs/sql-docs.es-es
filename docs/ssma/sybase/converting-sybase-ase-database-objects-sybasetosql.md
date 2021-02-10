---
description: Conversión de objetos de base de datos de Sybase ASE (SybaseToSQL)
title: Conversión de objetos de base de datos de Sybase ASE (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Converting Database Objects
ms.assetid: 509cb65d-2f54-427a-83d7-37919cc4e3e3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07560faaa3fb3316864216859f79cd5e1b7ff950
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056840"
---
# <a name="converting-sap-ase-database-objects-sybasetosql"></a>Conversión de objetos de base de datos de SAP ASE (SybaseToSQL)
Después de conectarse a SAP Adaptive Server Enterprise (ASE), conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a Azure SQL y establecer las opciones de asignación de datos y de proyecto, puede convertir objetos de base de datos de SAP Adaptive Server Enterprise (ASE) en objetos de base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL de Azure.  
  
## <a name="the-conversion-process"></a>El proceso de conversión  
La conversión de objetos de base de datos toma las definiciones de objetos de ASE, las convierte en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos similares o de SQL Azure y, a continuación, carga esta información en los metadatos de SSMA. No carga la información en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL. A continuación, puede ver los objetos y sus propiedades mediante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el explorador de metadatos de Azure SQL.
  
Durante la conversión, SSMA imprime los mensajes de salida en el panel de salida y los mensajes de error en el panel de **lista de errores** . Utilice la información de salida y de error para determinar si tiene que modificar las bases de datos de ASE o el proceso de conversión para obtener los resultados de la conversión deseada.  
  
## <a name="setting-conversion-options"></a>Establecer opciones de conversión  
Antes de convertir objetos, revise las opciones de conversión de proyectos en el cuadro de diálogo **configuración del proyecto** . Mediante este cuadro de diálogo, puede establecer cómo SSMA convierte las funciones y las variables globales. Para obtener más información, vea [configuración del proyecto &#40;conversión&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md).  
  
## <a name="converting-ase-database-objects"></a>Conversión de objetos de base de datos ASE  
Para convertir objetos de base de datos de ASE, seleccione primero los objetos que desea convertir y, a continuación, haga que SSMA realice la conversión. Para ver los mensajes de salida durante la conversión, en el menú **Ver** , seleccione **salida**.  
  
**Para convertir objetos ASE en SQL Server o SQL Azure sintaxis**  
  
1.  En Sybase Metadata Explorer, expanda el servidor ASE y, a continuación, expanda **bases** de datos.  
  
2.  Seleccionar los objetos que se van a convertir:  
  
    -   Para convertir todas las bases de datos, active la casilla situada junto a **bases de datos**.  
  
    -   Para convertir u omitir una base de datos, Active o desactive la casilla situada junto al nombre de la base de datos.  
  
    -   Para convertir u omitir esquemas individuales, expanda la base de datos, expanda **esquemas** y, a continuación, Active o desactive la casilla situada junto al esquema.  
  
    -   Para convertir u omitir una categoría de objetos, expanda el esquema y, a continuación, Active o desactive la casilla situada junto a la categoría.  
  
    -   Para convertir u omitir objetos individuales, expanda la carpeta categoría y, a continuación, Active o desactive la casilla situada junto al objeto.  
  
3.  Para convertir todos los objetos seleccionados, haga clic con el botón derecho en **bases de datos** y, a continuación, seleccione **convertir esquema**.  
  
    También puede convertir objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en la carpeta que lo contiene y, a continuación, seleccione **convertir esquema**.  
  
> [!NOTE]  
> Algunas de las funciones del sistema de SAP ASE no coinciden exactamente con las funciones del sistema SQL Server equivalentes en comportamiento. Para emular el comportamiento de SAP ASE, SSMA genera funciones definidas por el usuario en la base de datos de SQL Server convertida en un esquema denominado 2SS '. Dependiendo de la configuración del proyecto, algunas de las funciones del sistema SQL Server se reemplazan por estas funciones emuladas. SSMA crea las siguientes funciones definidas por el usuario:  

:::row:::
    :::column:::
        **char_length_nvarchar**  
        **char_length_varchar**  
        **charindex_nvarchar**  
        **charindex_varchar**  
        **hextoint**  
        **index_colorder**  
    :::column-end:::
    :::column:::
        **inttohex**  
        **ssma_current_time**  
        **ssma_datediff**  
        **ssma_datepart**  
        **substring_nvarchar**  
        **substring_varbinary**  
    :::column-end:::
    :::column:::
        **substring_varchar**  
        **to_unichar**  
        **uhighsurr**  
        **ulowsurr**  
    :::column-end:::
:::row-end:::

## <a name="objects-not-supported-in-azure-sql"></a>Objetos no admitidos en Azure SQL  
Las siguientes palabras clave de T-SQL las usa SSMA para SAP ASE durante la conversión a SQL Server local, pero estas palabras clave no se admiten en SQL Azure sintaxis de T-SQL:  

:::row:::
    :::column:::
        CHECKPOINT  
        CREATE/ALTER/DROP DEFAULT  
        CREATE/DROP RULE  
        DBCC TRACEOFF  
        DBCC TRACEON  
    :::column-end:::
    :::column:::
        GRANT/REVOKE/DENY ALL  
        KILL  
        READTEXT  
        SELECT INTO  
        SET OFFSETS  
    :::column-end:::
    :::column:::
        SETUSER  
        SHUTDOWN  
        WRITETEXT  
    :::column-end:::
:::row-end:::

## <a name="viewing-conversion-problems"></a>Ver problemas de conversión  
Es posible que algunos objetos de SAP ASE no se conviertan. Puede determinar las tasas de éxito de la conversión viendo el informe de conversión de resumen.  
  
**Para ver un informe de Resumen**  
  
1.  En el explorador de metadatos de Sybase, seleccione **bases** de datos.  
  
2.  En el panel derecho, seleccione la pestaña **Informe** .  
  
    Este informe muestra el informe de evaluación de resumen para todos los objetos de base de datos que se han evaluado o convertido. También puede ver un informe de resumen para objetos individuales:  
  
    -   Para ver el informe de una base de datos individual, seleccione la base de datos en el explorador de metadatos de Sybase.  
  
    -   Para ver el informe de un objeto de base de datos individual, seleccione el objeto en el explorador de metadatos de Sybase. Los objetos que tienen problemas de conversión tienen un icono de error rojo.  
  
En el caso de los objetos que no se pudieron convertir, puede ver la sintaxis que produjo el error de conversión.  
  
**Para ver los problemas de conversión individuales**  
  
1.  En el explorador de metadatos de Sybase, expanda **bases** de datos.  
  
2.  Expanda la base de datos que muestra un icono de error rojo.  
  
3.  Expanda la carpeta **esquemas** y, a continuación, expanda el esquema que muestra un icono de error rojo.  
  
4.  En el esquema, expanda una carpeta que tenga un icono de error rojo.  
  
5.  Seleccione el objeto que tiene un icono de error rojo.  
  
6.  En el panel derecho, seleccione la pestaña **Informe** .  
  
7.  En la parte superior de la pestaña **Informe** está una lista desplegable. Si la lista muestra **estadísticas**, cambie la selección a **origen**.  
  
    SSMA mostrará el código fuente y varios botones justo encima del código.  
  
8.  Seleccione **siguiente problema**, un icono de error rojo con una flecha que apunta a la derecha.  
  
    SSMA para SAP ASE resaltará el primer código fuente problemático que encuentre en el objeto actual.  
  
Para cada elemento que no se pueda convertir, debe determinar qué desea hacer con ese objeto:  
  
-   Puede editar el código fuente de los procedimientos y los desencadenadores en la pestaña **SQL** .  
  
-   Puede modificar el objeto de SAP ASE para quitar o revisar el código problemático. Para cargar el código actualizado en SSMA, debe actualizar los metadatos. Para obtener más información, consulte [conexión a SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).  
  
-   Puede excluir el objeto de la migración. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el explorador de metadatos de Azure SQL y el explorador de metadatos de Sybase, desactive la casilla situada junto al elemento antes de cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL y migrar datos desde el ase de SAP.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso del proceso de migración consiste en [cargar los objetos de base de datos convertidos en SQL Server/SQL Azure (SybaseToSQL)](./loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de SAP ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  

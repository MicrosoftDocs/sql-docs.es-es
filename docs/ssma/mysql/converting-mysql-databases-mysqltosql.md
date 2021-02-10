---
title: Conversión de bases de datos MySQL (MySQLToSQL) | Microsoft Docs
description: Obtenga información sobre cómo convertir objetos de base de datos MySQL en SQL Server o Azure SQL Database objetos con SSMA, después de conectar y establecer las opciones de asignación de datos y proyectos.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ac21850b-fb32-4704-9985-5759b7c688c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61298bb7e2c65e85959b52ab3750dc49a5105504
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069021"
---
# <a name="converting-mysql-databases-mysqltosql"></a>Conversión de bases de datos de MySQL (MySQLToSQL)
Después de conectarse a MySQL, conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure y establecer las opciones de asignación de datos y de proyecto, puede convertir objetos de base de datos MySQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos o Azure SQL Database.  
  
## <a name="the-conversion-process"></a>Proceso de conversión  
La conversión de objetos de base de datos toma las definiciones de objeto de MySQL, las convierte en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos similares o de SQL Azure y, a continuación, carga esta información en los metadatos de SSMA. No carga la información en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . A continuación, puede ver los objetos y sus propiedades mediante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos.  
  
Durante la conversión, SSMA imprime los mensajes de salida en el panel de salida y los mensajes de error en el panel de Lista de errores. Utilice la información de salida y de error para determinar si tiene que modificar las bases de datos de MySQL o el proceso de conversión para obtener los resultados de la conversión deseada.  
  
## <a name="setting-conversion-options"></a>Establecer opciones de conversión  
Antes de convertir objetos, revise las opciones de conversión de proyectos en el cuadro de diálogo **configuración del proyecto** . Mediante este cuadro de diálogo, puede establecer cómo SSMA convierte las tablas y los índices. Para obtener más información, vea [configuración del proyecto &#40;conversión&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
## <a name="conversion-results"></a>Resultados de la conversión  
En la tabla siguiente se muestra qué objetos de MySQL se convierten y los objetos resultantes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objetos MySQL|Objetos SQL Server resultantes|  
|-|-|  
|Tablas con objetos dependientes como índices|SSMA crea tablas con objetos dependientes. La tabla se convierte con todos los índices y restricciones. Los índices se convierten en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos independientes.<br /><br />La **asignación de tipos de datos espaciales** solo se puede realizar en el nivel de nodo de tabla.<br /><br />Para obtener más información sobre la configuración de la conversión de tablas, vea configuración de la [conversión](conversion-settings-mysqltosql.md)|  
|Functions|Si la función se puede convertir directamente a Transact-SQL, SSMA crea una función. En algunos casos, la función se debe convertir en un procedimiento almacenado. Esto puede hacerse mediante la **conversión de funciones** en la configuración del proyecto. En este caso, SSMA crea un procedimiento almacenado y una función que llama al procedimiento almacenado.<br /><br />**Opciones proporcionadas:**<br /><br />Convertir según la configuración del proyecto<br /><br />Convertir a función<br /><br />Convertir en procedimiento almacenado<br /><br />Para obtener más información sobre la configuración de la conversión de funciones, vea configuración de la [conversión](conversion-settings-mysqltosql.md)|  
|Procedimientos|Si el procedimiento se puede convertir directamente a Transact-SQL, SSMA crea un procedimiento almacenado. En algunos casos, se debe llamar a un procedimiento almacenado en una transacción autónoma. En este caso, SSMA crea dos procedimientos almacenados: uno que implementa el procedimiento y otro que se utiliza para llamar al procedimiento almacenado de implementación.|  
|Conversión de base de datos|Las bases de datos como objetos MySQL no se convierten directamente en SSMA para MySQL. Las bases de datos MySQL se tratan más como los nombres de esquema y todos los parámetros físicos se pierden durante la conversión. SSMA para MySQL usa la asignación de bases de datos de [MySQL a esquemas de SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md) para asignar objetos de la base de datos MySQL a un par de base de datos o esquema de SQL Server adecuado.|  
|Conversión de desencadenador|**SSMA crea desencadenadores basados en las siguientes reglas:**<br /><br />ANTES de que los desencadenadores se conviertan en desencadenadores INSTEAD OF T-SQL<br /><br />Los desencadenadores AFTER se convierten en después de los desencadenadores T-SQL con o sin iteraciones por filas.|  
|Ver conversión|SSMA crea vistas con objetos dependientes|  
|Conversión de instrucciones|-Cada objeto de instrucción SQL puede contener una sola instrucción de MySQL (como DDL, DML y otros tipos de instrucciones) o BEGIN... Bloque final.<br />-   **Conversión de múltiples instrucciones: Begin... END Block Conversion** la instrucción SQL también puede contener una instrucción BEGIN... END block como uno en el procedimiento, la función o la definición del desencadenador. Esos bloques deben convertirse de la misma manera que se convierten para los objetos de instrucción de MySQL únicos.|  
  
## <a name="converting-mysql-database-objects"></a>Conversión de objetos de base de datos MySQL  
Para convertir objetos de base de datos MySQL, primero debe seleccionar los objetos que desea convertir y, a continuación, usar SSMA para realizar la conversión. Para ver los mensajes de salida durante la conversión, en el menú **Ver** , seleccione **salida**.  
  
**Para convertir objetos de MySQL en SQL Server o SQL Azure sintaxis**  
  
1.  En el explorador de metadatos de MySQL, expanda el servidor MySQL y, a continuación, expanda **bases** de datos.  
  
2.  Seleccionar los objetos que se van a convertir:  
  
    -   Para convertir todos los esquemas, active la casilla situada junto a **bases de datos**.  
  
    -   Para convertir u omitir una base de datos, active la casilla situada junto al nombre de la base de datos.  
  
    -   Para convertir u omitir una categoría de objetos, expanda un esquema y, a continuación, Active o desactive la casilla situada junto a la categoría.  
  
    -   Para convertir u omitir objetos individuales, expanda la carpeta categoría y, a continuación, Active o desactive la casilla situada junto al objeto.  
  
3.  Para convertir todos los objetos seleccionados, haga clic con el botón derecho en **bases de datos** y seleccione **convertir esquema**.  
  
    También puede convertir objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta primaria y, a continuación, seleccione **convertir esquema**.  
  
## <a name="viewing-conversion-problems"></a>Ver problemas de conversión  
Es posible que algunos objetos de MySQL no se conviertan. Puede determinar las tasas de éxito de la conversión viendo el informe de conversión de resumen.  
  
**Para ver un informe de Resumen**  
  
1.  En el explorador de metadatos de MySQL, seleccione **bases** de datos.  
  
2.  En el panel derecho, seleccione la pestaña **Informe** .  
  
    Este informe muestra el informe de evaluación de resumen para todos los objetos de base de datos que se han evaluado o convertido. También puede ver un informe de resumen para objetos individuales:  
  
    -   Para ver el informe de un esquema individual, seleccione la base de datos en el explorador de metadatos de MySQL.  
  
    -   Para ver el informe de un objeto individual, seleccione el objeto en el explorador de metadatos de MySQL. Los objetos que tienen problemas de conversión tienen un icono de error rojo.  
  
En el caso de los objetos que no se pudieron convertir, puede ver la sintaxis que produjo el error de conversión.  
  
**Para ver los problemas de conversión individuales**  
  
1.  En el explorador de metadatos de MySQL, expanda **bases** de datos.  
  
2.  Expanda la base de datos que muestra un icono de error rojo.  
  
3.  En la base de datos, expanda una carpeta que tenga un icono de error rojo.  
  
4.  Seleccione el objeto que tiene un icono de error rojo.  
  
5.  En el panel derecho, haga clic en la pestaña **Informe** .  
  
6.  En la parte superior de la pestaña **Informe** está una lista desplegable. Si la lista muestra **estadísticas**, cambie la selección a **origen**.  
  
    SSMA mostrará el código fuente y varios botones justo encima del código.  
  
7.  Haga clic en el botón **siguiente problema** . Se trata de un icono de error rojo con una flecha que apunta a la derecha.  
  
    SSMA resaltará el primer código fuente problemático que encuentre en el objeto actual.  
  
Para cada elemento que no se pueda convertir, debe determinar qué desea hacer con ese objeto:  
  
-   Puede modificar el objeto en la base de datos MySQL para quitar o revisar el código problemático. Para cargar el código actualizado en SSMA, tendrá que actualizar los metadatos. Para obtener más información, consulte [conexión a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
-   Puede excluir el objeto de la migración. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure el explorador de metadatos y el explorador de metadatos de MySQL, desactive la casilla situada junto al elemento antes de cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure y migrar datos desde MySQL.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [cargar los objetos de base de datos convertidos en SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  

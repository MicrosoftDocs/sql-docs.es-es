---
title: Características de acceso incompatibles (AccessToSQL) | Microsoft Docs
description: Obtenga información acerca de posibles problemas de migración con características de base de datos de Access que no son compatibles con SQL Server y cómo abordarlas.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, features incompatible with SQL Azure
- Access databases, features incompatible with SQL Server
- dates
- default expressions
- foreign keys
- hyperlink columns
- incompatible Access features
- indexes
- indexes, length of
- keywords
- primary keys
- reference, incompatible features
- replication columns
- special characters
- unique indexes
- validation rules
ms.assetid: 99d45b9c-e3b9-4d56-8c25-b594b887ace1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 492add4bd73d81c8c25256e35ff1b3de37fc13de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044545"
---
# <a name="incompatible-access-features-accesstosql"></a>Características de acceso incompatibles (AccessToSQL)
No todas las características de base de datos de Access son compatibles con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Por ejemplo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Access tienen diferentes conjuntos de palabras clave reservadas. Los problemas como estos pueden impedir que la migración se realice correctamente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Use la tabla siguiente para obtener información acerca de posibles problemas de migración y lo que puede hacer con ellos.  
  
## <a name="database-settings-or-features-that-might-affect-migration"></a>Configuración de base de datos o características que pueden afectar a la migración  
  
|Característica o configuración de base de datos de Access|Problema de migración|  
|--------------------------------------|-------------------|  
|Las tablas de acceso no tienen índices únicos.|Si una tabla que no tiene un índice único se migra a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no se puede modificar la tabla después de la migración. Esto puede dar lugar a problemas de compatibilidad de aplicaciones.<br /><br />Cuando se convierten objetos de base de datos de Access, la ventana de salida muestra las tablas de acceso que no tienen índices únicos.<br /><br />Puede configurar el acceso para agregar una clave principal en la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla durante la conversión. Para obtener más información, vea [configuración del proyecto (conversión)](./project-settings-conversion-accesstosql.md).|  
|Las tablas de Access tienen columnas de replicación.|Si una tabla de Access que incluye columnas del sistema de replicación se migra a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la funcionalidad de replicación de jet se interrumpirá después de la migración.<br /><br />Después de la migración, considere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la posibilidad de usar la replicación para mantener copias sincronizadas de las bases de datos.|  
|Las tablas de acceso que tienen índices únicos contienen varios valores NULL.|Las tablas de acceso que tienen índices únicos con varios valores NULL no se pueden transferir a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , porque en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los índices únicos no permiten varios valores NULL. Se producirá un error de migración en estas tablas.<br /><br />SSMA marcará este problema en los informes de evaluación. Para crear un informe de evaluación, consulte [evaluación de objetos de base de datos de Access para la conversión](assessing-access-database-objects-for-conversion-accesstosql.md).<br /><br />Si existe este problema, debe asegurarse de que la clave principal no tiene valores NULL duplicados. O bien, debe quitar la clave principal o los índices únicos que contienen varios valores NULL.|  
|Las tablas de acceso contienen valores de fecha que están fuera del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intervalo.|El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo **DateTime** acepta fechas en el intervalo de 1 ene 1753 a 31 Dec 9999 solamente. El acceso acepta fechas en el intervalo de 1 de enero de 100 a 31 de diciembre de 9999.<br /><br />SSMA marcará este problema en los informes de evaluación. Para crear un informe de evaluación, consulte [evaluación de objetos de base de datos de Access para la conversión](assessing-access-database-objects-for-conversion-accesstosql.md).<br /><br />Puede configurar cómo SSMA resuelve las fechas que están fuera del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intervalo. Para obtener más información, vea [configuración del proyecto (migración)](./project-settings-migration-accesstosql.md).|  
|Las longitudes de índice en Access superan los 900 bytes.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los índices tienen un límite de 900 bytes para el tamaño total de las columnas de clave de índice. Si las tablas de Access usan índices mayores, SSMA mostrará una advertencia.<br /><br />Si continúa con la migración de datos, es posible que se produzca un error en la migración.|  
|Los nombres de objeto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de Access son palabras clave o contienen caracteres especiales.|El acceso y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen diferentes conjuntos de palabras clave reservadas y caracteres especiales. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aceptará objetos con nombre mediante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] palabras clave o que contengan caracteres especiales si usa identificadores entre corchetes o entre comillas, como "Select" o [Select]. p. Para obtener más información, vea "identificadores delimitados (Motor de base de datos)" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.<br /><br />**Nota:** Para usar comillas para delimitar los identificadores, establezca QUOTED_IDENTIFIER debe ser ON.<br /><br />Por ejemplo, `CREATE TABLE [schema](c1 [FOR])` es una instrucción válida, aunque **Schema** y **for** son palabras clave reservadas. Además, `CREATE TABLE [xxx*yyy](c1 x&y)` es una instrucción válida, aunque el nombre de la tabla y la columna contienen los caracteres especiales **\& #42;** y **&amp;** .<br /><br />Todas las consultas que hacen referencia a esos objetos también deben usar los nombres entre corchetes o comillas. Por ejemplo, `SELECT * FROM schema` se producirá un error en la consulta. La consulta correcta es: `SELECT * FROM [schema]` .<br /><br />Cuando se conviertan objetos de base de datos de Access, el panel de resultados mostrará todas las tablas de acceso que utilicen palabras clave o caracteres especiales. Puede modificar las tablas en Access y, a continuación, quitar y volver a agregar la base de datos; o bien, puede modificar las consultas que hacen referencia a esos objetos para que las consultas utilicen corchetes o comillas para delimitar los identificadores. Si no modifica las consultas, es posible que las aplicaciones de Access devuelvan errores u otros problemas.|  
|Los tamaños de campo se diferencian en las relaciones de clave principal y clave externa.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admite la funcionalidad jet de vincular columnas que tienen diferentes tipos de datos o tamaños con restricciones de clave externa.<br /><br />Al convertir los objetos de base de datos de Access, la ventana de salida mostrará las restricciones de clave principal y clave externa que no se convertirán en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede modificar los tipos de datos y los tamaños de las columnas de acceso para que coincidan y, a continuación, quitar y volver a agregar la base de datos de Access. O bien, puede migrar los datos, aunque estas restricciones no se creen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|Las tablas a las que se hace referencia en las relaciones de acceso no tienen una clave principal ni un índice único.|El acceso acepta la relación entre las tablas en las que la tabla a la que se hace referencia no tiene una clave principal o un índice único. Sin embargo, no es compatible con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Cuando se convierten objetos de base de datos de Access, la ventana de salida muestra las tablas que tienen relaciones, pero no una clave principal o un índice único. Puede modificar las tablas para agregar claves principales o índices únicos y, a continuación, quitar y volver a agregar la base de datos de Access. O bien, puede migrar los datos, aunque se interrumpirá la relación entre las tablas.|  
|Las tablas de Access tienen columnas de hipervínculo.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admite columnas de **hipervínculo** . En su lugar, las columnas se tratan como las columnas de memorando de acceso. De forma predeterminada, estas columnas se convertirán en columnas **nvarchar (Max)** en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede personalizar la asignación. Para obtener más información, vea [asignar tipos de datos de origen y de destino](mapping-source-and-target-data-types-accesstosql.md).|  
|Las expresiones de regla de validación o predeterminada contienen funciones de acceso que no se pueden convertir en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.|El acceso a las expresiones predeterminadas o a las reglas de validación puede incluir funciones del sistema de acceso o funciones definidas por el usuario que no se asignan [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ni SQL Azure. El uso de funciones que no se asignan a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ni SQL Azure impedirá cargar las expresiones predeterminadas o las reglas de validación en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.|  
  
## <a name="see-also"></a>Consulte también  
[Preparar las bases de datos de Access para la migración](preparing-access-databases-for-migration-accesstosql.md)  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  

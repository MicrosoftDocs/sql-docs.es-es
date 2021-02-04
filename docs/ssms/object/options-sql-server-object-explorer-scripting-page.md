---
description: Opciones (Explorador de objetos de SQL Server - página Script)
title: Opciones (Explorador de objetos de SQL Server - página Script)
ms.custom: seo-lt-2019
ms.date: 08/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.ObjectExplorerScripting
- VS.ToolsOptionsPages.Sql_Server_Object_Explorer.ObjectExplorerScripting
ms.assetid: 6105aec9-1b72-4cb2-bd24-fc35f6d95240
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cbaf09a88e5856bfd5c8f36826d8894b947e3c0d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250504"
---
# <a name="options-sql-server-object-explorer---scripting-page"></a>Opciones (Explorador de objetos de SQL Server - página Script)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Use esta página para establecer las opciones de scripting que se aplican a los siguientes comandos en los menús contextuales de objeto del **Explorador de objetos**:  
  
-   Comandos **Editar** para las tablas y vistas del usuario.  
  
-   Comandos **Script <object> como** para los objetos creados por el usuario.  
  
-   Comando **Modificar** para los objetos creados por el usuario.  
  
-   Esta página también establece los valores predeterminados de las opciones de scripting del **Asistente para generar scripts de SQL Server**.  
  
## <a name="remarks"></a>Observaciones  
Los comandos **Editar** y **Modificar** pueden generar resultados distintos a los del comando **Script <object> como** para el mismo valor de opción. Los comandos **Editar** y **Modificar** están diseñados para modificar objetos en la base de datos actual durante una sesión del Editor de consultas. El comando **Script<object> como** está diseñado para generar un script que se pueda usar posteriormente para crear objetos.  
  
## <a name="options"></a>Opciones  
Para especificar las opciones de scripts, seleccione las opciones de configuración disponibles en la lista situada a la derecha de cada opción.

> [!NOTE]
> Los valores de configuración predeterminados solo se aplican a la opción **Crear un script a partir de toda la base de datos y todos los objetos de esta** y pueden variar al usar la opción **Seleccionar objetos de base de datos específicos**.
  
### <a name="general-scripting-options"></a>Opciones generales de scripting  
**Delimitar instrucciones individuales**  
Separa instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] individuales mediante un separador de lotes. Para cambiar el separador de lotes predeterminado del **Editor de consultas**, seleccione **Herramientas**/**Opciones**/**Ejecución de la consulta**/**SQL Server**/**General**/**Separador de lotes**. El valor predeterminado es False. Para más información, consulte [GO (Transact-SQL)](../../t-sql/language-elements/sql-server-utilities-statements-go.md).  
  
**Incluir encabezados descriptivos**  
Agrega comentarios descriptivos al script dividiéndolo en secciones para cada objeto. El valor predeterminado es True. Para más información, consulte [/ *...* / (Comment) (Transact-SQL)](../../t-sql/language-elements/slash-star-comment-transact-sql.md).  
  
**Incluir la habilitación de la compresión vardecimal**  
Incluye las opciones de almacenamiento vardecimal. El valor predeterminado es False. Para obtener más información, consulte [sp_db_vardecimal_storage_format (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).  
  
**Crear scripts para el historial de cambios**  
Incluye información del historial de cambios en el script.  
  
**Generar script de catálogos de texto completo**  
Incluye un script para catálogos de texto completo. El valor predeterminado es False. Para más información, consulte [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md).  
  
**Incluir USE <database>**  
Agrega la instrucción USE DATABASE al script para crear objetos de base de datos en el contexto de la base de datos actual del **Explorador de objetos** . Cuando necesite usar el script en otra base de datos, seleccione False para omitir. El valor predeterminado es True. Para más información, consulte [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md).  
  
### <a name="object-scripting-options"></a>Opciones de scripting de objetos  

**Comprobar la existencia de objetos** Compruebe que existe un objeto con el nombre indicado antes de quitarlo o modificarlo, o bien que no existe un objeto con el nombre indicado antes de crearlo. Para más información, consulte [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md) y [EXISTS (Transact-SQL)](../../t-sql/language-elements/exists-transact-sql.md).

**Generar script para objetos dependientes**  
Genera un script para objetos adicionales que son necesarios cuando se ejecuta el script para el objeto seleccionado. El valor predeterminado es False.  
  
**Nombres de objeto de certificación de esquema**  
Califica nombres de objeto con el esquema de objetos. El valor predeterminado es False. Para más información, consulte [Crear un esquema de base de datos](../../relational-databases/security/authentication-access/create-a-database-schema.md).  

**Incluir opciones de compresión de datos en el script** Incluye las opciones de compresión de datos en el script. El valor predeterminado es False.

**Generar script de propiedades extendidas**  
Incluye propiedades extendidas en el script si el objeto tiene propiedades extendidas. El valor predeterminado es False. Para más información, consulte [sp_addextendedproperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md).  
  
**Incluir propietario**  
Incluye el propietario en el script generado. El valor predeterminado es False.  
  
**Generar script de permisos**  
Incluye permisos sobre los objetos de la base de datos en el script. El valor predeterminado es True. Para más información, consulte [Permisos](../../relational-databases/security/permissions-database-engine.md).  
  
### <a name="tableview-options"></a>Opciones de tabla o vista  
Las siguientes opciones solo se aplican a scripts para tablas o vistas.  
  
**Convertir tipos de datos definidos por el usuario en tipos base**  
Convierte los tipos de datos definidos por el usuario en los tipos base a partir de los cuales se crearon. Utilice el valor True cuando los tipos de datos definidos por el usuario de la base de datos de origen no existan en la base de datos en la que se va a ejecutar el script. Utilice el valor False para mantener los tipos de datos definidos por el usuario. El valor predeterminado es False. Para más información, consulte [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md).  
  
**Generar comandos SET ANSI PADDING**  
Agrega la instrucción SET ANSI_PADDING antes y después de cada instrucción CREATE TABLE. El valor predeterminado es True. Para más información, consulte [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
**Incluir intercalación**  
Incluye una intercalación en la definición de columna. El valor predeterminado es True. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
**Incluir propiedad IDENTITY**  
Incluye definiciones para el valor de inicialización IDENTITY y el incremento IDENTITY. El valor predeterminado es True. Para más información, consulte [IDENTITY (propiedad) (Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md).  
  
**Referencias de claves externas de certificación de esquema**  
Agrega el nombre de esquema a las referencias de tabla para las restricciones FOREIGN KEY. El valor predeterminado es True.  
  
**Generar script de reglas y valores predeterminados enlazados**  
Incluye las llamadas a los procedimientos almacenados enlazados **sp_bindefault** y **sp_bindrule** . El valor predeterminado es True. Para más información, consulte [sp_bindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md) y [sp_bindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md).  
  
**Generar script de restricciones CHECK**  
Agrega [restricciones CHECK](../../relational-databases/tables/unique-constraints-and-check-constraints.md) al script. El valor predeterminado es True.  
  
**Incluir valores predeterminados**  
Incluye valores predeterminados de columna en el script. El valor predeterminado es False. Para más información, consulte [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md).  
  
**Generar script de grupos de archivos**  
Especifica el grupo de archivos en la cláusula ON para definiciones de tabla. El valor predeterminado es False. Para más información, consulte [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
**Incluir claves externas en el script**  
Incluye [restricciones FOREIGN KEY](../../relational-databases/tables/primary-and-foreign-key-constraints.md) en el script. El valor predeterminado es False.  
  
**Incluir índices de texto completo**  
Incluye índices de texto completo en el script. El valor predeterminado es False. Para más información, consulte [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
**Incluir índices en el script**  
Incluye índices clúster, no clúster y XML en el script. El valor predeterminado es True. Para más información, consulte [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md).  
  
**Generar script de esquemas de partición**  
Incluye esquemas de particiones de tabla en el script. El valor predeterminado es False. Para más información, consulte [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
  
**Incluir claves principales en el script**  
Incluye [Restricciones entre claves principales y claves externas](../../relational-databases/tables/primary-and-foreign-key-constraints.md) en el script. El valor predeterminado es True.  
  
**Incluir estadísticas en el script**  
Incluye estadísticas definidas por el usuario en el script. El valor predeterminado es False. Para más información, consulte [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md).  
  
**Incluir desencadenadores**  
Incluye desencadenadores en el script. El valor predeterminado es False. Para más información, consulte [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md).  
  
**Incluir claves únicas en el script**  
Incluye [Restricciones UNIQUE y restricciones CHECK](../../relational-databases/tables/unique-constraints-and-check-constraints.md) en el script. El valor predeterminado es False.  
  
**Generar script de columnas de vista**  
Declara columnas de vista en encabezados de vista. El valor predeterminado es False. Para más información, consulte [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md).  
  
**Incluir nombres de sistema de DRI**  
Incluye nombres de restricción generados por el sistema para aplicar la integridad referencial declarativa. El valor predeterminado es False. Para más información, consulte [REFERENTIAL_CONSTRAINTS (Transact-SQL)](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md).  
  
### <a name="version-options"></a>Opciones de versión

**Coincidir configuración del script con origen** Si está habilitada, la versión de destino, la edición del motor y el tipo de motor de los scripts generados se establecerán en los valores del servidor del objeto que se va a incluir en el script. Esta acción deshabilitará (y omitirá) las otras opciones de versión. 

**Script para la edición del motor de base de datos** Los scripts generados se dirigirán a la [Edición del motor](/dotnet/api/microsoft.sqlserver.management.smo.edition) especificada.

**Script para tipo de motor de base de datos** Los scripts generados se dirigirán al [Tipo de motor de base de datos](/previous-versions/sql/sql-server-2014/ee642509(v=sql.120)) especificado.

**Script para versión de servidor**  
Los scripts generados se dirigirán a la versión especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las características nuevas de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] no se pueden incluir en scripts para versiones anteriores. Algunos scripts creados para [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] no pueden ejecutarse en servidores que ejecutan una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o en una base de datos con un [valor de nivel de compatibilidad de base de datos](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)anterior.  

## <a name="see-also"></a>Consulte también  
[Generar scripts (SQL Server Management Studio)](../scripting/generate-scripts-sql-server-management-studio.md)  

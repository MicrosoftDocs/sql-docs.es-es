---
title: Asignaciones de tipos de datos para un publicador de Oracle
description: Describe cómo especificar asignaciones de tipos de datos para un publicador de Oracle en SQL Server mediante SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Oracle publishing [SQL Server replication], data type mapping
- data types [SQL Server replication], Oracle publishing
- mapping data types [SQL Server replication]
ms.assetid: f172d631-3b8c-4912-bd0f-568366cd9870
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a7381574bccfa00aa58c1666d3aae2b842c32be1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076517"
---
# <a name="specify-data-type-mappings-for-an-oracle-publisher"></a>Especificar asignaciones de tipos de datos para un publicador de Oracle
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo especificar asignaciones de tipos de datos para un publicador de Oracle en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Aunque se proporciona un conjunto de asignaciones de tipo de datos predeterminado para los publicadores de Oracle, es posible que sea necesario especificar las diferentes asignaciones para una publicación determinada.  
  
 **En este tema**  
  
-   **Para especificar asignaciones de tipos de datos para un publicador de Oracle con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Especifique asignaciones de tipos de datos en la pestaña **Asignación de datos** del cuadro de diálogo **Propiedades del artículo: \<Article>** . Está disponible en la página **Artículos** del Asistente para nueva publicación y en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para más información sobre el uso del asistente y el acceso al cuadro de diálogo, vea [Crear una publicación a partir de una base de datos de Oracle](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-a-data-type-mapping"></a>Para especificar una asignación de tipos de datos  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla y haga clic en **Propiedades del artículo**.  
  
2.  Haga clic en **Establecer propiedades del artículo de tabla resaltado**.  
  
3.  En la pestaña **Asignación de datos** del cuadro de diálogo **Propiedades del artículo: \<Article>** , seleccione las asignaciones de la columna **Tipo de datos del suscriptor**:  
  
    -   Para algunos tipos de datos, solo hay una asignación posible, en cuyo caso la columna de la cuadrícula de propiedades es de solo lectura.  
  
    -   Para algunos tipos, hay más de un tipo que puede seleccionar. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] recomienda el uso de la asignación predeterminada a menos que la aplicación requiera una asignación diferente. Para más información, consulte [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md).  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Puede especificar asignaciones de tipo de datos personalizadas mediante programación con los procedimientos almacenados de la replicación. También puede establecer las asignaciones predeterminadas que se usan al asignar los tipos de datos entre [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y un sistema de administración de bases de datos (DBMS) que no sea de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para más información, consulte [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md).  
  
#### <a name="to-define-custom-data-type-mappings-when-creating-an-article-belonging-to-an-oracle-publication"></a>Para definir las asignaciones de tipo de datos personalizadas al crear un artículo que pertenece a una publicación de Oracle  
  
1.  Si aún no existe ninguna, cree una publicación de Oracle.  
  
2.  En el distribuidor, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique un valor de **0** para **\@use_default_datatypes**. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
3.  En el distribuidor, ejecute [sp_helparticlecolumns](../../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md) para ver la asignación existente de una columna en un artículo publicado.  
  
4.  En el distribuidor, ejecute [sp_changearticlecolumndatatype](../../../relational-databases/system-stored-procedures/sp-changearticlecolumndatatype-transact-sql.md). Especifique el nombre del publicador de Oracle para **\@publisher**, así como **\@publication**, **\@article** y **\@column** para definir la columna publicada. Especifique el nombre del tipo de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] al que asignar para **\@type**, así como **\@length**, **\@precision** y **\@scale**, cuando corresponda.  
  
5.  En el distribuidor, ejecute [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Esto crea la vista usada para generar la instantánea de la publicación de Oracle.  
  
#### <a name="to-specify-a-mapping-as-the-default-mapping-for-a-data-type"></a>Para especificar una asignación como la asignación predeterminada de un tipo de datos  
  
1.  (Opcional) En cualquier base de datos del distribuidor, ejecute [sp_getdefaultdatatypemapping](../../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md). Especifique **\@source_dbms**, **\@source_type**, **\@destination_dbms**, **\@destination_version** y cualquier otro parámetro necesario para identificar el DBMS de origen. Se devuelve información acerca del tipo de datos asignado actualmente en el DBMS de destino mediante los parámetros de salida.  
  
2.  (Opcional) En cualquier base de datos del distribuidor, ejecute [sp_helpdatatypemap](../../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md). Especifique **\@source_dbms** y cualquier otro parámetro necesario para filtrar el conjunto de resultados. Tenga en cuenta el valor de **mapping_id** para la asignación deseada en el conjunto de resultados.  
  
3.  En cualquier base de datos del distribuidor, ejecute [sp_setdefaultdatatypemapping](../../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md).  
  
    -   Si conoce el valor deseado de **mapping_id** obtenido en el paso 2, especifíquelo para **\@mapping_id**.  
  
    -   Si no conoce **mapping_id**, especifique los parámetros **\@source_dbms**, **\@source_type**, **\@destination_dbms**, **\@destination_type** y cualquier otro parámetro necesario para identificar una asignación existente.  
  
#### <a name="to-find-valid-data-types-for-a-given-oracle-data-type"></a>Para buscar los tipos de datos válidos para un tipo de datos de Oracle determinado  
  
1.  En cualquier base de datos del distribuidor, ejecute [sp_helpdatatypemap](../../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md). Especifique un valor de **ORACLE** para **\@source_dbms** y cualquier otro parámetro necesario para filtrar el conjunto de resultados.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 En este ejemplo se cambia una columna con un tipo de datos de Oracle NUMBER, de modo que se asigna al tipo de datos [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (38,38) de **ssNoVersion**, en lugar del tipo de datos predeterminado **float**.  
  
 [!code-sql[HowTo#sp_changecolumndatatype](../../../relational-databases/replication/codesnippet/tsql/specify-data-type-mappin_1.sql)]  
  
 Esta consulta de ejemplo devuelve las asignaciones predeterminadas y alternativas para el tipo de datos de Oracle 9 **CHAR**.  
  
 [!code-sql[HowTo#sp_helpcolumndatatype_char](../../../relational-databases/replication/codesnippet/tsql/specify-data-type-mappin_2.sql)]  
  
 Esta consulta de ejemplo devuelve las asignaciones predeterminadas para el tipo de datos de Oracle 9 **NUMBER** cuando se especifica sin escala o precisión.  
  
 [!code-sql[HowTo#sp_helpcolumndatatype_number](../../../relational-databases/replication/codesnippet/tsql/specify-data-type-mappin_3.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [Replicación de bases de datos heterogéneas](../../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Configurar un publicador de Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)  
  
  

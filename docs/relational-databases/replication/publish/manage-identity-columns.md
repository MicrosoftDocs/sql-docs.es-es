---
description: Administrar columnas de identidad
title: Administración de columnas de identidad | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server replication]
- merge replication [SQL Server replication], identity range management
- publishing [SQL Server replication], identity columns
- transactional replication, identity range management
- identity columns [SQL Server], replication
ms.assetid: 98892836-cf63-494a-bd5d-6577d9810ddf
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: aac4d9dd50a329f4395a6b7aa9efb116b5efc098
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076683"
---
# <a name="manage-identity-columns"></a>Administrar columnas de identidad
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo administrar columnas de identidad en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Cuando las inserciones del Suscriptor se replican de nuevo al Publicador, se deben administrar las columnas de identidad para evitar la asignación del mismo valor de identidad en el Suscriptor y el Publicador. La replicación puede administrar automáticamente intervalos de identidad o puede elegir procesar manualmente la administración de intervalos de identidad.  Para obtener información sobre las opciones de administración de intervalos de identidad proporcionadas por la replicación, vea [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
-   **Para administrar columnas de identidad con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Al publicar una tabla en más de una publicación, debe especificar las mismas opciones de administración de intervalos de identidad para ambas publicaciones. Para obtener más información, vea "Publicar tablas en más de una publicación" en [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
-   Para crear un número que se incremente automáticamente y que se pueda usar en varias tablas, o que se pueda llamar desde las aplicaciones sin hacer referencia a ninguna tabla, vea [Números de secuencia](../../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Especifique una opción de administración de columnas de identidad en la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** del Asistente para nueva publicación. Para obtener más información sobre cómo usar este asistente, vea [Crear una publicación](../../../relational-databases/replication/publish/create-a-publication.md). En el Asistente para nueva publicación:  
  
-   Si selecciona **Publicación de combinación** o **Publicación transaccional con suscripciones actualizables** en la página **Tipo de publicación** , seleccione la administración automática o manual de los intervalos de identidad (se recomienda utilizar la opción predeterminada: la administración automática). Una vez publicada la tabla, la propiedad no se puede modificar, pero otras propiedades relacionadas sí se pueden modificar.  
  
-   Si selecciona otros tipos de publicaciones, debe establecer la administración de los intervalos de identidad en manual.  
  
 Modifique los rangos de identidad y los umbrales en la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , disponible en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el acceso a este cuadro de diálogo, vea [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-an-identity-column-management-option"></a>Para especificar una opción de administración de las columnas de identidad  
  
1.  Si el publicador está ejecutando una versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] anterior a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], en la página **Tipo de publicación** del Asistente para nueva publicación, seleccione **Publicación de combinación** o **Publicación transaccional con suscripciones actualizables**.  
  
2.  En la página **Artículos** , seleccione una tabla con una columna de identidad.  
  
3.  Haga clic en **Propiedades del artículo** y, a continuación, haga clic en **Establecer propiedades del artículo de tabla resaltado**.  
  
4.  En la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , en la sección **Administración de intervalos de identidad**, establezca la propiedad **Administrar intervalos de identidad automáticamente** en **Automático** o **Manual** (para publicadores que ejecuten [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o una versión posterior), o en **True** o **False** (para publicadores que ejecuten una versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] anterior a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]).  
  
5.  Si seleccionó **Automático** o **True** en el paso 4, escriba los valores de las opciones en la siguiente tabla. Para obtener más información sobre cómo usar estos valores, vea la sección "Asignar intervalos de identidad"de [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
    |Opción|Value|Descripción|  
    |------------|-----------|-----------------|  
    |**Tamaño de intervalo del publicador**|Valor entero para el tamaño del intervalo (por ejemplo, 20 000).|Vea la sección "Asignar intervalos de identidad"de [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md).|  
    |**Tamaño de intervalo del suscriptor**|Valor entero para el tamaño del intervalo (por ejemplo, 10000).|Vea la sección "Asignar intervalos de identidad"de [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md).|  
    |**Porcentaje de umbral del intervalo**|Valor entero para el umbral de porcentaje (por ejemplo, 90 equivale a 90 por ciento).|Porcentaje del total de valores de identidad utilizados en un nodo antes de que se asigne un nuevo intervalo de identidad.<br /><br /> <br /><br /> Nota: Se debe especificar este valor, pero solo se usa en: suscriptores que usan suscripciones de actualización en cola y suscriptores de publicaciones de combinación que ejecuten [!INCLUDE[ssEW](../../../includes/ssew-md.md)] o versiones anteriores de otras ediciones de SQL Server. Para obtener más información, vea la sección "Asignar intervalos de identidad"de [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md).|  
    |**Valor de inicio del siguiente intervalo**|Valor entero. Solo lectura.|El valor en el que comenzará el siguiente intervalo. Por ejemplo, si el intervalo actual es 5001-6000, este valor será 6001.|  
    |**Valor máximo de identidad**|Valor entero. Solo lectura.|El valor más grande de la columna de identidad. Está determinado por el tipo de datos base de la columna.|  
    |**Incremento**|Valor entero. Solo lectura.|La cantidad en la que se debe incrementar o reducir el número de la columna de identidad para cada inserción: por lo general se establece en 1.|  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-modify-identity-ranges-and-thresholds-after-a-table-is-published"></a>Para modificar los intervalos de identidad y los umbrales una vez publicada la tabla  
  
1.  En la página **Artículos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla con una columna de identidad.  
  
2.  Haga clic en **Propiedades del artículo** y, a continuación, haga clic en **Establecer propiedades del artículo de tabla resaltado**.  
  
3.  En la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , en la sección **Administración de intervalos de identidad**, escriba valores para una o varias de las propiedades siguientes: **Tamaño de intervalo del publicador**, **Tamaño de intervalo del suscriptor** y **Porcentaje de umbral del intervalo**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Haga clic en **Aceptar** en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Puede usar los procedimientos almacenados de replicación para especificar las opciones de administración de intervalos de identidad cuando se crea un artículo.  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>Para habilitar la administración automática de intervalos de identidad al definir artículos para una publicación transaccional  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Si la tabla de origen que se va a publicar tiene una columna de identidad, especifique un valor de **auto** para **\@identityrangemanagementoption**, el intervalo de valores de identidad asignados al Publicador para **\@pub_identity_range**, el intervalo de valores de identidad asignados a cada Suscriptor para **\@identity_range** y el porcentaje de valores de identidad totales usados antes de que se asigne un nuevo intervalo de identidad para **\@threshold**. Para obtener más información sobre la definición de artículos, vea [Definir un artículo](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Asegúrese de que el tipo de datos de la columna de identidad es lo bastante grande como para admitir el intervalo total de identidades que se están asignando a todos los Suscriptores.  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>Para deshabilitar la administración automática de intervalos de identidad al definir artículos para una publicación transaccional  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique un valor de **manual** para **\@identityrangemanagementoption**. Para obtener más información sobre la definición de artículos, vea [Definir un artículo](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Asigne intervalos para identificar las columnas de artículo en el Suscriptor para evitar la generación de conflictos para actualizar los Suscriptores. Para obtener más información, vea la sección sobre cómo asignar intervalos para la administración manual de intervalos de identidad en el tema [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>Para habilitar la administración automática de intervalos de identidad al definir artículos para una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Si la tabla de origen que se va a publicar tiene una columna de identidad, especifique un valor de **auto** para **\@identityrangemanagementoption**, el intervalo de valores de identidad asignados a una suscripción de servidor para **\@pub_identity_range**, el intervalo de valores de identidad asignados al Publicador y cada suscripción de cliente para **\@identity_range**, y el porcentaje de valores de identidad totales usados antes de que se asigne un nuevo intervalo de identidad para **\@threshold**. Para obtener más información sobre cuándo se asignan los nuevos intervalos de identidad, vea Asignar intervalos de identidad en el tema [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md). Para obtener más información sobre la definición de artículos, vea [Definir un artículo](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Asegúrese de que el tipo de datos de la columna de identidad es lo bastante grande como para admitir el intervalo total de identidades que se están asignando a todos los Suscriptores, particularmente para los Suscriptores con suscripciones de servidor.  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>Para deshabilitar la administración automática de intervalos de identidad al definir artículos para una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Especifique uno de los valores siguientes para **\@identityrangemanagementoption**:  
  
    -   **manual** : los intervalos de identidad deben estar asignados manualmente para actualizar los Suscriptores.  
  
    -   **none** : las columnas de identidad en el Publicador no se definirán como columnas de identidad en el Suscriptor.  
  
     Para obtener más información sobre la definición de artículos, vea [Definir un artículo](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Asigne intervalos para identificar las columnas de artículo en el Suscriptor para evitar la generación de conflictos para actualizar los Suscriptores.  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>Para cambiar los valores de administración automática de intervalos de identidad para un artículo existente en una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md) y tenga en cuenta el valor de **identityrangemanagementoption** en el conjunto de resultados. Si este valor es **0**, la administración automática de intervalos de identidad no está habilitada.  
  
2.  Si el valor de **identityrangemanagementoption** en el conjunto de resultados es **1**, cambie la configuración de la siguiente manera:  
  
    -   Para cambiar los intervalos de identidad asignados, ejecute [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de **identity_range** o **pub_identity_range** para **\@property** y el nuevo valor del intervalo para **\@value**.  
  
    -   Para cambiar el umbral en el que los nuevos intervalos están asignados, ejecute [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de **threshold** para **\@property** y el nuevo valor umbral para **\@value**.  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-merge-publication"></a>Para cambiar la configuración de administración automática de intervalos de identidad para un artículo existente en una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) y tenga en cuenta el valor de **identity_support** en el conjunto de resultados. Si este valor es **0**, la administración automática de intervalos de identidad no está habilitada.  
  
2.  Si el valor de **identity_support** en el conjunto de resultados es **1**, cambie la configuración de la siguiente manera:  
  
    -   Para cambiar los intervalos de identidad asignados, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de **identity_range** o **pub_identity_range** para **\@property** y el nuevo valor del intervalo para **\@value**.  
  
    -   Para cambiar el umbral en el que los nuevos intervalos están asignados, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de **threshold** para **\@property** y el nuevo valor umbral para **\@value**. Para obtener más información sobre cuándo se asignan los nuevos intervalos de identidad, vea Asignar intervalos de identidad en el tema [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
    -   Para deshabilitar la administración automática de intervalos de identidad, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de **identityrangemanagementoption** para **\@property** y **manual** o **none** para **\@value**.  
  
## <a name="see-also"></a>Consulte también  
 [Peer-to-Peer Transactional Replication](../../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Replicar columnas de identidad](../../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  

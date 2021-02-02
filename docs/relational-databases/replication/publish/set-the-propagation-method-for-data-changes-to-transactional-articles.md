---
title: Establecimiento del método de propagación para cambios en artículos (transaccional)
description: Describe cómo establecer el método de propagación para los cambios de datos en artículos transaccionales para la replicación transaccional mediante SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, propagation methods
- propagating data changes [SQL Server replication]
ms.assetid: 0a291582-f034-42da-a1a3-29535b607b74
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 3a88f145a90b9f9f27cbdd9c78d0f233338e2bc8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076673"
---
# <a name="set-the-propagation-method-for-data-changes-to-transactional-articles"></a>Establecer el método de propagación para cambios de datos en artículos transaccionales
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo establecer el método de propagación para los cambios de datos en artículos transaccionales en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 De forma predeterminada, la replicación transaccional propaga los cambios a los suscriptores mediante un conjunto de procedimientos almacenados para cada artículo. Puede reemplazar estos procedimientos con procedimientos personalizados. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
-   **Para establecer el método de propagación para los cambios de datos en artículos transaccionales con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
## <a name="before-you-begin"></a>Antes de empezar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Tenga cuidado al editar cualquiera de los archivos de instantáneas generados por la replicación. Debe probar y admitir lógica personalizada en los procedimientos almacenados personalizados. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] no ofrece compatibilidad con lógica personalizada.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Especifique el método de propagación en la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , disponible en el Asistente para nueva publicación y el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el uso del asistente y el acceso al cuadro de diálogo, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Crear una publicación) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-the-propagation-method"></a>Para especificar el método de propagación  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla y haga clic en **Propiedades del artículo**.  
  
2.  Haga clic en **Establecer propiedades del artículo de tabla resaltado**.  
  
3.  En la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , en la sección **Entrega de instrucción**, especifique el método de propagación de cada operación con los menús **Formato de entrega para INSERT**, **Formato de entrega para UPDATE** y **Formato de entrega para DELETE**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  

#### <a name="to-generate-and-use-custom-stored-procedures"></a>Para generar y utilizar procedimientos almacenados personalizados  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla y haga clic en **Propiedades del artículo**.  
  
2.  Haga clic en **Establecer propiedades del artículo de tabla resaltado**.  
  
     En la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , en la sección **Entrega de instrucción**, seleccione la sintaxis CALL en el menú de formato de entrega correspondiente (**Formato de entrega para INSERT**, **Formato de entrega para UPDATE** o **Formato de entrega para DELETE**) y, después, escriba el nombre del procedimiento que se va a usar en **Procedimiento almacenado para INSERT**, **Procedimiento almacenado para DELETE** o **Procedimiento almacenado para UPDATE**. Para más información sobre la sintaxis CALL, vea la sección "Call syntax for stored procedures" (Sintaxis de llamada para procedimientos almacenados) de [Transactional Articles - Specify How Changes Are Propagated](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md) (Artículos transaccionales: Especificar cómo se propagan los cambios).  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
4.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  
  
5.  Cuando se genere la instantánea para la publicación, incluirá el procedimiento que ha especificado en el paso anterior. Los procedimientos utilizarán la sintaxis CALL que ha especificado, pero incluirán la lógica predeterminada que utilice la replicación.  
  
     Después de generar la instantánea, navegue a la carpeta de instantáneas de la publicación a la que pertenece este artículo y busque el archivo **.sch** con el mismo nombre que el artículo. Abra este archivo con el Bloc de notas u otro editor de texto, busque el comando CREATE PROCEDURE para los procedimientos almacenados insert, update o delete, y edite la definición del procedimiento para proporcionar una lógica personalizada para propagar los cambios de datos. Si la instantánea se vuelve a generar, debe volver a crear el procedimiento personalizado.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 La replicación transaccional le permite controlar cómo los cambios se propagan del publicador a los suscriptores; este método de propagación se puede establecer mediante programación cuando un artículo se crea y se cambia después con los procedimientos almacenados de la replicación.  
  
> [!NOTE]  
>  Puede especificar un método de propagación diferente para cada tipo de operación (inserción, actualización o eliminación) DML (lenguaje de manipulación de datos) que se produce en una fila de datos publicados.  
  
 Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
#### <a name="to-create-an-article-that-uses-transact-sql-commands-to-propagate-data-changes"></a>Para crear un artículo que use comandos Transact-SQL para propagar cambios de datos  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para **\@publication**, un nombre de artículo para **\@article**, el objeto de base de datos que se publica para **\@source_object** y un valor de **SQL** para, por lo menos, uno de los parámetros siguientes:  
  
    -   **\@ins_cmd** - controla la replicación de los comandos [INSERT](../../../t-sql/statements/insert-transact-sql.md).  
  
    -   **\@upd_cmd** - controla la replicación de los comandos [UPDATE](../../../t-sql/queries/update-transact-sql.md).  
  
    -   **\@del_cmd** - controla la replicación de los comandos [DELETE](../../../t-sql/statements/delete-transact-sql.md).  
  
    > [!NOTE]  
    >  Al especificar un valor de **SQL** para cualquiera de los parámetros anteriores, los comandos de ese tipo se replicarán al suscriptor como el comando [!INCLUDE[tsql](../../../includes/tsql-md.md)] correspondiente.  
  
     Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-create-an-article-that-does-not-propagate-data-changes"></a>Para crear un artículo que no propaga los cambios de datos  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para **\@publication**, un nombre de artículo para **\@article**, el objeto de base de datos que se publica para **\@source_object** y un valor de **NONE** para por lo menos uno de los parámetros siguientes:  
  
    -   **\@ins_cmd** - controla la replicación de los comandos [INSERT](../../../t-sql/statements/insert-transact-sql.md).  
  
    -   **\@upd_cmd** - controla la replicación de los comandos [UPDATE](../../../t-sql/queries/update-transact-sql.md).  
  
    -   **\@del_cmd** - controla la replicación de los comandos [DELETE](../../../t-sql/statements/delete-transact-sql.md).  
  
    > [!NOTE]  
    >  Al especificar un valor de **NONE** para cualquiera de los parámetros anteriores, los comandos de ese tipo no se replicarán al suscriptor.  
  
     Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-create-an-article-with-user-modified-custom-stored-procedures"></a>Para crear un artículo con procedimientos almacenados personalizados modificados por el usuario  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para **\@publication**, un nombre de artículo para **\@article**, el objeto de base de datos que se publica para **\@source_object**, un valor para la máscara de bits **\@schema_option** que contiene el valor **0x02** (habilita la generación automática de procedimientos almacenados personalizados) y, por lo menos, uno de los parámetros siguientes:  
  
    -   **\@ins_cmd**: especifica un valor de **CALL sp_MSins_* article_name***, en el que **_article_name_ *_ es el valor especificado para _* \@article**.  
  
    -   **\@del_cmd**: especifica un valor de **CALL sp_MSdel_*article_name*** o **XCALL sp_MSdel_* article_name***, en el que **_article_name_ *_ es el valor especificado para _* \@article**.  
  
    -   **\@upd_cmd**: especifica un valor de **SCALL sp_MSupd_* article_name***, **CALL sp_MSupd_* article_name***, **XCALL sp_MSupd_* article_name*** o **MCALL sp_MSupd_* article_name***, en el que **_article_name_ *_ es el valor especificado para _* \@article**.  
  
    > [!NOTE]  
    >  Para cada uno de los parámetros de comandos anteriores, puede especificar su propio nombre para los procedimientos almacenados que genera la replicación.  
  
    > [!NOTE]  
    >  Para más información sobre la sintaxis CALL, SCALL, XCALL y MCALL, vea [Transactional Articles - Specify How Changes Are Propagated](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md) (Artículos transaccionales: Especificar cómo se propagan los cambios).  
  
     Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Después de generar la instantánea, navegue a la carpeta de instantáneas de la publicación a la que pertenece este artículo y busque el archivo **.sch** con el mismo nombre que el artículo. Abra este archivo con Bloc de notas.exe, busque el comando CREATE PROCEDURE para los procedimientos almacenados de inserción, actualización o eliminación, y edite la definición del procedimiento para proporcionar una lógica personalizada para propagar los cambios de datos. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
#### <a name="to-create-an-article-with-custom-scripting-in-the-custom-stored-procedures-to-propagate-data-changes"></a>Para crear un artículo con scripting personalizado en los procedimientos almacenados personalizados y propagar los cambios de datos  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para **\@publication**, un nombre de artículo para **\@article**, el objeto de base de datos que se publica para **\@source_object**, un valor para la máscara de bits **\@schema_option** que contiene el valor **0x02** (habilita la generación automática de procedimientos almacenados personalizados) y, por lo menos, uno de los parámetros siguientes:  
  
    -   **\@ins_cmd**: especifica un valor de **CALL sp_MSins_* article_name***, en el que **_article_name_ *_ es el valor especificado para _* \@article**.  
  
    -   **\@del_cmd**: especifica un valor de **CALL sp_MSdel_*article_name*** o **XCALL sp_MSdel_* article_name***, en el que **_article_name_ *_ es el valor especificado para _* \@article**.  
  
    -   **\@upd_cmd**: especifica un valor de **SCALL sp_MSupd_* article_name***, **CALL sp_MSupd_* article_name***, **XCALL sp_MSupd_* article_name*** o **MCALL sp_MSupd_* article_name***, en el que **_article_name_ *_ es el valor especificado para _* \@article**.  
  
    > [!NOTE]  
    >  Para cada uno de los parámetros de comandos anteriores, puede especificar su propio nombre para los procedimientos almacenados que genera la replicación.  
  
    > [!NOTE]  
    >  Para más información sobre la sintaxis CALL, SCALL, XCALL y MCALL, vea [Transactional Articles - Specify How Changes Are Propagated](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md) (Artículos transaccionales: Especificar cómo se propagan los cambios).  
  
     Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  En la base de datos de publicación del publicador, use la instrucción [ALTER PROCEDURE](../../../t-sql/statements/alter-procedure-transact-sql.md) para modificar [sp_scriptpublicationcustomprocs](../../../relational-databases/system-stored-procedures/sp-scriptpublicationcustomprocs-transact-sql.md) de modo que devuelva un script [CREATE PROCEDURE](../../../t-sql/statements/create-procedure-transact-sql.md) para los procedimientos almacenados personalizados de inserción, actualización y eliminación. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
#### <a name="to-change-the-method-of-propagating-changes-for-an-existing-article"></a>Para cambiar el método de propagar los cambios para un artículo existente  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md). Especifique **\@publication**, **\@article**, un valor de **ins_cmd**, **upd_cmd** o **del_cmd** para **\@property** y el método de propagación adecuado para **\@value**.  
  
2.  Repita el paso 1 para cada método de propagación que se va a cambiar.  
  
## <a name="see-also"></a>Consulte también  
 [Especificar cómo se propagan los cambios para los artículos transaccionales](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)   
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Creación de una publicación)  
  
  

---
title: Implementación de un solucionador de conflictos personalizado (combinación)
description: Obtenga información sobre cómo implementar un solucionador de conflictos personalizado para una publicación de combinación en SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], stored procedure-based resolvers
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
ms.assetid: 76bd8524-ebc1-4d80-b5a2-4169944d6ac0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5b07107b7ee615e317a9375b652a1b3ef43ad3d8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077143"
---
# <a name="implement-a-custom-conflict-resolver-for-a-merge-article"></a>Implementación de un solucionador de conflictos personalizado para un artículo de mezcla
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo implementar un solucionador de conflictos personalizado para un artículo de mezcla en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../includes/tsql-md.md)] o un [solucionador personalizado basado en COM](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-custom-resolvers.md).  
  
 **En este tema**  
  
-   **Implemente un solucionador de conflictos personalizado para un artículo de mezcla con:**  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Un solucionador basado en COM](#COM)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Puede escribir su propio solucionador de conflictos personalizado como un procedimiento almacenado [!INCLUDE[tsql](../../includes/tsql-md.md)] en cada publicador. Durante la sincronización, este procedimiento almacenado se invoca cuando se encuentran conflictos en un artículo en el que se ha registrado el solucionador. El Agente de mezcla pasa información sobre la fila del conflicto a los parámetros necesarios del procedimiento. Los solucionadores de conflictos personalizados basados en el procedimiento almacenado siempre se crean en el publicador.  
  
> [!NOTE]  
>  Los resolvedores de procedimientos almacenados de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo se invocan para solucionar conflictos debidos a cambios de fila. No se pueden utilizar para solucionar otros tipos de conflictos como errores de inserción desencadenados por infracciones de CLAVE PRINCIPAL o por infracciones de restricción de índice único.
  
#### <a name="to-create-a-stored-procedure-based-custom-conflict-resolver"></a>Para crear un solucionador de conflictos personalizado basado en un procedimiento almacenado  
  
1.  En el publicador, ya sea en la publicación o en la base de datos **msdb** , cree un nuevo procedimiento almacenado del sistema que implemente los parámetros necesarios siguientes:  
  
    |Parámetro|Tipo de datos|Descripción|  
    |---------------|---------------|-----------------|  
    |**\@tableowner**|**sysname**|Nombre del propietario de la tabla en la que se resuelve un conflicto. Éste es el propietario de la tabla de la base de datos de publicación.|  
    |**\@tablename**|**sysname**|Nombre de la tabla en la que se resuelve un conflicto.|  
    |**\@rowguid**|**uniqueidentifier**|Identificador único de la fila que tiene el conflicto.|  
    |**\@subscriber**|**sysname**|Nombre del servidor desde donde se propaga un cambio conflictivo.|  
    |**\@subscriber_db**|**sysname**|Nombre de la base de datos desde donde se propaga un cambio conflictivo.|  
    |**\@log_conflict OUTPUT**|**int**|Establece si el proceso de mezcla debería registrar un conflicto para su posterior resolución:<br /><br /> **0** = No registre el conflicto.<br /><br /> **1** = El suscriptor es el perdedor del conflicto.<br /><br /> **2** = El publicador es el perdedor del conflicto.|  
    |**\@conflict_message OUTPUT**|**nvarchar(512)**|Mensaje que se va a proporcionar sobre la resolución si se registra el conflicto.|  
    |**\@destowner**|**sysname**|El propietario de la tabla publicada en el suscriptor.|  
  
     Este procedimiento almacenado utiliza los valores que pasa el Agente de mezcla a estos parámetros para implementar la lógica de resolución de conflictos personalizada. Debe devolver un conjunto de resultados de fila única que tenga una estructura idéntica a la tabla base y que contenga los valores de datos de la versión ganadora de la fila.  
  
2.  Conceda permisos EXECUTE para el procedimiento almacenado a todos los inicios de sesión utilizados por los suscriptores para conectarse al publicador.  

#### <a name="use-a-custom-conflict-resolver-with-a-new-table-article"></a>Uso de un solucionador de conflictos personalizado con un nuevo artículo de tabla  
  
1. Ejecute [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para definir un artículo. 
1. Especifique un valor de **MicrosoftSQL** **Server Stored Procedure Resolver** para el parámetro **\@article_resolver**. 
1. Especifique el nombre del procedimiento almacenado que implementa la lógica del solucionador de conflictos para el parámetro **\@resolver_info**. 

   Para más información, consulte [Definir un artículo](../../relational-databases/replication/publish/define-an-article.md).
  
#### <a name="to-use-a-custom-conflict-resolver-with-an-existing-table-article"></a>Para utilizar un solucionador de conflictos personalizado con un artículo de tabla existente  
  
1.  Ejecute [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), especificando **\@publication**, **\@article**, el valor **article_resolver** para **\@property** y el valor **MicrosoftSQL** **Server Stored ProcedureResolver** para **\@value**.  
  
2.  Ejecute [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), especificando **\@publication**, **\@article**, el valor **resolver_info** para **\@property** y el nombre del procedimiento almacenado que implementa la lógica de solucionador de conflictos para **\@value**.  
  
##  <a name="using-a-com-based-custom-resolver"></a><a name="COM"></a> Uso un solucionador personalizado basado en COM  
 El espacio de nombres <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> implementa una interfaz que le permite escribir una lógica de negocios compleja para administrar eventos y solucionar conflictos que se producen durante el proceso de sincronización de replicación de mezcla. Para más información, consulte [Implementar un controlador de lógica de negocios para un artículo de mezcla](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md). También puede escribir su propia lógica de negocios personalizada basada en código nativo para solucionar conflictos. Esta lógica está generada como componente COM y se compila en las bibliotecas de vínculo dinámico (DLL), usando productos como [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++. Este tipo de solucionador de conflictos personalizado basado en COM debe implementar la interfaz **ICustomResolver**, que está diseñada específicamente para la resolución de conflictos.  
  
#### <a name="to-create-and-register-a-com-based-custom-conflict-resolver"></a>Para crear y registrar un solucionador de conflictos personalizado basado en COM  
  
1.  En un entorno de creación compatible con COM, agregue referencias a la biblioteca del solucionador de conflictos personalizado.  
  
2.  Para un proyecto de Visual C++, use la directiva #import para importar esta biblioteca en su proyecto.  
  
3.  Cree una clase que implemente la interfaz **ICustomResolver** .  
  
4.  Implemente determinados métodos y propiedades.  
  
5.  Genere el proyecto para crear el archivo de biblioteca del solucionador de conflictos personalizado.  
  
6.  Implemente la biblioteca en el directorio que contiene la aplicación ejecutable del Agente de mezcla (normalmente \Microsoft SQL Server\100\COM).  
  
    > [!NOTE]  
    >  Se debe implementar un solucionador de conflictos personalizado en el Suscriptor para una suscripción de extracción, en el Distribuidor para una suscripción de inserción, o en el servidor web usado con sincronización web.  
  
7.  Registre la biblioteca del solucionador de conflictos personalizado mediante la ejecución de regsvr32.exe desde el directorio de implementación de la siguiente manera:  
  
    ```  
    regsvr32.exe mycustomresolver.dll  
    ```  
  
8.  En el Publicador, ejecute [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) para comprobar que la biblioteca no está registrada todavía como solucionador de conflictos personalizado.  
  
9. Para registrar la biblioteca como solucionador de conflictos personalizado, ejecute [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md) en el Distribuidor. Especifique el nombre descriptivo del objeto COM para **\@article_resolver**, el Id. de la biblioteca (CLSID) para **\@resolver_clsidy** un valor de **false** para **\@is_dotnet_assembly**.  
  
    > [!NOTE]  
    >  Cuando ya no se necesite, se puede eliminar del registro un solucionador de conflictos personalizado con [sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md).  
  
10. (Opcional) En un clúster, repita los pasos 6-9 para registrar el solucionador personalizado en todos los nodos del clúster. Estos pasos son necesarios para garantizar que el solucionador personalizado pueda cargar correctamente la conciliación tras una conmutación por error.
  
#### <a name="to-use-a-custom-conflict-resolver-with-a-new-table-article"></a>Para utilizar un solucionador de conflictos personalizado con un nuevo artículo de tabla  
  
1.  En el Publicador, ejecute [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) y tenga en cuenta el nombre descriptivo del solucionador que desee.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para definir un artículo. Especifique el nombre descriptivo del solucionador de artículos del paso 1 para **\@article_resolver**. Para más información, consulte [Definir un artículo](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-custom-conflict-resolver-with-an-existing-table-article"></a>Para utilizar un solucionador de conflictos personalizado con un artículo de tabla existente  
  
1.  En el Publicador, ejecute [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) y tenga en cuenta el nombre descriptivo del solucionador que desee.  
  
2.  Ejecute [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) especificando **\@publication**, **\@article**, un valor de **article_resolver** para **\@property** y el nombre descriptivo del solucionador de artículos del paso 1 para **\@value**.  
  

## <a name="see-also"></a>Consulte también  
 [Replicación de mezcla avanzada: detección y resolución de conflictos](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Conflictos de replicación de mezcla avanzada: solucionadores personalizados basados en COM](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-custom-resolvers.md)   
 [Prácticas recomendadas de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  

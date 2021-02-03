---
description: Replicación en suscriptores de tablas con optimización para memoria
title: Replicación en suscriptores de tablas con optimización para memoria | Microsoft Docs
ms.custom: ''
ms.date: 11/21/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
ms.assetid: 1a8e6bc7-433e-471d-b646-092dc80a2d1a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a2dd5e112230540c519fad2b4926d70f374bbcfc
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236327"
---
# <a name="replication-to-memory-optimized-table-subscribers"></a>Replicación en suscriptores de tablas con optimización para memoria
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Las tablas que actúan como instantáneas y suscriptores de replicación transaccional, excluida la replicación transaccional punto a punto, pueden configurarse como tablas optimizadas para memoria. Otras configuraciones de replicación no son compatibles con las tablas optimizadas para memoria. Esta característica está disponible a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
  
## <a name="two-configurations-are-required"></a>Son necesarias dos configuraciones  
  
-   **Configurar la base de datos de suscriptor para admitir la replicación a tablas optimizadas para memoria**  
  
     Establezca la propiedad **\@memory_optimized** en **true** mediante [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) o [sp_changesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md).  
  
-   **Configurar el artículo para admitir la replicación a tablas optimizadas para memoria**  
  
     Establezca la opción `@schema_option = 0x40000000000` para el artículo mediante [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) o [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md).  
  
#### <a name="to-configure-a-memory-optimized-table-as-a-subscriber"></a>Para configurar una tabla optimizada para memoria como suscriptor  
  
1.  Cree una publicación transaccional. Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Agregue artículos a la publicación. Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
     Si la configuración se realiza mediante [!INCLUDE[tsql](../../includes/tsql-md.md)] establezca el parámetro **\@schema_option** del procedimiento almacenado **sp_addarticle** en   
    **0x40000000000**.  
  
3.  En la ventana de propiedades del artículo, establezca **Habilitar optimización para memoria** en **true**.  
  
4.  Inicie el trabajo del Agente de instantáneas para generar la instantánea inicial de esta publicación. Para más información, consulte [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
5.  Ahora cree una nueva suscripción. En el **Asistente para nueva suscripción** , establezca **Suscripción optimizada para memoria** en **true**.  

 Las tablas con optimización para memoria deben empezar ahora a recibir actualizaciones del publicador.  
  
#### <a name="reconfigure-an-existing-transaction-replication"></a>Reconfiguración de una replicación de transacción existente  
  
1.  Vaya a las propiedades de suscripción de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y establezca **Suscripción optimizada para memoria** en **true**. Los cambios no se aplican hasta que se reinicializa la suscripción.  
  
     Si la configuración se realiza mediante [!INCLUDE[tsql](../../includes/tsql-md.md)], establezca el nuevo parámetro **\@memory_optimized** del procedimiento almacenado **sp_addsubscription** en true.  
  
2.  Vaya a las propiedades del artículo de una publicación en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y establezca **Habilitar optimización para memoria** en true.  
  
     Si la configuración se realiza mediante [!INCLUDE[tsql](../../includes/tsql-md.md)] establezca el parámetro **\@schema_option** del procedimiento almacenado **sp_addarticle** en   
    **0x40000000000**.  
  
3.  Las tablas con optimización para memoria no admiten índices agrupados. Para hacer posible esto, la replicación los convierte en índices no agrupados estableciendo **Convertir índice agrupado en no agrupado para un artículo optimizado para memoria** en true.  
  
     Si la configuración se realiza mediante [!INCLUDE[tsql](../../includes/tsql-md.md)], establezca el parámetro s **\@schema_option** del procedimiento almacenado **sp_addarticle** en **0x0000080000000000**.  
  
4.  Vuelva a generar la instantánea.  
  
5.  Reinicialice la suscripción.  
  
## <a name="remarks-and-restrictions"></a>Comentarios y restricciones  
 Solo se admite la replicación transaccional unidireccional. No se admite la replicación transaccional punto a punto.  
  
 Las tablas con optimización para memoria no se pueden publicar.  
  
 Las tablas de replicación del distribuidor no se pueden configurar como tablas optimizadas para memoria.  
  
 La replicación de mezcla no puede incluir tablas optimizadas para memoria.  
  
 En el suscriptor, las tablas implicadas en la replicación transaccional se pueden configurar como tablas optimizadas para memoria, pero las tablas del suscriptor deben cumplir los requisitos de las tablas optimizadas para memoria. Esto requiere las restricciones siguientes.  
 
-   Las tablas replicadas en tablas optimizadas para memoria de un suscriptor están limitadas a los tipos de datos permitidos en las tablas optimizadas para memoria. Para obtener más información, vea [Tipos de datos admitidos para OLTP en memoria](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).  
  
-   No todas las características de Transact-SQL son compatibles con las tablas optimizadas en memoria. Vea [Construcciones Transact-SQL no admitidas por OLTP en memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md) para obtener información detallada.  
  
##  <a name="modifying-a-schema-file"></a><a name="Schema"></a> Modificar un archivo de esquema  
  
-   Si se usa la opción `DURABILITY = SCHEMA_AND_DATA` de tabla optimizada para memoria, la tabla debe tener un índice de clave principal no clúster.  
  
-   ANSI_PADDING debe ser ON.  
  
  
  

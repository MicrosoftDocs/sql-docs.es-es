---
description: Establecer una base de datos en modo de usuario único
title: Establecer una base de datos en modo de usuario único | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- single-user mode [SQL Server], database option
ms.assetid: fb5254eb-b635-4b39-8361-136fd36f2b1f
author: stevestein
ms.author: sstein
ms.openlocfilehash: 68d806e83c389c256737da8054a788697f0bd773
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812992"
---
# <a name="set-a-database-to-single-user-mode"></a>Establecer una base de datos en modo de usuario único
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo establecer una base de datos definida por el usuario en modo de usuario único en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. El modo de usuario único se suele utilizar para operaciones de mantenimiento y especifica que solo un usuario puede tener acceso a la base de datos cada vez.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Requisitos previos](#Prerequisites)  
  
     [Seguridad](#Security)  
  
-   **Para establecer una base de datos en modo de usuario único, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Si hay otros usuarios conectados a la base de datos en el momento de establecer la base de datos en modo de usuario único, sus conexiones a la base de datos se cerrarán sin previo aviso.  
  
-   La base de datos permanece en modo de usuario único incluso si el usuario que estableció la opción cierra la sesión. A partir de ese momento, un usuario distinto, pero solo uno, puede conectarse a la base de datos.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Antes de establecer la base de datos como SINGLE_USER, compruebe que la opción AUTO_UPDATE_STATISTICS_ASYNC está establecida en OFF. Cuando esta opción se establece en ON, el subproceso en segundo plano que se usa para actualizar las estadísticas realiza una conexión con la base de datos y no se podrá tener acceso a la base de datos en modo de usuario único. Para obtener más información, vea [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-set-a-database-to-single-user-mode"></a>Para establecer una base de datos en modo de usuario único  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]y, a continuación, expándala.  
  
2.  Haga clic con el botón derecho en la base de datos que quiera cambiar y haga clic en **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades de la base de datos** , haga clic en la página **Opciones** .  
  
4.  En la opción **Restringir acceso** , seleccione el modo único ( **Single**).  
  
5.  Si hay otros usuarios conectados a la base de datos, aparecerá un mensaje **Conexiones abiertas** . Para cambiar la propiedad y cerrar el resto de conexiones, haga clic en **Sí**.  
  
 También puede utilizar este procedimiento para establecer la base de datos en modo de acceso múltiple (Multiple) o restringido (Restricted). Para obtener más información sobre las opciones de Restringir acceso, vea [Propiedades de la base de datos &#40;página Opciones&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-set-a-database-to-single-user-mode"></a>Para establecer una base de datos en modo de usuario único  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo la base de datos se establece en el modo `SINGLE_USER` para obtener acceso exclusivo. A continuación, el ejemplo establece el estado de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en `READ_ONLY` y devuelve el acceso a la base de datos para todos los usuarios. La opción de terminación `WITH ROLLBACK IMMEDIATE` se especifica en la primera instrucción `ALTER DATABASE` . Esto hará que todas las transacciones incompletas se reviertan y que el resto de las conexiones a la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] se desconecten de inmediato.  
  
 [!code-sql[DatabaseDDL#AlterDatabase8](../../relational-databases/databases/codesnippet/tsql/set-a-database-to-single_1.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
  

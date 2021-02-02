---
title: Consulta o modificación de las propiedades de una condición de administración basada en directivas
description: Obtenga información sobre cómo ver o modificar las propiedades de una condición de administración basada en directivas para SQL Server mediante SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: ''
ms.date: 10/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view policy conditions
- Policy-Based Management, modify policy conditions
ms.assetid: 890d7384-8444-4767-bb6f-f5debb155747
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ffdb11c75726469d61c54c9a2cc12ff18a31ca1a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076187"
---
# <a name="view-or-modify-the-properties-of-a-policy-based-management-condition"></a>Ver o modificar las propiedades de una condición de administración basada en directivas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo ver o modificar las propiedades de una condición de administración basada en directivas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  

  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-view-or-modify-a-conditions-properties"></a>Para ver o modificar las propiedades de una condición  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor que contiene la condición que desea ver o modificar.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic en el signo más para expandir la carpeta **Administración de directivas**.  
  
4.  Haga clic en el signo más para expandir la carpeta **Condiciones** .  
  
5.  Haga clic con el botón derecho en la condición que quiere ver o editar y seleccione **Propiedades**. Para más información sobre las opciones disponibles en el cuadro de diálogo **Abrir condición: -** _nombre_de_condición_, vea [Cuadro de diálogo Crear nueva condición o Abrir condición, página General](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md), [Cuadro de diálogo Abrir condición, página Directivas dependientes](../../relational-databases/policy-based-management/open-condition-dialog-box-dependent-policies-page.md), [Cuadro de diálogo Crear nueva condición o Abrir condición, página Descripción](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md) y [Cuadro de diálogo Edición avanzada &#40;condición&#41;](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md).  
  
6.  Cuando termine, haga clic en **Aceptar**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-view-a-conditions-properties"></a>Para ver las propiedades de una condición  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb;  
    GO  
    SELECT name,  
       description,  
       facet,  
       expression,  
       is_name_condition,  
       obj_name  
    FROM syspolicy_conditions;  
    GO  
  
    ```  
  
 Para obtener más información, vea [syspolicy_conditions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syspolicy-conditions-transact-sql.md).  
  
  

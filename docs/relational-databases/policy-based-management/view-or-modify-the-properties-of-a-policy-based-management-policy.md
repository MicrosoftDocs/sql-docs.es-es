---
title: Consulta o modificación de las propiedades de una directiva de administración basada en directivas
description: Obtenga información sobre cómo ver o modificar las propiedades de una directiva de administración basada en directivas para SQL Server mediante SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 10/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, modify policies
- Policy-Based Management, view policies
ms.assetid: ba805504-5db5-4731-a8da-a0e89cb20c37
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1947e5d6b1363cfab43607882f09988074d25eed
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076993"
---
# <a name="view-or-modify-the-properties-of-a-policy-based-management-policy"></a>Ver o modificar las propiedades de una directiva de administración basada en directivas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo ver o modificar las propiedades de una directiva de administración basada en directivas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-all-policies-on-an-object"></a>Para ver las propiedades de todas las directivas de un objeto  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor, objeto de servidor, base de datos u objeto de base de datos, elija **Directivas** y seleccione **Ver**. Para más información sobre las opciones disponibles en el cuadro de diálogo **Ver directivas -** _nombre_de_objeto_, vea [Cuadro de diálogo Ver directivas](../../relational-databases/policy-based-management/view-policies-dialog-box.md).  
  
2.  Cuando termine, haga clic en **Cerrar**.  
  
#### <a name="to-view-or-modify-a-specific-policys-properties"></a>Para ver o modificar las propiedades de una directiva específica  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor que contiene la directiva de administración basada en directivas que quiera ver o modificar.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic en el signo más para expandir la carpeta **Administración de directivas**.  
  
4.  Haga clic en el signo más para expandir la carpeta **Directivas** .  
  
5.  Haga clic con el botón derecho en la directiva que quiera ver o editar y seleccione **Propiedades**. Para más información sobre las opciones disponibles en el cuadro de diálogo **Abrir directiva -** _nombre_de_directiva_, vea [Cuadro de diálogo Crear nueva directiva o Abrir directiva, página General](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-general-page.md) y [Cuadro de diálogo Crear nueva directiva o Abrir directiva, página Descripción](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-description-page.md).  
  
6.  Cuando termine, haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-view-a-policys-properties"></a>Para ver las propiedades de una directiva  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb;  
    GO  
    SELECT name,  
       execution_mode,  
       description,  
       is_enabled,  
       job_id  
    FROM syspolicy_policies;  
    GO  
    ```  
  
 Para obtener más información, vea [syspolicy_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syspolicy-policies-transact-sql.md).  
  
  

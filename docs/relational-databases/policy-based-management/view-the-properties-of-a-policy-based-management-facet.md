---
title: Consulta de las propiedades de una faceta de administración basada en directivas
description: Obtenga información sobre cómo ver las propiedades de una faceta de administración basada en directivas en SQL Server mediante SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view facet properties
ms.assetid: 022a244c-c2e7-4467-b9a2-c7a27859be22
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 97f5cbfccd7cc086163b73642d0c6ec3e928427a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076197"
---
# <a name="view-the-properties-of-a-policy-based-management-facet"></a>Ver las propiedades de una faceta de administración basada en directivas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo ver las propiedades de una faceta de administración basada en directivas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para ver propiedades de una faceta mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-a-facet"></a>Para ver las propiedades de una faceta  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor que contiene la faceta cuyas propiedades desea ver.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic en el signo más para expandir la carpeta **Administración de directivas**.  
  
4.  Haga clic en el signo más para expandir la carpeta **Facetas** .  
  
5.  Haga clic con el botón derecho en la faceta cuyas propiedades quiere ver y seleccione **Propiedades**. Para más información sobre las opciones disponibles en el cuadro de diálogo **Propiedades de faceta -** _nombre_de_faceta_, vea [Cuadro de diálogo Propiedades de faceta, página General](../../relational-databases/policy-based-management/facet-properties-dialog-box-general-page.md), [Cuadro de diálogo Propiedades de faceta, página Directivas dependientes](../../relational-databases/policy-based-management/facet-properties-dialog-box-dependent-policies-page.md) y [Cuadro de diálogo Propiedades de faceta, página Condiciones dependientes](../../relational-databases/policy-based-management/facet-properties-dialog-box-dependent-conditions-page.md).  
  
6.  Cuando termine, haga clic en **Cerrar**.  


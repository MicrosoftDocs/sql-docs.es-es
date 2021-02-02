---
description: Crear una nueva condición de administración basada en directivas.
title: Creación de una nueva condición de administración basada en directivas | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policy conditions
ms.assetid: 8a612f7e-6c70-49db-a4de-48431e097cc5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f8eef68a189d9e3c3c8cce16de803ce3b87f0207
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048825"
---
# <a name="create-a-new-policy-based-management-condition"></a>Crear una nueva condición de administración basada en directivas.
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear una condición de administración basada en directivas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para crear una condición mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-condition"></a>Para crear una condición  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor en el que quiera crear una condición de administración basada en directivas.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic en el signo más para expandir la carpeta **Administración de directivas**.  
  
4.  Haga clic en el signo más para expandir la carpeta **Facetas** .  
  
5.  Haga clic con el botón derecho en la faceta en la que quiere crear una nueva condición y seleccione **Nueva condición**.  
  
6.  En el cuadro de diálogo **Crear nueva condición** , en el cuadro **Nombre** , escriba el nombre de la nueva condición.  
  
7.  Confirme que la faceta es correcta en la lista **Faceta** o seleccione una faceta diferente.  
  
8.  En **Expresión**, construya las expresiones de condiciones seleccionando una propiedad de faceta en el cuadro **Campo** , junto con su operador y valor asociados. Si agrega varias expresiones, estas pueden combinarse con **Y** u **O**. Para obtener más información sobre las opciones disponibles en este cuadro de diálogo, vea [Cuadro de diálogo Crear nueva condición o Abrir condición, página General](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md), [Cuadro de diálogo Crear nueva condición o Abrir condición, página Descripción](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md) y [Cuadro de diálogo Edición avanzada &#40;condición&#41;](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md).  
  
9. Cuando termine, haga clic en **Aceptar**.  
  
  

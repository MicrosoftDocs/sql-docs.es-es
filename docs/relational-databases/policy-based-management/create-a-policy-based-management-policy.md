---
description: Crear una directiva de administración basada en directivas
title: Creación de una directiva de administración basada en directivas | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policies
ms.assetid: b28bf963-89f9-4941-b6c1-6004fec347f1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ccc76880687d509b59e3171f1b6dfaa9029ad6f5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048804"
---
# <a name="create-a-policy-based-management-policy"></a>Crear una directiva de administración basada en directivas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear una directiva de administración basada en directivas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para crear una directiva mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-policy"></a>Para crear una directiva  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor en el que quiere crear una directiva de administración basada en directivas.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic en el signo más para expandir la carpeta **Administración de directivas**.  
  
4.  Haga clic con el botón derecho en la carpeta **Directivas** y, después, seleccione **Nueva directiva**.  
  
5.  En el cuadro de diálogo **Crear nueva directiva** , en el cuadro **Nombre** , escriba el nombre de la nueva directiva.  
  
6.  Si desea que la directiva se habilite en cuanto se cree, active la casilla **Habilitado** . Si el modo de evaluación es **A petición**, la casilla **Habilitado** no está disponible.  
  
7.  En la lista **Condición de comprobación** , seleccione una de las condiciones existentes o seleccione **Nueva condición**. Para modificar una condición, selecciónela y, después, haga clic en los puntos suspensivos ( **...** ). Para obtener más información, vea [Crear una nueva condición de administración basada en directivas.](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md) o [Ver o modificar las propiedades de una condición de administración basada en directivas](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md).  
  
8.  En el cuadro **Para destinos** , seleccione uno o varios tipos de destino para esta directiva. Algunas condiciones y facetas solo se pueden aplicar a ciertos tipos de destinos. Los conjuntos de destinos disponibles aparecen en el cuadro asociado. Expanda **Cada** para seleccionar una condición de filtrado para algunos tipos de destinos. Si en este cuadro no aparece ningún destino, la condición de comprobación se investiga en el nivel de servidor.  
  
9. En el cuadro **Modo de evaluación** , seleccione cómo se comportará esta directiva. Condiciones diferentes pueden tener distintos modos de evaluación válidos. Para obtener más información sobre los modos de evaluación válidos, vea [Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).  
  
10. Si la directiva se va a evaluar según una programación, establezca el modo de evaluación en **Al programar** y, a continuación, haga clic en **Seleccionar** para seleccionar una programación o haga clic en **Nuevo** para crear una nueva programación.  
  
11. Para limitar la directiva al subconjunto de los tipos de destino, en el cuadro **Restricción de servidor** , seleccione las condiciones de limitación o cree una condición nueva.  
  
     Para obtener más información acerca de las opciones disponibles en el cuadro de diálogo **Crear nueva directiva** , vea [Cuadro de diálogo Crear nueva directiva o Abrir directiva, página General](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-general-page.md) o [Cuadro de diálogo Crear nueva directiva o Abrir directiva, página Descripción](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-description-page.md).  
  
12. Cuando termine, haga clic en **Aceptar**.  


---
description: Importar una directiva de administración basada en directivas
title: Importar una directiva de administración basada en directivas | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, import policy
ms.assetid: 850b7ef9-d2b7-4754-bf04-7cb419ffb776
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d843da0059418602778c0b0b0b2de6f954a55e43
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048998"
---
# <a name="import-a-policy-based-management-policy"></a>Importar una directiva de administración basada en directivas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo importar una instancia de directiva de administración basada en directivas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Permisos
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.

  
##  <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio  
  
### <a name="to-import-a-policy-instance"></a>Para importar una instancia de directiva  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor en que residirá la instancia de directiva recién importada.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic en el signo más para expandir la carpeta **Administración de directivas**.  
  
4.  Haga clic con el botón derecho en la carpeta **Directivas** y seleccione **Importar directiva**.  
  
5.  En el cuadro de diálogo **Importar** , escriba la ruta de acceso y el nombre del archivo, o use el botón Examinar (**...**) para buscar el archivo XML que contiene la directiva y, después, seleccione el archivo. Para obtener más información acerca de las opciones disponibles en el cuadro de diálogo **Importar** , vea [Import Policies Dialog Box](../../relational-databases/policy-based-management/import-policies-dialog-box.md).  
  
6.  Cuando termine, haga clic en **Aceptar**.  


## <a name="example-policies"></a>Directivas de ejemplo
 Las directivas de ejemplo están disponibles en el [repositorio de código de ejemplo de SQL Server](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/epm-framework/sample-policies). Estas directivas se pueden importar y usar como base para sus propias directivas de administración basada en directivas.

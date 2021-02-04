---
description: Crear o editar un grupo de servidores (SQL Server Management Studio)
title: Crear o editar un grupo de servidores
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.editgroup.f1
- sql13.swb.newgroup.f1
helpviewer_keywords:
- Registered Servers [SQL Server], server groups
- server groups [SQL Server]
- groups [SQL Server], server
ms.assetid: d4a942bd-2dd1-42db-ad0e-e9a9ae5b856d
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: c180d44cf3ea9a0c878168eba4257e880c1ba965
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250941"
---
# <a name="create-or-edit-a-server-group-sql-server-management-studio"></a>Crear o editar un grupo de servidores (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo organizar los servidores de Servidores registrados en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] creando grupos de servidores y poniendo los servidores en los grupos de servidores. Puede crear grupos de servidores en cualquier momento en Servidores registrados o cuando registra los servidores.  

## <a name="SSMSProcedure"></a>

### <a name="to-create-a-server-group-in-registered-servers"></a>Para crear un grupo de servidores en Servidores registrados  

1. En Servidores registrados, haga clic en el tipo de servidor en la barra de herramientas Servidores registrados. Si Servidores registrados no está visible, haga clic en **Servidores registrados** en el menú **Ver** .  

2. Haga clic con el botón derecho en un servidor o un grupo de servidores, seleccione **Nuevo** y, después, haga clic en **Grupo de servidores**.  

3. En el cuadro de diálogo **Nuevo grupo de servidores** , en el cuadro de lista **Nombre del grupo** , escriba un nombre único para el grupo de servidores. El nombre del grupo de servidores debe ser único en la ubicación actual del árbol Servidores registrados.

4. En el cuadro de lista **Descripción del grupo** , puede escribir, si lo desea, un nombre descriptivo del grupo, por ejemplo, "Servidores financieros para América Latina".  

5. En el cuadro **Seleccionar una ubicación para el nuevo grupo de servidores** , haga clic en la ubicación en la que desee colocar el grupo y, a continuación, haga clic en **Guardar**.  

   > [!NOTE]
   >  También puede crear un nuevo grupo de servidores mientras está registrando un servidor. Para ello, haga clic en **Nuevo grupo** y, a continuación, rellene el cuadro de diálogo **Nuevo grupo** .  

## <a name="see-also"></a>Consulte también

[Registrar servidores](./register-servers.md)
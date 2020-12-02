---
description: How to Manage a CDC Instance
title: Cómo administrar una instancia CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 5d9e677f-b872-497d-9cde-472184a214ab
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c4c09c590d92d97ea56c795db6fe8676d5771277
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88426057"
---
# <a name="how-to-manage-a-cdc-instance"></a>How to Manage a CDC Instance

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En este procedimiento se describe cómo usar la Consola del diseñador CDC para administrar las operaciones de la instancia CDC en tiempo de ejecución.  
  
### <a name="to-manage-cdc-instance-operations"></a>Para administrar las operaciones de la instancia CDC  
  
1.  En el menú **Inicio** , seleccione **Consola del diseñador CDC**.  
  
2.  En el panel izquierdo, expanda **Captura de datos modificados** y expanda después el servicio que contiene la instancia que desea ver.  
  
3.  Seleccione el nombre de una instancia con la que desee trabajar.  
  
4.  En el panel **Acciones** situado en el lado derecho de la Consola del diseñador CDC, haga clic en la operación que desee realizar.  
  
     También puede hacer clic con el botón secundario en el nombre de la instancia en el panel izquierdo y seleccionar la operación que desee realizar.  
  
     Puede realizar las tareas siguientes:  
  
    -   **Iniciar**: para empezar a capturar cambios.  
  
    -   **Detener**: para dejar de capturar cambios.  
  
    -   **Restablecer**: haga clic en **Restablecer** para restablecer la instancia CDC a su estado inicial (vacío). Esta opción está disponible cuando la instancia CDC está detenida. Se eliminan todos los cambios de las tablas de cambios y el estado interno de la instancia CDC. Cuando se inicie la instancia CDC más adelante, la captura de cambios comenzará desde ese momento y solo incluirá las transacciones que comenzaron después de que se iniciara la instancia CDC.  
  
    -   **Eliminar**: para eliminar la instancia CDC.  
  
    -   **Script de registro de Oracle**: haga clic en **Oracle Logging Script** (Script de registro de Oracle) para mostrar el cuadro de diálogo Script de registro de Oracle con el script de registro complementario de Oracle. Para obtener información acerca de lo que puede hacer en este cuadro de diálogo, vea [Oracle Supplemental Logging Script](../../integration-services/change-data-capture/oracle-supplemental-logging-script.md).  
  
         **Nota**: al ejecutar los scripts de registro complementario, se abre el cuadro de diálogo Credenciales de Oracle para ejecutar script donde debe proporcionar un nombre de usuario y una contraseña válidos de Oracle. Para obtener información acerca de cómo proporcionar las credenciales adecuadas de Oracle, vea [Oracle Credentials for Running Script](../../integration-services/change-data-capture/oracle-credentials-for-running-script.md).  
  
    -   **Implementación de instancia CDC**: para generar un script de implementación para la instancia CDC. Para obtener información acerca de este cuadro de diálogo, vea [CDC Instance Deployment Script](../../integration-services/change-data-capture/cdc-instance-deployment-script.md).  
  
     Para obtener más información acerca de estas tareas, vea [Manage a CDC Instance](../../integration-services/change-data-capture/manage-a-cdc-instance.md).  
  
 También puede seleccionar **Propiedades** para editar las propiedades de configuración de la instancia CDC. Para obtener más información acerca de cómo editar las propiedades de la instancia CDC, vea [Edit Instance Properties](../../integration-services/change-data-capture/edit-instance-properties.md).  
  
  

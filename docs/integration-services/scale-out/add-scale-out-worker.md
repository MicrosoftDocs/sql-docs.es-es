---
title: Agregar un trabajo de escalabilidad horizontal de SSIS con el administrador de escalabilidad horizontal | Microsoft Docs
description: En este artículo se describe cómo agregar un trabajo de Escalabilidad horizontal de SSIS a un entorno de escalabilidad horizontal existente mediante el Administrador de escalabilidad horizontal.
ms.custom: performance
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 961ea14fc7663206a4d1ad86c6ca611be0f863d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354894"
---
# <a name="add-a-scale-out-worker-with-scale-out-manager"></a>Agregar un trabajo de escalabilidad horizontal con el administrador de escalabilidad horizontal

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



El administrador de escalabilidad horizontal de Integration Services simplifica el proceso de agregar un trabajo de escalabilidad horizontal al entorno existente de escalabilidad horizontal. 

Siga estos pasos para agregar un trabajo de escalabilidad horizontal a su topología de escalabilidad horizontal:

## <a name="1-install-scale-out-worker"></a>1. Instalar el trabajo de escalabilidad horizontal
En el Asistente para la instalación de SQL Server, seleccione Integration Services y Trabajo de escalabilidad horizontal en la página **Selección de características**. 
![Selección de características: Worker](media/feature-select-worker.PNG)

En la página **Configuración de escalabilidad horizontal de Integration Services: nodo de trabajo**, puede hacer clic en **Siguiente** para omitir la configuración por ahora y utilizar el **Administrador de escalabilidad horizontal** para efectuar la configuración después de la instalación.

Finalice el asistente para la instalación.

## <a name="2-open-the-firewall-on-the-scale-out-master-computer"></a>2. Abrir el firewall en el equipo del Servicio principal de escalabilidad horizontal
Abra el puerto especificado durante la instalación del Servicio principal de escalabilidad horizontal (8391, de manera predeterminada) y el puerto de SQL Server (1433, de manera predeterminada) en el Firewall de Windows del equipo en el que se ejecute.

## <a name="3-add-a-scale-out-worker-with-scale-out-manager"></a>3. Agregar un trabajo de escalabilidad horizontal con el administrador de escalabilidad horizontal
Ejecute SQL Server Management Studio como administrador y conéctese a la instancia de SQL Server del patrón de escalabilidad horizontal.

En el Explorador de objetos, haga clic con el botón derecho en **SSISDB** y seleccione **Administrar escalabilidad horizontal**. 

![Administrar escalabilidad horizontal](media/manage-scale-out.PNG)

En el cuadro de diálogo **Administrador de escalabilidad horizontal**, cambie a **Administrador del trabajo**. Seleccione **+** y siga las instrucciones que aparecen en el cuadro de diálogo **Connect Worker** (Conectar el trabajo). 

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, vea [Administrador de escalabilidad horizontal](integration-services-ssis-scale-out-manager.md).

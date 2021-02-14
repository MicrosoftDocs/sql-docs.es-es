---
title: Configuración de Advanced Data Security
titleSuffix: Azure Arc
description: Configuración de Advanced Data Security para una instancia de SQL Server habilitada para Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 2d7cdc1a04cfa8f3a1697eed68913f92174f972b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063230"
---
# <a name="configure-advanced-data-security-for-azure-arc-enabled-sql-server-instance"></a>Configuración de Advanced Data Security para una instancia de SQL Server habilitada para Azure Arc

Siga estos pasos para habilitar Advanced Data Security para las instancias locales de SQL Server.

## <a name="prerequisites"></a>Requisitos previos

* La instancia de SQL Server se incorpora a SQL Server habilitado para Arc. Siga estas instrucciones para [incorporar su instancia de SQL Server a SQL Server habilitado para Arc](connect.md).

* A la cuenta de usuario se le asigna uno de los [Roles de Security Center (RBAC)](/azure/security-center/security-center-permissions)

## <a name="create-a-log-analytics-workspace"></a>Creación de un área de trabajo de Log Analytics

1. Busque el tipo de recurso __Áreas de trabajo de Log Analytics__ y agregue una nueva mediante la hoja de creación.

   ![Crear área de trabajo nueva](media/configure-advanced-data-security/create-new-log-analytics-workspace.png)

   > [!NOTE]
   > Puede usar un área de trabajo de Log Analytics en cualquier región, así que, si ya tiene una, puede usarla. Pero se recomienda crearla en la misma región en la que se ha creado el recurso __Máquina - Azure Arc__.

1. Vaya a la página de información general del recurso Área de trabajo de Log Analytics y seleccione "Windows, Linux y otros orígenes". Copie el id. del área de trabajo y la clave principal para usarlos posteriormente.

   ![Hoja del área de trabajo de Log Analytics](media/configure-advanced-data-security/log-analytics-workspace-blade.png)

## <a name="install-microsoft-monitoring-agent-mma"></a>Instalación de Microsoft Monitoring Agent (MMA)

El siguiente paso solo es necesario si aún no ha configurado el agente MMA en la máquina remota.

1. Seleccione el recurso __Máquina - Azure Arc__ para el servidor físico o virtual en el que está instalada la instancia de SQL Server y agregue la extensión __Microsoft Monitoring Agent - Azure Arc__ mediante la característica **Extensiones**. Cuando se le pida que configure el área de trabajo de Log Analytics, use el identificador del área de trabajo y la clave principal que ha guardado en el paso anterior.

   ![Instalación de MMA](media/configure-advanced-data-security/install-mma-extension.png)

1. Una vez finalizada correctamente la validación, haga clic en **Crear** para iniciar el flujo de trabajo de implementación de la extensión MMA Arc. Cuando finaliza la implementación, el estado se actualiza a **Correcto**.

1. Para obtener más detalles, vea [Administración de extensiones con Azure Arc](/azure/azure-arc/servers/manage-vm-extensions)

## <a name="enable-azure-defender"></a>Habilitación de Azure Defender

A continuación, tendrá que habilitar Azure Defender para la instancia de SQL Server.

1. Vaya a Security Center y abra la página **Precios y configuración** de la barra lateral.

1. Seleccione el área de trabajo que ha configurado para la extensión MMA en el paso anterior.

1. Seleccione **Azure Defender activado**. Asegúrese de que la opción **Servidores SQL Server en máquinas** está habilitada.

   ![Actualización del área de trabajo](media/configure-advanced-data-security/enable-azure-defender.png)

 > [!NOTE]
   > El primer análisis para generar la valoración de vulnerabilidades se produce 24 horas después de la habilitación de Advanced Data Security. Después, se realizan exámenes automáticos el domingo de cada semana.

## <a name="explore"></a>Explorar

Examine las anomalías y amenazas de seguridad en Azure Security Center.

1. Abra el recurso SQL Server - Azure Arc y seleccione **Seguridad** en el menú izquierdo para ver las recomendaciones y alertas de esa instancia.

   ![Selección de título de seguridad](media/configure-advanced-data-security/security-heading-sql-server-arc.png)

1. Haga clic en cualquiera de las recomendaciones para ver los detalles de la vulnerabilidad en __Security Center__.

   ![Informe de vulnerabilidad](media/configure-advanced-data-security/vulnerabilities-report.png)

1. Haga clic en cualquier alerta de seguridad para obtener todos los detalles y examinar el ataque en más profundidad en [Azure Sentinel](/azure/sentinel/overview). El diagrama siguiente es un ejemplo de la alerta por fuerza bruta.

   ![Alerta por fuerza bruta](media/configure-advanced-data-security/brute-force-alert.png)

1. Haga clic en **Tomar medidas** para mitigar la alerta.

   ![Mitigación de alerta](media/configure-advanced-data-security/brute-force-alert-mitigation.png)

> [!NOTE]
> El vínculo general __Security Center__ de la parte superior de la página no usa la dirección URL del portal de vista previa, por lo que los recursos __SQL Server - Azure Arc__ no se ven ahí. Se recomienda seguir los vínculos de las recomendaciones o alertas individuales.

## <a name="next-steps"></a>Pasos siguientes

Puede investigar con más detalle las alertas de seguridad y los ataques mediante [Azure Sentinel](/azure/sentinel/overview). Siga estas instrucciones para [incorporar Azure Sentinel](/azure/sentinel/connect-data-sources).
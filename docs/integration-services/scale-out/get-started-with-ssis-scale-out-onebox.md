---
title: Introducción a la escalabilidad horizontal de SSIS en un único equipo | Microsoft Docs
description: En este artículo se muestra todo lo que se necesita saber para empezar a trabajar con Escalabilidad horizontal de SSIS en un único equipo.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: 1cf9077a9b500c9e5052b48aab13d325809bb297
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347387"
---
# <a name="get-started-with-integration-services-ssis-scale-out-on-a-single-computer"></a>Introducción a la escalabilidad horizontal de SSIS en un único equipo

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


En esta sección se proporcionan las instrucciones de configuración de la escalabilidad horizontal de Integration Services en un entorno de equipo único con la configuración predeterminada.

## <a name="1-install-sql-server-features"></a>1. Instalar las características de SQL Server
En el asistente para instalación de SQL Server, en la página **Selección de características**, seleccione los elementos siguientes:
-   Servicios de Motor de base de datos
-   Integration Services
    -   Patrón de escalabilidad horizontal
    -   Trabajo de escalabilidad horizontal

![Primera mitad de la lista de selección de características](media/feature-select-onebox1.PNG)

![Segunda mitad de la lista de selección de características](media/feature-select-onebox2.PNG)

En la página **Configuración del servidor**, haga clic en **Siguiente** para aceptar las cuentas de servicio y los tipos de inicio predeterminados.

En la página **Configuración del Motor de base de datos**, seleccione **Modo mixto** y haga clic en **Agregar usuario actual**. 

![Configuración del motor](media/engine-config.PNG)

En las páginas **Configuración de escalabilidad horizontal de Integration Services: nodo principal** y **Configuración de escalabilidad horizontal de Integration Services: nodo de trabajo**, haga clic en **Siguiente** para aceptar la configuración predeterminada del puerto y los certificados.

Finalice el Asistente para la instalación de SQL Server.

## <a name="2-install-sql-server-management-studio"></a>2. Instalación de SQL Server Management Studio

Descargue e instale [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="3-enable-scale-out"></a>3. Habilitar la escalabilidad horizontal
Abra SSMS y conéctese a la instancia local de SQL Server.
En el Explorador de objetos, haga clic con el botón derecho en **Catálogos de Integration Services** y seleccione **Crear catálogo**.

En el cuadro de diálogo **Crear catálogo**, la opción **Habilitar este servidor como servicio principal de Escalabilidad horizontal de SSIS** está activada de forma predeterminada.

## <a name="4-enable-a-scale-out-worker"></a>4. Habilitar un trabajo de escalabilidad horizontal
En SSMS, haga clic con el botón derecho en **SSISDB** y seleccione **Administrar escalabilidad horizontal**. 

![Administrar escalabilidad horizontal](media/manage-scale-out.PNG)

Se abrirá la aplicación del Administrador de escalabilidad horizontal de Integration Services. Para obtener más información, vea [Administrador de escalabilidad horizontal](integration-services-ssis-scale-out-manager.md).

Para habilitar un trabajo de escalabilidad horizontal, pase al **Administrador del trabajo** y seleccione el que quiera habilitar. Los trabajos están deshabilitados de manera predeterminada. Haga clic en **Habilitar trabajo** para habilitar el trabajo seleccionado.

## <a name="5-run-packages-in-scale-out"></a>5. Ejecutar paquetes en el escalado horizontal
Ahora ya está listo para ejecutar paquetes SSIS en escalabilidad horizontal. Para obtener más información, vea [Ejecutar paquetes en la escalabilidad horizontal de Integration Services (SSIS)](run-packages-in-integration-services-ssis-scale-out.md).

## <a name="next-steps"></a>Pasos siguientes
-   [Agregue un trabajo de escalabilidad horizontal con el administrador de escalabilidad horizontal](add-scale-out-worker.md).

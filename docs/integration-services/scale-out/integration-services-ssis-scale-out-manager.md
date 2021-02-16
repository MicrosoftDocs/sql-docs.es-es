---
title: Administrador de escalabilidad horizontal de SQL Server Integration Services | Microsoft Docs
description: En este artículo se describe la herramienta Administrador de escalado horizontal que se puede usar para administrar la escalabilidad horizontal de SSIS.
ms.custom: performance
ms.date: 12/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: a56792633259e2f8d224696f94c7095a0ec1ae96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347420"
---
# <a name="integration-services-scale-out-manager"></a>Administrador de escalabilidad horizontal de Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



El administrador de escalabilidad horizontal es una herramienta que permite administrar toda la topología de escalabilidad horizontal de SSIS desde una sola aplicación. De este modo, es posible evitar tener que realizar las tareas de administración y ejecutar los comandos Transact-SQL en varios equipos.

## <a name="open-scale-out-manager"></a>Abrir el Administrador de escalabilidad horizontal

Hay dos maneras de abrir el Administrador de escalabilidad horizontal.

### <a name="1-open-scale-out-manager-from-sql-server-management-studio"></a>1. Abrir el administrador de escalabilidad horizontal desde SQL Server Management Studio
Abra SQL Server Management Studio (SSMS) y conéctese a la instancia de SQL Server del Administrador de escalabilidad horizontal.

En el Explorador de objetos, haga clic con el botón derecho en **SSISDB** y seleccione **Administrar escalabilidad horizontal**.

![Administrar escalabilidad horizontal](media/manage-scale-out.PNG)

> [!NOTE]
> Se recomienda ejecutar SSMS como administrador, ya que algunas de las operaciones de administración de la escalabilidad horizontal, como agregar un trabajo de escalabilidad horizontal, requieren privilegios administrativos.

### <a name="2-open-scale-out-manager-by-running-managementtoolexe"></a>2. Abrir el Administrador de escalabilidad horizontal mediante la ejecución de ManagementTool.exe

Busque `ManagementTool.exe` en `%SystemDrive%\Program Files (x86)\Microsoft SQL Server\150\DTS\Binn\Management`. Haga clic con el botón derecho en **ManagementTool.exe** y seleccione **Ejecutar como administrador**. 

Cuando se abra, escriba el nombre de la instancia de SQL Server del Servicio principal de escalabilidad horizontal y conéctese para administrar el entorno de escalabilidad horizontal.

![Conexión del portal](media/portal-connect-new.png)

## <a name="tasks-available-in-scale-out-manager"></a>Tareas disponibles en el Administrador de escalabilidad horizontal
En el Administrador de escalabilidad horizontal, puede hacer lo siguiente:

### <a name="enable-scale-out"></a>Habilitar la escalabilidad horizontal
Después de conectarse a SQL Server, si la escalabilidad horizontal no está habilitada, puede hacer clic en **Habilitar**.

![Habilitar la escalabilidad horizontal en el portal](media/portal-enable-scale-out-new.PNG) 

### <a name="view-scale-out-master-status"></a>Ver el estado del patrón de escalabilidad horizontal
El estado del patrón de escalabilidad horizontal se muestra en la página **Panel**.

![Panel del portal](media/portal-dashboard-new.PNG)

### <a name="view-scale-out-worker-status"></a>Ver el estado del trabajo de escalabilidad horizontal
El estado del trabajo de escalabilidad horizontal se muestra en la página **Administrador del trabajo**. Puede seleccionar cada trabajo para ver su estado de forma individual.

![Administrador del trabajo del portal](media/portal-worker-manager-new.PNG)

### <a name="add-a-scale-out-worker"></a>Agregar un trabajo de escalabilidad horizontal
Para agregar un trabajo de escalabilidad horizontal, haga clic en **+** , en la parte inferior de la lista de trabajos de escalabilidad horizontal. 

Escriba el nombre del equipo del trabajo de escalabilidad horizontal que quiera agregar y haga clic en **Validar**. El Administrador de escalabilidad horizontal comprobará si el usuario actual tiene acceso a los almacenes de certificados del Servicio principal de escalabilidad horizontal y a los equipos de trabajo de escalabilidad horizontal.

![Conectar el trabajo](media/connect-worker-new.PNG)

Si la validación es correcta, el Administrador de escalabilidad horizontal intentará leer el archivo de configuración del servidor del trabajo y obtener la huella digital del certificado de este. Para obtener más información, vea [Trabajador de escalabilidad horizontal](integration-services-ssis-scale-out-worker.md). Si no es capaz de leer el archivo de configuración del trabajo, existen dos alternativas que puede utilizar para proporcionar el certificado de trabajo. 

- Puede especificar directamente la huella digital del certificado de trabajo.

    ![Certificado de trabajo 1](media/portal-cert1-new.PNG)

- O bien, puede proporcionar el archivo del certificado.

    ![Certificado de trabajo 2](media/portal-cert2-new.PNG)

Después de recopilar toda la información, el Administrador de escalabilidad horizontal le indicará qué acciones debe realizar. Normalmente, estas acciones incluyen la instalación del certificado, la actualización del archivo de configuración de servicio del trabajo y el reinicio del servicio del trabajo.

![Agregar confirmación 1 en el portal](media/portal-add-confirm1-new.PNG)

En caso de que no se pueda acceder a la configuración del trabajo, tendrá que actualizarla manualmente y reiniciar el servicio del trabajo.

![Agregar confirmación 2 en el portal](media/portal-add-confirm2-new.PNG)

Active la casilla **Confirmar** y, a continuación, haga clic en **Aceptar** para empezar a agregar un trabajo de escalabilidad horizontal.

### <a name="delete-a-scale-out-worker"></a>Eliminación del trabajo de escalabilidad horizontal
Para eliminar un trabajo de escalabilidad horizontal, selecciónelo y haga clic en **-** , en la parte inferior de la lista del trabajo en cuestión.

### <a name="enable-or-disable-a-scale-out-worker"></a>Habilitar o deshabilitar un trabajo de escalabilidad horizontal
Para habilitar o deshabilitar un trabajo de escalabilidad horizontal, selecciónelo y haga clic en **Habilitar trabajo** o **Deshabilitar trabajo**. Si el trabajo dispone de conexión, el estado del trabajo que aparece en el Administrador de escalabilidad horizontal cambiará según corresponda.

## <a name="edit-a-scale-out-worker-description"></a>Editar la descripción de un trabajo de escalabilidad horizontal
Para editar la descripción de un trabajo de escalabilidad horizontal, seleccione el trabajo en cuestión y haga clic en **Editar**. Cuando termine de editar la descripción, haga clic en **Guardar**.

![Guardar el trabajo en el portal](media/portal-save-worker-new.PNG)

## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte los siguientes artículos:
-   [Servicio principal de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Trabajo de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)

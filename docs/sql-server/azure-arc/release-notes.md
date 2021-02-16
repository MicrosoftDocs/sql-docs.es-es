---
title: 'SQL Server habilitado para Azure Arc: notas de la versión'
description: Notas de la versión más recientes
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: f53731685f5ba1723ebdd8d20064342808205566
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070310"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>Notas de la versión: SQL Server habilitado para Azure Arc (versión preliminar)

> [!NOTE]
> Como característica en versión preliminar, la tecnología que se presenta en este artículo está sujeta a los [términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="december-2020"></a>Diciembre de 2020

### <a name="breaking-change"></a>Cambio importante

En esta versión se incluye un [proveedor de recursos](/azure/azure-resource-manager/management/azure-services-resource-providers) actualizado denominado `Microsoft.AzureArcData`. Para poder seguir usando SQL Server habilitado para Azure Arc, debe registrar este proveedor de recursos. Vea las instrucciones de registro del proveedor de recursos en la sección [Requisitos previos](connect.md#prerequisites).

Si tiene recursos de SQL Server: Azure Arc existentes, siga estos pasos para migrarlos al espacio de nombres Microsoft.AzureArcData.

1. Inicie [Cloud Shell](https://shell.azure.com/). Para obtener más información, [lea más sobre PowerShell en Cloud Shell](/azure/cloud-shell/quickstart-powershell).

2. Cargue el script al shell con el comando siguiente:

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/manage/azure-arc-enabled-sql-server/migrate-to-azure-arc-data.ps1 -o migrate-to-azure-arc-data.ps1
    ```
3. Ejecute el script.  

    ```console
   ./migrate-to-azure-arc-data.ps1
    ```

> [!NOTE]
> - Para pegar los comandos en el shell, use `Ctrl-Shift-V` en Windows o `Cmd-v` en MacOS.
> - El comando `curl` copiará el script directamente en la carpeta principal asociada a la sesión de Cloud Shell.
> - El script le pedirá el nombre del grupo de recursos y generará un mensaje cuando se haya completado la migración.

### <a name="other-changes"></a>Otros cambios

* Se ha cambiado el nombre de la propiedad *TCPPorts* en el tipo de recurso de **SQL Server: Azure Arc** a *TCPStaticPorts*.
* Los permisos necesarios no son tan amplios como antes. Vea la sección [Permiso necesario](overview.md#required-permissions) para obtener más información.

### <a name="known-issues"></a>Problemas conocidos

* La propiedad *CreateTime* no se agregará a los recursos recién creados en el espacio de nombres AzureArcData, incluidos los de **SQL Server: Azure Arc**.

## <a name="october-2020"></a>Octubre de 2020

La actualización de octubre incluye las mejoras siguientes:

* La hoja Register Azure Arc enabled SQL Server (Registrar SQL Server habilitado para Azure Arc) ahora incluye la pestaña **Etiquetas**. Las etiquetas se incluyen en el script de registro y se reflejan en los recursos **SQL Server: Azure Arc**. Para obtener más información, vea [Conexión de la instancia de SQL Server a Azure Arc](connect.md).

* La entrada **Estado del entorno** ahora admite la activación de **SQL Assessment** desde el portal mediante la implementación de una extensión *CustomScriptExtension*. Para obtener más información, vea [Configuración de SQL Assessment](assess.md#run-on-demand-sql-assessment).

### <a name="known-issues"></a>Problemas conocidos

Los problemas siguientes se aplican a la versión de octubre:

* La conexión de instancias de SQL Server a Azure Arc requiere una cuenta que disponga de un amplio conjunto de permisos. Para obtener más información, vea [Permisos necesarios](overview.md#required-permissions).

## <a name="september-2020"></a>Septiembre de 2020

SQL Server habilitado para Azure Arc se publica como versión preliminar pública. SQL Server habilitado para Azure Arc extiende los servicios de Azure a instancias de SQL Server hospedadas fuera de Azure en el centro de datos del cliente, en el sistema perimetral o en un entorno de varias nubes.

Para obtener más información, vea [SQL Server habilitado para Azure Arc (versión preliminar)](overview.md).

### <a name="known-issues"></a>Problemas conocidos

Los problemas siguientes se aplican a la versión de septiembre:

* La hoja **Register Azure Arc enabled SQL Server** (Registrar SQL Server habilitado para Azure Arc) no admite la configuración de etiquetas personalizadas. Para agregar etiquetas personalizadas, abra el recurso **SQL Server: Azure Arc** tras el registro y cambie las etiquetas de la página **Información general**.

* La conexión de instancias de SQL Server a Azure Arc requiere una cuenta que disponga de un amplio conjunto de permisos. Para obtener más información, vea [Permisos necesarios](overview.md#required-permissions).

## <a name="next-steps"></a>Pasos siguientes

**¿Solo desea realizar algunas pruebas?**  Empiece a trabajar rápidamente con el [inicio rápido de SQL Server habilitado para Azure Arc](https://aka.ms/AzureArcSqlServerJumpstart).
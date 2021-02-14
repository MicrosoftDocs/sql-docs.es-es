---
title: Conexión a Azure Arc
titleSuffix: ''
description: Conexión de una instancia de SQL Server a Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 03841af487d6c715357b543798cbb6ac3c6d853e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078271"
---
# <a name="connect-your-sql-server-to-azure-arc"></a>Conexión de la instancia de SQL Server a Azure Arc

Puede conectar su instancia de SQL Server local a Azure Arc siguiendo estos pasos.

## <a name="prerequisites"></a>Requisitos previos

* La máquina tiene al menos una instancia de SQL Server instalada.
* El proveedor de recursos **Microsoft.AzureArcData** se ha registrado con uno de los métodos siguientes:  
    * Con Azure Portal:
        - Seleccione **Suscripciones**. 
        - Elija una suscripción
        - En **Configuración**, seleccione **Proveedores de recursos**.
        - Busque `Microsoft.AzureArcData` y seleccione **Registrar**.
    * Con PowerShell, ejecute `Register-AzResourceProvider -ProviderNamespace Microsoft.AzureArcData`.
    * Con la CLI, ejecute `az provider register --namespace 'Microsoft.AzureArcData`.

## <a name="generate-a-registration-script-for-sql-server"></a>Generación de un script de registro para SQL Server

En este paso, se generará un script que detecta todas las instancias de SQL Server instaladas en la máquina y las registra como recursos de __SQL Server: Azure Arc__. Si la máquina virtual o física que hospeda no está registrada con Azure Arc, el script lo hace automáticamente.

1. Busque el tipo de recurso __SQL Server: Azure Arc__ y agregue una nueva mediante la hoja de creación.

![Inicio de la creación](media/join/start-creation-of-sql-server-azure-arc-resource.png)

2. Revise los requisitos previos y vaya a la pestaña **Detalles del servidor**.  

3. Seleccione la suscripción, el grupo de recursos, la región de Azure y el sistema operativo host. Si es necesario, especifique también el proxy que usa la red para conectarse a Internet.

> [!IMPORTANT]
> Si la máquina que hospeda la instancia de SQL Server ya está [conectada a Azure Arc](/azure/azure-arc/servers/onboard-portal), asegúrese de seleccionar el mismo grupo de recursos que contiene el recurso __Máquina: Azure Arc__ correspondiente.

![Detalles del servidor](media/join/server-details-sql-server-azure-arc.png)

4. Vaya a la pestaña **Ejecutar script** y descargue el script de registro que se muestra. El portal genera el script para el sistema operativo que hospeda especificado.

![Descargar el script](media/join/download-script-sql-server-azure-arc.png)

## <a name="connect-the-installed-sql-server-instances-to-azure-arc"></a>Conexión de las instancias de SQL Server instaladas a Azure Arc

En este paso, tomará el script que se ha descargado de Azure Portal y lo ejecutará en la máquina virtual o física de destino. Como resultado, cada instancia de SQL Server instalada en la máquina se registrará como un recurso de __SQL Server: Azure Arc__. Además, si las máquinas no tienen instalado el agente de configuración de invitado, se instalará automáticamente y se registrará como un recurso de __Máquina: Azure Arc__.

> [!NOTE]
> Asegúrese de ejecutar el script con una cuenta que cumpla los requisitos mínimos de permisos descritos en [requisitos previos](overview.md#prerequisites).

### <a name="windows"></a>Windows

1. Inicie una instancia de administrador de __powershell.exe__ e inicie sesión en el módulo de PowerShell con sus credenciales de Azure. Siga las [instrucciones de inicio de sesión](/powershell/azure/install-az-ps#sign-in).

2. Ejecución del script descargado

   ```powershell
   & '.\RegisterSqlServerArc.ps1'
   ```

   > [!NOTE]
   > Es posible que vea incidencias la primera vez que realice esto si no ha instalado previamente el módulo AZ para PowerShell. En ese caso, siga las instrucciones del script para instalar y conectar su cuenta y volver a ejecutar el script.

### <a name="linux"></a>Linux

1. Use la CLI de Azure para iniciar sesión con las credenciales de Azure. Siga las [instrucciones de inicio de sesión](/cli/azure/authenticate-azure-cli).

2. Conceda el permiso de ejecución al script descargado y ejecútelo.

   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="register-sql-server-instances-on-multiple-machines"></a>Registro de instancias de SQL Server en varias máquinas

Se pueden conectar varias instancias de SQL Server instaladas en varias máquinas Windows o Linux a Azure Arc mediante el mismo script que se ha generado para una sola máquina. Siga las instrucciones acerca de cómo [conectar instancias de SQL Server a Azure Arc a gran escala](connect-at-scale.md).

## <a name="validate-the-sql-server---azure-arc-resources"></a>Validación de los recursos de SQL Server: Azure Arc

Vaya a [Azure Portal](https://ms.portal.azure.com/#home) y abra el recurso de __SQL Server: Azure Arc__ recientemente registrado que se va a validar.

![Validación del servidor de SQL conectado ](media/join/validate-sql-server-azure-arc.png)

## <a name="disconnect-your-sql-server-instance"></a>Desconexión de la instancia de SQL Server

Para desconectar la instancia de SQL Server de Azure Arc, vaya a Azure Portal, abra el recurso de __SQL Server: Azure Arc__ correspondiente esa instancia y haga clic en el botón **Anular registro**.

![Anulación del registro de SQL Server](media/join/unregister-sql-server-azure-arc.png)

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de la seguridad avanzada de datos para la instancia de SQL Server](configure-advanced-data-security.md)

* [Configuración de la evaluación de SQL a petición para la instancia de SQL Server](assess.md)
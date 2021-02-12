---
title: Conexión de instancias de SQL Server a Azure Arc a gran escala
titleSuffix: ''
description: En este artículo, obtendrá información sobre cómo conectar instancias de SQL Server como servidores de SQL habilitados para Azure Arc (versión preliminar) mediante una entidad de servicio.
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: b78c76504cb9ffb76b62c1867630760fe52cf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063190"
---
# <a name="connect-sql-server-instances-to-azure-arc-at-scale"></a>Conexión de instancias de SQL Server a Azure Arc a gran escala

Se pueden conectar varias instancias de SQL Server instaladas en varias máquinas Windows o Linux a Azure Arc mediante el mismo [script que se ha generado para una sola máquina](connect.md). El script conectará y registrará cada máquina y las instancias de SQL Server instaladas en él en Azure Arc. Para obtener la mejor experiencia, se recomienda usar una [entidad de servicio](/azure/active-directory/develop/app-objects-and-service-principals) de Azure Active Directory. Una entidad de servicio es una identidad de administración limitada especial a la que solo se concede el permiso mínimo necesario a fin de conectar máquinas a Azure y crear los recursos de Azure para un servidor habilitado para Azure Arc y un servidor de SQL Server habilitado para Azure Arc. Es más seguro que usar una cuenta con más privilegios, como un Administrador de inquilinos, y sigue nuestros procedimientos recomendados de seguridad de control de acceso.  

Los métodos de instalación para instalar y configurar el agente Connected Machine requieren que el método automatizado que se use tenga permisos de administrador en las máquinas. En Linux, mediante la cuenta raíz y, en Windows, como miembro del grupo local de administradores.

Antes de empezar, asegúrese de revisar los [requisitos previos](overview.md#prerequisites) y de que ha creado un rol personalizado que cumpla los permisos necesarios.

## <a name="connecting-multiple-sql-server-instances-on-windows-using-azure-powershell"></a>Conexión de varias instancias de SQL Server en Windows mediante Azure PowerShell

Cada máquina debe tener [Azure PowerShell](/powershell/azure/install-az-ps) instalado.

1. Cree la entidad de servicio mediante PowerShell con el cmdlet [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal). Asegúrese de almacenar la salida en una variable. De lo contrario, no podrá recuperar la contraseña que necesitará más adelante.

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role <your custom role>
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

   > [!NOTE]
   > Al crear una entidad de servicio, la cuenta debe ser Propietario o Administrador de acceso de los usuarios en la suscripción que se va a usar para la incorporación. Si no tiene permisos suficientes para crear asignaciones de roles, es posible que se cree la entidad de servicio, pero no podrá incorporar máquinas. Las instrucciones sobre cómo crear un rol personalizado se proporcionan en [Permisos necesarios](overview.md#required-permissions).

2. Recupere la contraseña almacenada en la variable `$sp`:

   ```azurepowershell-interactive
   $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
   $credential.GetNetworkCredential().password
   ```
3. Recupere el valor del id. de inquilino de la entidad de servicio:
 
   ```azurepowershell-interactive
   $tenantId= (Get-AzContext).Tenant.Id
   ```
4. Copie y guarde la contraseña, el id. de aplicación y el id. de inquilino con las prácticas de seguridad adecuadas. Si olvida o pierde la contraseña de la entidad de servicio, puede restablecerla mediante el cmdlet [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential).

   > [!NOTE]
   > Tenga en cuenta que Azure Arc para servidores no admite actualmente el inicio de sesión con un certificado, por lo que la entidad de servicio debe tener un secreto para autenticarse.

5. Descargue el script de PowerShell desde el portal siguiendo las instrucciones que se detallan en [Conexión del servidor de SQL Server con Azure Arc](connect.md).

6. Abra el script en una instancia de administrador de PowerShell ISE y reemplace las variables de entorno siguientes mediante los valores generados durante el aprovisionamiento de la entidad de servicio descrito anteriormente. Al principio, estas variables están vacías.

   ```azurepowershell-interactive
   $servicePrincipalAppId="{serviceprincipalAppID}"
   $servicePrincipalSecret="{serviceprincipalPassword}"
   $servicePrincipalTenantId="{serviceprincipalTenantId}"
   ```

7. Ejecución del script en cada máquina de destino

## <a name="connecting-multiple-sql-server-instances-on-linux-using-azure-cli"></a>Conexión de varias instancias de SQL Server en Linux mediante la CLI de Azure

Cada máquina de destino debe tener la [CLI de Azure instalada](/cli/azure/install-azure-cli). El script de registro iniciará sesión de forma automática en Azure con las credenciales de la entidad de servicio si se proporcionan y ningún otro usuario ya ha iniciado sesión. Siga los pasos siguientes para conectar instancias de SQL Server en varias máquinas Linux.

1. Cree una entidad de servicio mediante el comando ["az ad sp create-for-rbac"](/cli/azure/ad/sp#az_ad_sp_create_for_rbac).

   ```azurecli-interactive
   az ad sp create-for-rbac --name <your service principal name> --role <your custom role name>
   ```

   ```output
   { "appId": "d2ff754a-e10a-4eb6-9cdc-ce6e7a4687db",
     "displayName": "Arc-for-servers",
     "name": "http://Arc-for-servers",
     "password": {SomeRandomlyGeneratedPassword}",
     "tenant": "2530e75f-673b-4841-8270-47ca6a34ef4f"
   }
   ```

   > [!NOTE]
   > Al crear una entidad de servicio, la cuenta debe ser Propietario o Administrador de acceso de los usuarios en la suscripción que se va a usar para la incorporación. Si no tiene permisos suficientes para crear asignaciones de roles, es posible que se cree la entidad de servicio, pero no podrá incorporar máquinas. Las instrucciones sobre cómo crear un rol personalizado se proporcionan en [Permisos necesarios](overview.md#required-permissions).

2. Descargue el script de Linux desde el portal siguiendo las instrucciones que se detallan en [Conexión del servidor de SQL Server con Azure Arc](connect.md).

3. Reemplace las variables siguientes en el script mediante los valores que devuelve el comando "az ad sp create-for-rbac". Al principio, estas variables están vacías.

   ```bash
   servicePrincipalAppId="{serviceprincipalAppID}"
   servicePrincipalSecret="{serviceprincipalPassword}"
   servicePrincipalTenant="{serviceprincipalTenant}"
   ```

3. Ejecución del script en cada máquina de destino
 
   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="validate-successful-onboarding"></a>Validación de una incorporación correcta

Después de registrar las instancias de SQL Server con el servidor de SQL Server habilitado para Azure Arc (versión preliminar), vaya a [Azure Portal](https://aka.ms/azureportal) y vea los recursos de Azure Arc recientemente creados. Verá un nuevo recurso __Máquina: Azure Arc__ para cada máquina conectada y otro nuevo __SQL Server: Azure Arc__ para cada instancia de SQL Server registrada. 

![Incorporación correcta](./media/join-at-scale/successful-onboard.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo administrar la máquina con [Azure Policy](/azure/governance/policy/overview) para, por ejemplo, la [configuración de invitado](/azure/governance/policy/concepts/guest-configuration) de VM, la comprobación de que la máquina informa al área de trabajo de Log Analytics esperada, la habilitación de la supervisión con [Azure Monitor con máquinas virtuales](/azure/azure-monitor/insights/vminsights-enable-policy) y mucho más.

- Más información sobre el [agente de Log Analytics](/azure/azure-monitor/platform/log-analytics-agent). El agente de Log Analytics para Windows y Linux es necesario si desea supervisar de forma proactiva el sistema operativo y las cargas de trabajo que se ejecutan en la máquina, administrarlos mediante runbooks de Automation o soluciones como Update Management, o usar otros servicios de Azure como [Azure Security Center](/azure/security-center/security-center-intro).

- Obtenga información acerca de cómo [Configurar la instancia de SQL Server para la comprobación periódica del estado del entorno con la evaluación de SQL a petición](assess.md).

- Obtenga información sobre cómo [Configurar la seguridad de datos avanzada para la instancia de SQL Server](configure-advanced-data-security.md).
---
title: Errores comunes con las claves administradas por el cliente en Azure Key Vault
description: Obtenga información sobre cómo identificar y solucionar problemas de acceso y errores comunes con el cifrado de datos transparente (TDE) y claves administradas por el cliente en Azure Key Vault.
ms.custom: seo-lt-2019
helpviewer_keywords:
- troublshooting, tde akv
- tde akv configuration, troubleshooting
- tde troubleshooting
author: jaszymas
ms.prod: sql
ms.technology: security
ms.reviewer: vanto
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: jaszymas
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 033346cf1d00d1f2acbd6b8de0796027c80814a7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345371"
---
# <a name="common-errors-for-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault"></a>Errores comunes en el cifrado de datos transparente con claves administradas por el cliente en Azure Key Vault

[!INCLUDE[asdb-asdbmi-asa](../../../includes/applies-to-version/asdb-asdbmi-asa.md)]

En este artículo se describe cómo identificar y resolver los problemas de acceso a la clave de Azure Key Vault que causaron que una base de datos configurada para utilizar el cifrado de datos transparente (TDE) de [ con claves administradas por el cliente en Azure Key Vault](/azure/sql-database/transparent-data-encryption-byok-azure-sql) dejara de estar accesible.

## <a name="introduction"></a>Introducción
Cuando TDE está configurado para usar una clave administrada por el cliente en Azure Key Vault, es necesario el acceso continuo a este protector de TDE para que la base de datos esté en línea.  Si el servidor lógico de SQL pierde el acceso al protector de TDE administrado por el cliente en Azure Key Vault, una base de datos empezará a denegar todas las conexiones con su correspondiente mensaje de error y cambiará su estado a *Inaccesible* en Azure Portal.

Durante las primeras 8 horas, si se resuelve el problema de acceso a la clave de Azure Key Vault subyacente, la base de datos se restaurará y se conectará en línea automáticamente. Esto significa que para todos los escenarios de interrupción de la red intermitentes y temporales, no se requiere ninguna acción del usuario y la base de datos se conectará en línea automáticamente. En la mayoría de los casos, se requiere la intervención del usuario para resolver el problema de acceso a la clave del almacén de claves subyacente. 

Si ya no se necesita una base de datos inaccesible, puede eliminarse inmediatamente para dejar de incurrir en gastos. No se permiten todas las demás acciones en la base de datos hasta que se restaure el acceso a la clave de Azure Key Vault y la base de datos vuelva a estar en línea. Tampoco es posible cambiar en el servidor la opción de TDE de claves administradas por el cliente por la de claves administradas por el servicio mientras una base de datos cifrada con claves administradas por el cliente sea inaccesible. Esto es necesario para proteger los datos contra el acceso no autorizado mientras se han revocado los permisos para el protector de TDE. 

Cuando no se pueda obtener acceso a una base de datos durante más de 8 horas, ya no se realizará la restauración automática. Si tras dicho período se ha restaurado el acceso a la clave de Azure Key Vault requerido, debe volver a validar el acceso a la clave manualmente para que la base de datos vuelva a estar en línea. En este caso, volver a poner la base de datos en línea puede tardar una cantidad considerable de tiempo según el tamaño de la base de datos. Una vez que la base de datos vuelva a estar en línea, **se perderán** los valores configurados previamente, como el [grupo de conmutación por error](/azure/sql-database/sql-database-auto-failover-group), el historial de PITR y las etiquetas. Por tanto, se recomienda implementar un sistema de notificación mediante [Grupos de acciones](/azure/azure-monitor/platform/action-groups) que permita percatarse y resolver los problemas de acceso a la clave del almacén de claves subyacentes lo antes posible. 

## <a name="common-errors-causing-databases-to-become-inaccessible"></a>Errores comunes que hacen que las bases de datos dejen de estar accesibles

La mayoría de los problemas que se producen cuando se usa TDE con Key Vault se debe a una de las configuraciones erróneas siguientes:

### <a name="the-key-vault-is-unavailable-or-doesnt-exist"></a>El almacén de claves no está disponible o no existe.

- El almacén se claves se eliminó por error.
- El firewall se configuró para Azure Key Vault, pero no permite acceso a los servicios de Microsoft.
- Un error de red intermitente hace que el almacén de claves no esté disponible.

### <a name="no-permissions-to-access-the-key-vault-or-the-key-doesnt-exist"></a>No existen permisos para acceder al almacén de claves o la clave no existe.

- La clave se ha eliminado accidentalmente, se ha deshabilitado o ha expirado.
- La AppId de la instancia de SQL Server lógica se eliminó por error.
- La instancia de SQL Server lógica se movió a otra suscripción. Se debe crear una AppId distinta si el servidor lógico se movió a otra suscripción.
- Los permisos concedidos a la AppId para las claves no son suficientes (no incluyen los permisos Get, Wrap ni Unwrap).
- Se revocaron los permisos para la AppId de la instancia de SQL Server lógica.

## <a name="identify-and-resolve-common-errors"></a>Identificación y resolución de los errores comunes

En esta sección, se muestran los pasos para resolver los errores más comunes.

### <a name="missing-server-identity"></a>Falta la auditoría de servidor.

**Mensaje de error**

_401 AzureKeyVaultNoServerIdentity: la identidad del servidor no se ha configurado correctamente en el servidor. Póngase en contacto con el soporte técnico_.

**Detección**

Use el comando o el cmdlet siguiente para asegurarse de que se asignó una identidad a la instancia de SQL Server lógica:

- Azure PowerShell: [Get-AzureRMSqlServer](/powershell/module/AzureRM.Sql/Get-AzureRmSqlServer) 

- CLI de Azure: [az-sql-server-show](/cli/azure/sql/server#az-sql-server-show)

**Mitigación**

Use el comando o el cmdlet siguiente para configurar una identidad de Azure AD (una AppId) para la instancia de SQL Server lógica:

- Azure PowerShell: [Set-AzureRmSqlServer](/powershell/module/azurerm.sql/set-azurermsqlserver) con la opción `-AssignIdentity`.

- CLI de Azure: [az sql server update](/cli/azure/sql/server#az-sql-server-update) con la opción `--assign_identity`.

En Azure Portal, vaya al almacén de claves y, luego, a **Directivas de acceso**. Siga estos pasos: 

 1. Use el botón **Agregar nuevo** para agregar la AppId del servidor que creó en el paso anterior. 
 1. Asigne los permisos de clave siguientes: obtención, encapsulado y desencapsulado. 

Para más información, consulte [Asignar una entidad de Azure AD al servidor](/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure#assign-an-azure-ad-identity-to-your-server).

> [!IMPORTANT]
> Si la instancia de SQL Server lógica se movió a un suscriptor nuevo después de la configuración inicial de TDE con Key Vault, repita el paso para configurar la identidad de Azure AD y crear una nueva AppId. Luego, agregue la AppId al almacén de claves y asigne los permisos correctos a la clave. 
>

### <a name="missing-key-vault"></a>Falta el almacén de claves

**Mensaje de error**

_503 AzureKeyVaultConnectionFailed: la operación no ha podido completarse en el servidor debido a que se ha producido un error al intentar conectarse a Azure Key Vault_.

**Detección**

Para identificar un URI de clave y el almacén de claves:

1. Use el comando o el cmdlet siguiente para obtener el URI de clave de una instancia de SQL Server lógica específica:

    - Azure PowerShell: [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey)

    - CLI de Azure: [az-sql-server-tde-key-show](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-show) 

1. Use el URI de clave para identificar el almacén de claves:

    - Azure PowerShell: se pueden inspeccionar las propiedades de la variable $MyServerKeyVaultKey para obtener detalles sobre el almacén de claves.

    - CLI de Azure: inspeccione el protector de cifrado de servidor devuelto para obtener más información sobre el almacén de claves.

**Mitigación**

Confirme que el almacén de claves está disponible:

- Asegúrese de que el almacén de claves está disponible y de que la instancia de SQL Server lógica tiene acceso.
- Si el almacén de claves está protegido detrás de un firewall, asegúrese de que la casilla para permitir que los servicios de Microsoft pueden acceder al almacén de claves esté activada.
- Si el almacén de claves se eliminó por error, debe completar la configuración desde el inicio.


### <a name="missing-key"></a>Falta una clave

**Mensajes de error**

_404 ServerKeyNotFound: no se ha encontrado la clave de servidor solicitada en la suscripción actual_. 

_409 ServerKeyDoesNotExists: la clave del servidor no existe_.

**Detección**

Para identificar un URI de clave y el almacén de claves:

- Use el cmdlet o los comandos que aparecen en [Falta el almacén de claves](#missing-key-vault) para identificar el URI de clave que se agrega a la instancia de SQL Server lógica. La ejecución de los comandos devuelve la lista de las claves.

**Mitigación**

Confirme que el protector de TDE está presente en Key Vault:

1. Identifique el almacén de claves y vaya a él en Azure Portal.
1. Asegúrese de que está la clave identificada por el URI de clave.

### <a name="missing-permissions"></a>Faltan permisos

**Mensaje de error**

_401 AzureKeyVaultMissingPermissions: el servidor no encuentra los permisos necesarios en Azure Key Vault_.

**Detección**

Para identificar el URI de clave y el almacén de claves: 

- Use el cmdlet o los comandos que aparecen en [Falta el almacén de claves](#missing-key-vault) para identificar el almacén de claves que se usa en la instancia de SQL Server lógica.

**Mitigación**

Confirme que la instancia de SQL Server lógica tiene permisos para el almacén de claves y los permisos correctos para acceder a la clave:

- En Azure Portal, vaya al almacén de claves > **Directivas de acceso**. Busque la AppId de la instancia de SQL Server lógica.  
- Si está la AppId, asegúrese de que tiene los permisos de clave siguientes: obtención, encapsulado y desencapsulado.
- Si la AppId no está, agréguela con el botón **Agregar nuevo**. 

## <a name="getting-tde-status-from-the-activity-log"></a>Obtención del estado de TDE del registro de actividad

Para permitir la supervisión del estado de la base de datos debido a problemas de acceso a la clave de Azure Key Vault, se registrarán los siguientes eventos en el [Registro de actividad](/azure/service-health/alerts-activity-log-service-notifications) para el identificador de recurso en función de la dirección URL de Azure Resource Manager y Subscription+Resourcegroup+ServerName+DatabseName: 

**Evento cuando el servicio pierde el acceso a la clave de Azure Key Vault**

EventName: MakeDatabaseInaccessible 

Estado: Iniciado 

Descripción: La base de datos ha perdido el acceso a la clave del almacén de claves de Azure y ahora es inaccesible: <error message>   

 

**Evento cuando comienza el tiempo de espera de 8 horas para la restauración automática** 

EventName: MakeDatabaseInaccessible 

Estado: InProgress 

Descripción: La base de datos está esperando a que el usuario vuelva a establecer el acceso a la clave del almacén de claves de Azure en un plazo de 8 horas.   

 

**Evento cuando la base de datos vuelve a estar en línea automáticamente**

EventName: MakeDatabaseAccessible 

Estado: Correcto 

Descripción: Se ha restablecido el acceso a la clave del almacén de claves de Azure y la base de datos ahora está en línea. 

 

**Evento cuando el problema no se ha resuelto en 8 horas y el acceso a la clave de Azure Key Vault debe validarse manualmente** 

EventName: MakeDatabaseInaccessible 

Estado: Correcto 

Descripción: No se puede tener acceso a la base de datos y el usuario debe resolver los errores del almacén de claves de Azure y restablecer el acceso a la clave del almacén de claves de Azure mediante la clave de revalidación. 

 

**Evento cuando la base de datos se pone en línea después de volver a validar la clave manualmente**

EventName: MakeDatabaseAccessible 

Estado: Correcto 

Descripción: Se ha restablecido el acceso a la clave del almacén de claves de Azure y la base de datos ahora está en línea. 

 

**Evento cuando la revalidación del acceso a la clave de Azure Key Vault se ha realizado correctamente y la base de datos vuelve a estar en línea**

EventName: MakeDatabaseAccessible 

Estado: Iniciado 

Descripción: Se ha iniciado la restauración del acceso a la base de datos a la clave del almacén de claves de Azure. 

 

**Evento cuando se ha producido un error en la revalidación del acceso a la clave de Azure Key Vault**

EventName: MakeDatabaseAccessible 

Estado: Con error 

Descripción: Se ha producido un error al restaurar el acceso de la base de datos a la clave del almacén de claves de Azure. 


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [Azure Resource Health](/azure/service-health/resource-health-overview).
- Configure los [Grupos de acciones](/azure/azure-monitor/platform/action-groups) para recibir notificaciones y alertas en función de sus preferencias, por ejemplo, correo electrónico/SMS/inserciones/voz, aplicación lógica, webhook, ITSM o Runbook de automatización.
---
title: Configuración de la administración extensible de claves de Cifrado de datos transparente (TDE) con Azure Key Vault
description: Instale y configure el conector de SQL Server para Azure Key Vault.
ms.custom: seo-lt-2019
ms.date: 11/25/2020
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management
- EKM, with key vault setup
- SQL Server Connector, setup
- SQL Server Connector
- TDE, AKV, EKM
ms.assetid: c1f29c27-5168-48cb-b649-7029e4816906
author: Rupp29
ms.author: arupp
ms.openlocfilehash: 83a1b1c7e923362678a013de24b211a9da70be0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345379"
---
# <a name="set-up-sql-server-tde-extensible-key-management-by-using-azure-key-vault"></a>Configuración de Administración extensible de claves de TDE de SQL Server mediante Azure Key Vault

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

En este artículo, instalará y configurará el conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para Azure Key Vault.  
  
## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a usar Azure Key Vault con la instancia de SQL Server, asegúrese de que cumple los requisitos previos siguientes:  
  
- Debe tener una suscripción de Azure.
  
- Instale [Azure PowerShell, versión 5.2.0 o posterior](/powershell/azure/).  

- Cree una instancia de Azure Active Directory (Azure AD).

- Familiarícese con los principios del almacenamiento de Administración extensible de claves (EKM) con Azure Key Vault mediante la revisión de [(EKM) con Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  

- Instale la versión de Visual Studio C++ Redistributable basada en la versión de SQL Server que ejecute:
  
  Versión de SQL Server  | Versión de Visual Studio C++ Redistributable
  ---------|---------
  2008, 2008 R2, 2012, 2014 | [Paquetes de Visual C++ Redistributable para Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
  2016 | [Visual C++ Redistributable para Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)

## <a name="step-1-set-up-an-azure-ad-service-principal"></a>Paso 1: Configuración de una entidad de servicio de Azure AD

Para conceder permisos de acceso a la instancia de SQL Server al almacén de claves de Azure, necesita una cuenta de entidad de servicio en Azure AD.  
  
1. Inicie sesión en [Azure Portal](https://ms.portal.azure.com/) de cualquiera de estas maneras:

    - Seleccione el botón **Azure Active Directory**.

      ![Captura de pantalla del panel "Servicios de Azure"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-login-portal.png)

    - Seleccione **Más servicios** y, después, en el cuadro **Todos los servicios**, escriba **Azure Active Directory**.

      ![Captura de pantalla del panel "Todos los servicios de Azure"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-select-aad.png)  

1. Siga estos pasos para registrar una aplicación con Azure Active Directory. (Para obtener instrucciones detalladas paso a paso, vea la sección "Get an identity for the application" (Obtención de una identidad para la aplicación) de la [entrada de blog de Azure Key Vault](/archive/blogs/kv/azure-key-vault-step-by-step)).

    a. En el panel **Información general de Azure Active Directory**, seleccione en **Registros de aplicaciones**.

    ![Captura de pantalla del panel "Información general de Azure Active Directory"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-app-register.png)

    b. En el panel **Registros de aplicaciones**, seleccione **Nuevo registro**.

    ![Captura de pantalla del panel "Registros de aplicaciones"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-registration.png)  

    c. En el panel **Registrar una aplicación**, escriba el nombre para el usuario de la aplicación y, después, seleccione **Registrar**.

    ![Captura de pantalla del panel "Registrar una aplicación"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-register.png)

    d. En el panel de la izquierda, seleccione **Certificados y secretos** y luego **Nuevo secreto de cliente**.

    ![Captura de pantalla del panel "Certificados y secretos"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-certs-secrets.png)  

    e. En **Agregar un secreto de cliente**, escriba una descripción y una expiración adecuada y, después, seleccione **Agregar**.

    ![Captura de pantalla de la sección "Agregar un secreto de cliente"](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-add-secret.png)  

    f. En el panel **Certificados y secretos**, en **"Valor"** , seleccione el botón **Copiar** situado junto al valor del secreto de cliente que se va a usar para crear una clave asimétrica en SQL Server.

    ![Captura de pantalla del valor del secreto](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-secret.png)  

    g. En el panel de la izquierda, seleccione **Información general** y, después en el cuadro **Id. de aplicación (cliente)** , copie el valor que se va a usar para crear una clave asimétrica en SQL Server.

    ![Captura de pantalla del valor "Id. de aplicación (cliente)" en el panel Información general](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-appid.png)  

## <a name="step-2-create-a-key-vault"></a>Paso 2: Creación de un Almacén de claves

Seleccione el método que quiera usar para crear un almacén de claves.

## <a name="azure-portal"></a>[Azure Portal](#tab/portal)

### <a name="create-a-key-vault-by-using-the-azure-portal"></a>Creación de un almacén de claves mediante Azure Portal

Puede usar Azure Portal para crear el almacén de claves y, después, agregarle una entidad de seguridad de Azure AD.

1. Cree un grupo de recursos.

   Todos los recursos de Azure que se crean a través de Azure Portal deben estar incluidos en un grupo de recursos, que se crea para hospedar el almacén de claves. El nombre del recurso de este ejemplo es *ContosoDevRG*. Elija un grupo de recursos y un nombre del almacén de claves propios, ya que todos los nombres de almacén de claves deben ser únicos y globales.

   En el panel **Crear un grupo de recursos**, en **Detalles del proyecto**, escriba los valores y después seleccione **Revisar y crear**.

      ![Captura de pantalla del panel "Crear un grupo de recursos"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-resource-group.png)  

1. Cree un almacén de claves.

    En el panel **Crear almacén de claves**, seleccione la pestaña **Aspectos básicos**, escriba los valores adecuados y, después, seleccione **Revisar y crear**.

    ![Captura de pantalla del panel "Crear almacén de claves"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-key-vault.png)  

1. En el panel **Directivas de acceso**, seleccione **Agregar directiva de acceso**.

    ![Captura de pantalla del vínculo "Agregar directiva de acceso" del panel "Directivas de acceso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-access-policy.png)  

1. En el panel **Agregar directiva de acceso**, siga estos pasos:
  
    a. En la lista desplegable **Configurar a partir de una plantilla (opcional)** , seleccione **Administración de claves**.

    b. En el panel de la izquierda, seleccione la pestaña **Permisos clave** y, después, compruebe que las casillas **Obtener**, **Enumerar**, **Desencapsular clave** y **Encapsular clave** estén activadas.

    c. Seleccione **Agregar**.

    ![Captura de pantalla del panel "Agregar directiva de acceso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-access-policy-permission.png)

1. En el panel de la izquierda, seleccione la pestaña **Seleccionar la entidad de seguridad** y, después, siga estos pasos:

    a. En el panel **Entidad de seguridad**, en **Seleccionar**, empiece a escribir el nombre de la aplicación de Azure AD y, después, en la lista de resultados, seleccione la aplicación que quiera agregar.

    ![Captura de pantalla del cuadro de búsqueda de aplicaciones del panel Entidad de seguridad](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal.png)  

    b. Seleccione el botón **Seleccionar** para agregar la entidad de seguridad al almacén de claves.

    ![Captura de pantalla del botón Seleccionar del panel Entidad de seguridad](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-principal.png)

    c. En la parte inferior izquierda, seleccione **Agregar** para guardar los cambios.

    ![Captura de pantalla del botón Agregar del panel "Agregar directiva de acceso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal-new.png)

1. En el panel de **Key Vault**, seleccione **Claves** y escriba un nombre de almacén de claves. Use el tipo de clave **RSA** y el tamaño de clave RSA **2048**. Establezca las fechas de activación y expiración según corresponda y configure **¿Habilitado?** en **Sí**.

   ![Captura de pantalla de la página "Crear clave"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-key-vault-key.png)  

1. En el panel **Directivas de acceso**, seleccione **Guardar**.
  
   ![Captura de pantalla del botón Guardar del panel "Agregar directiva de acceso"](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-access-policy.png)  

## <a name="powershell"></a>[PowerShell](#tab/powershell)

### <a name="create-a-key-vault-and-key-by-using-powershell"></a>Creación de una clave y un almacén de claves mediante PowerShell

El motor de base de datos de SQL Server usa el almacén de claves y la clave que cree aquí para la protección de claves de cifrado.  
  
> [!IMPORTANT]
> La suscripción en la que se crea el almacén de claves debe estar en la misma instancia predeterminada de Azure AD en la que se haya creado la entidad de servicio de Azure AD. Si quiera usar otra instancia de Azure AD que no sea la predeterminada para crear una entidad de servicio para el Conector de SQL Server, debe cambiar la instancia predeterminada de Azure AD en la cuenta de Azure antes de crear el almacén de claves. Para obtener información sobre cómo cambiar la instancia predeterminada de Azure AD por la que quiere usar, vea la sección "Preguntas más frecuentes" de [Mantenimiento y solución de problemas del Conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB).  
  
1. Instale e inicie sesión en [Azure PowerShell 5.2.0 o una versión posterior](/powershell/azure/) con el comando siguiente:  
  
    ```powershell  
    Connect-AzAccount  
    ```  
  
    La instrucción devuelve:  
  
    ```console  
    Environment           : AzureCloud  
    Account               : <account_name>  
    TenantId              : <tenant_id>  
    SubscriptionId        : <subscription_id>  
    CurrentStorageAccount :  
    ```  
  
    > [!NOTE]  
    > Si tiene varias suscripciones y quiere especificar una para usar con el almacén, ejecute `Get-AzSubscription` para ver las suscripciones y `Select-AzSubscription` para elegir la correcta. De lo contrario, PowerShell seleccionará una de forma predeterminada.  
  
1. Cree un grupo de recursos.

    Todos los recursos de Azure que se crean a través de PowerShell deben estar incluidos en grupos de recursos, que se crean para hospedar el almacén de claves. El nombre del recurso de este ejemplo es *ContosoDevRG*. Elija un grupo de recursos y un nombre del almacén de claves propios, ya que todos los nombres de almacén de claves deben ser únicos y globales.  
  
    ```powershell  
    New-AzResourceGroup -Name ContosoDevRG -Location 'East Asia'  
    ```  
  
    La instrucción devuelve:  
  
    ```console
    ResourceGroupName: ContosoDevRG  
    Location         : eastasia  
    ProvisioningState: Succeeded  
    Tags             :
    ResourceId       : /subscriptions/<subscription_id>/  
                        resourceGroups/ContosoDevRG  
    ```  
  
    > [!NOTE]
    > Para el parámetro `-Location`, use el comando `Get-AzureLocation` a fin de identificar cómo especificar una ubicación alternativa a la de este ejemplo. Si necesita más información, escriba **Get-Help Get-AzureLocation**.  
  
1. Cree un almacén de claves.

    El cmdlet `New-AzKeyVault` requiere un nombre de grupo de recursos, un nombre de almacén de claves y una ubicación geográfica. Por ejemplo, para un almacén de claves denominado `ContosoEKMKeyVault`, ejecute:  
  
    ```powershell  
    New-AzKeyVault -VaultName 'ContosoEKMKeyVault' `  
       -ResourceGroupName 'ContosoDevRG' -Location 'East Asia'  
    ```  
  
    Registre el nombre de su almacén de claves.  
  
    La instrucción devuelve:

    ```console
    Vault Name                       : ContosoEKMKeyVault  
    Resource Group Name              : ContosoDevRG  
    Location                         : East Asia  
    ResourceId                       : /subscriptions/<subscription_id>/  
                                        resourceGroups/ContosoDevRG/providers/  
                                        Microsoft/KeyVault/vaults/ContosoEKMKeyVault  
    Vault URI: https://ContosoEKMKeyVault.vault.azure.net  
    Tenant ID                        : <tenant_id>  
    SKU                              : Standard  
    Enabled For Deployment?          : False  
    Enabled For Template Deployment? : False  
    Enabled For Disk Encryption?     : False  
    Access Policies                  :  
             Tenant ID              : <tenant_id>  
             Object ID              : <object_id>  
             Application ID         :
             Display Name           : <display_name>  
             Permissions to Keys    : get, create, delete, list, update, import,
                                      backup, restore  
             Permissions to Secrets : all  
    Tags                             :  
    ```  
  
1. Conceda permisos para que la entidad de servicio de Azure AD acceda al almacén de claves de Azure.  
  
    Puede autorizar a otros usuarios y aplicaciones a usar el Almacén de claves.  En el ejemplo, se usará la entidad de servicio creada en el [Paso 1: Configuración de una entidad de servicio de Azure AD](#step-1-set-up-an-azure-ad-service-principal) para autorizar la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    > [!IMPORTANT]
    > La entidad de servicio de Azure AD debe tener al menos los permisos *get*, *list*,*wrapKey* y *unwrapKey* para el almacén de claves.  
  
    Como se muestra en el comando siguiente, se usa el **Id. de aplicación (cliente)** del [Paso 1: Configuración de una entidad de servicio de Azure AD](#step-1-set-up-an-azure-ad-service-principal) para el parámetro `ServicePrincipalName`. `Set-AzKeyVaultAccessPolicy` se ejecuta en modo silencioso sin resultados si se ejecuta correctamente.  
  
    ```powershell  
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoEKMKeyVault' `  
      -ServicePrincipalName 9A57CBC5-4C4C-40E2-B517-EA677 `  
      -PermissionsToKeys get, list, wrapKey, unwrapKey  
    ```  
  
    Llame al cmdlet `Get-AzKeyVault` para confirmar los permisos. En la salida de la instrucción en `Access Policies`, debería ver el nombre de la aplicación de Azure AD como otro inquilino con acceso a este almacén de claves.  
  
1. Genere una clave asimétrica en el almacén de claves. Puede hacerlo de dos maneras: importar una clave existente o crear una.  

     > [!NOTE]
     > SQL Server solo admite claves RSA de 2048 bits y 3072 bits, y claves RSA-HSM de 2048 bits y 3072 bits.

### <a name="best-practices"></a>Procedimientos recomendados

Para garantizar una recuperación rápida de las claves y la capacidad de acceder a los datos fuera de Azure, se recomiendan los procedimientos recomendados siguientes:

- Cree la clave de cifrado localmente en un dispositivo de módulo de seguridad de hardware (HSM) local. Asegúrese de usar una clave RSA 2048 o 3072 asimétrica, para que sea compatible con SQL Server.
- Importe la clave de cifrado al almacén de claves de Azure. Este proceso se describe en las secciones siguientes.
- Antes de usar la clave por primera vez en el almacén de claves de Azure, cree una copia de seguridad de la clave. Para obtener más información, vea el comando [Backup-AzureKeyVaultKey]().
- Siempre que realice cambios en la clave (por ejemplo, si agrega ACL, etiquetas o atributos de clave), asegúrese de realizar otra copia de seguridad de la clave del almacén de claves de Azure.

  > [!NOTE]
  > Crear una copia de seguridad de una clave es una operación esencial de Azure Key Vault que devuelve un archivo que se puede guardar en cualquier lugar.

### <a name="types-of-keys"></a>Tipos de claves

Puede generar cuatro tipos de claves en un almacén de claves de Azure que funcionarán con SQL Server. Claves RSA asimétricas de 2048 bits y 3072 bits, y claves RSA-HSM de 2048 bits y 3072 bits.
  
- **Protegidas mediante software**: procesada en el software y cifrada en reposo. Las operaciones en claves protegidas mediante software se producen en máquinas virtuales de Azure. Se recomienda este tipo para las claves que no se usan en una implementación de producción.  

- **Protegidas con HSM:** se crean y protegen mediante un módulo de seguridad de hardware para disponer de seguridad adicional. El coste es de aproximadamente 1 USD por versión de clave.  
  
    > [!IMPORTANT]
    > Para el Conector de SQL Server, use solo los caracteres "a-z", "A-Z", "0-9" y guiones ("-"), con un límite de 26 caracteres.
    > El uso de otras versiones de clave bajo el mismo nombre de clave en el almacén de claves de Azure No funciona con el Conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para girar una clave del almacén de claves de Azure que use [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vea los pasos de Sustitución de claves en la sección "A. Instrucciones de mantenimiento para el Conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]" en [Mantenimiento y solución de problemas del Conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).  

### <a name="import-an-existing-key"></a>Importación de una clave existente
  
Si dispone de una clave RSA de 2048 bits protegida mediante software existente, puede cargarla al almacén de claves de Azure. Por ejemplo, si tiene un archivo PFX denominado *softkey.pfx* guardado en la unidad `C:\\` y quiere cargarlo en el almacén de claves de Azure, ejecute el comando siguiente para establecer la variable `securepfxpwd` para una contraseña de `12987553` para el archivo PFX:  
  
``` powershell  
$securepfxpwd = ConvertTo-SecureString -String '12987553' `  
  -AsPlainText -Force  
```  

Después puede ejecutar el comando siguiente para importar la clave desde el archivo PFX, que la protege mediante software en el servicio de Key Vault (opción recomendada):  
  
``` powershell  
    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' `  
      -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' `  
      -KeyFilePassword $securepfxpwd $securepfxpwd  -Destination 'HSM'  
```  

> [!CAUTION]
> Para escenarios de producción, se recomienda encarecidamente importar la clave asimétrica, porque permite al administrador custodiarla en un sistema de custodia de claves. Si la clave asimétrica se crea en el almacén, no se puede custodiar, porque la clave privada no puede salir nunca del almacén. Las claves que se usan para proteger datos críticos se deben custodiar. Si se pierde una clave asimétrica, los datos se perderán de forma definitiva.  

### <a name="create-a-new-key"></a>Creación de una nueva clave

Como alternativa, puede crear una clave de cifrado directamente en el almacén de claves de Azure y protegerla mediante software o con HSM.  En este ejemplo se va a crear una clave protegida mediante software con el cmdlet `Add-AzureKeyVaultKey`:  

``` powershell  
Add-AzureKeyVaultKey -VaultName 'ContosoEKMKeyVault' `  
  -Name 'ContosoRSAKey0' -Destination 'Software'  
```  
  
La instrucción devuelve:  
  
```console
Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes  
Key        :  {"kid":"https:contosoekmkeyvault.azure.net/keys/  
                ContosoRSAKey0/<guid>","dty":"RSA:,"key_ops": ...  
VaultName  : contosodevkeyvault  
Name       : contosoRSAKey0  
Version    : <guid>  
Id         : https://contosoekmkeyvault.vault.azure.net:443/  
              keys/ContosoRSAKey0/<guid>  
```  

> [!IMPORTANT]
> El almacén de claves admite varias versiones de la clave con mismo nombre, pero las claves que use el conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no deberían tener versiones ni revertirse. Si el administrador quiere revertir la clave que se usa para el cifrado de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], debe crear una con otro nombre en el almacén de claves y usarla para cifrar la clave de cifrado de datos (DEK).  

---

## <a name="step-3-install-the-ssnoversion-connector"></a>Paso 3: Instalación del conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

Descargue el conector de SQL Server desde el [Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=521700). La descarga la debe realizar el administrador del equipo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

> [!NOTE]
> - Las versiones 1.0.0.440 y anteriores del conector de SQL Server se han reemplazado y ya no se admiten en entornos de producción, y se siguen las instrucciones de la página [Mantenimiento y solución de problemas del Conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) en [Actualización del conector de SQL Server](sql-server-connector-maintenance-troubleshooting.md#upgrade-of--connector).
> - A partir de la versión 1.0.3.0, el conector de SQL Server notifica los mensajes de error pertinentes a los registros de eventos de Windows para la solución de problemas.
> - A partir de la versión 1.0.4.0, hay compatibilidad con las nubes de Azure privadas, entre las que se incluyen Azure China, Azure Alemania y Azure Government.
> - Hay un cambio importante en la versión 1.0.5.0, relacionado con el algoritmo de huella digital. Puede experimentar un error de restauración de base de datos después de actualizar a la versión 1.0.5.0. Para obtener más información, vea el [artículo de KB 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - A partir de la versión 1.0.5.0 (marca de tiempo: septiembre de 2020), el Conector de SQL Server admite el filtrado de mensajes y la lógica de reintento de solicitud de red.
> - **A partir de la versión 1.0.5.0 actualizada (marca de tiempo: noviembre de 2020), el Conector de SQL Server admite las claves RSA 2048, RSA 3072, RSA-HSM 2048 y RSA-HSM 3072.**
  
  ![Captura de pantalla del Asistente para la instalación de Conector de SQL Server](../../../relational-databases/security/encryption/media/ekm/ekm-connector-install.png)  
  
El conector se instala de forma predeterminada en C:\Archivos de programa\Conector de SQL Server para Microsoft Azure Key Vault. Esta ubicación se puede cambiar durante la instalación. Si lo cambia, ajuste los scripts de la sección siguiente.  
  
No hay ninguna interfaz para el conector, pero si está instalado correctamente, se instalará *Microsoft.AzureKeyVaultService.EKM.dll* en el equipo. Este ensamblado es la DLL del proveedor de servicios criptográficos EKM que se debe registrar con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante la instrucción `CREATE CRYPTOGRAPHIC PROVIDER`.  
  
La instalación del conector de SQL Server también le permite descargar, si quiere, los scripts de muestra del cifrado de SQL Server.  
  
Para ver las explicaciones de código de error, los valores de configuración o las tareas de mantenimiento para el conector de SQL Server, vea:  
  
- [A. Instrucciones de mantenimiento para el conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixA)  
- [C. Explicaciones de código de error para el conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixC)  
  
## <a name="step-4-configure-ssnoversion"></a>Paso 4: Configurar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Para obtener una nota sobre los niveles de permisos mínimos necesarios para cada acción de esta sección, vea [B. Preguntas más frecuentes](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB).  
  
1. Ejecute *sqlcmd.exe* o abra [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Studio.  
  
1. Ejecute el script [!INCLUDE[tsql](../../../includes/tsql-md.md)] siguiente para configurar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con el fin de usar EKM:  
  
    ```sql  
    -- Enable advanced options.  
    USE master;  
    GO  

    EXEC sp_configure 'show advanced options', 1;  
    GO  
    RECONFIGURE;  
    GO  

    -- Enable EKM provider  
    EXEC sp_configure 'EKM provider enabled', 1;  
    GO  
    RECONFIGURE;  
    ```  
  
1. Registre el Conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] como proveedor EKM con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    Cree un proveedor de servicios criptográficos mediante el Conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], que es un proveedor EKM para el almacén de claves de Azure.
    En este ejemplo, el nombre del proveedor es `AzureKeyVault_EKM`.  
  
    ```sql  
    CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM
    FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';  
    GO  
    ```  

    > [!NOTE]
    > La ruta de acceso al archivo no puede tener una longitud superior a 256 caracteres.  
  
1. Configure una credencial de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para un inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a fin de usar el almacén de claves.  
  
    Debe agregar una credencial a cada inicio de sesión que va a realizar el cifrado con una clave del almacén de claves. Estas pueden incluir:  
  
    - Un inicio de sesión de administrador de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que use el almacén de claves para configurar y administrar los escenarios de cifrado de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    - Otros inicios de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que podrían habilitar TDE u otras características de cifrado de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    Hay una asignación de uno a uno entre credenciales e inicios de sesión. Es decir, cada inicio de sesión debe tener una credencial única.  
  
    Modifique este script de [!INCLUDE[tsql](../../../includes/tsql-md.md)] como se indica a continuación:  
  
    - Edite el argumento `IDENTITY` (`ContosoEKMKeyVault`) para dirigirlo al almacén de claves de Azure.
      - Si usa la *versión global de Azure*, reemplace el argumento `IDENTITY` por el nombre del almacén de claves de Azure de [Paso 2: Creación de un almacén de claves](#step-2-create-a-key-vault).
      - Si usa una *nube privada de Azure* (por ejemplo, Azure Government, Azure China 21Vianet o Azure Alemania), reemplace el argumento `IDENTITY` por el URI del almacén que se devuelve en el paso 3 de la sección [Creación de un almacén de claves y una clave mediante PowerShell](#create-a-key-vault-and-key-by-using-powershell). No incluya "https://" en el URI del almacén.
    - Reemplace la primera parte del argumento `SECRET` con el id. de cliente de Azure Active Directory del [Paso 1: Configuración de una entidad de servicio de Azure AD](#step-1-set-up-an-azure-ad-service-principal). En este ejemplo, el **id. de cliente** es `9A57CBC54C4C40E2B517EA677E0EFA00`.  
  
      > [!IMPORTANT]
      > Asegúrese de quitar los guiones del id. de aplicación (cliente).  
  
    - Complete la segunda parte del argumento `SECRET` con el **secreto de cliente** del [Paso 1: Configuración de una entidad de servicio de Azure AD](#step-1-set-up-an-azure-ad-service-principal).  En este ejemplo, el secreto de cliente es `08:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m`. La última cadena del argumento `SECRET` será una secuencia larga de letras y números, sin guiones.  
  
    ```sql  
    USE master;  
    CREATE CREDENTIAL sysadmin_ekm_cred
        WITH IDENTITY = 'ContosoEKMKeyVault',                            -- for public Azure
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.usgovcloudapi.net', -- for Azure Government
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.azure.cn',          -- for Azure China 21Vianet
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.microsoftazure.de', -- for Azure Germany
               --<----Application (Client) ID ---><--Azure AD app (Client) ID secret-->
        SECRET = '9A57CBC54C4C40E2B517EA677E0EFA0008:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM;  
  
    -- Add the credential to the SQL Server administrator's domain login
    ALTER LOGIN [<domain>\<login>]  
    ADD CREDENTIAL sysadmin_ekm_cred;  
    ```  
  
    Para obtener un ejemplo del uso de variables con el argumento `CREATE CREDENTIAL` y de cómo quitar mediante programación los guiones del id. de cliente, vea [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md).  
  
1. Abra la clave del almacén de claves de Azure en la instancia de SQL Server.  

    Con independencia de que haya creado una clave o haya importado una clave asimétrica como se describe en el [Paso 2: Creación de un almacén de claves](#step-2-create-a-key-vault), tendrá que abrir la clave. Para abrir la clave, proporcione su nombre en el script [!INCLUDE[tsql](../../../includes/tsql-md.md)] siguiente.  
  
    - Reemplace `EKMSampleASYKey` con el nombre que le gustaría tener en la clave de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
    - Reemplace `ContosoRSAKey0` con el nombre de la clave en el almacén de claves de Azure.  
  
    ```sql  
    CREATE ASYMMETRIC KEY EKMSampleASYKey
    FROM PROVIDER [AzureKeyVault_EKM]  
    WITH PROVIDER_KEY_NAME = 'ContosoRSAKey0',  
    CREATION_DISPOSITION = OPEN_EXISTING;  
    ```  

1. Cree un inicio de sesión con la clave asimétrica de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que ha creado en el paso anterior.

     ```sql  
    --Create a Login that will associate the asymmetric key to this login
    CREATE LOGIN TDE_Login
    FROM ASYMMETRIC KEY EKMSampleASYKey;
    ```  

1. Cree un inicio de sesión a partir de la clave asimétrica en SQL Server. Quite la asignación de credenciales del Paso 4: Configuración de SQL Server, para que las credenciales se puedan asignar al nuevo inicio de sesión.

     ```sql  
    --Now drop the credential mapping from the original association
    ALTER LOGIN [<domain>\<login>]
    DROP CREDENTIAL sysadmin_ekm_cred;
    ```

1. Modifique el nuevo inicio de sesión y asígnele las credenciales EKM.

     ```sql  
    --Now add the credential mapping to the new Login
    ALTER LOGIN TDE_Login
    ADD CREDENTIAL sysadmin_ekm_cred;
    ```  

1. Cree una base de datos de prueba que se cifrará con la clave del de claves de Azure.

     ```sql
    --Create a test database that will be encrypted with the Azure key vault key
    CREATE DATABASE TestTDE
    ```  

1. Cree una clave de cifrado de base de datos mediante la clave asimétrica (EKMSampleASYKey).

    ```sql  
    --Create an ENCRYPTION KEY using the ASYMMETRIC KEY (EKMSampleASYKey)
    CREATE DATABASE ENCRYPTION KEY
    WITH ALGORITHM = AES_256
    ENCRYPTION BY SERVER ASYMMETRIC KEY EKMSampleASYKey;
    ```
  
1. Cifre la base de datos de prueba. Establezca ENCRYPTION ON para habilitar TDE.

     ```sql  
    --Enable TDE by setting ENCRYPTION ON
    ALTER DATABASE TestTDE
    SET ENCRYPTION ON;  
     ```  

1. Limpie los objetos de prueba. Elimine todos los objetos que se hayan creado en este script de prueba.

    ```sql  
    -- CLEAN UP
    USE Master
    ALTER DATABASE [TestTDE] SET SINGLE_USER WITH ROLLBACK IMMEDIATE
    DROP DATABASE [TestTDE]

    ALTER LOGIN [TDE_Login] DROP CREDENTIAL [sysadmin_ekm_cred]
    DROP LOGIN [TDE_Login]

    DROP CREDENTIAL [sysadmin_ekm_cred]  

    USE MASTER
    DROP ASYMMETRIC KEY [EKMSampleASYKey]
    DROP CRYPTOGRAPHIC PROVIDER [AzureKeyVault_EKM]
    ```  

Para obtener scripts de ejemplo, vea el blog en [Cifrado de datos transparente de SQL Server y administración extensible de claves con Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).

## <a name="next-steps"></a>Pasos siguientes  
  
Ahora que ha completado la configuración básica, vea [Uso del conector de SQL Server con características de cifrado de SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).
  
## <a name="see-also"></a>Consulte también  

- [Administración extensible de claves con Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)
- [Mantenimiento y solución de problemas del Conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)

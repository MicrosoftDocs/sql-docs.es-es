---
title: Error del Conector de SQL Server y registro de la información
description: En este artículo se describe la habilitación de errores y el registro del Conector de SQL Server mediante la modificación de entradas del registro.
ms.date: 10/08/2020
ms.localizationpriority: medium
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: how-to
author: rupp29
ms.author: arupp
ms.openlocfilehash: 3a674994d7028a8f4455c26d6f705b9da7b5ee08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018688"
---
# <a name="sql-server-connector-error-and-information-logging"></a>Error del Conector de SQL Server y registro de la información

En este artículo se describe la modificación de entradas del registro para habilitar el error del Conector de SQL Server y el registro de la información.

## <a name="sql-server-connector-for-microsoft-azure-key-vault"></a>Conector de SQL Server para Azure Key Vault

El [Conector de SQL Server para Microsoft Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45344) permite el cifrado de SQL Server para usar Microsoft Azure Key Vault como proveedor de administración extensible de claves (EKM) con el fin de proteger sus claves de cifrado.

La [descarga](https://www.microsoft.com/download/details.aspx?id=45344) incluye el Conector de SQL Server y los scripts de ejemplo para permitir que un administrador de SQL Server obtenga información sobre cómo configurar el Conector de SQL Server y habilitar escenarios de cifrado de SQL Server. Para más información, vea [Administración extensible de claves con Azure Key Vault (SQL Server)](./extensible-key-management-using-azure-key-vault-sql-server.md).

Use el [foro de Azure Key Vault](https://social.msdn.microsoft.com/Forums/AzureKeyVault) para formular preguntas, compartir información y hablar sobre el Conector de SQL Server.

> [!NOTE]
> Durante la ejecución normal, el archivo DLL del Conector de SQL Server creará dinámicamente entradas del registro para establecer la conectividad con Azure Key Vault y crear la clave `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\SQL Server Cryptographic Provider]`. La cuenta de inicio de SQL Server debe ser un administrador local o su cuenta de servicio debe ser **NT SERVICE\MSSQLSERVER**

## <a name="upgrade-sql-server-connector-to-the-latest-version"></a>Actualización del Conector de SQL Server a la versión más reciente

Para actualizar el Conector de SQL Server (versión: 1.0.5.0, a fecha de septiembre de 2020) a la versión más reciente del proveedor de cifrado de DLL, siga estos pasos.

### <a name="upgrade"></a>Actualizar

1. Detenga el servicio SQL Server mediante el Administrador de configuración de SQL Server.
1. Desinstale la versión anterior mediante **Panel de control\Programas\Programas y características**.
    1. Nombre de la aplicación: Conector de SQL Server para Azure Key Vault
    1. Versión: 15.0.300.96
    1. Fecha del archivo DLL: 30/01/2018 15:00
1. Instale (actualice) un nuevo Conector de SQL Server para Microsoft Azure Key Vault.
    1. Versión: 15.0.2000.367
    1. Fecha del archivo DLL: 11/09/2020 5:17
1. Inicie el servicio SQL Server.
1. Pruebe que se pueda a las bases de datos cifradas.

### <a name="rollback"></a>Reversión

1. Detenga el servicio SQL Server mediante el Administrador de configuración de SQL Server.
1. Desinstale la nueva versión mediante **Panel de control\Programas\Programas y características**.
    1. Nombre de la aplicación: Conector de SQL Server para Azure Key Vault
    1. Versión: 15.0.2000.367
    1. Fecha del archivo DLL: 11/09/2020 5:17
1. Instale la versión anterior del Conector de SQL Server para Microsoft Azure Key Vault.
    1. Versión: 15.0.300.96
    1. Fecha del archivo DLL: 30/01/2018 15:00
1. Inicie el servicio SQL Server.
1. Pruebe que se pueda a las bases de datos cifradas.

> [!NOTE]
> - Las versiones 1.0.0.440 y anteriores del Conector de SQL Server se han reemplazado y ya no se admiten en entornos de producción. Para obtener más información sobre la solución de problemas del Conector de SQL Server, consulte las [instrucciones de mantenimiento y solución de problemas del Conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).
> - A partir de la versión 1.0.3.0, el conector de SQL Server notifica los mensajes de error pertinentes a los registros de eventos de Windows para la solución de problemas.
> - A partir de la versión [1.0.4.0 (versión 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi), hay compatibilidad con las nubes de Azure privadas, entre las que se incluyen Azure China, Azure Alemania y Azure Government.
> - Hay un cambio importante en la versión 1.0.5.0, relacionado con el algoritmo de huella digital. Puede experimentar un error de restauración de base de datos después de actualizar a la versión 1.0.5.0. Para obtener más información, vea el [artículo de KB 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - **A partir de la versión 1.0.5.0 (con una fecha de archivo de septiembre de 2020), el Conector de SQL Server admite el filtrado de mensajes y la lógica de reintento de solicitud de red.**
> - *La versión anterior del Conector de SQL Server también es la versión [1.0.5.0 (versión 15.0.300.96); fecha de archivo de enero de 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)* . Actualice el Conector de SQL Server a la versión más reciente si tiene algún problema.

**Requisitos del sistema**: versiones de SQL Server admitidas:

- SQL Server 2019 RTM Enterprise de 64 bits
- SQL Server 2017 RTM Enterprise de 64 bits
- SQL Server 2016 RTM Enterprise de 64 bits
- SQL Server 2014 RTM Enterprise de 64 bits
- SQL Server 2012 SP2 Enterprise de 64 bits
- SQL Server 2012 SP1 CU6 Enterprise de 64 bits
- SQL Server 2008 R2 SP2 CU8 Enterprise de 64 bits

En las versiones 2008 y 2012 de SQL Server anteriores a las mencionadas anteriormente, es necesario instalar la revisión especificada en el artículo [https://support2.microsoft.com/kb/2859713](https://support2.microsoft.com/kb/2859713) de Knowledge Base.

El Conector de SQL Server para Microsoft Azure Key Vault también requiere la versión 4.5.1 de .NET en la máquina virtual de Microsoft SQL Server en Azure. Esta biblioteca debe instalarse antes de instalar el Conector de SQL Server.

Debe tener instalada la versión adecuada de Visual Studio C++ redistribuible, según la versión de SQL Server que está ejecutando:

- Para las versiones 2008, 2008 R2, 2012 y 2014 de SQL Server, instale Visual C++ Redistributable 2013.

- Para SQL Server 2016, instale Visual C++ Redistributable 2015.

## <a name="modify-windows-registry-steps"></a>Pasos para modificar el Registro de Windows

Modifique las entradas del Registro para que el Conector de SQL Server pueda registrar errores y eventos de información en el Registro de eventos de aplicaciones de Windows.

> [!CAUTION]
> Siga los pasos de esta sección con cuidado y bajo su propia responsabilidad. Es posible que se produzcan problemas graves si el registro se modifica de forma incorrecta. Antes de modificar el Registro, [realice una copia de seguridad de este para restaurarlo](https://support.microsoft.com/help/322756) si tiene algún problema.

1. Existen dos formas de abrir el Editor del Registro en Windows 10:
    - En el cuadro de búsqueda de la barra de tareas, escriba **regedit**. Después, seleccione el resultado principal del Editor del Registro (aplicación de escritorio).

    ![Apertura de EKM regedit](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-open.png "Apertura de EKM regedit")
    - Mantenga presionado o haga clic con el botón derecho en el botón Inicio y seleccione Ejecutar. Escriba **regedit** en el cuadro de diálogo y seleccione **Aceptar**.

   ![Inicio de EKM regedit](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-start.png "Inicio de EKM regedit")

1. Vaya a esta clave del Registro:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\**

    ![EKM regedit AKV](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv.png "EKM regedit AKV")  

1. Agregue una nueva clave en **Azure Key Vault** con el nombre `Log`:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![Registro de EKM regedit AKV](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log.png "Registro de EKM regedit AKV.png")  

1. Debajo de la clave **Log**, agregue un valor DWORD (32 bits) con el nombre `Level`:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![Registro de DWORD EKM regedit AKV](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-dword.png "Registro de DWORD EKM regedit AKV")  

1. Establezca el valor de DWORD como el nivel de registro adecuado (0,1,2):
   1. 0 (información): **valor predeterminado**
   1. 1 (error)
   1. 2 (sin registro)

   ![Nivel de registro EKM regedit AKV](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-level.png "Nivel de registro EKM regedit AKV")  

Las entradas del Registro descritas en este artículo se encuentran en esta clave:

```console
\Computer
    \HKEY_LOCAL_MACHINE
       \SOFTWARE
          \Microsoft
             \SQL Server Cryptographic Provider
                \Azure Key Vault
                   \Log\
                      <Level>
```

Opcionalmente, puede usar la línea de comandos para generar la clave:

```cmd
--Create the logging parameter using (Administrator) Command Line:
REG ADD "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level /t REG_DWORD /d 1 

--Validate the new registry entry
REG QUERY "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level
```

Las entradas del Registro de eventos de aplicaciones que son mensajes que faltan también se pueden corregir con una entrada del Registro. El registro de eventos puede contener el mensaje `The description for Event ID 0 from source SQL Server Connector for Microsoft Azure Key Vault cannot be found...`.  

```cmd
--Create the registry entry to enable missing messages (this works with any version)
REG ADD "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile /t REG_EXPAND_SZ /d "C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll"

--Validate the new registry entry
REG QUERY "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile
```

## <a name="related-articles"></a>Artículos relacionados

- Para obtener otros scripts de ejemplo, vea el blog en [Cifrado de datos transparente de SQL Server y administración extensible de claves con Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
- [Administración extensible de claves (EKM)](extensible-key-management-ekm.md)  
- [Administración extensible de claves con Azure Key Vault](extensible-key-management-using-azure-key-vault-sql-server.md)
- [Mantenimiento y solución de problemas del conector de SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)
---
title: Introducción a SQL Server (en Linux) en la nube
titleSuffix: SQL Server
description: Obtenga información sobre cómo instalar SQL Server en Red Hat Enterprise Linux (RHEL), en SUSE Linux Enterprise Server (SLES) o en Ubuntu en la nube que prefiera.
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: ae7502fcba11d9892cbcf8ed2c94ef205e0abc53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273136"
---
# <a name="quickstart-run-sql-server-in-the-cloud"></a>Inicio rápido: Ejecutar SQL Server en la nube
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este inicio rápido, instalará SQL Server en Red Hat Enterprise Linux (RHEL), en SUSE Linux Enterprise Server (SLES) o en Ubuntu en la nube que prefiera. Vaya a [Aprovisionamiento de una máquina virtual Linux con SQL Server en Azure Portal](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%252fsql%252ftoc%252ftoc.json) para ejecutar SQL Server en Linux en Azure.

> [!NOTE]
> Si elige ejecutar una edición de pago de SQL Server, deberá traer su propia licencia (BYOL).

## <a name="amazon-web-services"></a>Amazon Web Services
1.  Cree una AMI de Linux con al menos 2 GB de memoria desde Marketplace. 
    * [RHEL 7.3+](https://aws.amazon.com/marketplace/pp/B00KWBZVK6)
    * [SLES v12 SP2+](https://aws.amazon.com/marketplace/pp/B00PMM99PI)
    * [Ubuntu 16.04](https://aws.amazon.com/marketplace/pp/B01JBL2M0O)
1.  Conéctese a la AMI mediante SSH.
1.  Siga el inicio rápido de la distribución de Linux que haya elegido: 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  Establezca una configuración de conexiones remotas: 
    * Abra la [consola de Amazon EC2]( https://console.aws.amazon.com/ec2/).
    * En el panel de navegación, elija **Security Groups** (Grupos de seguridad). 
    * Seleccione **Inbound, Edit, Add Rule** (Entrante, Editar, Agregar regla).
    * Agregue una regla de entrada para permitir el tráfico en el puerto en el que SQL Server escucha (el puerto TCP predeterminado 1433).

    
## <a name="digital-ocean"></a>Digital Ocean
1. Inicie sesión en el [panel de control](https://cloud.digitalocean.com/login) y haga clic en Create a droplet (Crear un droplet).
1. Elija un droplet de Ubuntu 16.04 con al menos 2 GB de memoria.
1. Conéctese al droplet mediante SSH.
1. Siga el [inicio rápido de Ubuntu](quickstart-install-connect-ubuntu.md).
1. Establezca una configuración de conexiones remotas:
    * En la parte superior del panel de control, use el vínculo **Networking** (Redes) y, después, seleccione **Firewalls**.
    * Agregue una regla de entrada para permitir el tráfico en el puerto en el que SQL Server escucha (el puerto TCP predeterminado 1433).
    
## <a name="google-cloud-platform"></a>Google Cloud Platform
1.  Cree una imagen de Linux con al menos 2 GB de memoria desde Cloud Launcher. 
    * [RHEL 7.3+](https://console.cloud.google.com/launcher/details/rhel-cloud/rhel-7)
    * [SLES v12 SP4](https://console.cloud.google.com/launcher/details/suse-cloud/sles-12)
    * [Ubuntu 16.04](https://console.cloud.google.com/launcher/details/ubuntu-os-cloud/ubuntu-xenial)
1.  Conéctese a la imagen mediante SSH.
1.  Siga el inicio rápido de la distribución de Linux que haya elegido: 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  Establezca una configuración de conexiones remotas: 
    * Vaya a [Firewall Rules](https://console.cloud.google.com/networking/firewalls) (Reglas de firewall).
    * Agregue una regla de entrada para permitir el tráfico en el puerto en el que SQL Server escucha (el puerto TCP predeterminado 1433).
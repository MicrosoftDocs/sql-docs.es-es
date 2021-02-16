---
title: Configuración de SSIS en Linux con ssis-conf
description: En este artículo se explica cómo configurar SQL Server Integration Services (SSIS) en Linux con la utilidad ssis-conf.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: da546bddb3b7b45753b224539a546f96663704c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348192"
---
# <a name="configure-sql-server-integration-services-on-linux-with-ssis-conf"></a>Configuración de SQL Server Integration Services en Linux con ssis-conf

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ejecute el script de configuración `ssis-conf` cuando instale SQL Server Integration Services (SSIS) para Red Hat Enterprise Linux y Ubuntu. Para obtener más información sobre la instalación de SSIS, vea [Instalar SQL Server Integration Services (SSIS) en Linux](sql-server-linux-setup-ssis.md).

También puede usar la utilidad `ssis-conf` para configurar las siguientes propiedades:

| Get-Help | Descripción |
|-------------|---------------------------------------------------------------------|
| set-edition | Establece la edición de SQL Server.                                       |
| telemetry   | Habilita o deshabilita el servicio de telemetría de SQL Server Integration Services. |
| instalar       | Inicializa y configura Microsoft SQL Server Integration Services.      |
|||

## <a name="run-ssis-conf"></a>Ejecución de ssis-conf

Los ejemplos de este artículo ejecutan `ssis-conf` al especificar la ruta de acceso completa: `/opt/ssis/bin/ssis-conf`. Si va a esa ubicación antes de ejecutar `ssis-conf`, puede ejecutar la utilidad en el contexto del directorio actual: `./ssis-conf`.

Asegúrese de ejecutar los comandos que se describen en este artículo con privilegios raíz. Por ejemplo, ejecute `sudo /opt/ssis/bin/ssis-conf setup` y no `/opt/ssis/bin/ssis-conf setup`.

Para ejecutar estos comandos con mensajes en el idioma que prefiera, puede especificar una configuración regional. Por ejemplo, para recibir mensajes en chino, ejecute el siguiente comando: `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="use-set-edition-to-set-the-edition-of-sql-server-integration-services"></a>Uso de set-edition para establecer la edición de SQL Server Integration Services

La edición de SSIS se alinea con la edición de SQL Server.

Escriba el comando siguiente: `$ sudo /opt/ssis/bin/ssis-conf set-edition`.

Después de escribir el comando, se recibe el siguiente mensaje:

```
Choose an edition of SQL Server:

1) Evaluation (free, no production use rights, 180-day limit)

2) Developer (free, no production use rights)

3) Express (free)

4) Web (PAID)

5) Standard (PAID)

6) Enterprise (PAID)

7) Enterprise Core (PAID)

8) I bought a license through a retail sales channel and have a product key to enter.

Details about editions can be found at https://go.microsoft.com/fwlink/?LinkId=852748&clcid=0x409.

Use of PAID editions of this software requires separate licensing through a Microsoft Volume Licensing program.

By choosing a PAID edition, you are verifying that you have the appropriate number of licenses in place to install and run this software.

Enter your edition (1-8):
```

Si escribe un valor de 1 a 7, el sistema configura una edición gratuita o DE PAGO. Si escribe 8, la utilidad le pide que escriba la clave del producto que ha adquirido:

```
Enter the 25-character product key:
```

## <a name="use-telemetry-to-configure-customer-feedback"></a>Usar telemetría para configurar los comentarios de los clientes

El comando `telemetry` determina si SSIS envía comentarios a Microsoft.

En el caso de las ediciones gratuitas (es decir, las ediciones Express, Developer y Evaluation), el servicio de telemetría siempre está habilitado. Si tiene una edición gratuita, no puede usar el comando `telemetry` para deshabilitar la telemetría.

Escriba el comando siguiente: `$ sudo /opt/ssis/bin/ssis-conf telemetry`.

En las ediciones DE PAGO, después de escribir el comando, se recibe el siguiente mensaje:

```
Send feature usage data to Microsoft. Feature usage data includes information about your hardware configuration and how you use SQL Server Integration Services.

[Yes/No]:
```

Si selecciona **Sí**, el servicio de telemetría se habilita y comienza a ejecutarse. El servicio se inicia automáticamente después de cada arranque. Si selecciona **No**, el servicio de telemetría se detiene y se deshabilita.

## <a name="use-setup-to-initialize-and-set-up-microsoft-sql-server-integration-services"></a>Usar la instalación para inicializar y configurar Microsoft SQL Server Integration Services

Use el comando `setup` cada vez que instale SSIS.

Escriba el comando siguiente: `sudo /opt/ssis/bin/ssis-conf setup`.

La utilidad le pide que confirme o proporcione valores para los siguientes elementos:
-   Licencia del producto
-   Contrato de licencia para el usuario final
-   Servicio de telemetría
-   El idioma empleado por Integration Services

Para ejecutar el comando `setup` con mensajes en el idioma que prefiera, puede especificar una configuración regional. Por ejemplo, para recibir mensajes en chino, ejecute el siguiente comando: `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="ssisconf-format"></a>Formato de ssis.conf

El siguiente archivo `/var/opt/ssis/ssis.conf` proporciona un ejemplo de cada valor.

En SQL Server, puede cambiar la configuración del sistema si modifica los valores del archivo `mssql.conf`. En SSIS, *no puede* cambiar la configuración del sistema si modifica los valores del archivo `ssis.conf`. El archivo `ssis.conf` muestra solo los resultados de la instalación. Si quiere cambiar la configuración de SSIS, puede eliminar el archivo `ssis.conf` y volver a ejecutar el comando `setup`.

Este es un archivo `ssis.conf` de ejemplo. Cada campo se corresponde con el resultado de un paso de instalación.

```
[LICENSE]
                       
registered = Y        
                       
pid = enterprisecore  
                       
[EULA]
                       
accepteula = Y        
                       
[TELEMETRY]
                       
enabled = Y           
                       
[language]
                       
lcid = 2052
```

## <a name="related-content-about-ssis-on-linux"></a>Contenido relacionado sobre SSIS en Linux
-   [Extracción, transformación y carga de datos en Linux con SSIS](sql-server-linux-migrate-ssis.md)
-   [Instalar SQL Server Integration Services (SSIS) en Linux](sql-server-linux-setup-ssis.md)
-   [Limitaciones y problemas conocidos de SSIS en Linux](sql-server-linux-ssis-known-issues.md)
-   [Programación de la ejecución de paquetes de SQL Server Integration Services en Linux con cron](sql-server-linux-schedule-ssis-packages.md)

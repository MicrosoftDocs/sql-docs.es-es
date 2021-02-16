---
title: Programación de paquetes SSIS en Linux con cron
description: En este artículo se explica cómo programar paquetes de SQL Server Integration Services (SSIS) en Linux con el servicio cron.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 75ac9be9d128f76157cee054674ecfe29de06063
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346428"
---
# <a name="schedule-sql-server-integration-services-package-execution-on-linux-with-cron"></a>Programación de la ejecución de paquetes de SQL Server Integration Services en Linux con cron

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Al ejecutar SQL Server Integration Services (SSIS) y SQL Server en Windows, puede automatizar la ejecución de paquetes SSIS mediante el Agente SQL Server. En cambio, al ejecutar SQL Server y SSIS en Linux, la utilidad del Agente SQL Server no está disponible para programar trabajos en Linux. En su lugar, puede usar el servicio cron, que se utiliza ampliamente en las plataformas Linux para automatizar la ejecución de paquetes.

En este artículo se proporcionan ejemplos en los que se muestra cómo automatizar la ejecución de paquetes SSIS. Los ejemplos están escritos para ejecutarse en Red Hat Enterprise. El código es similar para otras distribuciones de Linux, como Ubuntu.

## <a name="prerequisites"></a>Prerequisites

Antes de usar el servicio cron para ejecutar trabajos, compruebe si se está ejecutando en el equipo.

Para comprobar el estado del servicio cron, use el siguiente comando: `systemctl status crond.service`.

Si el servicio no está activo (es decir, no se está ejecutando), póngase en contacto con su administrador para configurar el servicio cron correctamente.

## <a name="create-jobs"></a>Creación de trabajos

Un trabajo de cron es una tarea que puede configurar para que se ejecute de forma periódica en un intervalo especificado. El trabajo puede ser tan sencillo como un comando que normalmente escribiría directamente en la consola o ejecutaría como un script del shell.

Para facilitar la administración y el mantenimiento, se recomienda colocar los comandos de ejecución de paquetes en un script que contenga un nombre descriptivo.

Aquí tiene un ejemplo de un script sencillo del shell para ejecutar un paquete. Solo contiene un comando, pero puede agregar más comandos según sea necesario.

```bash
# A simple shell script that contains a simple package execution command
# Script name: SSISpackageName.daily

/opt/ssis/bin/dtexec /F yourSSISpackageName.dtsx >> $HOME/tmp/out 2>&1
```

## <a name="schedule-jobs-with-the-cron-service"></a>Programación de trabajos con el servicio cron

Una vez que haya definido los trabajos, puede programarlos para que se ejecuten automáticamente mediante el servicio cron.

Para agregar el trabajo para que cron lo ejecute, agregue el trabajo en el archivo crontab. Para abrir el archivo crontab en un editor en el que pueda agregar o actualizar el trabajo, use el siguiente comando: `crontab -e`.

Para programar el trabajo descrito anteriormente para que se ejecute diariamente a las 2:10, agregue la siguiente línea al archivo crontab:

```
# run <SSIS package name> at 2:10 AM every day
10 2 \* \* \* $/HOME/SSIS/jobs/SSISpackageName.daily
```

Guarde el archivo crontab y salga del editor.

Para comprender el formato del comando de ejemplo, consulte la información de la siguiente sección.
 
## <a name="format-of-a-crontab-file"></a>Formato de un archivo crontab

En la imagen siguiente se muestra la descripción del formato de la línea de trabajo que se agrega al archivo crontab.

![Descripción del formato de la entrada en el archivo crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-job-definition.png)

Para obtener una descripción más detallada del formato del archivo crontab, use el siguiente comando: `man 5 crontab`.

Aquí tiene un ejemplo parcial de la salida que ayuda a explicar el ejemplo de este artículo:

![Descripción parcial detallada del formato crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-crontab-format.png)

## <a name="related-content-about-ssis-on-linux"></a>Contenido relacionado sobre SSIS en Linux
-   [Extracción, transformación y carga de datos en Linux con SSIS](sql-server-linux-migrate-ssis.md)
-   [Instalar SQL Server Integration Services (SSIS) en Linux](sql-server-linux-setup-ssis.md)
-   [Configurar SQL Server Integration Services en Linux con ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitaciones y problemas conocidos de SSIS en Linux](sql-server-linux-ssis-known-issues.md)

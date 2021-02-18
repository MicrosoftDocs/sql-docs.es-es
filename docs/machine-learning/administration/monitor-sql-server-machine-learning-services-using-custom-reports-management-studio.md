---
title: Supervisar scripts con informes personalizados
description: Use informes personalizados en SQL Server Management Studio (SSMS) para supervisar la ejecución de scripts externos (Python y R) y de los recursos usados, diagnosticar problemas y ajustar el rendimiento en SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0c3874177691832bd6fa66f81c3fe94c726ff76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337074"
---
# <a name="monitor-python-and-r-script-execution-using-custom-reports-in-sql-server-management-studio"></a>Supervisar la ejecución de scripts de R y Python mediante informes personalizados en SQL Server Management Studio
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Use informes personalizados en [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) para supervisar la ejecución de scripts externos (Python y R) y de los recursos usados, diagnosticar problemas y ajustar el rendimiento en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

En estos informes, puede ver detalles como:

- Sesiones activas de R o Python
- Opciones de configuración de la instancia
- Estadísticas de ejecución para trabajos de aprendizaje automático
- Eventos extendidos para R Services
- Lista de paquetes de Python o R instalados en la instancia actual

En este artículo se explica cómo instalar y usar los informes personalizados proporcionados para SQL Server Machine Learning Services.

Para obtener más información sobre los informes de SQL Server Management Studio, vea [Informes personalizados en Management Studio](../../ssms/object/custom-reports-in-management-studio.md).

## <a name="how-to-install-the-reports"></a>Cómo instalar los informes

Los informes se diseñan mediante SQL Server Reporting Services, pero pueden utilizarse directamente desde SQL Server Management Studio. No es necesario que Reporting Services esté instalado en la instancia de SQL Server.

Para usar estos informes, siga estos pasos:

1. Descargue los [informes personalizados de SSMS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) para SQL Server Machine Learning Services desde GitHub.

   ::: moniker range="=azuresqldb-mi-current"
   >[!NOTE]
   > No se admite el informe personalizado **ML Services: configuración de instancia** no es compatible con Azure SQL Managed Instance.
   ::: moniker-end

2. Copiar los informes en Management Studio

    1. Ubique la carpeta de informes personalizados utilizada por SQL Server Management Studio. De forma predeterminada, los informes personalizados se almacenan en esta carpeta (donde **user_name** es el nombre de usuario de Windows):

        `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

       También puede especificar una carpeta diferente o crear subcarpetas.

    2. Copie los archivos *.RDL descargados en la carpeta de informes personalizados.

3. Ejecutar los informes en Management Studio

    1. En Management Studio, haga clic en el nodo **Bases de datos** de la instancia donde quiere ejecutar los informes.

    2. Haga clic en **Informes** y, después, en **Informes personalizados**.

    3. En el cuadro de diálogo **Abrir archivo** , ubique la carpeta de informes personalizados.

    4. Seleccione uno de los archivos RDL que descargó y, luego, haga clic en **Abrir**.

## <a name="reports"></a>Informes

El repositorio de [informes personalizados de SSMS en GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) incluye los siguientes informes:

| Informe | Descripción |
|-|-|
| Sesiones activas | Usuarios que están conectados actualmente a la instancia de SQL Server y ejecutan un script de Python o R. |
| Configuración | Configuración de la instalación de Machine Learning Services y propiedades del tiempo de ejecución de Python o R. |
| Configurar una instancia | Configure Machine Learning Services. |
| Estadísticas de ejecución | Estadísticas de ejecución de Machine Learning Services. Por ejemplo, puede obtener el número total de ejecuciones de scripts externos y el número de ejecuciones paralelas. |
| Eventos extendidos | Eventos extendidos que están disponibles para obtener más información sobre la ejecución de scripts externos. |
| Paquetes | Lista de los paquetes de R o Python instalados en la instancia de SQL Server y sus propiedades, como la versión y el nombre. |
| Resource Usage | Vista de la CPU, la memoria, el consumo de E/S de SQL Server y la ejecución de scripts externos. También puede ver la configuración de la memoria para grupos de recursos externos. |

## <a name="next-steps"></a>Pasos siguientes

- [Supervisar SQL Server Machine Learning Services mediante vistas de administración dinámica (DMV)](monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)
- [Supervisar scripts de R y Python con eventos extendidos en SQL Server Machine Learning Services](extended-events.md)

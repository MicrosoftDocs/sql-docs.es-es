---
title: Extensión de Instancia administrada de Azure°SQL
description: Uso de Azure Data Studio con una instancia administrada de Azure SQL Managed Instance
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: alanyu, maghan, sstein
ms.custom: ''
ms.date: 10/07/2019
ms.openlocfilehash: 0c450da20d1e63e654201ccfcaf650dbebb57b4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052113"
---
# <a name="azure-sql-managed-instance-dashboard-for-azure-data-studio-preview"></a>Panel de Azure SQL Managed Instance para Azure Data Studio (versión preliminar)

La extensión de Azure SQL Managed Instance proporciona un panel para trabajar con una instancia de [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-index) en [Azure Data Studio](https://github.com/Microsoft/azuredatastudio). Esta extensión proporciona las características siguientes:

- Muestra las propiedades de SQL Managed Instance, incluidos los núcleos virtuales y el almacenamiento usado.
- Supervisa el uso de CPU y de almacenamiento de las dos horas anteriores.
- Muestra recomendaciones de ajustes y advertencias de configuración.
- Muestra el estado de las réplicas de bases de datos.
- Muestra los registros de errores con filtros.

## <a name="install"></a>Instalar

Puede instalar la versión oficial de esta extensión. Siga los pasos descritos en la [documentación de Azure Data Studio](./add-extensions.md).
En el panel **Extensiones**, busque "Instancia administrada" e instálela ahí. Una vez que esté instalada, recibirá notificaciones automáticamente sobre las futuras actualizaciones de la extensión.

Con la extensión instalada, verá la pestaña **Instancia administrada** en Azure Data Studio. Aquí puede encontrar información específica para su instancia administrada.

## <a name="properties"></a>Propiedades

Esta extensión muestra las características técnicas y el uso de algunos recursos de su instancia administrada.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-5.png" alt-text="Propiedades de la instancia administrada":::

En el panel superior se muestran los detalles siguientes:

- **Propiedades**. Obtenga información básica sobre su instancia administrada, incluidos los núcleos virtuales, la memoria y el almacenamiento disponibles. Conozca también su nivel de servicio actual, la generación de hardware y las características de E/S, como el rendimiento de escritura del registro de la instancia o las características de rendimiento de E/S de los archivos.
- **Almacenamiento SSD local**. En el nivel de servicio de uso general, los archivos de **TempDB** se almacenan de forma local. En el nivel de servicio crítico para la empresa, _todos_ los archivos de base de datos se almacenan en unidades de estado sólido locales. En esta sección, puede ver cuánto espacio del almacenamiento local usa su instancia administrada.
- **Azure Premium Disk Storage**. Si tiene el nivel de servicio de uso general, los archivos de base de datos del sistema y del usuario se almacenan en Azure Premium. En esta sección, puede ver la cantidad de datos usados, el número de archivos y el almacenamiento disponible. Esta sección está vacía en el nivel de servicio crítico para la empresa.
- **Uso de los recursos**. Vea el porcentaje de almacenamiento y CPU que ha usado su instancia administrada durante las dos horas anteriores. De este modo, puede aumentar el tamaño de la instancia si está cerca del límite.

## <a name="recommendations"></a>Recomendaciones

Si selecciona el segundo panel en la pestaña **Instancia administrada**, obtendrá recomendaciones y alertas que le ayudarán a optimizar su instancia administrada.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-6.png" alt-text="Recomendaciones para la instancia administrada":::

Es posible que vea algunas de las siguientes recomendaciones:

- **Se está alcanzando el límite de espacio de almacenamiento**. Elimine los datos innecesarios o aumente el tamaño de almacenamiento de la instancia. Las bases de datos que alcanzan el límite de almacenamiento podrían no procesar, o incluso no leer, las consultas.
- **Se está alcanzando el límite de rendimiento de la instancia**. Le informa cuando la carga está cerca del límite de su nivel de servicio: 22 MB/s para el nivel de servicio de uso general o 48 MB/s para el nivel de servicio crítico para la empresa. Tenga en cuenta que la instancia administrada limitará la carga para garantizar que se puedan realizar copias de seguridad.
- **Presión de memoria**. Una baja duración prevista de la página o una gran cantidad de estadísticas de espera `PAGEIOLATCH` podrían indicar que la instancia expulsa páginas de la memoria e intenta constantemente cargar más páginas del disco.
- **Límites del archivo de registro**. Si los archivos de registro están llegando a los [límites de E/S en el nivel de servicio de uso general](/azure/sql-database/sql-database-managed-instance-resource-limits#file-io-characteristics-in-general-purpose-tier), es posible que tenga que aumentar el tamaño del archivo de registro para obtener un mejor rendimiento.
- **Límites de los archivos de datos**. Si sus archivos de datos están llegando a los [límites de E/S en el nivel de servicio de uso general](/azure/sql-database/sql-database-managed-instance-resource-limits#file-io-characteristics-in-general-purpose-tier), es posible que tenga que aumentar el tamaño del archivo para obtener un mejor rendimiento. Este problema podría generar presión de memoria y ralentizar las copias de seguridad.
- **Problemas de disponibilidad**. Un gran número de archivos de registro virtuales puede afectar al rendimiento. Si se produce un error de proceso, estos problemas pueden dar lugar a una recuperación de bases de datos más larga en el nivel de servicio de uso general.

Revise periódicamente estas recomendaciones, investigue las causas principales y tome medidas para corregir cualquier problema. La extensión SQL Managed Instance proporciona scripts que puede ejecutar para mitigar algunos de los problemas detectados.

## <a name="replicas"></a>Réplicas

El tercer panel de la pestaña **Instancia administrada** muestra el estado de las réplicas de base de datos en su instancia administrada.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-7.png" alt-text="Réplicas de la instancia administrada":::

En el nivel de servicio de uso general, cada base de datos tiene una única réplica (principal). En una instancia de nivel de servicio crítico para la empresa, cada base de datos tiene una réplica principal y tres secundarias, una de las cuales se usa para las cargas de trabajo de solo lectura. En el panel **Réplicas**, puede supervisar el proceso de sincronización y comprobar que todas las réplicas secundarias están sincronizadas con la réplica principal.

## <a name="logs"></a>Registros

En el cuarto panel de **Instancia administrada** se muestran las entradas más recientes y relevantes del registro de errores de SQL.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-8.png" alt-text="Entradas de registro de la instancia administrada":::

Aunque su instancia administrada genera un gran número de entradas de registro, la mayoría de ellas son internas o de información del sistema. Además, algunas entradas del registro muestran los nombres de las bases de datos físicas (valores `GUID`) en lugar de los nombres de las bases de datos lógicas reales.

La extensión SQL Managed Instance filtra las entradas de registro innecesarias según el [método Dimitri Furman](https://techcommunity.microsoft.com/t5/DataCAT/Azure-SQL-DB-Managed-Instance-sp-readmierrorlog/ba-p/305506). La extensión también muestra los nombres de los archivos lógicos reales en lugar de los nombres físicos.

## <a name="reporting-problems"></a>Informar de problemas

Si tiene problemas con la extensión SQL Managed Instance, vaya al [proyecto de extensión de GitHub](https://github.com/JocaPC/AzureDataStudio-Managed-Instance/issues) e informe del problema.

## <a name="code-of-conduct"></a>Código de conducta

El proyecto ha adoptado el [Código de conducta de código abierto de Microsoft](https://opensource.microsoft.com/codeofconduct/).

Para obtener más información, vea las [Preguntas más frecuentes sobre el código de conducta](https://opensource.microsoft.com/codeofconduct/faq/), o póngase en contacto con [opencode@microsoft.com](mailto:opencode@microsoft.com) si tiene preguntas o comentarios.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, visite el [proyecto de GitHub](https://github.com/JocaPC/AzureDataStudio-Managed-Instance/).

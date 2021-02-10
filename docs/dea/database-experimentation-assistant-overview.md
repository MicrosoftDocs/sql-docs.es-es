---
title: Información general sobre el Asistente para experimentación con bases de datos
description: Obtenga más información sobre Asistente para experimentación con bases de datos (DEA), por ejemplo, cómo evaluar una versión de destino de SQL Server para una carga de trabajo específica.
ms.date: 12/12/2019
ms.prod: sql
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 09d334d7cfcc313cd36d3b59a6b47c64d441f3ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017075"
---
# <a name="overview-of-database-experimentation-assistant"></a>Información general de Asistente para experimentación con bases de datos

Asistente para experimentación con bases de datos (DEA) es una solución de experimentación para las actualizaciones de SQL Server. DEA puede ayudarle a evaluar una versión de destino de SQL Server para una carga de trabajo específica. Los clientes que se actualizan desde versiones anteriores de SQL Server (a partir de 2005) a versiones más recientes de SQL Server pueden utilizar las métricas de análisis que proporciona la herramienta.

Las métricas de análisis de DEA incluyen:

- Consultas que tienen errores de compatibilidad.
- Consultas degradadas y planes de consulta.
- Otros datos de comparación de carga de trabajo.

Los datos de comparación pueden dar lugar a una mayor confianza y ayudar a garantizar una experiencia de actualización correcta.

## <a name="get-dea"></a>Obtener DEA

Para instalar DEA, [Descargue](https://www.microsoft.com/download/details.aspx?id=54090) la versión más reciente de la herramienta. A continuación, ejecute el archivo de **DatabaseExperimentationAssistant.exe** .

## <a name="solution-architecture-for-comparing-workloads"></a>Arquitectura de la solución para comparar cargas de trabajo

En el diagrama siguiente se muestra la arquitectura de la solución para una comparación de carga de trabajo. La comparación de la carga de trabajo usa DEA y Distributed Replay durante una actualización de SQL Server 2008 a SQL Server 2016.

![Arquitectura de la solución de comparación de carga de trabajo](./media/database-experimentation-assistant-overview/dea-overview-compare-solution-architecture.png)

## <a name="dea-prerequisites"></a>Requisitos previos de DEA

A continuación se indican algunos requisitos previos para ejecutar DEA:

- Requisito de hardware mínimo: una máquina de un solo núcleo con 3,5 GB de RAM.
- Requisito de hardware ideal: una CPU de ocho núcleos (con 3,5 GB de RAM o más). Los procesadores con más de ocho núcleos no mejoran los tiempos de ejecución de DEA.
- Se necesita un 33% de tamaño de seguimiento de rendimiento adicional para almacenar las bases de datos de análisis de informes, B y.

## <a name="configure-dea"></a>Configuración de DEA

En la arquitectura de entorno de requisito previo, se recomienda instalar DEA *en el mismo equipo que el controlador de Distributed Replay*. Esta práctica evita llamadas entre equipos y simplifica la configuración.

### <a name="required-configuration-for-workload-comparison-using-dea"></a>Configuración necesaria para la comparación de cargas de trabajo mediante DEA

DEA se conecta a los servidores de bases de datos mediante la autenticación de Windows. Asegúrese de que el usuario que ejecuta DEA puede conectarse a los servidores de bases de datos (origen, destino y análisis) mediante la autenticación de Windows.

**Capturar requisitos de configuración**

La captura de un seguimiento requiere que el usuario ejecute DEA:

- Puede conectarse al servidor de base de datos de origen mediante la autenticación de Windows.
- Tiene derechos de administrador del servidor en el servidor de base de datos de origen.

Además, la cuenta de servicio que ejecuta el servidor de base de datos de origen requiere acceso de escritura a la ruta de acceso de la carpeta de seguimiento.

Para obtener más información, consulte preguntas más frecuentes [sobre la captura de seguimiento](database-experimentation-assistant-capture-trace.md#frequently-asked-questions-about-trace-capture).

**Requisitos de configuración de reproducción**

La reproducción de un seguimiento requiere que el usuario ejecute DEA:

- Puede conectarse al servidor de base de datos de destino mediante la autenticación de Windows.
- Tiene derechos sysadmin en el servidor de base de datos de destino.

Además, la reproducción de un seguimiento requiere lo siguiente:

- La cuenta de servicio que ejecuta los servidores de base de datos de destino tiene acceso de escritura a la ruta de acceso de la carpeta de seguimiento.
- La cuenta de servicio que ejecuta Distributed Replay clientes puede conectarse al servidor de base de datos de destino mediante la autenticación de Windows.
- Los puertos TCP se abren para las solicitudes entrantes en el controlador de Distributed Replay. DEA se comunica con el controlador de Distributed Replay mediante el uso de interfaces COM.

Para obtener más información, consulte preguntas más frecuentes [sobre la reproducción de seguimiento](database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay).

**Requisitos de configuración de análisis**

Para realizar el análisis, es necesario que el usuario ejecute DEA:

- Puede conectarse al servidor de base de datos de análisis mediante la autenticación de Windows.
- Tiene derechos de administrador del servidor en el servidor de base de datos de origen.

Para obtener más información, consulte preguntas más frecuentes [sobre los informes de análisis](database-experimentation-assistant-create-report.md#frequently-asked-questions-about-analysis-reports).

## <a name="set-up-telemetry"></a>Configuración de la telemetría

DEA tiene una característica habilitada para Internet que puede enviar información de telemetría a Microsoft para su uso en la mejora de la experiencia del producto. La información recopilada también se guarda en el equipo para la auditoría local, por lo que siempre puede ver lo que se ha recopilado. Todos los archivos de registro de DEA se guardan en la carpeta% Temp% \\ DEA.

Los datos de telemetría se pueden recopilar en cuatro tipos de eventos:

- **TraceEvent**: eventos de uso para la aplicación (por ejemplo, "desencadenar detención de captura").
- **Excepción**: excepción producida durante el uso de la aplicación.
- **DiagnosticEvent**: un registro de eventos para ayudar con el diagnóstico cuando se producen problemas (*no* se envían a Microsoft).
- **FeedbackEvent**: comentarios del usuario enviados a través de la aplicación.

Recopilar y enviar datos de telemetría es opcional. Para especificar los eventos que se recopilan y si los eventos recopilados se envían a Microsoft, siga estos pasos:

1. Vaya a la ubicación en la que se ha instalado DEA (por ejemplo, C: \\ archivos de programa (x86) \\ Microsoft Corporation \\ Asistente para experimentación con bases de datos).
2. Abra y modifique los archivos. config **DEA.exe.config** (para la aplicación) y **DEACmd.exe.config** (para la CLI) para resolver el escenario según corresponda:
    - Para dejar de recopilar un tipo de evento, establezca el valor de *evento* (por ejemplo, **TraceEvent**) en **false**. Para empezar a recopilar el evento de nuevo, establezca el valor en **true**.
    - Para dejar de guardar las copias locales de los eventos, establezca el valor de **TraceLoggerEnabled** en **false**. Para volver a guardar las copias locales, establezca el valor en **true**.
    - Para dejar de enviar eventos a Microsoft, establezca el valor de **AppInsightsLoggerEnabled** en **false**. Para empezar a enviar eventos a Microsoft de nuevo, establezca el valor en **true**.

DEA se rige por la [declaración de privacidad de Microsoft](https://aka.ms/dea-privacy).

## <a name="see-also"></a>Consulte también

- El artículo [información general sobre el proceso de comparación de cargas de trabajo](database-experimentation-assistant-get-started.md), que explica el proceso implicado en la comparación de cargas de trabajo en dos entornos.

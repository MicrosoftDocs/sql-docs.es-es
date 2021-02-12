---
title: Uso de widgets de información para supervisar servidores y bases de datos
description: Aprenda a usar widgets de información de Azure Data Studio para convertir las consultas que supervisan los servidores y las bases de datos en visualizaciones detalladas.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18, sqlfreshmay19, seo-lt-2019
ms.date: 05/14/2019
ms.openlocfilehash: 2b8f54b66852df0ce0300e950ba712a803a7a012
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040175"
---
# <a name="manage-servers-and-databases-with-insight-widgets-in-azure-data-studio"></a>Administración de servidores y bases de datos con widgets de información en Azure Data Studio

Los widgets de información toman las consultas de Transact-SQL (T-SQL) que se usan para supervisar los servidores y las bases de datos y las convierten en visualizaciones llenas de información.

La información adopta la forma de gráficos personalizables que se agregan a los paneles de supervisión de servidores y bases de datos. Vea información rápida de los servidores y las bases de datos, profundice en más detalles e inicie las acciones de administración que defina.

Puede compilar espectaculares paneles de administración de servidores y bases de datos de forma similar al ejemplo siguiente:

![Panel de base de datos](media/insight-widgets/database-dashboard.png)

Para lanzarse y comenzar a crear distintos tipos de widgets de información, vea los siguientes tutoriales:

- [Compilación de un widget de información personalizada](tutorial-build-custom-insight-sql-server.md)
- *Habilitación de widgets de información integrados*
  - [Habilitación de la información de supervisión de rendimiento](tutorial-qds-sql-server.md)
  - [Habilitación de la información de uso de espacio de tabla](tutorial-table-space-sql-server.md)

## <a name="sql-queries"></a>Consultas SQL

Azure Data Studio intenta evitar la incorporación de otro lenguaje u otra interfaz de usuario pesada, así que intenta usar T-SQL lo máximo posible con una configuración de JSON mínima. La configuración de widgets de información con T-SQL aprovecha el incontable número de orígenes existentes de consultas de T-SQL útiles que se pueden convertir en widgets llenos de información.

Los widgets de información se componen de una o dos consultas de T-SQL:
* La *consulta de widget de información* es obligatoria y es la que devuelve los datos que aparecen en el widget.
* La *consulta de detalles de información* solo es necesaria si se va a crear una página de detalles de información.

Una consulta de widget de información define un conjunto de datos que representa un recuento o un gráfico. La consulta de detalles de información se usa para presentar información de detalles relevante en formato tabular en el panel de detalles de información. 

Azure Data Studio ejecuta consultas de widget de información y asigna el conjunto de resultados de la consulta al conjunto de datos de un gráfico que luego representa. Cuando los usuarios abren los detalles de la información, se ejecuta la consulta de detalles de información y se imprime el resultado en una vista de cuadrícula en el cuadro de diálogo.

La idea básica es escribir una consulta de T-SQL de forma que se pueda usar como conjunto de datos de un recuento, un gráfico y un widget gráfico. 

## <a name="summary"></a>Resumen

La consulta de T-SQL y su conjunto de resultados determinan el comportamiento del widget de información. La escritura de una consulta para un tipo de gráfico o la asignación de un tipo de gráfico adecuado para una consulta existente es la consideración clave para compilar un widget de información eficaz.



## <a name="additional-resources"></a>Recursos adicionales
- [Editor de consultas](tutorial-sql-editor.md)


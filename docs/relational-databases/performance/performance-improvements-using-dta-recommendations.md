---
title: Mejoras de rendimiento recomendadas por DTA
description: Obtenga más información sobre cómo el Asistente para la optimización de motor de base de datos puede recomendar una combinación de índices de almacén de columnas y de filas al examinar una carga de trabajo de la base de datos determinada en SQL Server.
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, performance improvements
ms.assetid: 2e51ea06-81cb-4454-b111-da02808468e6
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 8c64f567d8da74a2eb15b5647a2b259ba52193b4
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678956"
---
# <a name="performance-improvements-using-database-engine-tuning-advisor-dta-recommendations"></a>Mejoras de rendimiento mediante las recomendaciones del Asistente para la optimización de motor de base de datos (DTA)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


---
El rendimiento del almacenamiento de datos y las cargas de trabajo analíticas pueden beneficiarse de los índices de **almacén de columnas** , especialmente para las consultas que necesitan examinar tablas de gran tamaño. Los índices de **almacén de filas** (árbol B+) son más eficaces para las consultas que tienen acceso a cantidades relativamente pequeñas de datos que buscan un valor o un rango de valores determinados. Puesto que los índices de almacén de filas pueden entregar filas ordenadas, también pueden reducir el costo de ordenación en los planes de ejecución de consultas. Por tanto, la elección de la combinación de índices de almacén de columnas y de filas que se van a generar depende de la carga de trabajo de la aplicación.

El Asistente para la optimización de motor de base de datos (DTA), a partir de SQL Server 2016, puede recomendar una **combinación adecuada de índices de almacén de columnas y de filas** al examinar una carga de trabajo de la base de datos determinada. 

Para demostrar las ventajas de las recomendaciones de DTA en el rendimiento de las cargas de trabajo, hemos probado varias cargas de trabajo de clientes reales. Para cada carga de trabajo de cliente, dejamos que DTA examinara las consultas individuales, así como la carga de trabajo completa de las consultas. Tuvimos en cuenta tres alternativas:
  
  1. **Solo el almacén de columnas** : genere solo los índices de almacén de columnas para todas las tablas sin usar DTA. 
  2. **DTA (solo el almacén de filas)** : ejecute DTA con la opción para recomendar solo los índices de almacén.
  3. **DTA (almacén de filas y de columnas)** : ejecute DTA con la opción para recomendar índices de almacén de filas y de columnas.  
   
En cada caso, implementamos los índices recomendados. Informamos sobre el promedio de tiempo de CPU (en milisegundos) en varias ejecuciones de la consulta o la carga de trabajo. En la figura siguiente se traza el tiempo de CPU en milisegundos de las cargas de trabajo de dos bases de datos de cliente distintas. Tenga en cuenta que el eje y (tiempo de CPU) utiliza una escala logarítmica.   


![Captura de pantalla de un gráfico de barras en la que se muestra el rendimiento del almacén de columnas y de filas del DTA.](../../relational-databases/performance/media/dta-columnstore-rowstore-performance.gif)



**Necesario para los diseños físicos mixtos** : el primer conjunto de barras correspondientes a la consulta 1 del cliente 1. DTA (almacén de filas y de columnas) recomienda un conjunto de 4 índices de columnas y 6 índices de almacén de filas, con lo que se obtiene un tiempo de CPU entre 2,5 y 4 veces inferior en comparación con solo el índice de almacén de columnas y DTA (solo el almacén de filas). Esto muestra las ventajas de los diseños físicos mixtos que constan de indices de almacén de filas y de columnas *incluso para una sola consulta* . 

**Eficacia de las recomendaciones de índices de almacén de filas** : el segundo y tercer conjunto de barras (correspondiente a la consulta 2 del cliente 1 y a la consulta del cliente 1 del cliente 2) son casos en los que las consultas tienen predicados de filtro selectivos que aprovechan los índices de almacén de filas adecuados. Para estas dos consultas, DTA (solo el almacén de filas) y DTA (almacén de filas y de columnas) recomienda solo los índices de almacén de filas. Estos ejemplos también demuestran que incluso cuando se invoca DTA con la opción para recomendar índices de almacén de columnas, el enfoque basado en costos garantiza que recomiende un índice de almacén solo si la carga de trabajo puede realmente beneficiarse de él.

**Eficacia de las recomendaciones de índices de almacén de columnas** : el cuarto conjunto de barras correspondiente a la consulta 2 del cliente 2 representa un caso en el que la consulta examina las tablas de gran tamaño que aprovecharían los índices de almacén de columnas. DTA (solo el almacén de filas) genera una recomendación cuyo tiempo de CPU es superior en comparación con cuando estaban los índices de almacén de columnas. DTA (almacén de filas y de columnas) recomienda los índices de almacén de columnas adecuados, con lo que los hace coincidir con el rendimiento de ejecución de consultas de la opción de solo el almacén de columnas.

**Eficacia de las recomendaciones para la carga de trabajo con varias consultas** : el conjunto final de barras correspondiente a la carga de trabajo completa del cliente 2 ejemplifica la capacidad de DTA para examinar varias consultas en la carga de trabajo con el objetivo de recomendar un conjunto apropiado de índices de almacén de columnas y de filas que pueda mejorar el costo de ejecución de la carga de trabajo general. DTA (almacén de filas y de columnas) recomienda 4 índices de almacén de columnas y decenas de índices de almacén de filas que dan lugar a una mejora de orden de magnitud de la carga de trabajo en comparación con la opción que crea solo los índices de almacén de columnas; y aproximadamente una mejora entre 4 y 5 veces superior en comparación con DTA (solo el almacén de filas).

En resumen, en los ejemplos anteriores se muestra la capacidad de DTA para aprovechar adecuadamente los índices de almacén de columnas y de filas compatibles en el motor de base de datos de SQL Server, y se recomienda una combinación adecuada de índices que puede reducir significativamente el tiempo de CPU para la carga de trabajo. 

<a name="see-also"></a>Consulte también
---
[Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)

[Recomendaciones de índice de almacén de columnas en el Asistente para la optimización de motor de base de datos (DTA)](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md)

[Descripción de los índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-overview.md)

[Índices de almacén de columnas para el almacenamiento de datos](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)

[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)

[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)




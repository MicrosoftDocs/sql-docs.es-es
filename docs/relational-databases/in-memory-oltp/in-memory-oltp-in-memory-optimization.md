---
title: OLTP en memoria (optimización en memoria) | Microsoft Docs
description: Use estos ejemplos y recursos de OLTP en memoria para mejorar considerablemente el rendimiento en SQL Server.
ms.custom: ''
ms.date: 11/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- In-Memory OLTP
- memory-optimized tables
ms.assetid: e1d03d74-2572-4a55-afd6-7edf0bc28bdb
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8d1dfd8b3d09adb4c3fdb0ad0d2bd02b1044f69a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351251"
---
# <a name="in-memory-oltp-and-memory-optimization"></a>OLTP en memoria y optimización en memoria

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] puede mejorar considerablemente el rendimiento de escenarios de datos transitorios, carga de datos, ingesta de datos y procesamiento de transacciones.  Para saltar al código básico y al conocimiento que necesita para probar rápidamente su propia tabla optimizada para memoria y procedimiento almacenado con compilación nativa, consulte
 -  [Inicio rápido 1: Tecnologías de OLTP en memoria para acelerar el rendimiento de Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md).  
 
Hemos cargado en YouTube un [**vídeo de 17 minutos**](#anchorname-17minute-video) que explica OLTP en memoria en SQL Server y muestra las ventajas de rendimiento.

Para información más detallada de OLTP en memoria y una revisión de los escenarios que ven beneficios de rendimiento gracias a la tecnología:

- [Información general y escenarios de uso](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)
 
 Cabe decir que [!INCLUDE[hek_2](../../includes/hek-2-md.md)] es la tecnología de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la que se mejora el rendimiento del procesamiento de transacciones. Para obtener más información sobre la tecnología de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la que se mejora el rendimiento de las consultas analíticas y de los informes, vea la [Guía de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
 Se han realizado varias mejoras de OLTP en memoria de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], además de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. El área expuesta de Transact-SQL aumentó para facilitar la migración de aplicaciones de base de datos. Se agregó compatibilidad para realizar operaciones ALTER para tablas optimizadas para memoria y procedimientos almacenados con compilación nativa, con el fin de facilitar el mantenimiento de las aplicaciones.
  
> [!NOTE]  
>  **Prueba**  
>   
>  OLTP en memoria está disponible en las bases de datos Azure SQL en el nivel Premium y crítico para la empresa y en grupos elásticos. Para una introducción a OLTP en memoria y al Almacén de columnas en Azure SQL Database, consulte [Optimizar el rendimiento con las tecnologías In-Memory en SQL Database](/azure/azure-sql/in-memory-oltp-overview).  
  

## <a name="in-this-section"></a>En esta sección  
 Esta sección proporciona los temas siguientes:  
  
|Tema|Descripción|  
|-----------|-----------------|  
|[Inicio rápido 1: Tecnologías de OLTP en memoria para acelerar el rendimiento de Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)|Profundice en OLTP en memoria|
|[Información general y escenarios de uso](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)|Información general sobre OLTP en memoria y cuáles son los escenarios que verán beneficios en el rendimiento.|
|[Requisitos para usar tablas con optimización para memoria](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)|Describe los requisitos de hardware y software y las instrucciones para utilizar tablas optimizadas para memoria.|  
|[Ejemplos de código de OLTP en memoria](./sample-database-for-in-memory-oltp.md)|Contiene ejemplos de código en los que se muestra cómo crear y utilizar una tabla optimizada para memoria.|  
|[Tablas optimizadas para la memoria](./sample-database-for-in-memory-oltp.md)|Presenta las tablas optimizadas para memoria.|  
|[Variables de tabla con optimización para memoria](./faster-temp-table-and-table-variable-by-using-memory-optimization.md)|Ejemplo de código que muestra cómo utilizar una variable de tabla optimizada para memoria en lugar de una variable de tabla tradicional para reducir el uso de tempdb.|  
|[Índices de las tablas con optimización para memoria](./indexes-for-memory-optimized-tables.md)|Presenta los índices optimizados para memoria.|  
|[Procedimientos almacenados compilados de forma nativa](./a-guide-to-query-processing-for-memory-optimized-tables.md)|Presenta los procedimientos almacenados compilados de forma nativa.|  
|[Administrar memoria para OLTP en memoria](/previous-versions/sql/sql-server-2016/dn465872(v=sql.130))|Descripción y administración del uso de memoria en el sistema.|  
|[Crear y administrar el almacenamiento de objetos optimizados para memoria](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)|Describe los archivos delta y de datos, que almacenan información sobre las transacciones en tablas optimizadas para memoria.|  
|[Hacer copia de seguridad, restaurar y recuperar tablas con optimización para memoria](/previous-versions/sql/sql-server-2016/dn624160(v=sql.130))|Describe las copias de seguridad, las restauraciones y las recuperaciones para tablas optimizadas para memoria.|  
|[Compatibilidad de Transact-SQL con OLTP en memoria](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)|Describe la compatibilidad de [!INCLUDE[tsql](../../includes/tsql-md.md)] para [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Compatibilidad con alta disponibilidad para bases de datos de OLTP en memoria](../../relational-databases/in-memory-oltp/high-availability-support-for-in-memory-oltp-databases.md)|Describe los grupos de disponibilidad y los clústeres de conmutación por error en [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Compatibilidad de SQL Server con OLTP en memoria](./transact-sql-support-for-in-memory-oltp.md)|Enumera la sintaxis nueva y la actualizada, y las características que admiten tablas optimizadas para memoria.|  
|[Migrar a OLTP en memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)|Describe cómo migrar las tablas basadas en disco a tablas optimizadas para memoria.|  
| &nbsp; | &nbsp; |

## <a name="links-to-other-websites"></a>Vínculos a otros sitios web

En esta sección se proporcionan vínculos a otros sitios web que contienen información sobre OLTP en memoria en SQL Server.

- [**Vídeo** que explica OLTP en memoria y muestra las ventajas de rendimiento](#anchorname-17minute-video)

- [Demostración de rendimiento de OLTP en memoria v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

-   [Notas del producto sobre características internas de OLTP en memoria de SQL Server](./sql-server-in-memory-oltp-internals-for-sql-server-2016.md)  

-   [Comparación de características de almacén de columnas y OLTP en memoria de SQL Server](https://download.microsoft.com/download/D/0/0/D0075580-6D72-403D-8B4D-C3BD88D58CE4/SQL_Server_2016_In_Memory_OLTP_and_Columnstore_Comparison_White_Paper.pdf)

-   Novedades de OLTP en memoria en SQL Server 2016, [parte 1](/archive/blogs/sqlserverstorageengine/in-memory-oltp-whats-new-in-sql2016-ctp3) y [parte 2](/archive/blogs/sqlserverstorageengine/whats-new-for-in-memory-oltp-in-sql-server-2016-since-ctp3)
  
-   [OLTP en memoria y los patrones de carga de trabajo comunes y consideraciones para la migración](/previous-versions/dn673538(v=msdn.10))  
  
-   [Blog de OLTP en memoria](https://cloudblogs.microsoft.com/sqlserver/2013/06/26/sql-server-2014-in-memory-technologies-blog-series-introduction/)  

## <a name="17-minute-video-indexed"></a><a name="anchorname-17minute-video"></a>Vídeo de 17 minutos, indexado

- _Título del vídeo:_ &nbsp; **OLTP en memoria en SQL Server 2016**
- _Fecha de publicación:_ &nbsp;10/03/2019, en `YouTube.com`.
- _Duración:_ &nbsp; 17:32 &nbsp; &nbsp; (consulte el siguiente [**índice**](#anchorname-index-17minute-video) para obtener los vínculos al vídeo).
- _Hospedado por:_ &nbsp; Jos de Bruijn, administrador de programas sénior de SQL Server

### <a name="demo-can-be-downloaded"></a>La demostración se puede descargar

En la marca de tiempo 08:09, el vídeo ejecuta una demostración dos veces. Puede descargar la el código fuente para la demostración de rendimiento ejecutable que se usa en el vídeo mediante el vínculo siguiente:

- [Demostración de rendimiento de OLTP en memoria v1.0, código fuente](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

Los pasos generales que se muestran en el vídeo son los siguientes:

1. En primer lugar, la demostración se ejecuta con una tabla normal.
2. A continuación, vemos una edición optimizada para memoria de la tabla que se va a crear y rellenar con unos pocos clics en SQL Server Management Studio (SSMS.exe).
3. Luego, la demostración se vuelve a ejecutar con la tabla optimizada para memoria. Se mide una mejora enorme de velocidad.

### <a name="index-to-each-section-in-the-video"></a><a name="anchorname-index-17minute-video"></a>Índice a cada sección del vídeo

| Vínculo de marca de tiempo | Título de la sección |
| :------------- | :------------ |
| A.&nbsp; [00:00](https://www.youtube.com/watch?v=l5l5eophmK4&t=0) | El comienzo. |
| <br/>B.&nbsp; [00:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=56) | <br/>Por qué a los clientes les tendría que importar OLTP en memoria. |
| &nbsp; &nbsp; [01:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=63) | El hardware moderno requiere una arquitectura moderna del sistema de bases de datos. |
| &nbsp; &nbsp; [02:10](https://www.youtube.com/watch?v=l5l5eophmK4&t=130) | Explosión en los datos que se generan; las operaciones tienen que ser instantáneas (latencia baja). |
| &nbsp; &nbsp; [03:19](https://www.youtube.com/watch?v=l5l5eophmK4&t=199) | Reducir el TCO: haga más cosas con los recursos que tiene. |
| <br/>C.&nbsp; [03:33](https://www.youtube.com/watch?v=l5l5eophmK4&t=213) | <br/>Qué es OLTP en memoria.<br/>Rendimiento optimizado mediante tecnología optimizada para memoria. |
| &nbsp; &nbsp; [05:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=303) | Procesamiento de transacciones hasta 30 veces más rápido. |
| &nbsp; &nbsp; [05:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=322) | Los datos totalmente duraderos sobreviven a los errores del servidor. |
| &nbsp; &nbsp; [06:15](https://www.youtube.com/watch?v=l5l5eophmK4&t=375) | Totalmente integrado en SQL Server. Por lo tanto, no hay que aprender nuevos lenguajes ni herramientas. |
| &nbsp; &nbsp; [07:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=442) | Se publicó por primera vez en SQL Server 2014, pero con mejoras importantes en 2016. |
| &nbsp; &nbsp; [07:58](https://www.youtube.com/watch?v=l5l5eophmK4&t=558) | Disponible en Azure SQL Database también (en la nube). |
| <br/>D.&nbsp; [08:09](https://www.youtube.com/watch?v=l5l5eophmK4&t=489) | <br/>Demostración de rendimiento.<br/> Ejecute la demostración con una tabla normal. |
| &nbsp; &nbsp; [09:11](https://www.youtube.com/watch?v=l5l5eophmK4&t=551) | Menú contextual de SSMS: **Informes** y **análisis del rendimiento de las transacciones** |
| &nbsp; &nbsp; [10:38](https://www.youtube.com/watch?v=l5l5eophmK4&t=638) | Menú contextual de SSMS: **Asesor de optimización en memoria**<br/> &nbsp; &nbsp; Cree realmente una tabla optimizada para memoria a partir de una tabla normal, además de migrar los datos. |
| &nbsp; &nbsp; [11:28](https://www.youtube.com/watch?v=l5l5eophmK4&t=688) | Vuelva a ejecutar la demostración, vea una mejora de 45x en la velocidad. |
| <br/>E.&nbsp; [12:17](https://www.youtube.com/watch?v=l5l5eophmK4&t=737) | <br/>OLTP en memoria más fácil de usar en SQL Server 2016 (en comparación con 2014). |
| &nbsp; &nbsp; [12:43](https://www.youtube.com/watch?v=l5l5eophmK4&t=763) | Análisis simplificado para ayudar con la migración de aplicaciones. |
| &nbsp; &nbsp; [13:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=783) | Reducción de la complejidad de la migración de aplicaciones a través de una mayor compatibilidad con el lenguaje Transact-SQL (por ejemplo, con claves externas y desencadenadores). |
| &nbsp; &nbsp; [13:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=836) | Mejora en la capacidad de administración.<br/> &nbsp; &nbsp; Por ejemplo, cambie el esquema y los índices, actualice automáticamente las estadísticas. |
| <br/>F.&nbsp; [14:46](https://www.youtube.com/watch?v=l5l5eophmK4&t=886) | <br/>Escalabilidad mejorada. |
| &nbsp; &nbsp; [15:12](https://www.youtube.com/watch?v=l5l5eophmK4&t=912) | Tablas grandes optimizadas para memoria (hasta 2 TB por base de datos). |
| &nbsp; &nbsp; [15:34](https://www.youtube.com/watch?v=l5l5eophmK4&t=934) | Escalado incluso mejor. |
| &nbsp; &nbsp; [16:41](https://www.youtube.com/watch?v=l5l5eophmK4&t=1001) | Haga más cosas con los recursos que ya tiene. |
| <br/>G.&nbsp; [16:53](https://www.youtube.com/watch?v=l5l5eophmK4&t=1013) | <br/>Comentarios finales. (Termina en 17:32). |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Consulte también  
 [Características de la base de datos](../databases/databases.md)  

---
title: Asistente para la optimización de motor de base de datos | Microsoft Docs
description: Obtenga más información sobre cómo usar el Asistente para la optimización de motor de base de datos para solucionar problemas, ajustar un gran conjunto de consultas, analizar cambios de diseño y administrar el espacio de almacenamiento en SQL Server.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.general.f1
ms.assetid: 50dd0a0b-a407-4aeb-bc8b-b02a793aa30a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b7e9a6c6aed8af08b565bfc5af46448c81e54ed2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236653"
---
# <a name="database-engine-tuning-advisor"></a>Database Engine Tuning Advisor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El Asistente para la optimización de motor de base de datos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (DTA) analiza las bases de datos y hace recomendaciones que puede usar para optimizar el rendimiento de las consultas. Puede usar el Asistente para la optimización de motor de base de datos a fin de seleccionar y crear un conjunto óptimo de índices, vistas indizadas o particiones de tabla sin necesidad de conocer detalladamente la estructura de la base de datos ni el funcionamiento interno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Con DTA, puede realizar las siguientes tareas.  
  
-   Solucionar problemas del rendimiento de una consulta específica  
  
-   Optimizar un conjunto grande de consultas en una o varias bases de datos  
  
-   Realizar análisis condicionales de exploración de posibles cambios de diseño físicos  
  
-   Administrar el espacio de almacenamiento  
  
## <a name="database-engine-tuning-advisor-benefits"></a>Ventajas del Asistente para la optimización de motor de base de datos  
 La optimización del rendimiento de las consultas puede ser difícil sin un conocimiento completo de la estructura de la base de datos y de las consultas que se ejecutan en ella. El **Asistente para la optimización de motor de base de datos (DTA)** puede facilitar esta tarea mediante el análisis de la caché del plan de consulta actual o de la carga de trabajo de las consultas de [!INCLUDE[tsql](../../includes/tsql-md.md)] que crea, y con la recomendación de un diseño físico adecuado. Para administradores de bases de datos más avanzadas, DTA expone un mecanismo eficaz para realizar análisis condicionales de exploración de diferentes alternativas de diseño físico. DTA puede proporcionar la siguiente información.  
  
-   Recomendar la mejor combinación de índices de [almacén de filas](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md) y de columnas para las bases de datos mediante el uso del optimizador de consultas para analizar las consultas de una carga de trabajo.  
  
-   Recomendar particiones alineadas y no alineadas para las bases de datos a las que se hace referencia en una carga de trabajo.  
  
-   Recomendar vistas indizadas para las bases de datos a las que se hace referencia en una carga de trabajo.  
  
-   Analizar los efectos de los cambios propuestos en aspectos tales como el uso de ííndices, la distribución de consultas entre tablas y el rendimiento de las consultas de la carga de trabajo.  
  
-   Recomendar métodos para optimizar la base de datos con respecto a un pequeño conjunto de consultas problemáticas.  
  
-   Permitirle personalizar la recomendación mediante la especificación de opciones avanzadas como, por ejemplo, las restricciones de espacio en disco.  
  
-   Proporcionar informes que resuman los efectos de la implementación de las recomendaciones en una carga de trabajo concreta.  

-   Considerar alternativas en las que se ofrezcan posibles opciones de diseño en forma de configuraciones hipotéticas para que el Asistente para la optimización de motor de base de datos pueda evaluarlas.

-   Ajuste las cargas de trabajo de una variedad de orígenes como Almacén de consultas de SQL Server, Caché del plan, archivos o tablas de Archivos de seguimiento de SQL Server o un archivo .SQL.

  
El Asistente para la optimización de motor de base de datos está diseñado para controlar los siguientes tipos de cargas de trabajo de consulta:  
  
-   Solo consultas de proceso de transacciones en línea (OLTP)  
  
-   Solo consultas de procesamiento analítico en línea (OLAP)  
  
-   Consultas OLTP y OLAP mixtas  
  
-   Cargas de trabajo con muchas consultas (más consultas que modificaciones de datos)  
  
-   Cargas de trabajo con muchas actualizaciones (más modificaciones de datos que consultas)  
  
## <a name="dta-components-and-concepts"></a>Componentes y conceptos de DTA  
 **Interfaz gráfica de usuario del Asistente para la optimización de motor de base de datos**  
 Una interfaz fácil de usar en la que puede especificar la carga de trabajo y seleccionar otras opciones de optimización.  
  
 **dta** (utilidad)  
 Versión del símbolo del sistema del Asistente para la optimización de motor de base de datos. La utilidad **dta** está diseñada para permitir usar la funcionalidad del Asistente para la optimización de motor de base de datos en aplicaciones y scripts.  
  
 **carga de trabajo**  
 Archivo de script Transact-SQL, archivo de seguimiento o tabla de seguimiento que contenga una carga de trabajo representativa para las bases de datos que desea optimizar. A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], puede especificar la memoria caché del plan como carga de trabajo.  A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede [especificar el Almacén de datos de consultas como carga de trabajo](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md). 
  
 **Archivo de entrada XML**  
 Un archivo con formato XML que el Asistente para la optimización de motor de base de datos puede usar para optimizar las cargas de trabajo. El archivo de entrada XML admite las opciones avanzadas de optimización que no están disponibles en la GUI ni en la utilidad **dta** .  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 El Asistente para la optimización de motor de base de datos tiene las siguientes limitaciones y restricciones.  
  
-   No puede agregar o quitar índices únicos o índices que aplican restricciones `PRIMARY KEY` o `UNIQUE`.  
  
-   No puede analizar una base de datos que esté configurada en modo de usuario único.  
  
-   Si especifica un espacio en disco máximo en las recomendaciones de optimización que supere el espacio disponible real, el Asistente para la optimización de motor de base de datos usa el valor especificado. Sin embargo, al ejecutar el script de recomendaciones para implementarlo, el script puede generar un error si antes no se agrega más espacio en disco. El espacio en disco máximo puede especificarse mediante la opción **-B** de la utilidad **dta** o especificando un valor en el cuadro de diálogo **Opciones avanzadas de optimización** .  
  
-   Por motivos de seguridad, el Asistente para la optimización de motor de base de datos no puede optimizar una carga de trabajo de una tabla de seguimiento que resida en un servidor remoto. Para evitar esta limitación, puede usar un archivo de seguimiento en lugar de una tabla de seguimiento o copiar la tabla de seguimiento en el servidor remoto.  
  
-   Al imponer restricciones, como las impuestas al especificar el espacio en disco máximo en las recomendaciones de optimización (mediante la opción **-B** o el cuadro de diálogo **Opciones avanzadas de optimización** ), el Asistente para la optimización de motor de base de datos puede verse forzado a quitar algunos índices existentes. En ese caso, la recomendación resultante del Asistente para la optimización de motor de base de datos puede producir lo contrario a la mejora esperada.  
  
-   Al especificar una restricción para limitar el tiempo de optimización (mediante la opción **-A** con la utilidad **dta** o activando **Limitar tiempo de optimización** en la pestaña **Opciones de optimización** ), el Asistente para la optimización de motor de base de datos puede exceder ese límite de tiempo para generar la mejora esperada exacta e informes de análisis de la parte de la carga de trabajo que se ha consumido hasta ahora.  
  
-   El Asistente para la optimización de motor de base de datos no hace recomendaciones en las siguientes circunstancias:  
  
    1.  La tabla que se está optimizando contiene menos de 10 páginas de datos.  
  
    2.  Los índices recomendados no ofrecen claras posibilidades de mejora del rendimiento de las consultas respecto al diseño de la base de datos física actual.  
  
    3.  El usuario que ejecuta el Asistente para la optimización de motor de base de datos no es miembro del rol de base de datos **db_owner** ni del rol fijo de servidor **sysadmin** . Las consultas de la carga de trabajo se analizan en el contexto de seguridad del usuario que ejecuta el Asistente para la optimización de motor de base de datos. El usuario debe ser miembro del rol de base de datos **db_owner** .  
  
-   El Asistente para la optimización de motor de base de datos almacena la información de optimización de la sesión y otros datos en la base de datos **msdb** . Si se realizan cambios en la base de datos **msdb** , existe el riesgo de que se pierdan los datos de optimización de la sesión. Para eliminar este riesgo, implemente una estrategia de copia de seguridad adecuada para la base de datos **msdb** .  
  
## <a name="performance-considerations"></a>Consideraciones de rendimiento  
 El Asistente para la optimización de motor de base de datos puede consumir muchos recursos de procesador y memoria durante el análisis. Para evitar que el servidor de producción se ralentice, siga una de estas estrategias:  
  
-   Optimice las bases de datos cuando el servidor esté libre. El Asistente para la optimización de motor de base de datos puede afectar al rendimiento de las tareas de mantenimiento.  
  
-   Utilice la característica de servidor de prueba/producción. Para obtener más información, vea  [Reducir la carga de optimización del servidor de producción](../../relational-databases/performance/reduce-the-production-server-tuning-load.md).  
  
-   Especifique solo las estructuras de diseño de la base de datos física que desee que el Asistente para la optimización de motor de base de datos analice. El Asistente para la optimización de motor de base de datos proporciona muchas opciones, pero especifica solo las necesarias.  
  
## <a name="dependency-on-xp_msver-extended-stored-procedure"></a>Dependencia en el procedimiento almacenado extendido xp_msver  
 El Asistente para la optimización de motor de base de datos depende del procedimiento almacenado extendido **xp_msver** para poder ofrecer una funcionalidad completa. Este procedimiento almacenado extendido está activado de manera predeterminada. El Asistente para la optimización de motor de base de datos usa este procedimiento almacenado extendido para obtener el número de procesadores y la memoria disponible del equipo en el que reside la base de datos que está optimizando. Si **xp_msver** no está disponible, el Asistente para la optimización de motor de base de datos adopta las características de hardware del equipo donde se ejecuta el Asistente para la optimización de motor de base de datos. Si no están disponibles las características de hardware del equipo donde se ejecuta el Asistente para la optimización de motor de base de datos, se presuponen un procesador y 1.024 MB de memoria.  
  
 Esta dependencia afecta a las recomendaciones de partición porque el número de particiones recomendadas depende de estos dos valores (número de procesadores y memoria). La dependencia afecta además a los resultados de optimización si utiliza un servidor de prueba para optimizar el servidor de producción. En este escenario, el Asistente para la optimización de motor de base de datos usa **xp_msver** para obtener propiedades de hardware del servidor de producción. Después de optimizar la carga de trabajo en el servidor de prueba, el Asistente para la optimización de motor de base de datos usa estas propiedades de hardware para generar una recomendación. Para obtener más información, vea [xp_msver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-msver-transact-sql.md).  
  
## <a name="database-engine-tuning-advisor-tasks"></a>Tareas del Asistente para la optimización de motor de base de datos  
 En la tabla siguiente se enumeran las tareas comunes del Asistente para la optimización de motor de base de datos y los temas en los que se describe cómo realizarlas.  
  
|Tarea del Asistente para la optimización de motor de base de datos|Tema|  
|-----------------------------------------|-----------|  
|Inicializar e iniciar el Asistente para la optimización de motor de base de datos.<br /><br /> Crear una carga de trabajo mediante la especificación de la memoria caché del plan, la creación de un script o la generación de un archivo o una tabla de seguimiento.<br /><br /> Optimizar una base de datos mediante la herramienta de interfaz gráfica de usuario del Asistente para la optimización de motor de base de datos.<br /><br /> Crear archivos de entrada XML para optimizar cargas de trabajo.<br /><br /> Ver descripciones de las opciones de la interfaz de usuario del Asistente para la optimización de motor de base de datos.|[Iniciar y utilizar el Asistente para la optimización de motor de base de datos](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)|  
|Ver los resultados de la operación de optimización de la base de datos.<br /><br /> Seleccionar e implementar las recomendaciones de optimización.<br /><br /> Realizar análisis de exploración condicionales en la carga de trabajo.<br /><br /> Revisar sesiones de optimización existentes, clonar sesiones basándose en las existentes <br />o editar recomendaciones de optimización existentes para su posterior evaluación o implementación.<br /><br /> Ver descripciones de las opciones de la interfaz de usuario del Asistente para la optimización de motor de base de datos.|[Ver y trabajar con la salida del Asistente para la optimización de motor de base de datos](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)|  
  
  

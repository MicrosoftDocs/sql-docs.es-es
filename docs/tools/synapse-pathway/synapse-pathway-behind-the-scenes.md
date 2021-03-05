---
title: Versión preliminar de Azure Synapse Pathway, entre bambalinas
description: Información técnica detallada sobre la manera en que Azure Synapse Pathway traduce el código.
author: anshul82-ms
ms.author: anrampal.
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: conceptual
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-concept
ms.openlocfilehash: dbd362e53b5bfcd916c53e90d6f66c8fb44f0374
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873146"
---
# <a name="azure-synapse-pathway-preview-behind-the-scenes"></a>Versión preliminar de Azure Synapse Pathway, entre bambalinas
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

El objetivo de Azure Synapse Pathway consiste en conservar la intención funcional del código original al optimizarlo para Synapse SQL. Synapse Pathway usa un proceso de tres fases para traducir código SQL de un sistema de origen a Azure Synapse SQL.

Cada una de las fases conserva y aumenta el conocimiento del origen, incluidos los metadatos específicos del origen, para garantizar la máxima calidad de la traducción.

 ![Azure Synapse Pathway](./media/technical-deep-dive/behind-the-scene.png)

## <a name="stage-1--lexing-and-parsing"></a>Fase 1: tokenización y análisis

El análisis del lenguaje SQL es un problema que se ha resuelto muchas veces. Hay muchos analizadores comerciales y de código abierto que resultan de ayuda para el proceso subyacente de tomar una instrucción de origen, dividirla en tokens lógicos y, después, ejecutarla con respecto a un conjunto o a reglas del analizador para garantizar la coherencia del lenguaje. 

Synapse Pathway define gramáticas de origen que permiten a la herramienta identificar y procesar la entrada SQL en un árbol de sintaxis abstracta (AST) aumentado que se usa en el procesamiento posterior. 

## <a name="stage-2---augmented-abstract-syntax-tree-ast"></a>Fase 2: árbol de sintaxis abstracta (AST) aumentado

Synapse Pathway define una representación común de todos los objetos en un árbol de sintaxis abstracta (AST) ampliado. El AST de Synapse Pathway incluye instrucciones adicionales o metadatos fragmentados que se usan para tomar la decisión adecuada al traducir código entre los dos sistemas.

Al realizar un seguimiento no solo de que un token es una función, sino del requisito del tipo de sistema de origen, los componentes de generación de scripts pueden tomar decisiones más inteligentes sobre la traducción a Synapse SQL.

Por ejemplo, la función de origen de la función absoluta se define de la manera siguiente:

```sql  
ABS( float_expression ) 
```

Azure Synapse SQL define la función absoluta como se indica a continuación:

```sql  
ABS ( numeric_expression )  
```

En este sencillo caso, Synapse Pathway entiende que la conversión en Synapse SQL de "float" a "numeric" es una [conversión](../../t-sql/functions/cast-and-convert-transact-sql?view=azure-sqldw-latest#implicit-conversions) implícita y no requiere ninguna conversión adicional. En resumen, es una traducción de código sencilla, limpia y eficaz.

El hecho de conservar esta metainformación sobre las instrucciones y los fragmentos de origen resulta de ayuda en el caso de diferencias estructurales entre las plataformas (por ejemplo, en conversiones en la lógica de cancelación para predicados de condiciones de búsqueda en una cláusula WHERE).

## <a name="stage-3---syntax-generation"></a>Fase 3: generación de la sintaxis

La última fase del proceso consiste en generar la sintaxis para Synapse SQL. Con la estructura de AST generada a partir de los archivos de origen, Synapse Pathway escribe cada objeto DDL en un archivo individual. Los generadores de la sintaxis usan conocimientos detallados de la plataforma de destino para optimizar las instrucciones.

Por ejemplo, un patrón común que suele verse en escenarios de carga de datos consiste en eliminar primero todo el contenido de una tabla de almacenamiento provisional y, luego, cargar los datos de otra tabla de almacenamiento provisional mediante un modo INSERT/SELECT.

```sql  
DELETE staging.table1 ALL;
INSERT INTO staging.table1…
FROM staging.table2;
```

Synapse SQL tiene una ruta optimizada para este escenario: [CREATE TABLE AS SELECT](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-develop-ctas). La instrucción CTAS es una operación basada en lotes con un registro mínimo que impulsa un alto rendimiento mediante el uso de toda la infraestructura de proceso disponible. Sin esta información sobre Synapse SQL, las herramientas suelen generar una instrucción TRUNCATE e INSERT/SELECT.

```sql  
TRUNCATE TABLE staging.table1;
INSERT INTO staging.table1…
FROM staging.table2;
```

Aunque esto no es incorrecto, este código se puede optimizar a DROP TABLE y CTAS para conseguir un mayor rendimiento.

```sql  
DROP TABLE staging.table1;
CREATE TABLE staging.table1
WITH
(
    -- Derived from the original table definition 
    DISTRIBUTION = HASH(column1),
    -- Derived from the original table definition
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT  * FROM staging.table2;
```

## <a name="next-steps"></a>Pasos siguientes

- [Descarga de Azure Synapse Pathway](synapse-pathway-download.md)
- [P+F](pathway-faq.md)


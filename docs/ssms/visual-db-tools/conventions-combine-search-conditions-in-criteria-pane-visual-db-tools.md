---
description: Convenciones para combinar condiciones de búsqueda en el panel Criterios (Visual Database Tools)
title: Convenciones para combinar condiciones de búsqueda en el panel Criterios
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search conditions [SQL Server], combining
- precedence [SQL Server], Criteria pane
- search criteria [SQL Server], combining conditions
- multiple OR clauses
- combining search conditions
- OR operator
- AND, Criteria pane
- multiple AND clauses
ms.assetid: d4859be5-ff5b-48b2-a101-ad40c6dbcc68
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: fe9c75b0b173bf2f72437233d2c06305b5041132
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355200"
---
# <a name="conventions-for-combining-search-conditions-in-the-criteria-pane-visual-database-tools"></a>Convenciones para combinar condiciones de búsqueda en el panel Criterios (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Puede crear consultas que incluyan un número arbitrario de condiciones de búsqueda vinculadas con un número arbitrario de operadores AND y OR. Una consulta con una combinación de cláusulas AND y OR puede llegar a ser compleja, por lo que le será de ayuda entender cómo se interpreta una consulta cuando se ejecuta y cómo se representa en el [panel Criterios](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md) y en el [panel SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
> [!NOTE]  
> Para detalles sobre las condiciones de búsqueda que solo contienen un operador AND u OR, consulte [Especificar varias condiciones de búsqueda para una columna &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-one-column-visual-database-tools.md) y [Especificar varias condiciones de búsqueda para varias columnas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-multiple-columns-visual-database-tools.md).  
  
Más adelante encontrará información sobre:  
  
-   La prioridad de los operadores AND y OR en consultas que contienen los dos tipos de operador.  
  
-   Cómo se relacionan lógicamente entre sí las condiciones en las cláusulas AND y OR.  
  
-   Cómo representa el Diseñador de consultas y vistas las consultas que contienen los operadores AND y OR en el panel Criterios.  
  
Para ayudarle a entender lo que se trata a continuación, suponga que trabaja con una tabla `employee` que contiene las columnas `hire_date`, `job_lvl`y `status`. En los ejemplos se supone que necesita conocer cierta información, como la antigüedad de un empleado en la compañía (es decir, la fecha de contratación del empleado), el tipo de trabajo que realiza (su categoría laboral) y su estado (por ejemplo, jubilado).  
  
## <a name="precedence-of-and-and-or"></a>Prioridad de AND y OR  
Cuando se ejecuta una consulta, primero se evalúan las cláusulas vinculadas con operadores AND y después las vinculadas con operadores OR.  
  
> [!NOTE]  
> El operador NOT tiene prioridad sobre AND y OR.  
  
Por ejemplo, para buscar empleados con una antigüedad de más de cinco años en puestos de categoría inferior o empleados con puestos de categoría intermedia sin tener en cuenta su fecha de contratación, puede crear una cláusula WHERE como la siguiente:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   job_lvl = 100 OR  
   job_lvl = 200  
```  
  
Para pasar por alto la prioridad predeterminada de AND sobre OR, puede escribir las condiciones específicas entre paréntesis en el panel SQL. Siempre se evalúan primero las condiciones que van entre paréntesis. Por ejemplo, para buscar todos los empleados con una antigüedad superior a cinco años en puestos inferiores o intermedios, puede crear una cláusula WHERE como la siguiente:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
> [!TIP]  
> Para mayor claridad, se recomienda escribir siempre paréntesis al combinar las cláusulas AND y OR en lugar de basarse en las prioridades predeterminadas.  
  
## <a name="how-and-works-with-multiple-or-clauses"></a>Cómo funciona AND con varias cláusulas OR  
Comprender cómo se relacionan las cláusulas AND y OR cuando se combinan le ayudará a crear y comprender consultas complejas en el Diseñador de consultas y vistas.  
  
Si vincula varias condiciones con el operador lógico AND, el primer conjunto de condiciones vinculadas con AND se aplica a todas las condiciones del segundo conjunto. Es decir, una condición vinculada con otra condición mediante AND se distribuye a todas las condiciones del segundo conjunto. Por ejemplo, la siguiente representación esquemática muestra una condición AND vinculada a un conjunto de condiciones OR:  
  
```  
A AND (B OR C)  
```  
  
La representación anterior es equivalente lógicamente a la siguiente representación esquemática, que muestra cómo se aplica la condición AND al segundo conjunto de condiciones:  
  
```  
(A AND B) OR (A AND C)  
```  
  
Este principio distributivo afecta al uso del Diseñador de consultas y vistas. Por ejemplo, suponga que busca todos empleados con una antigüedad superior a cinco años en puestos inferiores o intermedios. Escriba la siguiente cláusula WHERE en la instrucción del panel SQL:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
La cláusula vinculada mediante AND se aplica a las dos cláusulas vinculadas mediante OR. Una forma explícita de expresar esto es repetir la condición AND una vez por cada condición de la cláusula OR. La siguiente instrucción es más explícita (y más larga) que la instrucción anterior, pero es equivalente lógicamente a ella:  
  
```  
WHERE    (hire_date < '01/01/95' ) AND  
  (job_lvl = 100) OR   
  (hire_date < '01/01/95' ) AND   
  (job_lvl = 200)  
```  
  
Se aplica el principio de distribuir cláusulas AND en cláusulas OR independientemente del número de condiciones individuales utilizadas. Por ejemplo, suponga que desea encontrar todos los empleados con puestos de categoría superior o intermedia con una antigüedad de más de cinco años o que estén jubilados. La cláusula WHERE sería parecida a ésta:  
  
```  
WHERE   
   (job_lvl = 200 OR job_lvl = 300) AND  
   (hire_date < '01/01/95' ) OR (status = 'R')  
```  
  
Después de distribuir estas condiciones vinculadas con AND, la cláusula WHERE se parecerá a ésta:  
  
```  
WHERE   
   (job_lvl = 200 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 200 AND status = 'R') OR  
   (job_lvl = 300 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 300 AND status = 'R')  
```  
  
## <a name="how-multiple-and-and-or-clauses-are-represented-in-the-criteria-pane"></a>Cómo se representan varias cláusulas AND y OR en el panel Criterios  
El Diseñador de consultas y vistas representa las condiciones de búsqueda en el [panel Criterios](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md). Sin embargo, en algunos casos en los que hay varias cláusulas vinculadas con AND y OR, la representación del panel Criterios podría no ser la esperada. Además, si modifica una consulta en el panel Criterios o el [panel Diagrama](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md), podría suceder que la instrucción SQL mostrada sea distinta de la que escribió.  
  
En general, estas reglas definen cómo se van a mostrar las cláusulas AND y OR en el panel Criterios:  
  
-   Todas las condiciones vinculadas con AND aparecerán o bien en la misma columna **O...** o bien en la columna de cuadrícula **Filtro** .  
  
-   Todas las condiciones vinculadas con OR aparecerán en columnas **O…** separadas.  
  
-   Si el resultado lógico de una combinación de cláusulas AND y OR es que AND se distribuye en varias cláusulas OR, esto se representará de forma explícita en el panel Criterios donde la cláusula AND se repetirá tantas veces como sea necesario.  
  
Por ejemplo, podría crear en el panel SQL una condición de búsqueda como la siguiente, en la que dos cláusulas vinculadas mediante AND tienen prioridad sobre una tercera cláusula vinculada mediante OR:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  (job_lvl = 100) OR   
  (status = 'R')  
```  
  
El Diseñador de consultas y vistas representa esta cláusula WHERE en el panel Criterios de la siguiente manera:  
  
![Precedencia de la cláusula OR en el panel Criterios](../../ssms/visual-db-tools/media/vs_criteriapane1.gif "Precedencia de la cláusula OR en el panel Criterios")  
  
Sin embargo, si las cláusulas OR vinculadas tienen prioridad sobre una cláusula AND, se repetirá la cláusula AND para cada cláusula OR. Esto hace que la cláusula AND se distribuya en cada cláusula OR. Por ejemplo, en el panel SQL podría crearse una cláusula WHERE como la siguiente:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ( (job_lvl = 100) OR   
  (status = 'R') )  
```  
  
El Diseñador de consultas y vistas representa esta cláusula WHERE en el panel Criterios de la siguiente manera:  
  
![Varias cláusulas AND y OR en el panel Criterios](../../ssms/visual-db-tools/media/vs_criteriapane2.gif "Varias cláusulas AND y OR en el panel Criterios")  
  
Si las cláusulas OR vinculadas solo incluyen una columna de datos, el Diseñador de consultas y vistas puede colocar la cláusula OR completa en una sola celda de la cuadrícula y así se evita la necesidad de repetir la cláusula AND. Por ejemplo, en el panel SQL podría crearse una cláusula WHERE como la siguiente:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ((status = 'R') OR (status = 'A'))  
```  
  
El Diseñador de consultas y vistas representa esta cláusula WHERE en el panel Criterios de la siguiente manera:  
  
![Cláusulas OR vinculadas definidas en el panel Criterios](../../ssms/visual-db-tools/media/vs_criteriapane3.gif "Cláusulas OR vinculadas definidas en el panel Criterios")  
  
Si cambia la consulta (cambia, por ejemplo, uno de los valores en el panel Criterios), el Diseñador de consultas y vistas volverá a crear la instrucción SQL en el panel SQL. La nueva instrucción SQL reflejará lo que se muestra en el panel Criterios en lugar de lo que se indica en la instrucción original. Por ejemplo, si el panel Criterios contiene cláusulas AND distribuidas, se volverá a crear la instrucción resultante en el panel SQL con cláusulas AND explícitas. Para obtener información detallada, vea "Cómo funciona AND con varias cláusulas OR" anteriormente en este tema.  
  
## <a name="see-also"></a>Vea también  
[Especificar criterios de búsqueda (Visual Database Tools)](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  

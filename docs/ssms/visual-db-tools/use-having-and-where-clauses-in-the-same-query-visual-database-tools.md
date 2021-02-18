---
description: Utilizar cláusulas HAVING y WHERE en la misma consulta (Visual Database Tools)
title: Uso de cláusulas HAVING y WHERE en la misma consulta
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search criteria [SQL Server], excluding rows
- search criteria [SQL Server], WHERE clause
- search conditions [SQL Server], HAVING clause
- row excluded in search [SQL Server]
- search criteria [SQL Server], HAVING clause
- HAVING clause, search criteria
- search conditions [SQL Server], WHERE clause
- WHERE clause, search criteria
- excluding rows
ms.assetid: 1e07cf56-b4b7-4c49-8ddd-c276812a7148
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: cdaf2096589b5a1b16753c79c7eac6cda08fff68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350446"
---
# <a name="use-having-and-where-clauses-in-the-same-query-visual-database-tools"></a>Utilizar cláusulas HAVING y WHERE en la misma consulta (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
En algunas ocasiones, será conveniente excluir algunas filas de los grupos (utilizando una cláusula WHERE) antes de aplicar una condición a los grupos como un todo (utilizando una cláusula HAVING).  
  
Una cláusula HAVING es como una cláusula WHERE, pero que solo se aplica a los grupos en su totalidad (es decir, a las filas del conjunto de resultados que representa los grupos), a diferencia de la cláusula WHERE, que se aplica a filas individuales. Una consulta puede contener tanto una cláusula WHERE como una cláusula HAVING. En tal caso:  
  
-   La cláusula WHERE se aplica primero a las filas individuales de las tablas u objetos con valores de tabla del panel Diagrama. Solo se agrupan las filas que cumplen las condiciones de la cláusula WHERE.  
  
-   La cláusula HAVING se aplica a continuación a las filas del conjunto de resultados. Solo aparecen en el resultado de la consulta los grupos que cumplen las condiciones HAVING. Solo puede aplicar una cláusula HAVING a las columnas que también aparecen en la cláusula GROUP BY o en una función de agregado.  
  
Imagine, por ejemplo, que va a combinar las tablas `titles` y `publishers` para crear una consulta que muestre el precio medio de los libros de un conjunto de editoriales. Solo quiere ver el precio medio de un conjunto específico de editores: los del estado de California, por ejemplo. Y además, solo quiere ver el precio medio si éste es superior a 10,00 USD.  
  
Puede establecer la primera condición incluyendo una cláusula WHERE, que descarta todas las editoriales que no están en California, antes de calcular los precios medios. La segunda condición requiere una cláusula HAVING, ya que se basa en los resultados de la agrupación y del resumen de los datos. La instrucción SQL resultante podría tener la forma siguiente:  
  
```  
SELECT titles.pub_id, AVG(titles.price)  
FROM titles INNER JOIN publishers  
   ON titles.pub_id = publishers.pub_id  
WHERE publishers.state = 'CA'  
GROUP BY titles.pub_id  
HAVING AVG(price) > 10  
```  
  
Puede crear tanto cláusulas HAVING como WHERE en el panel Criterios. De forma predeterminada, si especifica una condición de búsqueda para una columna, la condición entra a formar parte de la cláusula HAVING. No obstante, puede cambiar la condición para que sea una cláusula WHERE.  
  
Puede crear una cláusula WHERE y una cláusula HAVING en las que intervenga la misma columna. Para ello, debe agregar la columna dos veces al panel Criterios y especificar, a continuación, una instancia como parte de la cláusula HAVING y la otra como parte de la cláusula WHERE.  
  
### <a name="to-specify-a-where-condition-in-an-aggregate-query"></a>Para especificar una condición WHERE en una consulta de funciones agregadas  
  
1.  Especifique los grupos de la consulta. Para detalles, consulte [Agrupar filas en los resultados de la consulta](../../ssms/visual-db-tools/group-rows-in-query-results-visual-database-tools.md).  
  
2.  Si aún no está en el panel Criterios, agregue la columna en la que desea que se base la condición WHERE.  
  
3.  Borre la columna **Resultados** , a menos que la columna de datos forme parte de la cláusula GROUP BY o esté incluida en una función de agregado.  
  
4.  En la columna **Filtro** , especifique la condición WHERE. El Diseñador de consultas y vistas agrega la condición a la cláusula HAVING de la instrucción SQL.  
  
    > [!NOTE]  
    > En la consulta mostrada en el ejemplo de este procedimiento se combinan dos tablas: `titles` y `publishers`.  
  
    En este punto de la consulta, la instrucción SQL contiene una cláusula HAVING:  
  
    ```  
    SELECT titles.pub_id, AVG(titles.price)  
    FROM titles INNER JOIN publishers   
       ON titles.pub_id = publishers.pub_id  
    GROUP BY titles.pub_id  
    HAVING publishers.state = 'CA'  
    ```  
  
5.  En la columna **Agrupar por** , seleccione **Where** de la lista de opciones de agrupación y resumen. El Diseñador de consultas y vistas quita la condición de la cláusula HAVING en la instrucción SQL y la agrega a la cláusula WHERE.  
  
    La instrucción SQL cambia para incluir en su lugar una cláusula WHERE:  
  
    ```  
    SELECT titles.pub_id, AVG(titles.price)  
    FROM titles INNER JOIN publishers   
       ON titles.pub_id = publishers.pub_id  
    WHERE publishers.state = 'CA'  
    GROUP BY titles.pub_id  
    ```  
  
## <a name="see-also"></a>Vea también  
[Ordenar y agrupar los resultados de la consulta](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Resumir los resultados de la consulta](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
  

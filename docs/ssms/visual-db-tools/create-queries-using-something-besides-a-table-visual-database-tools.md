---
description: Crear consultas a partir de otro objeto distinto de una tabla (Visual Database Tools)
title: Crear consultas a partir de otro objeto distinto de una tabla
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], queries
- queries [SQL Server], creating
ms.assetid: 8e4a1f0a-8a42-4733-be8d-e21d6dbddb33
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 95a432ca8912b4d306538906d11bb2a3d5464e5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350450"
---
# <a name="create-queries-using-something-besides-a-table-visual-database-tools"></a>Crear consultas a partir de otro objeto distinto de una tabla (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Al escribir una consulta de recuperación, puede enunciar qué columnas y qué filas desea obtener y dónde tiene que buscar el procesador de consultas los datos originales. Normalmente, estos datos originales están formados por una o varias tablas combinadas entre sí. Pero los datos originales pueden proceder de orígenes que no sean tablas. De hecho, pueden provenir de vistas, consultas, sinónimos o funciones definidas por el usuario que devuelven una tabla.  
  
## <a name="using-a-view-in-place-of-a-table"></a>Utilizar una vista en lugar de una tabla  
Puede seleccionar filas en una vista. Por ejemplo, supongamos que la base de datos incluye una vista denominada "ExpensiveBooks", en la que cada fila describe un título cuyo precio supera 19,99. La definición de vista puede presentar el siguiente aspecto:  
  
```  
SELECT *  
FROM titles  
WHERE price > 19.99  
```  
  
Si desea seleccionar los libros de psicología caros, solo tiene que seleccionar los libros de psicología de la vista ExpensiveBooks. El código SQL resultante puede presentar el siguiente aspecto:  
  
```  
SELECT *  
FROM ExpensiveBooks  
WHERE type = 'psychology'  
```  
  
De igual manera, una vista puede participar en una operación JOIN. Por ejemplo, puede buscar las ventas de libros caros combinando simplemente la tabla de ventas (sales) con la vista ExpensiveBooks. El código SQL resultante puede presentar el siguiente aspecto:  
  
```  
SELECT *  
FROM sales   
         INNER JOIN   
         ExpensiveBooks   
         ON sales.title_id   
         =  ExpensiveBooks.title_id  
```  
  
Para obtener más información sobre cómo agregar una vista a una consulta, consulte [Agregar tablas a las consultas (Visual Database Tools)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-query-in-place-of-a-table"></a>Utilizar una consulta en lugar de una tabla  
Puede seleccionar filas de una consulta. Por ejemplo, supongamos que ya ha escrito una consulta que recupera títulos e identificadores de los libros escritos por varios autores. El aspecto de SQL puede ser el siguiente:  
  
```  
SELECT   
     titles.title_id, title, type  
FROM   
     titleauthor   
         INNER JOIN  
         titles   
         ON titleauthor.title_id   
         =  titles.title_id   
GROUP BY   
     titles.title_id, title, type  
HAVING COUNT(*) > 1  
```  
  
Seguidamente, puede escribir otra consulta que se base en este resultado. Por ejemplo, puede escribir una consulta que recupere los libros de psicología escritos por varios autores. Para escribir esta consulta nueva, puede utilizar la consulta existente como origen de datos de la nueva consulta. El código SQL resultante puede presentar el siguiente aspecto:  
  
```  
SELECT   
    title  
FROM   
    (  
    SELECT   
        titles.title_id,   
        title,   
        type  
    FROM   
        titleauthor   
            INNER JOIN  
            titles   
            ON titleauthor.title_id   
            =  titles.title_id   
    GROUP BY   
        titles.title_id,   
        title,   
        type  
    HAVING COUNT(*) > 1  
    )   
    co_authored_books  
WHERE     type = 'psychology'  
```  
  
El texto resaltado muestra la consulta existente utilizada como origen de los datos de la nueva consulta. Tenga en cuenta que la consulta nueva utiliza un alias ("co_authored_books") para la consulta existente. Para obtener más información sobre los alias, consulte [Crear alias de tabla (Visual Database Tools)](../../ssms/visual-db-tools/create-table-aliases-visual-database-tools.md) y [Crear alias de columna (Visual Database Tools)](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
De igual manera, una consulta puede participar en una operación JOIN. Por ejemplo, puede buscar las ventas de libros caros escritos por varios autores simplemente mediante la combinación de la vista ExpensiveBooks con la consulta que recupera los libros escritos por varios autores. El código SQL resultante puede presentar el siguiente aspecto:  
  
```  
SELECT   
    ExpensiveBooks.title  
FROM   
    ExpensiveBooks   
        INNER JOIN  
        (  
        SELECT   
            titles.title_id,   
            title,   
            type  
        FROM   
            titleauthor   
                INNER JOIN  
                titles   
                ON titleauthor.title_id   
                =  titles.title_id   
        GROUP BY   
            titles.title_id,   
            title,   
            type  
        HAVING COUNT(*) > 1  
        )  
```  
  
Para obtener más información sobre cómo agregar una consulta a otra consulta, consulte [Agregar tablas a las consultas (Visual Database Tools)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-user-defined-function-in-place-of-a-table"></a>Usar una función definida por el usuario en lugar de una tabla  
En SQL Server 2000 o superior, puede crear una función definida por el usuario que devuelva una tabla. Estas funciones son útiles para la realización de lógica compleja o de procedimientos.  
  
Por ejemplo, suponga que la tabla employee (empleados) contiene una columna adicional, employee.manager_emp_id, y que existe una clave externa de manager_emp_id a employee.emp_id. Dentro de cada fila de la tabla employee, la columna manager_emp_id se refiere al jefe del empleado. Para ser más exactos, se referirá al emp_id del jefe del empleado. Puede crear una función definida por el usuario que devuelva una tabla que contenga una fila para cada empleado que trabaje en una determinada jerarquía organizativa de un director de alto nivel. Puede llamar a la función fn_GetWholeTeam y diseñarla para que acepte una variable de entrada: el valor emp_id del director cuyo equipo quiere recuperar.  
  
Puede escribir una consulta que utilice la función fn_GetWholeTeam como origen de datos. El código SQL resultante puede presentar el siguiente aspecto:  
  
```  
SELECT *   
FROM   
     fn_GetWholeTeam ('VPA30890F')  
```  
  
"VPA30890F" es el emp_id del director cuya organización desea recuperar. Para obtener más información sobre cómo agregar una función definida por el usuario a una consulta, consulte [Agregar tablas a las consultas (Visual Database Tools)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md). Para obtener una descripción completa de las funciones definidas por el usuario, consulte [Funciones definidas por el usuario](../../relational-databases/user-defined-functions/user-defined-functions.md).  

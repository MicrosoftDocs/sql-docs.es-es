---
title: MSSQLSERVER_4104 | Microsoft Docs
description: El identificador formado por varias partes especificado no se ha podido asignar a una entidad existente. Vea una explicación del error y las posibles resoluciones.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4104 (Database Engine error)
ms.assetid: 52dc32d8-97ad-4ef0-834d-2e68f215d007
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9f28d2e4cf04427887584cab6ee27de80024eae7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185116"
---
# <a name="mssqlserver_4104"></a>MSSQLSERVER_4104
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|4104|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|ALG_MULTI_ID_BAD|  
|Texto del mensaje|El identificador formado por varias partes "%.*ls" no se pudo enlazar.|  
  
## <a name="explanation"></a>Explicación  
El nombre de una entidad en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conoce como su *identificador*. Los identificadores se usan siempre que se hace referencia a entidades, por ejemplo, al especificar nombres de tabla y de columna en una consulta. Un identificador formado por varias partes contiene uno o varios calificadores como prefijo del identificador. Por ejemplo, un identificador de tabla puede tener como prefijo calificadores como el nombre de la base de datos y el nombre del esquema que la contienen, o un identificador de columna puede tener como prefijo calificadores como el nombre o el alias de una tabla.  
  
El error 4104 indica que el identificador formado por varias partes especificado no se pudo asignar a una entidad existente. Este error se puede devolver en las condiciones siguientes:  
  
-   El calificador proporcionado como prefijo para un nombre de columna no corresponde a ningún nombre de alias o tabla utilizado en la consulta.  
  
    Por ejemplo, la instrucción siguiente utiliza un alias de tabla (`Dept`) como prefijo de columna, pero no se hace referencia al alias de tabla en la cláusula FROM.  
  
    ```  
    SELECT Dept.Name FROM HumanResources.Department;  
    ```  
  
    En las instrucciones siguientes, se especifica un identificador de columna formado por varias partes `TableB.KeyCol` en la cláusula WHERE como parte de una condición JOIN entre dos tablas; sin embargo, no se hace referencia explícita a `TableB` en la consulta.  
  
    ```  
    DELETE FROM TableA WHERE TableA.KeyCol = TableB.KeyCol;  
    ```  
  
    ```  
    SELECT 'X' FROM TableA WHERE TableB.KeyCol = TableA.KeyCol;  
    ```  
  
-   En la cláusula FROM se proporciona un nombre de alias para la tabla, pero el calificador proporcionado para una columna es el nombre de la tabla. Por ejemplo, la instrucción siguiente utiliza el nombre de tabla `Department` como prefijo de columna; sin embargo, la tabla tiene un alias (`Dept`) al que se hace referencia en la cláusula FROM.  
  
    ```  
    SELECT Department.Name FROM HumanResources.Department AS Dept;  
    ```  
  
    Cuando se utiliza un alias, el nombre de la tabla no se puede utilizar en ninguna otra parte de la instrucción.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede determinar si el identificador formado por varias partes hace referencia a una columna con una tabla como prefijo o a una propiedad de un tipo de datos definido por el usuario (UDT) de CLR con una columna como prefijo. Esto sucede porque a las propiedades de las columnas UDT se hace referencia utilizando el separador punto (.) entre el nombre de la columna y el nombre de la propiedad del mismo modo que un nombre de columna tiene como prefijo un nombre de tabla. En el siguiente ejemplo se crean dos tablas: `a` y `b`. La tabla `b` contiene la columna `a`, que usa un UDT `dbo.myudt2` de CLR como tipo de datos. La instrucción SELECT contiene un identificador `a.c2` formado por varias partes.  
  
    ```  
    CREATE TABLE a (c2 int);   
    GO  
    ```  
  
    ```  
    CREATE TABLE b (a dbo.myudt2);   
    GO  
    ```  
  
    ```  
    SELECT a.c2 FROM a, b;   
    ```  
  
    Suponiendo que el UDT `myudt2` no tiene una propiedad denominada `c2`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede determinar si el identificador `a.c2` hace referencia a la columna `c2` de la tabla `a` o a la columna `a`, propiedad `c2` de la tabla `b`.  
  
## <a name="user-action"></a>Acción del usuario  
  
-   Haga coincidir los prefijos de columna con los nombres de tabla o con los nombres de alias especificados en la cláusula FROM de la consulta. Si se define un alias para un nombre de tabla en la cláusula FROM, solo puede utilizar el alias como calificador para las columnas asociadas a esa tabla.  
  
    Las instrucciones anteriores que hacen referencia a la tabla `HumanResources.Department` se pueden corregir de la forma siguiente:  
  
    ```  
    SELECT Dept.Name FROM HumanResources.Department AS Dept;  
    GO  
    ```  
  
    ```  
    SELECT Department.Name FROM HumanResources.Department;  
    GO  
    ```  
  
-   Asegúrese de que todas las tablas se especifican en la consulta y que las condiciones JOIN entre las tablas se especifican correctamente. La instrucción DELETE anterior se puede corregir de la forma siguiente:  
  
    ```  
    DELETE FROM dbo.TableA  
    WHERE TableA.KeyCol = (SELECT TableB.KeyCol   
                            FROM TableB   
                            WHERE TableA.KeyCol = TableB.KeyCol);  
    GO  
    ```  
  
    La instrucción SELECT anterior para `TableA` se puede corregir de la forma siguiente:  
  
    ```  
    SELECT 'X' FROM TableA, TableB WHERE TableB.KeyCol = TableA.KeyCol;  
    ```  
  
    or  
  
    ```  
    SELECT 'X' FROM TableA INNER JOIN TableB ON TableB.KeyCol = TableA.KeyCol;  
    ```  
  
-   Utilice nombres únicos y definidos claramente para los identificadores. De este modo, el código es más fácil de leer y mantener, y también se reduce al mínimo el riesgo de que haya referencias ambiguas a varias entidades.  
  
## <a name="see-also"></a>Consulte también  
[MSSQLSERVER_107](~/relational-databases/errors-events/mssqlserver-107-database-engine-error.md)  
[Identificadores de base de datos](~/relational-databases/databases/database-identifiers.md)  
  

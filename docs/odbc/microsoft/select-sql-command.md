---
description: Seleccione - comando SQL
title: Comando SELECT-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- select [ODBC]
ms.assetid: 2149c3ca-3a71-446d-8d53-3d056e2f301a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fc6ae552bc4e6e8bd681aa3d47ffadeaf81fd0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99153521"
---
# <a name="select---sql-command"></a>Seleccione - comando SQL
Recupera datos de una o más tablas.  
  
 El controlador ODBC de Visual FoxPro admite la sintaxis nativa del lenguaje Visual FoxPro para este comando. Para obtener información específica del controlador, consulte los **comentarios del controlador**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SELECT [ALL | DISTINCT]  
   [Alias.] Select_Item [AS Column_Name]  
   [, [Alias.] Select_Item [AS Column_Name] ...]   
FROM [DatabaseName!]Table [Local_Alias]  
   [, [DatabaseName!]Table [Local_Alias] ...]   
[WHERE JoinCondition [AND JoinCondition  
...]  
   [AND | OR FilterCondition [AND | OR FilterCondition ...]]]  
[GROUP BY GroupColumn [, GroupColumn ...]]  
[HAVING FilterCondition]  
[UNION [ALL] SELECTCommand]  
[ORDER BY Order_Item [ASC | DESC] [, Order_Item [ASC | DESC] ...]]  
```  
  
## <a name="arguments"></a>Argumentos  
  
> [!NOTE]  
>  Una *subconsulta*, a la que se hace referencia en los argumentos siguientes, es una selección dentro de una SELECT y debe incluirse entre paréntesis. Puede tener hasta dos subconsultas en el mismo nivel (no anidadas) en la cláusula WHERE. (Vea la sección de los argumentos). Las subconsultas pueden contener varias condiciones de combinación.  
  
 [TODOS LOS &#124; DISTINTOS]   [*Alias*.] *Select_Item* [como *Column_Name*] [, [*alias*.] *Select_Item* [como *Column_Name*]...]  
 La cláusula SELECT especifica los campos, las constantes y las expresiones que se muestran en los resultados de la consulta.  
  
 De forma predeterminada, todas las filas se muestran en los resultados de la consulta.  
  
 DISTINCt excluye los duplicados de las filas de los resultados de la consulta.  
  
> [!NOTE]  
>  Puede usar DISTINCt solo una vez por cada cláusula SELECT.  
  
 *Alias*. califica nombres de elementos coincidentes. Cada elemento que se especifica con *Select_Item* genera una columna de los resultados de la consulta. Si dos o más elementos tienen el mismo nombre, incluya el alias de la tabla y un punto delante del nombre del elemento para evitar que se dupliquen las columnas.  
  
 *Select_Item* especifica un elemento que se va a incluir en los resultados de la consulta. Un elemento puede ser uno de los siguientes:  
  
-   Nombre de un campo de una tabla en la cláusula FROM.  
  
-   Constante que especifica que el mismo valor constante va a aparecer en cada fila de los resultados de la consulta.  
  
-   Expresión que puede ser el nombre de una función definida por el usuario.  
  
 **Funciones definidas por el usuario con SELECT**  
  
 Aunque el uso de funciones definidas por el usuario en la cláusula SELECT tiene ventajas obvias, también debe tener en cuenta las siguientes restricciones:  
  
-   La velocidad de las operaciones realizadas con SELECT puede estar limitada por la velocidad a la que se ejecutan dichas funciones definidas por el usuario. Las manipulaciones de gran volumen que implican funciones definidas por el usuario se pueden lograr mejor mediante el uso de funciones de API y definidas por el usuario escritas en C o en lenguaje ensamblador.  
  
-   La única manera confiable de pasar valores a las funciones definidas por el usuario invocadas desde SELECT es mediante la lista de argumentos que se pasa a la función cuando se invoca.  
  
-   Incluso si experimenta y detecta una manipulación supuestamente prohibida que funciona correctamente en una determinada versión de FoxPro, no hay ninguna garantía de que seguirá funcionando en versiones posteriores.  
  
 Además de estas restricciones, las funciones definidas por el usuario son aceptables en la cláusula SELECT. Sin embargo, recuerde que el uso de SELECT puede ralentizar el rendimiento.  
  
 Las siguientes funciones de campo están disponibles para su uso con un elemento SELECT que sea un campo o una expresión que contenga un campo:  
  
-   AVG (*Select_Item*): calcula el promedio de una columna de datos numéricos.  
  
-   COUNT (*Select_Item*): cuenta el número de elementos seleccionados en una columna. COUNT (*) cuenta el número de filas del resultado de la consulta.  
  
-   MIN (*Select_Item*): determina el valor más pequeño de *Select_Item* en una columna.  
  
-   MAX (*Select_Item*): determina el valor mayor de *Select_Item* en una columna.  
  
-   SUM (*Select_Item*): suma una columna de datos numéricos.  
  
 No se pueden anidar funciones de campo.  
  
 COMO *Column_Name*  
 Especifica el encabezado de una columna en el resultado de la consulta. Esto resulta útil cuando *Select_Item* es una expresión o contiene una función de campo y se desea asignar un nombre descriptivo a la columna. *Column_Name* puede ser una expresión, pero no puede contener caracteres (por ejemplo, espacios) que no se permiten en los nombres de campos de tabla.  
  
 DESDE [*nombreDeBaseDeDatos*!] *Tabla* [*Local_Alias*] [, [*nombreDeBaseDeDatos*!] *Tabla* [*Local_Alias*]...]  
 Muestra las tablas que contienen los datos que recupera la consulta. Si no hay ninguna tabla abierta, Visual FoxPro muestra el cuadro de diálogo **abrir** para que pueda especificar la ubicación del archivo. Una vez abierta, la tabla permanece abierta después de completarse la consulta.  
  
 *DatabaseName*! especifica el nombre de una base de datos distinta de la especificada con el origen de datos. Debe incluir el nombre de la base de datos que contiene la tabla si la base de datos no se especifica con el origen de datos. Incluya el delimitador de signo de exclamación (!) después del nombre de la base de datos y antes del nombre de la tabla.  
  
 *Local_Alias* especifica un nombre temporal para la tabla denominada en la *tabla*. Si especifica un alias local, debe utilizar el alias local en lugar del nombre de tabla en la instrucción SELECT. El alias local no afecta al entorno de Visual FoxPro.  
  
 WHERE *JoinCondition* [and *JoinCondition* ...]    [AND &#124; o *FilterCondition* [y &#124; o *FilterCondition* ...]]  
 Indica a Visual FoxPro que solo incluya determinados registros en los resultados de la consulta. DONDE es necesario para recuperar los datos de varias tablas.  
  
 *JoinCondition* especifica campos que vinculan las tablas en la cláusula FROM. Si incluye más de una tabla en una consulta, debe especificar una condición de combinación para cada tabla después de la primera.  
  
> [!IMPORTANT]  
>  Cuando cree condiciones de combinación, tenga en cuenta la siguiente información:  
  
-   Si incluye dos tablas en una consulta y no especifica una condición de combinación, cada registro de la primera tabla se combina con cada registro de la segunda tabla, siempre y cuando se cumplan las condiciones del filtro. Este tipo de consulta puede generar resultados largos.  
  
-   Tenga cuidado al combinar tablas con campos vacíos porque Visual FoxPro coincide con campos vacíos. Por ejemplo, si se une en CUSTOMER.ZIP y INVOICE.ZIP y si el cliente contiene 100 códigos postales vacíos y la factura contiene 400 códigos postales vacíos, el resultado de la consulta contiene 40.000 registros adicionales resultantes de los campos vacíos. Utilice la función **Empty ()** para eliminar los registros vacíos del resultado de la consulta.  
  
-   Debe utilizar el operador AND para conectar varias condiciones de combinación. Cada condición de combinación tiene el formato siguiente:  
  
     *FieldName1 Comparison FieldName2*  
  
     *FieldName1* es el nombre de un campo de una tabla, *FieldName2* es el nombre de un campo de otra tabla y la *comparación* es uno de los operadores descritos en la tabla siguiente.  
  
|Operador|De comparación|  
|--------------|----------------|  
|=|Igual|  
|==|Exactamente igual|  
|LIKE|SQL LIKE|  
|<>,! =, #|No igual a|  
|>|Más de|  
|>=|Mayor o igual que|  
|<|Menor que|  
|<=|Menor o igual que|  
  
 Cuando se usa el operador = con cadenas, actúa de forma diferente, dependiendo de la configuración de SET ANSI. Cuando SET ANSI está establecido en OFF, Visual FoxPro trata las comparaciones de cadenas de una manera familiar para los usuarios de xBase. Cuando SET ANSI está establecido en ON, Visual FoxPro sigue los estándares ANSI para las comparaciones de cadenas. Vea [set ANSI](../../odbc/microsoft/set-ansi-command.md) y [set Exact](../../odbc/microsoft/set-exact-command.md) para obtener más información sobre cómo Visual FoxPro realiza comparaciones de cadenas.  
  
 *FilterCondition* especifica los criterios que los registros deben cumplir para que se incluyan en los resultados de la consulta. Puede incluir tantas condiciones de filtro en una consulta como desee, conectarlas con el operador AND u OR. También puede usar el operador NOT para invertir el valor de una expresión lógica, o puede usar **Empty ()** para comprobar si hay un campo vacío. *FilterCondition* puede tomar cualquiera de los formatos de los ejemplos siguientes:  
  
 **Ejemplo 1** *FieldName1 Comparison FieldName2*  
  
 `customer.cust_id = orders.cust_id`  
  
 **Ejemplo 2** *expresión de comparación de FieldName*  
  
 `payments.amount >= 1000`  
  
 **Ejemplo 3** de *comparación de FieldName* (*subconsulta*)  
  
 `company < ALL ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Cuando la condición de filtro incluye ALL, el campo debe cumplir la condición de comparación de todos los valores generados por la subconsulta antes de que su registro esté incluido en los resultados de la consulta.  
  
 **Ejemplo 4** de la *comparación de FIELDNAME* cualquier &#124; alguna (*subconsulta*)  
  
 `company < ANY ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Cuando la condición de filtro incluye alguna o alguna, el campo debe cumplir la condición de comparación para al menos uno de los valores generados por la subconsulta.  
  
 En el ejemplo siguiente se comprueba si los valores del campo están dentro de un intervalo especificado de valores:  
  
 **Ejemplo 5** *FieldName* [Not] between *Start_Range* y *End_Range*  
  
 `customer.postalcode BETWEEN 90000 AND 99999`  
  
 En el ejemplo siguiente se comprueba si al menos una fila cumple los criterios de la subconsulta. Cuando la condición de filtro incluye EXISTs, la condición de filtro se evalúa como true (. T.) a menos que la subconsulta se evalúe como el conjunto vacío.  
  
 **Ejemplo 6** [Not] exists (*subconsulta*)  
  
 `EXISTS ;`  
  
 `(SELECT * FROM orders WHERE customer.postalcode =`  
  
 `orders.postalcode)`  
  
 **Ejemplo 7** *FieldName* [no] en *Value_Set*  
  
 `customer.postalcode NOT IN ("98052","98072","98034")`  
  
 Cuando la condición de filtro incluye en, el campo debe contener uno de los valores antes de que su registro esté incluido en los resultados de la consulta.  
  
 **Ejemplo 8** *FieldName* [no] in (*subconsulta*)  
  
 `customer.cust_id IN ;`  
  
 `(SELECT orders.cust_id FROM orders WHERE orders.city="Seattle")`  
  
 Aquí el campo debe contener uno de los valores devueltos por la subconsulta antes de que su registro esté incluido en los resultados de la consulta.  
  
 **Ejemplo 9** *FieldName* [Not] like *cExpression*  
  
 `customer.country NOT LIKE "USA"`  
  
 Esta condición de filtro busca cada campo que coincida con *cExpression*. Puede usar el signo de porcentaje (%) y caracteres comodín de subrayado (_) como parte de *cExpression*. El carácter de subrayado representa un único carácter desconocido en la cadena.  
  
 AGRUPAr por *GroupColumn* [, *GroupColumn* ...]  
 Agrupa las filas de la consulta en función de los valores de una o más columnas. *GroupColumn* puede ser uno de los siguientes:  
  
-   Nombre de un campo de tabla normal.  
  
-   Campo que incluye una función de campo SQL.  
  
-   Expresión numérica que indica la ubicación de la columna en la tabla de resultados. (El número de la columna situada más a la izquierda es 1).  
  
 TENER *FilterCondition*  
 Especifica una condición de filtro que los grupos deben cumplir para que se incluyan en los resultados de la consulta. HAVING debe usarse con GROUP BY y puede incluir tantas condiciones de filtro como desee, conectadas mediante el operador AND u OR. También puede usar NOT para invertir el valor de una expresión lógica.  
  
 *FilterCondition* no puede contener una subconsulta.  
  
 Una cláusula HAVING sin una cláusula GROUP BY se comporta como una cláusula WHERE. Puede usar los alias locales y las funciones de campo en la cláusula HAVING. Use una cláusula WHERE para obtener un rendimiento más rápido si la cláusula HAVING no contiene funciones de campo.  
  
 [UNION [ALL] *SELECTCommand*]  
 Combina los resultados finales de una selección con los resultados finales de otra SELECT. De forma predeterminada, UNION comprueba los resultados combinados y elimina las filas duplicadas. Use paréntesis para combinar varias cláusulas UNION.  
  
 ALL impide que UNION elimine filas duplicadas de los resultados combinados.  
  
 Las cláusulas UNION siguen estas reglas:  
  
-   No se puede utilizar UNION para combinar subconsultas.  
  
-   Ambos comandos SELECT deben tener el mismo número de columnas en el resultado de la consulta.  
  
-   Cada columna de los resultados de la consulta de una selección debe tener el mismo tipo de datos y el mismo ancho que la columna correspondiente de la otra SELECT.  
  
-   Solo la selección final puede tener una cláusula ORDER BY, que debe hacer referencia a las columnas de salida por número. Si se incluye una cláusula ORDER BY, afectará al resultado completo.  
  
 También puede utilizar la cláusula UNION para simular una combinación externa.  
  
 Al combinar dos tablas en una consulta, solo se incluyen en la salida los registros con valores coincidentes en los campos de combinación. Si un registro de la tabla primaria no tiene un registro correspondiente en la tabla secundaria, el registro de la tabla primaria no se incluye en la salida. Una combinación externa permite incluir todos los registros de la tabla primaria en la salida, junto con los registros coincidentes en la tabla secundaria. Para crear una combinación externa en Visual FoxPro, debe usar un comando SELECT anidado, como en el ejemplo siguiente:  
  
```  
SELECT customer.company, orders.order_id, orders.emp_id ;  
FROM customer, orders ;  
WHERE customer.cust_id = orders.cust_id ;  
UNION ;  
SELECT customer.company, 0, 0 ;  
FROM customer ;  
WHERE customer.cust_id NOT IN ;  
(SELECT orders.cust_id FROM orders)  
```  
  
> [!NOTE]  
>  Asegúrese de incluir el espacio que precede inmediatamente a cada punto y coma. De lo contrario, recibirá un error.  
  
 La sección del comando antes de la cláusula UNION selecciona los registros de las dos tablas que tienen valores coincidentes. No se incluyen las empresas del cliente que no tienen facturas asociadas. La sección del comando después de la cláusula UNION selecciona los registros de la tabla Customer que no tienen registros coincidentes en la tabla Orders.  
  
 En cuanto a la segunda sección del comando, tenga en cuenta lo siguiente:  
  
-   La instrucción SELECT dentro de los paréntesis se procesa en primer lugar. Esta instrucción crea una selección de todos los números de cliente en la tabla Orders.  
  
-   La cláusula WHERE encuentra todos los números de cliente de la tabla Customer que no están en la tabla Orders. Dado que la primera sección del comando proporcionó todas las compañías que tenían un número de cliente en la tabla Orders, todas las compañías de la tabla Customer se incluyen ahora en los resultados de la consulta.  
  
-   Dado que las estructuras de las tablas incluidas en una Unión deben ser idénticas, hay dos marcadores de posición en la segunda instrucción SELECT que representan *Orders.order_id* y *Orders.emp_id* de la primera instrucción SELECT.  
  
    > [!NOTE]  
    >  Los marcadores de posición deben ser del mismo tipo que los campos que representan. Si el campo es un tipo de fecha, el marcador de posición debe ser {//}. Si el campo es un campo de caracteres, el marcador de posición debe ser una cadena vacía ("").  
  
 ORDER BY *Order_Item* [ASC &#124; Desc] [, *Order_Item* [ASC &#124; Desc]...]  
 Ordena los resultados de la consulta en función de los datos de una o varias columnas. Cada *Order_Item* debe corresponder a una columna de los resultados de la consulta y puede ser una de las siguientes:  
  
-   Campo de una tabla FROM que también es un elemento Select de la cláusula SELECT principal (no en una subconsulta).  
  
-   Expresión numérica que indica la ubicación de la columna en la tabla de resultados. (La columna situada más a la izquierda es el número 1).  
  
 ASC especifica un orden ascendente para los resultados de la consulta, de acuerdo con el elemento o los elementos del pedido, y es el valor predeterminado para ORDER BY.  
  
 DESC especifica un orden descendente para los resultados de la consulta.  
  
 Los resultados de la consulta aparecen sin ordenar si no se especifica un pedido con ORDER BY.  
  
## <a name="remarks"></a>Observaciones  
 SELECT es un comando SQL integrado en Visual FoxPro como cualquier otro comando de Visual FoxPro. Cuando se usa SELECT para plantear una consulta, Visual FoxPro interpreta la consulta y recupera los datos especificados de las tablas. Puede crear una consulta SELECT desde la ventana del símbolo del sistema o desde un programa de Visual FoxPro (como con cualquier otro comando de Visual FoxPro).  
  
> [!NOTE]  
>  SELECT no respeta la condición de filtro actual especificada con SET FILTER.  
  
## <a name="driver-remarks"></a>Notas del controlador  
 Cuando la aplicación envía la instrucción SQL de ODBC SELECT al origen de datos, el controlador ODBC de Visual FoxPro convierte el comando en el comando SELECT de Visual FoxPro sin traducción a menos que el comando contenga una secuencia de escape ODBC. Los elementos incluidos en una secuencia de escape de ODBC se convierten a la sintaxis de Visual FoxPro. Para obtener más información sobre el uso de secuencias de escape de ODBC, vea [funciones de fecha y hora](../../odbc/microsoft/time-and-date-functions-visual-foxpro-odbc-driver.md) y en la *Referencia del programador de Microsoft ODBC*, vea [secuencias de escape en ODBC](../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INSERT-SQL](../../odbc/microsoft/insert-sql-command.md)   
 [ESTABLECER ANSI](../../odbc/microsoft/set-ansi-command.md)   
 [ESTABLECER EXACTO](../../odbc/microsoft/set-exact-command.md)

---
description: 'SELECT: cláusula FOR (Transact-SQL)'
title: Cláusula FOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FOR
- FOR CLAUSE
- FOR_TSQL
- FOR_CLAUSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- XML option [SQL Server]
- BROWSE option
- FOR clause [Transact-SQL]
ms.assetid: 08a6f084-8f73-4f2a-bae4-3c7513dc99b9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 99b47c3d47e7403e04cfff4b62ff5c1d78bb6071
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207546"
---
# <a name="select---for-clause-transact-sql"></a>SELECT: cláusula FOR (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Use la cláusula FOR para especificar una de las siguientes opciones para los resultados de la consulta.
  
-   Permita las actualizaciones mientras se visualizan los resultados de la consulta en un cursor del modo de exploración mediante la especificación de **FOR BROWSE**.  
  
-   Aplique formato XML a los resultados de la consulta mediante la especificación de **FOR XML**.  
  
-   Aplique formato JSON a los resultados de la consulta mediante la especificación de **FOR JSON**.  

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
[ FOR { BROWSE | <XML> | <JSON>} ]  
  
<XML> ::=  
XML   
{   
    { RAW [ ( 'ElementName' ) ] | AUTO }   
    [   
        <CommonDirectivesForXML>   
        [ , { XMLDATA | XMLSCHEMA [ ( 'TargetNameSpaceURI' ) ] } ]   
        [ , ELEMENTS [ XSINIL | ABSENT ]   
    ]  
  | EXPLICIT   
    [   
        <CommonDirectivesForXML>   
        [ , XMLDATA ]   
    ]  
  | PATH [ ( 'ElementName' ) ]   
    [  
        <CommonDirectivesForXML>   
        [ , ELEMENTS [ XSINIL | ABSENT ] ]  
    ]  
}   
  
<CommonDirectivesForXML> ::=   
[ , BINARY BASE64 ]  
[ , TYPE ]  
[ , ROOT [ ( 'RootName' ) ] ]  
  
<JSON> ::=  
JSON   
{   
    { AUTO | PATH }   
    [   
        [ , ROOT [ ( 'RootName' ) ] ]  
        [ , INCLUDE_NULL_VALUES ]  
        [ , WITHOUT_ARRAY_WRAPPER ]  
    ]  
  
}
```
  
## <a name="for-browse"></a>FOR BROWSE

 BROWSE  
 Especifica que se permiten las actualizaciones mientras se visualizan los datos en el cursor del modo de exploración de DB-Library. Una tabla se puede explorar en una aplicación si la tabla incluye una columna **timestamp**, la tabla tiene un índice único y la opción FOR BROWSE está al final de las instrucciones SELECT enviadas a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]
> No se puede usar HOLDLOCK \<lock_hint> en una instrucción SELECT que incluya la opción FOR BROWSE.
  
 FOR BROWSE no puede aparecer en instrucciones SELECT combinadas mediante el operador UNION.  
  
> [!NOTE]
> Cuando las columnas de clave de índice único de una tabla pueden aceptar valores NULL, y la tabla está en la parte interna de la combinación externa, el índice no se admite en el modo de exploración.  
  
 El modo de exploración permite examinar las filas de la tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y actualizar los datos de la tabla fila por fila. Para tener acceso a una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una aplicación en el modo de exploración, debe usar una de las dos opciones siguientes:  
  
-   La instrucción SELECT que usa para tener acceso a los datos de la tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe finalizar con las palabras clave **FOR BROWSE**. Al activar la opción **FOR BROWSE** para usar el modo de exploración, se crean tablas temporales.  
  
-   Debe ejecutar la instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] siguiente para activar el modo de exploración mediante la opción **NO_BROWSETABLE**:  
  
    ```sql
    SET NO_BROWSETABLE ON  
    ```  
  
     Al activar la opción **NO_BROWSETABLE**, todas las instrucciones SELECT se comportan como si la opción **FOR BROWSE** se anexara a las instrucciones. Aun así, la opción **NO_BROWSETABLE** no crea las tablas temporales que la opción **FOR BROWSE** suele usar para enviar los resultados a la aplicación.  
  
 Cuando se intenta tener acceso a los datos de las tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de exploración mediante una consulta SELECT que implica una instrucción de combinación externa, y cuando se define un índice único en la tabla que está presente en el lado interno de una instrucción de combinación externa, el modo de exploración no admite el índice único. El modo de exploración únicamente admite el índice único cuando todas las columnas de clave de índice único pueden aceptar valores NULL. El modo de exploración no admite el índice único si se cumplen las condiciones siguientes:  
  
-   Intenta tener acceso a los datos de las tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de exploración mediante una consulta SELECT que implica una instrucción de combinación externa.  
  
-   Un índice único se define en la tabla que está presente en el lado interno de una instrucción de combinación externa.  
  
 Para reproducir este comportamiento en el modo de exploración, siga estos pasos:  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cree una base de datos denominada SampleDB.  
  
2.  En la base de datos SampleDB, cree una tabla tleft y una tabla tright que contengan ambas una única columna que se denomine c1. Defina un índice único en la columna c1 de la tabla tleft y establezca la columna para aceptar valores NULL. Para ello, ejecute las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes en una ventana de consulta adecuada:  
  
    ```sql
    CREATE TABLE tleft(c1 INT NULL UNIQUE) ;  
    GO   
    CREATE TABLE tright(c1 INT NULL) ;  
    GO  
    ```  
  
3.  Inserte varios valores en las tablas tleft y tright. Asegúrese de insertar un valor NULL en la tabla tleft. Para ello, ejecute las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes en la ventana de consulta:  
  
    ```sql
    INSERT INTO tleft VALUES(2) ;  
    INSERT INTO tleft VALUES(NULL) ;  
    INSERT INTO tright VALUES(1) ;  
    INSERT INTO tright VALUES(3) ;  
    INSERT INTO tright VALUES(NULL) ;  
    GO  
    ```  
  
4.  Active la opción **NO_BROWSETABLE**. Para ello, ejecute las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes en la ventana de consulta:  
  
    ```sql
    SET NO_BROWSETABLE ON ;  
    GO  
    ```  
  
5.  Obtenga acceso a los datos de las tablas tleft y tright utilizando una instrucción de combinación externa en la consulta SELECT. Asegúrese de que la tabla tleft está en el lado interno de la instrucción de combinación externa. Para ello, ejecute las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes en la ventana de consulta:  
  
    ```sql
    SELECT tleft.c1   
    FROM tleft   
    RIGHT JOIN tright   
    ON tleft.c1 = tright.c1   
    WHERE tright.c1 <> 2 ;
    ```  
  
     Observe la salida siguiente en el panel Resultados:  
  
     c1  
  
     ---\-  
  
     NULL  
  
     NULL  
  
 Después de ejecutar la consulta SELECT para obtener acceso a las tablas en el modo de exploración, el conjunto de resultados de la consulta SELECT contiene dos valores NULL para la columna c1 de la tabla tleft debido a la definición de la instrucción de combinación externa derecha. Por consiguiente, en el conjunto de resultados no puede distinguir entre los valores NULL que procedían de la tabla y los incluidos por la instrucción de combinación externa derecha. Puede recibir resultados incorrectos si debe omitir los valores NULL del conjunto de resultados.  
  
> [!NOTE]
> Si las columnas que están incluidas en el índice único no aceptan valores NULL, todos los valores NULL en el conjunto de resultados fueron incluidos por la instrucción de combinación externa.  
  
## <a name="for-xml"></a>FOR XML

 XML  
 Especifica que el resultado de una consulta se devolverá como documento XML. Se debe especificar uno de los siguientes modos XML: RAW, AUTO, EXPLICIT. Para más información sobre los datos XML y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md).  
  
 RAW [ **("** _NombreDeElemento_ **")** ]  
 Obtiene el resultado de la consulta y transforma cada fila del conjunto de resultados en un elemento XML con un identificador genérico \<row /> como etiqueta del elemento. Opcionalmente, puede especificar un nombre para el elemento de fila. La salida XML resultante usa el parámetro *ElementName* especificado como el elemento de fila generado para cada fila. Para obtener más información, vea [Usar el modo RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).
  
 AUTO  
 Devuelve los resultados de la consulta en un árbol anidado XML sencillo. Cada tabla de la cláusula FROM, para la que al menos se presenta una columna en la cláusula SELECT, se representa como elemento XML. A las columnas presentadas en la cláusula SELECT se les asignan los atributos de elemento apropiados. Para obtener más información, vea [Usar el modo AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md).  
  
 EXPLICIT  
 Especifica que la forma del árbol XML resultante está definida explícitamente. Con este modo, es necesario escribir las consultas de una cierta manera, de modo que se pueda especificar explícitamente información adicional sobre la anidación deseada. Para obtener más información, vea [Usar el modo EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md).  
  
 XMLDATA  
 Devuelve el esquema XDR insertado, pero no agrega el elemento raíz al resultado. Si se especifica XMLDATA, el esquema XDR se agrega al documento.  
  
> [!IMPORTANT]
> La directiva XMLDATA está **en desuso**. Utilice la XSD generación en los modos RAW y AUTO. No hay sustitución para la directiva XMLDATA en modo EXPLICIT. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  

_Suprimir los saltos de línea no deseados:_ es posible que quiera usar SQL Server Management Studio (SSMS) para emitir una consulta en la que se use la cláusula FOR XML. A veces, se devuelve una gran cantidad de código XML y se muestra en una celda de la cuadrícula. La cadena XML podría ser más larga de lo que una celda de cuadrícula de SSMS puede contener en una sola línea. En estos casos, es posible que SSMS inserte caracteres de salto de línea entre los segmentos largos de toda la cadena XML. Esos saltos de línea podrían aparecer en el medio de una subcadena que no se debería dividir entre líneas. Puede evitar los saltos de línea mediante una conversión AS XMLDATA. Esta solución también se puede aplicar cuando se usa FOR JSON PATH. La técnica se describe en Stack Overflow y se muestra en la siguiente instrucción SELECT de Transact-SQL de ejemplo:

- [Using SQL Server FOR XML: Convert Result Datatype to Text/varchar/string whatever?](https://stackoverflow.com/questions/5655332/using-sql-server-for-xml-convert-result-datatype-to-text-varchar-string-whate/5658758#5658758) (Uso de SQL Server FOR XML: conversión del tipo de datos del resultado a text/varchar/string u otro valor)

    ```sql
    SELECT CAST(
        (SELECT column1, column2
            FROM my_table
            FOR XML PATH('')
        )
            AS VARCHAR(MAX)
    ) AS XMLDATA ;
    ```

<!-- The preceding Stack Overflow example is per MicrosoftDocs/sql-docs Issue 1501.  2019-01-06 -->

 XMLSCHEMA [ **("** _URI_del_espacio_de_nombres_de_destino_ **")** ]  
 Devuelve el esquema XSD insertado. Opcionalmente puede especificar un URI de espacio de nombres de destino al especificar esta directiva, que devuelve el espacio de nombres especificado en el esquema. Para obtener más información, vea [Generar un esquema XSD insertado](../../relational-databases/xml/generate-an-inline-xsd-schema.md).  
  
 ELEMENTS  
 Especifica que las columnas se devuelven como subelementos. Sin embargo, se les asignan atributos XML. Esta opción solo se admite en los modos RAW, AUTO y PATH. Para obtener más información, vea [Usar el modo RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
 XSINIL  
 Especifica que se va a crear un elemento con el atributo **xsi:nil** establecido en **True** para los valores de columna NULL. Esta opción solo se puede especificar con la directiva ELEMENTS. Para más información, consulte:

- [Generar elementos para valores NULL mediante el parámetro XSINIL](../../relational-databases/xml/generate-elements-for-null-values-with-the-xsinil-parameter.md).
- [FOR XML en la instrucción SELECT](../../relational-databases/xml/for-xml-sql-server.md)
  
 ABSENT  
 Indica que para los valores de columna NULL, no se agregarán los elementos XML correspondientes en el resultado XML. Especifique esta opción solo con ELEMENTS.  
  
 PATH [ **("** _NombreDeElemento_ **")** ]  
 Genera un contenedor del elemento de \<row> para cada fila en el conjunto de resultados. Opcionalmente, especifique un nombre de elemento para el contenedor del elemento \<row>. Si se proporciona una cadena vacía, como FOR XML PATH ( **''** ), no se genera un elemento contenedor. El uso de PATH puede proporcionar una alternativa más sencilla a consultas escritas con la directiva EXPLICIT. Para obtener más información, vea [Usar el modo PATH con FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md).  
  
 BINARY BASE64  
 Especifica que la consulta devuelve los datos binarios en el formato codificado BINARY BASE64. Al recuperar datos binarios mediante el modo RAW y EXPLICIT, se debe especificar esta opción. Éste es el valor predeterminado en el modo AUTO.  
  
 TYPE  
 Especifica que la consulta devuelve un resultado de tipo **xml**. Para más información, consulte [TYPE Directive in FOR XML Queries](../../relational-databases/xml/type-directive-in-for-xml-queries.md).  
  
 ROOT [ **("** _NombreDeLaRaíz_ **")** ]  
 Especifica que se va a agregar un solo elemento de nivel superior al XML resultante. También se puede especificar el nombre del elemento raíz que se generará. Si no se especifica el nombre de raíz opcional, se agrega el elemento \<root> predeterminado.  
  
 Para más información, vea [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md).  
  
 **Ejemplo de FOR XML**  
  
 En el siguiente ejemplo se especifica `FOR XML AUTO` con las opciones `TYPE` y `XMLSCHEMA`. Debido a la opción `TYPE`, el conjunto de resultados que se devuelve al cliente es de tipo **xml**. La opción `XMLSCHEMA` especifica que el esquema XSD insertado está incluido en los datos XML devueltos y la opción `ELEMENTS` especifica que el resultado XML está centrado en elementos.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.BusinessEntityID, FirstName, LastName, PhoneNumber AS Phone  
FROM Person.Person AS p  
JOIN Person.PersonPhone AS pph ON p.BusinessEntityID  = pph.BusinessEntityID  
WHERE LastName LIKE 'G%'  
ORDER BY LastName, FirstName   
FOR XML AUTO, TYPE, XMLSCHEMA, ELEMENTS XSINIL;  
```  
  
## <a name="for-json"></a>FOR JSON

 JSON  
 Especifique FOR JSON para devolver los resultados de una consulta con formato de texto JSON. También tiene que especificar uno de los siguientes modos JSON: AUTO o PATH. Para más información sobre la cláusula **FOR JSON**, vea [Dar formato JSON a los resultados de consulta con FOR JSON &#40;SQL Server&#41;](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md).  
  
 AUTO  
 Aplique formato a la salida JSON automáticamente según la estructura de la instrucción **SELECT**.  
            Para ello, especifique **FOR JSON AUTO**. Para más información y ejemplos, vea [Aplicar formato a la salida JSON automáticamente con el modo AUTO &#40;SQL Server&#41;](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md).  
  
 PATH  
 Para mantener el control total sobre el formato de la salida JSON, especifique   
            **FOR JSON PATH**. El modo **PATH** le permite crear objetos contenedores y anidar propiedades complejas. Para más información y ejemplos, vea [Format Nested JSON Output with PATH Mode &#40;SQL Server&#41;](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md) (Aplicar formato a la salida JSON automáticamente con el modo PATH &#40;SQL Server&#41;).  
  
 INCLUDE_NULL_VALUES  
 Para incluir valores NULL en la salida JSON, especifique la opción **INCLUDE_NULL_VALUES** con la cláusula **FOR JSON**. Si no especifica esta opción, la salida no incluye las propiedades JSON de los valores NULL en los resultados de la consulta. Para más información y ejemplos, vea [Inclusión de valores Null en la salida de JSON con la opción INCLUDE_NULL_VALUES &#40;SQL Server&#41;](../../relational-databases/json/include-null-values-in-json-include-null-values-option.md).  
  
 ROOT [ **("** _NombreDeLaRaíz_ **")** ]  
 Para agregar un solo elemento de nivel superior a la salida JSON, especifique la opción **ROOT** con la cláusula **FOR JSON**. Si no especifica la opción **ROOT** , la salida JSON no tiene un elemento raíz. Para más información y ejemplos, vea [Agregar un nodo raíz a la salida JSON con la opción ROOT &#40;SQL Server&#41;](../../relational-databases/json/add-a-root-node-to-json-output-with-the-root-option-sql-server.md).  
  
 WITHOUT_ARRAY_WRAPPER  
 Para quitar los corchetes que rodean la salida JSON de manera predeterminada, especifique la opción **WITHOUT_ARRAY_WRAPPER** con la cláusula **FOR JSON**. Si no especifica esta opción, la salida JSON aparecerá entre corchetes. Use la opción **WITHOUT_ARRAY_WRAPPER** para generar un objeto JSON único como salida.  Para obtener más información, vea [Quitar corchetes de la salida JSON con la opción WITHOUT_ARRAY_WRAPPER &#40;SQL Server&#41;](../../relational-databases/json/remove-square-brackets-from-json-without-array-wrapper-option.md).  
  
 Para obtener más información, vea [Format Query Results as JSON with FOR JSON &#40;SQL Server&#41; (Aplicar el formato JSON a los resultados de la consulta con FOR JSON &#40;SQL Server&#41;)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md).  
  
## <a name="see-also"></a>Consulte también

 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)


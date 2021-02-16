---
description: Conversión no determinista de las cadenas de fecha literales en valores DATE
title: Conversión no determinista de literales de fecha | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 45540a538982f75033f35ad759782317983dc958
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071819"
---
# <a name="nondeterministic-conversion-of-literal-date-strings-into-date-values"></a>Conversión no determinista de las cadenas de fecha literales en valores DATE

Tenga cuidado al permitir la conversión de las cadenas de caracteres en tipos de datos DATE. El motivo es que estas conversiones a menudo son _no deterministas_.

Para controlar estas conversiones no deterministas debe tener en cuenta la configuración de [SET LANGUAGE](../statements/set-language-transact-sql.md) (ESTABLECER IDIOMA) y [SET DATEFORMAT](../statements/set-dateformat-transact-sql.md) (ESTABLECER FORMATO DE FECHA).



## <a name="set-language-example-month-name-in-polish"></a>Ejemplo de SET LANGUAGE: nombre del mes en polaco

- `SET LANGUAGE Polish;`

Una cadena de caracteres puede ser el nombre de un mes. ¿Pero el nombre está en inglés, polaco, croata o en otro idioma? Y, ¿se establecerá la sesión del usuario en el idioma correcto correspondiente?

Por ejemplo, considere la palabra _listopad_, que es el nombre de un mes. Qué mes es depende del idioma que el sistema SQL considera que se usa:
- Si es polaco, _listopad_ se traduce como el undécimo mes (_noviembre_ en español).
- Si es polaco, _listopad_ se traduce como el décimo mes (_octubre_ en español).

#### <a name="code-example-of-set-language"></a>Ejemplo de código de SET LANGUAGE

```sql
--SELECT alias FROM sys.syslanguages ORDER BY alias;

DECLARE @yourInputDate  NVARCHAR(32) = '28 listopad 2018';

SET LANGUAGE Polish;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Polish];

SET LANGUAGE Croatian;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Croatian];

SET LANGUAGE English;


/***  Actual output:  For the two months, note the 11 versus the 10.
SL_Polish
2018-11-28

SL_Croatian
2018-10-28
***/
```



## <a name="set-dateformat-example"></a>Ejemplo de SET DATEFORMAT

- `SET DATEFORMAT dmy;`

El formato **dmy** anterior dice que el significado de la cadena de fecha de ejemplo “01-03-2018” se interpretaría como _el primer día de marzo del año 2018_.

Si en su lugar se ha especificado **mdy**, la misma cadena “01-03-2018” significaría _el tercer día de enero de 2018_.

Y si se ha especificado **ymd**, no hay ninguna garantía de cuáles serían los resultados. El valor numérico de “2018” es demasiado grande para ser un día.
<!--
The preceding claim of "no guarantee" might be incorrect, in the minds of the SQL query engine Developer team?
-->

#### <a name="specific-countries"></a>Países específicos

En Japón y China, se utiliza el DATEFORMAT **ymd**. Los elementos del formato están en una secuencia que va de la unidad mayor a la menor. Por lo tanto, este formato es útil cuando se quiere ordenar. Este formato se considera el _formato internacional_. Es internacional porque los cuatro dígitos del año son inequívocos y ningún país en la tierra usa el formato arcaico **ydm**.

En otros países, como Alemania y Francia, el DATEFORMAT es **dmy**, lo que significa **“dd-mm-aaaa”**. El formato **dmy** no es adecuado para ordenar, pero es una secuencia razonable de la unidad más pequeña a la mayor.

Los Estados Unidos y los Estados Federados de Micronesia son los únicos países que utilizan **mdy**, que no se puede ordenar. La secuencia mixta del formato coincide con un patrón para decir fechas en comunicación verbal.

#### <a name="code-example-of-set-dateformat-mdy-versus-dmy"></a>Ejemplo de código de SET DATEFORMAT: *mdy* frente a *dmy*

El ejemplo de código de Transact-SQL siguiente usa la misma cadena de caracteres de fecha con tres opciones distintas de DATEFORMAT. Una ejecución del código genera el resultado que se muestra en el comentario:

```sql
DECLARE @yourDateString NVARCHAR(10) = '12-09-2018';
PRINT @yourDateString + '  = the input.';

SET DATEFORMAT dmy;
SELECT CONVERT(DATE, @yourDateString) AS [DMY-Interpretation-of-input-format];

SET DATEFORMAT mdy;
SELECT CONVERT(DATE, @yourDateString) AS [MDY-Interpretation-of-input-format];

SET DATEFORMAT ymd;
SELECT CONVERT(DATE, @yourDateString) AS [YMD-Interpretation--?--NotGuaranteed];


/***  Actual output:
12-09-2018  = the input.

DMY-Interpretation-of-input-format
2018-09-12

MDY-Interpretation-of-input-format
2018-12-09

YMD-Interpretation--?--NotGuaranteed
2018-12-09
***/
```

En el ejemplo de código anterior, el último ejemplo tiene una discrepancia entre el formato **ymd** y la cadena de entrada. El tercer nodo de la cadena de entrada representa un valor numérico que es demasiado grande para ser un día. Microsoft no garantiza el valor de salida de este tipo de errores.

#### <a name="convert-offers-explicit-codes-for-_deterministic_-control-of-date-formats"></a>CONVERT ofrece códigos explícitos para control _determinista_ de formatos de fecha

El artículo de documentación de CAST y CONVERT enumera códigos explícitos que puede usar con la función CONVERT para controlar de forma _determinista_ las conversiones de fecha. Cada mes el artículo está entre nuestras páginas más visitadas.

- [CAST y CONVERT (Transact-SQL): Estilos de fecha y hora](../functions/cast-and-convert-transact-sql.md#date-and-time-styles)
- [CAST y CONVERT (Transact-SQL): Determinadas conversiones de fecha y hora son no deterministas](../functions/cast-and-convert-transact-sql.md#certain-datetime-conversions-are-nondeterministic)



## <a name="compatibility-level-90-and-above"></a>Nivel de compatibilidad 90 o superior

En SQL Server 2000, el nivel de compatibilidad era 80. Para configuraciones del nivel de 80 o inferiores, las conversiones de fecha implícita eran deterministas.

A partir de SQL Server 2005 y su nivel de compatibilidad 90, las conversiones de fecha implícitas pasaron a ser no deterministas. Las conversiones de fecha se convirtieron en dependientes en SET LANGUAGE y SET DATEFORMAT a partir del nivel 90.

#### <a name="unicode"></a>Unicode

<!-- The next live sentence needs an explanatory example!  N'somethingHere?'.
-->
La conversión de los datos de caracteres no Unicode entre intercalaciones también se considera no determinista.



## <a name="see-also"></a>Consulte también

- [Establecer un idioma de la sesión](../../relational-databases/collations/set-a-session-language.md)
- [Tipos de datos y funciones de fecha y hora (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md)
- [FORMAT (Transact-SQL)](../functions/format-transact-sql.md)
- [ISDATE (Transact-SQL)](../functions/isdate-transact-sql.md)



<!--
This new article is linked-to by the following articles (at least initially on 2018/11/19).....
...
* docs/relational-databases/views/create-indexed-views.md
* docs/relational-databases/indexes/indexes-on-computed-columns.md
* docs/t-sql/functions/cast-and-convert-transact-sql.md
...
As a reaction to public PR 1279, this approach of creating a new article to link to is a better alternative than a docs/includes/ approach.
GeneMi (MightyPen), 2018/11/19
-->


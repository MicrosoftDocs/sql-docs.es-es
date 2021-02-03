---
description: Compatibilidad con la intercalación y Unicode
title: Compatibilidad con la intercalación y Unicode | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- binary collations [SQL Server]
- expression-level collations [SQL Server]
- Windows collations [SQL Server]
- globalization [SQL Server], about globalization
- supplementary character support
- code pages [SQL Server], about code pages
- collations [SQL Server], about collations
- Unicode [SQL Server], collations
- database-level collations [SQL Server]
- reading order
- column-level collations
- locales [SQL Server], about locales
- locales [SQL Server]
- code pages [SQL Server]
- SQL Server collations
- UTF-8
- UTF-16
- UTF8
- UTF16
- UCS2
- server-level collations [SQL Server]
ms.assetid: 92d34f48-fa2b-47c5-89d3-a4c39b0f39eb
author: pmasl
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e2a5f205d7182d1ffedf07b885e7411920ca7b27
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251210"
---
# <a name="collation-and-unicode-support"></a>Compatibilidad con la intercalación y Unicode
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporcionan propiedades de distinción entre mayúsculas y minúsculas, acentos y reglas de ordenación para los datos. Las intercalaciones que se usan con tipos de datos de caracteres, como **char** y **varchar**, dictan la página de códigos y los caracteres correspondientes que se pueden representar para ese tipo de datos. 

Si va a instalar una instancia nueva de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], restaurar una copia de seguridad de la base de datos o conectar el servidor a bases de datos cliente, es importante conocer los requisitos de configuración regional, el criterio de ordenación y la distinción entre mayúsculas y minúsculas, y acentos de los datos con los que se trabaja. Para obtener una lista de las intercalaciones disponibles en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md).    
    
Al seleccionar una intercalación para el servidor, base de datos, columna o expresión, se asignan ciertas características a los datos. Estas características afectan a los resultados de muchas operaciones de la base de datos. Por ejemplo, cuando se crea una consulta con `ORDER BY`, es posible que el criterio de ordenación del conjunto de resultados dependa de la intercalación que se aplica a la base de datos o que se dicta en una cláusula `COLLATE` en el nivel de expresión de la consulta.    
    
Para hacer el mejor uso posible de la compatibilidad con la intercalación en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se deben conocer los términos que se definen en este tema y cómo se relacionan con las características de los datos.    
    
##  <a name="collation-terms"></a><a name="Terms"></a> Términos de intercalación    
    
-   [Intercalación](#Collation_Defn) 
    - [Conjuntos de intercalación](#Collation_sets)
    - [Niveles de intercalación](#Collation_levels)
-   [Configuración regional](#Locale_Defn)    
-   [Página de códigos](#Code_Page_Defn)    
-   [Criterio de ordenación](#Sort_Order_Defn)    
    
###  <a name="collation"></a><a name="Collation_Defn"></a> Intercalación    
Una intercalación especifica los patrones de bits que representan a cada carácter de un conjunto de datos. Las intercalaciones también determinan las reglas que ordenan y comparan los datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite almacenar los objetos que tienen intercalaciones diferentes en una sola base de datos. En las columnas que no sean Unicode, la configuración de intercalación especifica la página de códigos de los datos y qué caracteres se pueden representar. Los datos que se mueven entre columnas que no sean Unicode se deben convertir de la página de códigos de origen a la de destino.    
    
Los resultados de las instrucciones de[!INCLUDE[tsql](../../includes/tsql-md.md)] pueden variar cuando se ejecutan en el contexto de bases de datos distintas que tengan una configuración de intercalación diferente. Si es posible, use una intercalación normalizada para su organización. De esta manera no tiene que especificar la intercalación en todos los caracteres o expresiones Unicode. Si debe trabajar con objetos que tienen configuraciones de intercalación y de página de códigos diferentes, conviene codificar las consultas para tener en cuenta las reglas de prioridad de intercalación. Para obtener más información, vea [Prioridad de intercalación (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).    
    
Las opciones asociadas con una intercalación son la distinción de mayúsculas y minúsculas, la distinción de acentos, la distinción de kana, la distinción de ancho y la distinción de selector de variación. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] presenta una opción adicional para la codificación [UTF-8](https://www.wikipedia.org/wiki/UTF-8). 

Para especificar estas opciones se anexan al nombre de la intercalación. Por ejemplo, la intercalación **Japanese_Bushu_Kakusu_100_CS_AS_KS_WS_UTF8** es una intercalación con distinción entre mayúsculas y minúsculas, distinción de acentos, distinción de tipos de kana, distinción de ancho y con codificación UTF-8. Otro ejemplo: la intercalación **Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS** no distingue entre mayúsculas y minúsculas ni acentos, distingue los tipos de kana, el ancho, el selector de variación y usa codificación distinta de Unicode. 

El comportamiento asociado a estas diversas opciones se describe en la tabla siguiente:    
    
|Opción|Descripción|    
|------------|-----------------|    
|Distinguir mayúsculas de minúsculas (\_CS)|Distingue entre letras mayúsculas y minúsculas. Si se selecciona esta opción, las letras minúsculas se ordenan por delante de sus versiones en mayúsculas. Si esta opción no está seleccionada, la intercalación no distinguirá mayúsculas de minúsculas. Es decir, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera las versiones mayúscula y minúscula de las letras como letras idénticas a efectos de ordenación. Puede seleccionar explícitamente no distinguir entre mayúsculas y minúsculas especificando \_CI.|   
|Distinguir acentos (\_AS)|Distingue entre caracteres acentuados y no acentuados. Por ejemplo, "a" no es igual a "ấ". Si esta opción no está seleccionada, la intercalación no distinguirá los acentos. Es decir, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera las versiones acentuadas y no acentuadas de las letras como letras idénticas a efectos de ordenación. Puede seleccionar explícitamente no distinguir acentos especificando \_AI.|    
|Distinguir kana (\_KS)|Distingue entre dos tipos de caracteres kana japoneses: Hiragana y Katakana. Si esta opción no está seleccionada, la intercalación no distinguirá los caracteres kana. Es decir, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera los caracteres Hiragana y Katakana como caracteres iguales a efectos de ordenación. La omisión de esta opción es el único método para especificar que no se distinguen los caracteres kana.|   
|Distinguir ancho (\_WS)|Distingue entre caracteres de ancho total y ancho medio. Si no se selecciona esta opción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera que la representación de ancho completo y de ancho medio del mismo carácter son idénticas con fines de ordenación. La omisión de esta opción es el único método para especificar no distinción de ancho.|  
|Distinguir selector de variación (\_VSS)|Distingue entre varios selectores de variación ideográfica en las intercalaciones del japonés **Japanese_Bushu_Kakusu_140** y **Japanese_XJIS_140**, introducidas en [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]. Una secuencia de variación consta de un carácter base y de un selector de variación adicional. Si no se selecciona esta opción \_VSS, la intercalación no distinguirá el selector de variación y este no se tendrá en cuenta en la comparación. Es decir, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera que los caracteres que se basan en el mismo carácter base con diferentes selectores de variación son idénticos con fines de ordenación. Para más información, vea [Unicode Ideographic Variation Database](https://www.unicode.org/reports/tr37/) (Base de datos de variaciones ideográficas de Unicode).<br/><br/> Las intercalaciones que distinguen selectores de variación (\_VSS) no se admiten en los índices de búsqueda de texto completo. Los índices de búsqueda de texto completo solo admiten opciones que distinguen acentos (\_AS), que distinguen kana (\_KS) y que distinguen ancho (\_WS). Los motores XML y CLR de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admiten selectores de variación (\_VSS).|      
|Binario (\_BIN)<sup>1</sup>|Ordena y compara datos de las tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] basándose en los patrones de bits definidos para cada carácter. El orden binario distingue entre mayúsculas y minúsculas, y acentos. El orden binario es también el más rápido. Para más información, vea la sección [Intercalaciones binarias](#Binary-collations) de este artículo.|      
|Punto de código binario (\_BIN2)<sup>1</sup> | Ordena y compara datos de las tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] según los puntos de código Unicode de datos Unicode. Para los datos que no son Unicode, el punto de código binario usa comparaciones idénticas a las de las ordenaciones binarias.<br/><br/> La ventaja de usar un criterio de ordenación de punto de código binario es que no es necesaria ninguna reordenación de los datos en aplicaciones que comparan los datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordenados. Por consiguiente, el criterio de ordenación de punto de código binario proporciona un desarrollo más simple de las aplicaciones y posibles aumentos en el rendimiento. Para más información, vea la sección [Intercalaciones binarias](#Binary-collations) de este artículo.|
|UTF-8 (\_UTF8)|Permite que los datos con codificación UTF-8 se almacenen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si no se selecciona esta opción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa el formato de codificación distinto de Unicode predeterminado para los tipos de datos aplicables. Para más información, vea la sección [Compatibilidad con UTF-8](#utf8) de este artículo.| 

<sup>1</sup> Si están seleccionadas las opciones Binario o Punto de código binario, las opciones Distinguir mayúsculas de minúsculas (\_CS), Distinguir acentos (\_AS), Distinguir kana (\_KS) y Distinguir ancho (\_WS) no estarán disponibles.      

#### <a name="examples-of-collation-options"></a>Ejemplos de opciones de intercalación
Cada intercalación se combina como una serie de sufijos para definir la distinción entre mayúsculas y minúsculas, acentos, ancho o kana. En estos ejemplos se describe el comportamiento del criterio de ordenación de diversas combinaciones de sufijos.

|Sufijo de intercalación de Windows|Descripción del orden|
|------------|-----------------| 
|\_BIN<sup>1</sup>|Orden binario|
|\_BIN2<sup>1, 2</sup>|Criterio de ordenación Punto de código binario|
|\_CI_AI<sup>2</sup>|No distingue mayúsculas de minúsculas, ni acentos, ni kana, ni ancho.|
|\_CI_AI_KS<sup>2</sup>|No distingue mayúsculas de minúsculas, ni acentos, ni ancho pero sí distingue Kana.|
|\_CI_AI_KS_WS<sup>2</sup>|No distingue mayúsculas de minúsculas, ni acentos, pero sí distingue Kana y ancho.|
|\_CI_AI_WS<sup>2</sup>|No distingue mayúsculas de minúsculas, ni acentos, ni Kana pero sí distingue ancho.|
|\_CI_AS<sup>2</sup>|No distingue mayúsculas de minúsculas, ni Kana ni ancho, pero sí acentos.|
|\_CI_AS_KS<sup>2</sup>|No distingue mayúsculas de minúsculas, ni ancho, pero sí acentos y Kana.|
|\_CI_AS_KS_WS<sup>2</sup>|No distingue mayúsculas de minúsculas, pero sí distingue acentos, Kana y ancho.|
|\_CI_AS_WS<sup>2</sup>|No distingue entre mayúsculas y minúsculas, ni Kana, pero sí acentos y ancho.|
|\_CS_AI<sup>2</sup>|Distingue mayúsculas de minúsculas, pero no distingue acentos, ni Kana, ni ancho.|
|\_CS_AI_KS<sup>2</sup>|Distingue mayúsculas de minúsculas y Kana, pero no distingue acentos ni ancho.|
|\_CS_AI_KS_WS<sup>2</sup>|Distingue mayúsculas de minúsculas, Kana y ancho, pero no distingue acentos.|
|\_CS_AI_WS<sup>2</sup>|Distingue mayúsculas de minúsculas y ancho, pero no distingue Kana ni acentos.|
|\_CS_AS<sup>2</sup>|Distingue mayúsculas de minúsculas y acentos, pero no distingue Kana ni ancho.|
|\_CS_AS_KS<sup>2</sup>|Distingue mayúsculas de minúsculas, acento y Kana, pero no distingue ancho.|
|\_CS_AS_KS_WS<sup>2</sup>|Distingue mayúsculas de minúsculas, acentos, Kana y ancho.|
|\_CS_AS_WS<sup>2</sup>|Distingue mayúsculas de minúsculas, acentos y ancho, pero no distingue Kana.|

<sup>1</sup> Si están seleccionadas las opciones Binario o Punto de código binario, las opciones Distinguir mayúsculas de minúsculas (\_CS), Distinguir acentos (\_AS), Distinguir kana (\_KS) y Distinguir ancho (\_WS) no estarán disponibles.    

<sup>2</sup> La adición de la opción UTF-8 (\_UTF8) permite codificar datos Unicode mediante UTF-8. Para más información, vea la sección [Compatibilidad con UTF-8](#utf8) de este artículo. 

### <a name="collation-sets"></a><a name="Collation_sets"></a> Conjuntos de intercalación

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite los siguientes conjuntos de intercalación:    

-  [Intercalaciones de Windows](#Windows-collations)
-  [Intercalaciones binarias](#Binary-collations)
-  [Intercalaciones de SQL Server](#SQL-collations)
    
#### <a name="windows-collations"></a><a name="Windows-collations"></a> Intercalaciones de Windows    
Las intercalaciones de Windows definen reglas para almacenar los datos de caracteres que se basan en una configuración regional del sistema Windows asociada. En una intercalación de Windows, se puede implementar una comparación de datos no Unicode con el mismo algoritmo que la de los datos Unicode. Las reglas de intercalación básicas de Windows especifican qué alfabeto o idioma se usa cuando se aplica una ordenación de diccionario. Las reglas también especifican la página de códigos que se usa para almacenar los datos de caracteres que no son Unicode. Tanto la ordenación Unicode y como la ordenación no Unicode son compatibles con comparaciones de cadenas de una determinada versión de Windows. Esto proporciona coherencia entre los tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y permite a los desarrolladores ordenar las cadenas de sus aplicaciones mediante las mismas reglas que usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, vea [Nombre de intercalación de Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).    
    
#### <a name="binary-collations"></a><a name="Binary-collations"></a> Intercalaciones binarias    
Las intercalaciones binarias ordenan los datos según la secuencia de valores codificados definidos por la configuración regional y el tipo de datos. Distinguen mayúsculas de minúsculas Una intercalación binaria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] define la configuración regional y la página de códigos ANSI que se usa. Esto exige un criterio de ordenación binario. Como son relativamente simples, las intercalaciones binarias ayudan a mejorar el rendimiento de la aplicación. En los tipos de datos que no son Unicode, las comparaciones de datos se basan en los puntos de código que se definen en la página de códigos ANSI. En tipos de datos Unicode, las comparaciones de datos dependen de los puntos de código Unicode. En las intercalaciones binarias de tipos de datos Unicode, la configuración regional no se tiene en cuenta a la hora de ordenar los datos. Por ejemplo, **Latin_1_General_BIN** y **Japanese_BIN** generan resultados de orden idénticos cuando se usan en datos Unicode. Para más información, vea [Nombre de intercalación de Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).   
    
Hay dos tipos de intercalaciones binarias en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

-  Las intercalaciones **BIN** heredadas, que han realizado una comparación de punto de código a punto de código incompleta para los datos Unicode. Estas intercalaciones binarias heredadas comparaban el primer carácter como WCHAR, seguido de una comparación byte a byte. En una intercalación BIN solo el primer carácter se ordena de acuerdo al punto de código y el resto de ellos se ordenan según sus valores de byte.

-  Las intercalaciones **BIN2** más recientes, que implementan una comparación pura de punto de código. En una intercalación BIN2 todos los caracteres se ordenan de acuerdo a sus puntos de código. Como la plataforma de Intel tiene una arquitectura "little endian", los caracteres de codificación Unicode siempre se intercambian por bytes.     
    
#### <a name="sql-server-collations"></a><a name="SQL-collations"></a> Intercalaciones de SQL Server    
Las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQL_\*) son compatibles en cuanto al criterio de ordenación con las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las reglas de ordenación de diccionario para datos que no son Unicode son incompatibles con cualquier rutina de ordenación suministrada por los sistemas operativos Windows. Sin embargo, la ordenación de datos Unicode es compatible con una versión especial de las reglas de ordenación de Windows. Como las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usan reglas de comparación diferentes para los datos Unicode y para los que no son Unicode, ve resultados diferentes en las comparaciones de los mismos datos, dependiendo del tipo de datos subyacente. Para más información, vea [Nombre de intercalación de SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md). 

Durante la configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la opción de intercalación de instalación predeterminada viene determinada por la configuración regional del sistema operativo (SO). Puede cambiar las intercalaciones de nivel de servidor durante la instalación o si modifica la configuración regional del SO antes de la instalación. Para la compatibilidad con versiones anteriores, la intercalación predeterminada se establece en la versión más antigua disponible que esté asociada a cada configuración regional concreta. Por tanto, esta no es siempre la intercalación recomendada. Para aprovechar al máximo las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cambie la configuración de instalación predeterminada para que use las intercalaciones de Windows. Por ejemplo, para la configuración regional del sistema operativo "Inglés (Estados Unidos)" (página de códigos 1252), la intercalación predeterminada durante la instalación es **SQL_Latin1_General_CP1_CI_AS** y se puede cambiar a la intercalación de Windows homóloga más cercana, **Latin1_General_100_CI_AS_SC**.
    
> [!NOTE]    
> Al actualizar una instancia en idioma inglés de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se pueden especificar intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQL_\*) para la compatibilidad con las instancias existentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Como la intercalación predeterminada de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se define durante la instalación, asegúrese de especificar con cuidado la configuración de la intercalación cuando se cumplan las condiciones siguientes:    
>     
> -   El código de la aplicación depende del comportamiento de las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anteriores.    
> -   Se deben almacenar datos de caracteres que reflejen varios idiomas.    
    
### <a name="collation-levels"></a><a name="Collation_levels"></a> Niveles de intercalación
Se admite el establecimiento de intercalaciones en los siguientes niveles de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:    

-  [Intercalaciones de nivel de servidor](#Server-level-collations)
-  [Intercalaciones de nivel de base de datos](#Database-level-collations)
-  [Intercalaciones de nivel de columna](#Column-level-collations)
-  [Intercalaciones de nivel de expresión](#Expression-level-collations)

#### <a name="server-level-collations"></a><a name="Server-level-collations"></a> Intercalaciones de nivel de servidor   
La intercalación predeterminada de servidor se determina durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y se convierte en la intercalación predeterminada de las bases de datos del sistema y de todas las bases de datos del usuario. 

En la tabla siguiente se muestran las designaciones predeterminadas de intercalación, determinadas por la configuración regional del sistema operativo (SO), incluidos los respectivos identificadores de código de idioma (LCID) de Windows y SQL:

|Configuración regional de Windows|LCID de Windows|LCID de SQL|Intercalación predeterminada|
|---------------|---------|---------|---------------|
|Afrikáans (Sudáfrica)|0x0436|0x0409|Latin1_General_CI_AS|
|Albanés (Albania)|0x041c|0x041c|Albanian_CI_AS|
|Alsaciano (Francia)|0x0484|0x0409|Latin1_General_CI_AS|
|Amárico (Etiopía)|0x045e|0x0409|Latin1_General_CI_AS|
|Árabe (Argelia)|0x1401|0x0401|Arabic_CI_AS|
|Árabe (Bahréin)|0x3c01|0x0401|Arabic_CI_AS|
|Árabe (Egipto)|0x0c01|0x0401|Arabic_CI_AS|
|Árabe (Iraq)|0x0801|0x0401|Arabic_CI_AS|
|Árabe (Jordania)|0x2c01|0x0401|Arabic_CI_AS|
|Árabe (Kuwait)|0x3401|0x0401|Arabic_CI_AS|
|Árabe (Líbano)|0x3001|0x0401|Arabic_CI_AS|
|Árabe (Libia)|0x1001|0x0401|Arabic_CI_AS|
|Árabe (Marruecos)|0x1801|0x0401|Arabic_CI_AS|
|Árabe (Omán)|0x2001|0x0401|Arabic_CI_AS|
|Árabe (Qatar)|0x4001|0x0401|Arabic_CI_AS|
|Árabe (Arabia Saudí)|0x0401|0x0401|Arabic_CI_AS|
|Árabe (Siria)|0x2801|0x0401|Arabic_CI_AS|
|Árabe (Túnez)|0x1c01|0x0401|Arabic_CI_AS|
|Árabe (E. A. U.)|0x3801|0x0401|Arabic_CI_AS|
|Árabe (Yemen)|0x2401|0x0401|Arabic_CI_AS|
|Armenio (Armenia)|0x042b|0x0419|Latin1_General_CI_AS|
|Asamés (India)|0x044d|0x044d|No disponible en el nivel de servidor|
|Azerbaiyano (Azerbaiyán, cirílico)|0x082c|0x082c|Obsoleta, no disponible en el nivel de servidor|
|Azerbaiyano (Azerbaiyán, latino)|0x042c|0x042c|Obsoleta, no disponible en el nivel de servidor|
|Baskir (Rusia)|0x046d|0x046d|Latin1_General_CI_AI|
|Vasco (España)|0x042d|0x0409|Latin1_General_CI_AS|
|Bielorruso (Bielorrusia)|0x0423|0x0419|Cyrillic_General_CI_AS|
|Bengalí (Bangladesh)|0x0845|0x0445|No disponible en el nivel de servidor|
|Bengali (India)|0x0445|0x0439|No disponible en el nivel de servidor|
|Bosnio (cirílico, Bosnia-Herzegovina)|0x201a|0x201a|Latin1_General_CI_AI|
|Bosnio (latino, Bosnia-Herzegovina)|0x141a|0x141a|Latin1_General_CI_AI|
|Bretón (Francia)|0x047e|0x047e|Latin1_General_CI_AI|
|Búlgaro (Bulgaria)|0x0402|0x0419|Cyrillic_General_CI_AS|
|Catalán (España)|0x0403|0x0409|Latin1_General_CI_AS|
|Chino (Hong Kong ZAE, RPC)|0x0c04|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Chinese (Macao SAR)|0x1404|0x1404|Latin1_General_CI_AI|
|Chino (Macao)|0x21404|0x21404|Latin1_General_CI_AI|
|Chino (RPC)|0x0804|0x0804|Chinese_PRC_CI_AS|
|Chino (RPC)|0x20804|0x20804|Chinese_PRC_Stroke_CI_AS|
|Chinese (Singapore)|0x1004|0x0804|Chinese_PRC_CI_AS|
|Chinese (Singapore)|0x21004|0x20804|Chinese_PRC_Stroke_CI_AS|
|Chino (Taiwán)|0x30404|0x30404|Chinese_Taiwan_Bopomofo_CI_AS|
|Chino (Taiwán)|0x0404|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Corso (Francia)|0x0483|0x0483|Latin1_General_CI_AI|
|Croata (Bosnia y Herzegovina, latino)|0x101a|0x041a|Croatian_CI_AS|
|Croata (Croacia)|0x041a|0x041a|Croatian_CI_AS|
|Checo (República Checa)|0x0405|0x0405|Czech_CI_AS|
|Danés (Dinamarca)|0x0406|0x0406|Danish_Norwegian_CI_AS|
|Dari (Afganistán)|0x048c|0x048c|Latin1_General_CI_AI|
|Divehi (Maldivas)|0x0465|0x0465|No disponible en el nivel de servidor|
|Neerlandés (Bélgica)|0x0813|0x0409|Latin1_General_CI_AS|
|Neerlandés (Países Bajos)|0x0413|0x0409|Latin1_General_CI_AS|
|Inglés (Australia)|0x0c09|0x0409|Latin1_General_CI_AS|
|Inglés (Belice)|0x2809|0x0409|Latin1_General_CI_AS|
|Inglés (Canadá)|0x1009|0x0409|Latin1_General_CI_AS|
|Inglés (Caribe)|0x2409|0x0409|Latin1_General_CI_AS|
|Inglés (India)|0x4009|0x0409|Latin1_General_CI_AS|
|Inglés (Irlanda)|0x1809|0x0409|Latin1_General_CI_AS|
|Inglés (Jamaica)|0x2009|0x0409|Latin1_General_CI_AS|
|Inglés (Malasia)|0x4409|0x0409|Latin1_General_CI_AS|
|Inglés (Nueva Zelanda)|0x1409|0x0409|Latin1_General_CI_AS|
|Inglés (Filipinas)|0x3409|0x0409|Latin1_General_CI_AS|
|Inglés (Singapur)|0x4809|0x0409|Latin1_General_CI_AS|
|Inglés (Sudáfrica)|0x1c09|0x0409|Latin1_General_CI_AS|
|Inglés (Trinidad y Tobago)|0x2c09|0x0409|Latin1_General_CI_AS|
|Inglés (Reino Unido)|0x0809|0x0409|Latin1_General_CI_AS|
|Spanish (Traditional Sort) - Spain|0x0409|0x0409|SQL_Latin1_General_CP1_CI_AS|
|Inglés (Zimbabue)|0x3009|0x0409|Latin1_General_CI_AS|
|Estonio (Estonia)|0x0425|0x0425|Estonian_CI_AS|
|Feroés (Islas Feroe)|0x0438|0x0409|Latin1_General_CI_AS|
|Filipino (Filipinas)|0x0464|0x0409|Latin1_General_CI_AS|
|Finés (Finlandia)|0x040b|0x040b|Finnish_Swedish_CI_AS|
|Francés (Bélgica)|0x080c|0x040c|French_CI_AS|
|Francés (Canadá)|0x0c0c|0x040c|French_CI_AS|
|Francés (Francia)|0x040c|0x040c|French_CI_AS|
|Francés (Luxemburgo)|0x140c|0x040c|French_CI_AS|
|Francés (Mónaco)|0x180c|0x040c|French_CI_AS|
|Francés (Suiza)|0x100c|0x040c|French_CI_AS|
|Frisón (Países Bajos)|0x0462|0x0462|Latin1_General_CI_AI|
|Gallego|0x0456|0x0409|Latin1_General_CI_AS|
|Georgiano (Georgia)|0x10437|0x10437|Georgian_Modern_Sort_CI_AS|
|Georgiano (Georgia)|0x0437|0x0419|Latin1_General_CI_AS|
|Alemán (alfabetización de libreta de teléfonos, DIN)|0x10407|0x10407|German_PhoneBook_CI_AS|
|Alemán (Austria)|0x0c07|0x0409|Latin1_General_CI_AS|
|Alemán (Alemania)|0x0407|0x0409|Latin1_General_CI_AS|
|Alemán (Liechtenstein)|0x1407|0x0409|Latin1_General_CI_AS|
|Alemán (Luxemburgo)|0x1007|0x0409|Latin1_General_CI_AS|
|Alemán (Suiza)|0x0807|0x0409|Latin1_General_CI_AS|
|Griego (Grecia)|0x0408|0x0408|Greek_CI_AS|
|Groenlandés (Groenlandia)|0x046f|0x0406|Danish_Norwegian_CI_AS|
|Gujarati (India)|0x0447|0x0439|No disponible en el nivel de servidor|
|Hausa (Nigeria, latino)|0x0468|0x0409|Latin1_General_CI_AS|
|Hebreo (Israel)|0x040d|0x040d|Hebrew_CI_AS|
|Hindi (India)|0x0439|0x0439|No disponible en el nivel de servidor|
|Húngaro (Hungría)|0x040e|0x040e|Hungarian_CI_AS|
|Húngaro técnico|0x1040e|0x1040e|Hungarian_Technical_CI_AS|
|Islandés (Islandia)|0x040f|0x040f|Icelandic_CI_AS|
|Igbo (Nigeria)|0x0470|0x0409|Latin1_General_CI_AS|
|Indonesio (Indonesia)|0x0421|0x0409|Latin1_General_CI_AS|
|Inuktitut (Canadá, latino)|0x085d|0x0409|Latin1_General_CI_AS|
|Inuktitut (silábico, Canadá)|0x045d|0x045d|Latin1_General_CI_AI|
|Irlandés (Irlanda)|0x083c|0x0409|Latin1_General_CI_AS|
|Italiano (Italia)|0x0410|0x0409|Latin1_General_CI_AS|
|Italiano (Suiza)|0x0810|0x0409|Latin1_General_CI_AS|
|Japonés (Japón XJIS)|0x0411|0x0411|Japanese_CI_AS|
|Japonés (Japón)|0x040411|0x40411|Latin1_General_CI_AI|
|Canarés (India)|0x044b|0x0439|No disponible en el nivel de servidor|
|Kazajo (Kazajistán)|0x043f|0x043f|Kazakh_90_CI_AS|
|Jemer (Camboya)|0x0453|0x0453|No disponible en el nivel de servidor|
|Quiché (Guatemala)|0x0486|0x0c0a|Modern_Spanish_CI_AS|
|Kinyarwanda (Ruanda)|0x0487|0x0409|Latin1_General_CI_AS|
|Konkani (India)|0x0457|0x0439|No disponible en el nivel de servidor|
|Coreano (Orden del diccionario coreano)|0x0412|0x0412|Korean_Wansung_CI_AS|
|Kirguizo (Kirguizistán)|0x0440|0x0419|Cyrillic_General_CI_AS|
|Lao (R.D.P. de Laos)|0x0454|0x0454|No disponible en el nivel de servidor|
|Letón (Letonia)|0x0426|0x0426|Latvian_CI_AS|
|Lituano (Lituania)|0x0427|0x0427|Lithuanian_CI_AS|
|Bajo sorbio (Alemania)|0x082e|0x0409|Latin1_General_CI_AS|
|Luxemburgués (Luxemburgo)|0x046e|0x0409|Latin1_General_CI_AS|
|Macedonio (Macedonia del Norte)|0x042f|0x042f|Macedonian_FYROM_90_CI_AS|
|Malayo (Brunéi Darussalam)|0x083e|0x0409|Latin1_General_CI_AS|
|Malayo (Malasia)|0x043e|0x0409|Latin1_General_CI_AS|
|Malayalam (India)|0x044c|0x0439|No disponible en el nivel de servidor|
|Maltés (Malta)|0x043a|0x043a|Latin1_General_CI_AI|
|Maorí (Nueva Zelanda)|0x0481|0x0481|Latin1_General_CI_AI|
|Mapuche (Chile)|0x047a|0x047a|Latin1_General_CI_AI|
|Maratí (India)|0x044e|0x0439|No disponible en el nivel de servidor|
|Mohawk (Canadá)|0x047c|0x047c|Latin1_General_CI_AI|
|Mongol (Mongolia)|0x0450|0x0419|Cyrillic_General_CI_AS|
|Mongol (RPC)|0x0850|0x0419|Cyrillic_General_CI_AS|
|Nepalí (Nepal)|0x0461|0x0461|No disponible en el nivel de servidor|
|Noruego (Bokmål, Noruega)|0x0414|0x0414|Latin1_General_CI_AI|
|Noruego (nynorsk, Noruega)|0x0814|0x0414|Latin1_General_CI_AI|
|Occitano (Francia)|0x0482|0x040c|French_CI_AS|
|Odia (India)|0x0448|0x0439|No disponible en el nivel de servidor|
|Pastún (Afganistán)|0x0463|0x0463|No disponible en el nivel de servidor|
|Persa (Irán)|0x0429|0x0429|Latin1_General_CI_AI|
|Polaco (Polonia)|0x0415|0x0415|Polish_CI_AS|
|Portugués (Brasil)|0x0416|0x0409|Latin1_General_CI_AS|
|Portugués (Portugal)|0x0816|0x0409|Latin1_General_CI_AS|
|Punjabí (India)|0x0446|0x0439|No disponible en el nivel de servidor|
|Quechua (Bolivia)|0x046b|0x0409|Latin1_General_CI_AS|
|Quechua (Ecuador)|0x086b|0x0409|Latin1_General_CI_AS|
|Quechua (Perú)|0x0c6b|0x0409|Latin1_General_CI_AS|
|Rumano (Rumanía)|0x0418|0x0418|Romanian_CI_AS|
|Romanche (Suiza)|0x0417|0x0417|Latin1_General_CI_AI|
|Ruso (Rusia)|0x0419|0x0419|Cyrillic_General_CI_AS|
|Sakha (Rusia)|0x0485|0x0485|Latin1_General_CI_AI|
|Sami (inari, Finlandia)|0x243b|0x083b|Latin1_General_CI_AI|
|Sami (Lule, Noruega)|0x103b|0x043b|Latin1_General_CI_AI|
|Sami (lule, Suecia)|0x143b|0x083b|Latin1_General_CI_AI|
|Sami (septentrional, Finlandia)|0x0c3b|0x083b|Latin1_General_CI_AI|
|Sami (septentrional, Noruega)|0x043b|0x043b|Latin1_General_CI_AI|
|Sami (septentrional, Suecia)|0x083b|0x083b|Latin1_General_CI_AI|
|Sami (skolt, Finlandia)|0x203b|0x083b|Latin1_General_CI_AI|
|Sami (meridional, Noruega)|0x183b|0x043b|Latin1_General_CI_AI|
|Sami (meridional, Suecia)|0x1c3b|0x083b|Latin1_General_CI_AI|
|Sánscrito (India)|0x044f|0x0439|No disponible en el nivel de servidor|
|Serbio (cirílico, Bosnia-Herzegovina)|0x1c1a|0x0c1a|Latin1_General_CI_AI|
|Serbio (latino, Bosnia-Herzegovina)|0x181a|0x081a|Latin1_General_CI_AI|
|Serbio (cirílico, Serbia)|0x0c1a|0x0c1a|Latin1_General_CI_AI|
|Serbio (latino, Serbia)|0x081a|0x081a|Latin1_General_CI_AI|
|Sesotho sa Leboa/sotho septentrional (Sudáfrica)|0x046c|0x0409|Latin1_General_CI_AS|
|Setswana/tswana (Sudáfrica)|0x0432|0x0409|Latin1_General_CI_AS|
|Cingalés (Sri Lanka)|0x045b|0x0439|No disponible en el nivel de servidor|
|Eslovaco (Eslovaquia)|0x041b|0x041b|Slovak_CI_AS|
|Esloveno (Eslovenia)|0x0424|0x0424|Slovenian_CI_AS|
|Español (Argentina)|0x2c0a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Bolivia)|0x400a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Chile)|0x340a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Colombia)|0x240a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Costa Rica)|0x140a|0x0c0a|Modern_Spanish_CI_AS|
|Español (República Dominicana)|0x1c0a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Ecuador)|0x300a|0x0c0a|Modern_Spanish_CI_AS|
|Español (El Salvador)|0x440a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Guatemala)|0x100a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Honduras)|0x480a|0x0c0a|Modern_Spanish_CI_AS|
|Español (México)|0x080a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Nicaragua)|0x4c0a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Panamá)|0x180a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Paraguay)|0x3c0a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Perú)|0x280a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Puerto Rico)|0x500a|0x0c0a|Modern_Spanish_CI_AS|
|Español (España)|0x0c0a|0x0c0a|Modern_Spanish_CI_AS|
|Español (España, tradicional)|0x040a|0x040a|Traditional_Spanish_CI_AS|
|Español (Estados Unidos)|0x540a|0x0409|Latin1_General_CI_AS|
|Español (Uruguay)|0x380a|0x0c0a|Modern_Spanish_CI_AS|
|Español (Venezuela)|0x200a|0x0c0a|Modern_Spanish_CI_AS|
|Swahili (Kenia)|0x0441|0x0409|Latin1_General_CI_AS|
|Sueco (Finlandia)|0x081d|0x040b|Finnish_Swedish_CI_AS|
|Sueco (Suecia)|0x041d|0x040b|Finnish_Swedish_CI_AS|
|Sirio (Siria)|0x045a|0x045a|No disponible en el nivel de servidor|
|Tayiko (Tayikistán)|0x0428|0x0419|Cyrillic_General_CI_AS|
|Tamazight (latino, Argelia)|0x085f|0x085f|Latin1_General_CI_AI|
|Tamil (India)|0x0449|0x0439|No disponible en el nivel de servidor|
|Tatar (Rusia)|0x0444|0x0444|Cyrillic_General_CI_AS|
|Telugu (India)|0x044a|0x0439|No disponible en el nivel de servidor|
|Tailandés (Tailandia)|0x041e|0x041e|Thai_CI_AS|
|Tibetano (RPC)|0x0451|0x0451|No disponible en el nivel de servidor|
|Turco (Turquía)|0x041f|0x041f|Turkish_CI_AS|
|Turcomano (Turkmenistán)|0x0442|0x0442|Latin1_General_CI_AI|
|Uigur (RPC)|0x0480|0x0480|Latin1_General_CI_AI|
|Ucraniano (Ucrania)|0x0422|0x0422|Ukrainian_CI_AS|
|Alto sorbio (Alemania)|0x042e|0x042e|Latin1_General_CI_AI|
|Urdú (Pakistán)|0x0420|0x0420|Latin1_General_CI_AI|
|Uzbeko (cirílico, Uzbekistán)|0x0843|0x0419|Cyrillic_General_CI_AS|
|Uzbeko (Uzbekistán, latín)|0x0443|0x0443|Uzbek_Latin_90_CI_AS|
|Vietnamita (Vietnam)|0x042a|0x042a|Vietnamese_CI_AS|
|Galés (Reino Unido)|0x0452|0x0452|Latin1_General_CI_AI|
|Wolof (Senegal)|0x0488|0x040c|French_CI_AS|
|Xhosa/isiXhosa (Sudáfrica)|0x0434|0x0409|Latin1_General_CI_AS|
|Yi (RPC)|0x0478|0x0409|Latin1_General_CI_AS|
|Yoruba (Nigeria)|0x046a|0x0409|Latin1_General_CI_AS|
|Zulú/isiZulu (Sudáfrica)|0x0435|0x0409|Latin1_General_CI_AS|

Después de que haya asignado una intercalación al servidor, solo la puede cambiar si exporta todos los objetos y datos de base de datos, vuelve a compilar la base de datos *master* e importa todos los objetos y datos de base de datos. En lugar de cambiar la intercalación predeterminada de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede especificar la intercalación deseada al crear una base de datos o una columna de base de datos.    

Para consultar la intercalación del servidor de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], use la función `SERVERPROPERTY`:

```sql
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));
```

Para consultar todas las intercalaciones disponibles del servidor, utilice la siguiente función integrada de `fn_helpcollations()`:

```sql
SELECT * FROM sys.fn_helpcollations();
```
    
#### <a name="database-level-collations"></a><a name="Database-level-collations"></a> Intercalaciones de base de datos    
Cuando se crea o modifica una base de datos, se puede usar la cláusula `COLLATE` de la instrucción `CREATE DATABASE` o `ALTER DATABASE` para especificar la intercalación de base de datos predeterminada. Si no se especifica ninguna intercalación, se asigna a la base de datos la intercalación de servidor.    
    
No se puede cambiar la intercalación de las bases de datos del sistema a menos que se cambie la intercalación del servidor.
    
La intercalación de base de datos se usa para todos los metadatos de la base de datos, y es la predeterminada para todas las columnas de cadena, los objetos temporales, los nombres de variable y cualquier otra cadena que se use en la base de datos. Cuando se cambia la intercalación de una base de datos de usuario, pueden producirse conflictos de intercalación cuando las consultas en la base de datos tienen acceso a tablas temporales. Las tablas temporales se almacenan siempre en la base de datos del sistema de *tempdb*, que usa la intercalación de la instancia. Es posible que las consultas que comparan datos de caracteres entre la base de datos de usuario y *tempdb* generen un error si las intercalaciones producen un conflicto en la evaluación de los datos de caracteres. Puede resolver esta incidencia si especifica la cláusula `COLLATE` en la consulta. Para más información, vea [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md).    

> [!NOTE]
> No se puede cambiar la intercalación una vez creada la base de datos en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

La intercalación de una base de datos de usuario se puede cambiar con una instrucción `ALTER DATABASE` similar a la siguiente:

```sql
ALTER DATABASE myDB COLLATE Greek_CS_AI;
```

> [!IMPORTANT]
> La alteración de la intercalación de nivel de base de datos no afecta a las intercalaciones de nivel de columna o de expresión.

Puede recuperar la intercalación actual de una base de datos mediante una instrucción similar a la siguiente:

```sql
SELECT CONVERT (VARCHAR(50), DATABASEPROPERTYEX('database_name','collation'));
```

#### <a name="column-level-collations"></a><a name="Column-level-collations"></a> Intercalaciones de columna    
Cuando cree o modifique una tabla, puede especificar intercalaciones para cada columna de cadena de caracteres mediante la cláusula `COLLATE`. Si no especifica ninguna intercalación, se asigna a la columna la intercalación predeterminada de la base de datos.    

La intercalación de una columna se puede cambiar con una instrucción `ALTER TABLE` similar a la siguiente:

```sql
ALTER TABLE myTable ALTER COLUMN mycol NVARCHAR(10) COLLATE Greek_CS_AI;
```
    
#### <a name="expression-level-collations"></a><a name="Expression-level-collations"></a> Intercalaciones de expresión    
Las intercalaciones de nivel de expresión se establecen cuando se ejecuta una instrucción y afectan al modo en que se devuelve un conjunto de resultados. Esto permite que los resultados de la ordenación `ORDER BY` sean específicos de la configuración regional. Para implementar intercalaciones de nivel de expresión, use una cláusula `COLLATE` como la siguiente:    
    
```sql    
SELECT name FROM customer ORDER BY name COLLATE Latin1_General_CS_AI;    
```    
    
###  <a name="locale"></a><a name="Locale_Defn"></a> Configuración regional    
Una configuración regional es un conjunto de información que está asociado a una ubicación o referencia cultural. La información puede incluir el nombre e identificador del idioma hablado, la escritura que se usa para escribir el idioma y las convenciones culturales. Las intercalaciones pueden estar asociadas a una o varias configuraciones regionales. Para obtener más información, vea [Id. de configuración regional asignados por Microsoft](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c).    
    
###  <a name="code-page"></a><a name="Code_Page_Defn"></a> Página de códigos    
Una página de códigos es un juego ordenado de caracteres en un script determinado en el que un índice numérico, o un valor de punto de código, está asociado con cada carácter. Una página de códigos de Windows se denomina normalmente *juego de caracteres* o *charset*. Las páginas de códigos se usan para ofrecer compatibilidad con los juegos de caracteres y las distribuciones de teclado que se usan en distintas configuraciones regionales del sistema Windows.     
 
###  <a name="sort-order"></a><a name="Sort_Order_Defn"></a> Criterio de ordenación    
El criterio de ordenación especifica cómo se ordenan los valores de datos. El orden afecta a los resultados de la comparación de los datos. Los datos se ordenan con las intercalaciones, y se pueden optimizar mediante los índices.    
    
##  <a name="unicode-support"></a><a name="Unicode_Defn"></a> Compatibilidad con Unicode    
Unicode es un estándar que permite asignar puntos de código con caracteres. Como se ha diseñado para abarcar todos los caracteres de todos los idiomas del mundo, no es preciso usar otras páginas de códigos para controlar los distintos juegos de caracteres.

### <a name="unicode-basics"></a>Conceptos básicos de Unicode
El almacenamiento de datos en varios idiomas dentro de una misma base de datos es difícil de controlar cuando solo se usan datos de tipo carácter y páginas de códigos. También es difícil encontrar una página de códigos para la base de datos que pueda almacenar todos los caracteres específicos de cada idioma. Además, es difícil asegurar la traducción adecuada de los caracteres especiales cuando se leen o actualizan desde clientes diferentes que ejecutan distintas páginas de códigos. Las bases de datos que admiten clientes internacionales siempre deberían usar tipos de datos Unicode en vez de tipos de datos no Unicode.

Por ejemplo, imaginemos que una base de datos de clientes en Norteamérica tiene que administrar tres idiomas:

-  Nombres y direcciones en español para México
-  Nombres y direcciones en francés para Quebec
-  Nombres y direcciones en inglés para el resto de Canadá y para Estados Unidos

Cuando use solo columnas de caracteres y páginas de códigos, debe prestar atención para asegurarse de que la base de datos esté instalada con una página de códigos que controle los caracteres de los tres idiomas. También debe prestar atención para asegurar la traducción adecuada de los caracteres de uno de los idiomas cuando se lean en clientes que ejecuten una página de códigos para otro idioma.
   
> [!NOTE]
> Las páginas de códigos que un cliente usa se determinan en la configuración del sistema operativo (SO). Para establecer las páginas de códigos del cliente en los sistemas operativos Windows, use **Configuración regional** en el Panel de control.    

Sería difícil elegir una página de códigos para los tipos de datos de carácter que admita todos los caracteres que precisa un público mundial. La forma más fácil de administrar los datos de caracteres en las bases de datos internacionales es usar siempre un tipo de datos que admita Unicode. 

### <a name="unicode-data-types"></a>Tipos de datos Unicode
Si almacena datos de caracteres que reflejan varios idiomas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores), use tipos de datos Unicode (**nchar**, **nvarchar** y **ntext**) en lugar de tipos de datos que no sean Unicode (**char**, **varchar** y **text**). 

> [!NOTE]
> Para los tipos de datos Unicode, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] puede representar hasta 65 535 caracteres mediante UCS-2, o el intervalo completo de Unicode (1 114 111 caracteres) si se usan caracteres adicionales. Para más información sobre cómo habilitar caracteres adicionales, vea [Caracteres adicionales](#Supplementary_Characters).

Como alternativa, a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], si se usa una intercalación compatible con UTF-8 (\_UTF8), los tipos de datos anteriores que no son Unicode (**char** y **varchar**) se convierten en tipos de datos Unicode con la codificación UTF-8. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] no cambia el comportamiento de los tipos de datos Unicode que existieran antes (**nchar**, **nvarchar** y **ntext**), que seguirán usando la codificación UCS-2 o UTF-16. Para más información, vea [Diferencias de almacenamiento entre UTF-8 y UTF-16](#storage_differences).

### <a name="unicode-considerations"></a>Consideraciones de Unicode
Hay limitaciones significativas asociadas a los tipos de datos no Unicode. Esto se debe a que un equipo que no es Unicode está limitado a usar una única página de códigos. Es posible que experimente una ganancia de rendimiento al usar Unicode, ya que requiere menos conversiones de páginas de códigos. Las intercalaciones Unicode se deben seleccionar de forma individual en el nivel de expresión, base de datos o columna porque no se admiten en el nivel de servidor.    

Al mover los datos de un servidor a un cliente, los controladores de cliente anteriores podrían no reconocer la intercalación del servidor. Esto puede ocurrir al mover los datos de un servidor Unicode a un cliente no Unicode. La mejor opción podría ser actualizar el sistema operativo cliente para que las intercalaciones del sistema subyacentes se actualicen. Si el cliente tiene instalado software cliente de base de datos, se puede considerar la posibilidad de aplicar a dicho software una actualización de servicio.    
    
> [!TIP]
> También puede intentar utilizar una intercalación diferente para los datos del servidor. Elija una intercalación que se asigne a una página de códigos en el cliente.    
>
> Para usar las intercalaciones UTF-16 disponibles en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores) a fin de mejorar la búsqueda y la ordenación de algunos caracteres Unicode (solo en las intercalaciones de Windows), puede seleccionar una de las intercalaciones de caracteres adicionales (\_SC), o bien una de las de la versión 140.    
 
Para usar las intercalaciones UTF- 8 disponibles en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] a fin de mejorar la búsqueda y la ordenación de algunos caracteres Unicode (solo en las intercalaciones de Windows), debe seleccionar las intercalaciones compatibles con la codificación UTF-8 (\_UTF8).
 
-   La marca UTF8 se puede aplicar a:    
    -   Intercalaciones lingüísticas que ya son compatibles con caracteres adicionales (\_SC) o reconocimiento de la distinción de selector de variación (\_VSS)
    -   Intercalación binaria BIN2<sup>1</sup>
    
-   La marca UTF8 no se puede aplicar a:    
    -   Intercalaciones lingüísticas que no son compatibles con caracteres adicionales (\_SC) o reconocimiento de la distinción de selector de variación (\_VSS)
    -   Intercalaciones binarias BIN o BIN2<sup>2</sup>
    -   Intercalaciones SQL\_*  
    
<sup>1</sup> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 3.0 reemplazó la intercalación **UTF8_BIN2** por **Latin1_General_100_BIN2_UTF8**.        
<sup>2</sup> Hasta con [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3.    
    
Para evaluar completamente los problemas relacionados con el uso de tipos de datos Unicode y no Unicode, pruebe su escenario para cuantificar las diferencias de rendimiento en su entorno. Se recomienda normalizar la intercalación que se usa en los sistemas de una organización e implementar servidores y clientes Unicode siempre que sea posible.    
    
En muchos casos, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interactúa con otros servidores o clientes, y es posible que la organización use varios estándares de acceso a datos entre las aplicaciones y las instancias de servidor. Los clientes[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son uno de los dos tipos principales:    
    
-   **Clientes Unicode** que usan OLE DB y Conectividad abierta de bases de datos (ODBC) versión 3.7 o posterior.    
-   **Clientes no Unicode** que usan DB-Library y ODBC versión 3.6 o anterior.    
    
En la tabla siguiente se proporciona información sobre cómo usar datos multilingües con varias combinaciones de servidores Unicode y no Unicode:    
    
|Server|Remoto|Beneficios o limitaciones|    
|------------|------------|-----------------------------|    
|Unicode|Unicode|Dado que los datos Unicode se usan en todo el sistema, este escenario proporciona el máximo rendimiento y protección frente a daños de los datos recuperados. Se trata de la situación con Objetos de datos ActiveX (ADO), OLE DB y ODBC versión 3.7 o posterior.|    
|Unicode|No Unicode|En este escenario y especialmente con las conexiones entre un servidor que ejecuta un sistema operativo más reciente y un cliente que ejecuta una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o bien un sistema operativo anterior, puede haber limitaciones o producirse errores al mover los datos a un equipo cliente. Los datos Unicode del servidor intentan asignarse a una página de códigos correspondiente en el cliente no Unicode para convertir los datos.|    
|No Unicode|Unicode|No es una configuración idónea para usar datos multilingües. No puede escribir datos Unicode en el servidor no Unicode. Es probable que se produzcan problemas si los datos se envían a servidores externos a la página de códigos del servidor.|    
|No Unicode|No Unicode|Se trata de un escenario muy limitado para datos multilingües. Puede usar solo una única página de códigos.|    
    
##  <a name="supplementary-characters"></a><a name="Supplementary_Characters"></a> Caracteres adicionales    
Unicode Consortium asigna a cada carácter un punto de código único, que es un valor en el intervalo comprendido entre 000000 y 10FFFF. Los caracteres usados con más frecuencia tienen valores de punto de código en el intervalo comprendido entre 000000 y 00FFFF (65 535 caracteres) que se ajustan a una palabra de 8 o 16 bits en memoria y en disco. Este intervalo normalmente se designa como el plano multilingüe básico (BMP). 

Pero Unicode Consortium estableció 16 "planos" adicionales de caracteres, cada uno de ellos del mismo tamaño que el BMP. Esta definición ofrece a Unicode la posibilidad de representar 1 114 112 caracteres Unicode (es decir, 2<sup>16</sup> * 17 caracteres) dentro del intervalo de puntos de código comprendido entre 000000 y 10FFFF. Los caracteres con valores de punto de código mayores que 00FFFF requieren entre dos y cuatro palabras consecutivas de 8 bits (UTF-8) o dos palabras consecutivas de 16 bits (UTF-16). Estos caracteres ubicados más allá del BMP se denominan *caracteres adicionales* y las dos palabras consecutivas adicionales de 8 o 16 bits se denominan *pares suplentes*. Para más información sobre los caracteres adicionales, los suplentes y los pares suplentes, consulte [el estándar Unicode](http://www.unicode.org/standard/standard.html).    

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona tipos de datos como **nchar** y **nvarchar** para almacenar datos Unicode en el intervalo BMP (de 000000 a 00FFFF), que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] codifica mediante UCS-2. 

[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] introdujo una nueva familia de intercalaciones de caracteres adicionales (\_SC) que se pueden usar con los tipos de datos **nchar**, **nvarchar** y **sql_variant** para representar el intervalo de caracteres Unicode completo (de 000000 a 10FFFF). Por ejemplo: **Latin1_General_100_CI_AS_SC** o, si se usa una intercalación japonesa, **Japanese_Bushu_Kakusu_100_CI_AS_SC**. 
 
[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] amplía la compatibilidad de los caracteres adicionales a los tipos de datos **char** y **varchar** con las nuevas intercalaciones habilitadas para UTF-8 ([\_UTF8](#utf8)). Estos tipos de datos también son capaces de representar el intervalo completo de caracteres Unicode.   

> [!NOTE]
> A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], las nuevas intercalaciones \_140 admiten de forma automática caracteres adicionales.

Si utiliza caracteres adicionales:    
    
-   Los caracteres adicionales se pueden utilizar en las operaciones de ordenación y comparación en las versiones de intercalación 90 o mayores.    
-   Todas las intercalaciones de la versión 100 admiten la ordenación lingüística con caracteres adicionales.    
-   Los caracteres adicionales no son compatibles con metadatos, como en los nombres de objetos de base de datos.    
-   La marca SC se puede aplicar a:    
    -   Intercalaciones de la versión 90    
    -   Intercalaciones de la versión 100    
    
-   La marca SC no se puede aplicar a:    
    -   Intercalaciones de Windows sin versión de la versión 80    
    -   Intercalaciones binarias BIN o BIN2    
    -   Intercalaciones SQL\*    
    -   Intercalaciones de la versión 140 (no necesitan la marca SC, dado que ya admiten caracteres adicionales)    
    
En la siguiente tabla se compara el comportamiento de algunas funciones de cadena y algunos operadores de cadena cuando usan caracteres adicionales con y sin intercalación de caracteres adicionales (SCA):    
    
|Función u operador de cadena|Con una intercalación SCA|Sin una intercalación SCA|    
|---------------------------------|--------------------------|-----------------------------|    
|[CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)<br /><br /> [LEN](../../t-sql/functions/len-transact-sql.md)<br /><br /> [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)|El par suplente de UTF-16 se cuenta como un solo punto de código.|El par suplente de UTF-16 se cuenta como dos puntos de código.|    
|[LEFT](../../t-sql/functions/left-transact-sql.md)<br /><br /> [REPLACE](../../t-sql/functions/replace-transact-sql.md)<br /><br /> [REVERSE](../../t-sql/functions/reverse-transact-sql.md)<br /><br /> [RIGHT](../../t-sql/functions/right-transact-sql.md)<br /><br /> [SUBSTRING](../../t-sql/functions/substring-transact-sql.md)<br /><br /> [STUFF](../../t-sql/functions/stuff-transact-sql.md)|Estas funciones tratan los pares suplentes como un solo punto de código y funcionan de la forma esperada.|Es posible que estas funciones dividan cualquier par suplente y provoquen resultados inesperados.|    
|[NCHAR](../../t-sql/functions/nchar-transact-sql.md)|Devuelve el carácter correspondiente al valor del punto de código Unicode especificado en el intervalo comprendido entre 0 y 0x10FFFF. Si el valor especificado está en el intervalo comprendido entre 0 y 0xFFFF, se devuelve un carácter. Con valores más altos, se devuelve el suplente correspondiente.|Un valor mayor que 0xFFFF devuelve NULL en vez del suplente correspondiente.|    
|[UNICODE](../../t-sql/functions/unicode-transact-sql.md)|Devuelve un punto de código UTF-16 en el intervalo comprendido entre 0 y 0x10FFFF.|Devuelve un punto de código UCS-2 en el intervalo comprendido entre 0 y 0xFFFF.|    
|[Hacer coincidir un carácter comodín](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)<br /><br /> [Carácter comodín - caracteres no coincidentes](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)|Se admiten caracteres adicionales para todas las operaciones de caracteres comodín.|No se admiten caracteres adicionales para estas operaciones de caracteres comodín. Se admiten otros operadores de caracteres comodín.|    
    
## <a name="gb18030-support"></a><a name="GB18030"></a> Compatibilidad con GB18030    
GB18030 es un estándar independiente que se usa en la República Popular China para codificar caracteres chinos. En GB18030, los caracteres pueden tener una longitud de 1, 2 o 4 bytes. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite caracteres de codificación GB18030, reconociéndolos en el momento de su entrada en un servidor procedentes de una aplicación del lado cliente y convirtiéndolos y almacenándolos de forma nativa como caracteres Unicode. Una vez almacenados en el servidor, se tratan como caracteres Unicode en las operaciones siguientes. 

Puede usar cualquier intercalación china, preferentemente la más reciente: la versión 100. Todas las intercalaciones de nivel \_100 admiten la ordenación lingüística con caracteres GB18030. Si los datos incluyen caracteres adicionales (pares suplentes), puede usar las intercalaciones SC disponibles en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] para mejorar la búsqueda y la ordenación.    

> [!NOTE]
> Asegúrese de que las herramientas de cliente, como [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], usan la fuente Dengxian para mostrar correctamente las cadenas que contienen caracteres con codificación GB18030.
    
## <a name="complex-script-support"></a><a name="Complex_script"></a> Compatibilidad con escritura compleja    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede admitir la entrada, el almacenamiento, el cambio, y la visualización de escrituras complejas. Entre los ejemplos de escritura compleja se encuentran los siguientes tipos:    
    
-   Escritura que incluye la combinación de texto de derecha a izquierda y de izquierda a derecha, caso de una combinación de textos en árabe e inglés.    
-   Escritura cuyos caracteres cambian de forma dependiendo de su posición, o al combinarse con otros caracteres como, por ejemplo, los caracteres del árabe, el índico y el tailandés.    
-   Idiomas como el tailandés que necesitan diccionarios internos para reconocer palabras porque no existen cortes entre ellas.    
    
Las aplicaciones de base de datos que interactúan con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deben utilizar controles que sean compatibles con escritura compleja. Los controles de formulario estándar de Windows creados en código administrado están habilitados para escritura compleja.    

## <a name="japanese-collations-added-in--sssql17-md"></a><a name="Japanese_Collations"></a> Intercalaciones japonesas agregadas en [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]
 
A partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], se admiten nuevas familias de intercalaciones japonesas, con las permutaciones de varias opciones (\_CS, \_AS, \_KS, \_WS y \_VSS). 

Para escuchar en estas intercalaciones, puede consultar [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]:      

```sql 
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name LIKE 'Japanese_Bushu_Kakusu_140%' OR Name LIKE 'Japanese_XJIS_140%'
``` 

Todas las nuevas intercalaciones tienen compatibilidad integrada con los caracteres adicionales, por lo que ninguna de las nuevas intercalaciones **\_140** tienen (o necesitan) la marca SC.

Estas intercalaciones se admiten en los índices, las tablas optimizadas para memoria, los índices de almacén de columnas y los módulos compilados de forma nativa de [!INCLUDE[ssde_md](../../includes/ssde_md.md)].

<a name="ctp23"></a>

## <a name="utf-8-support"></a><a name="utf8"></a> Compatibilidad con UTF-8
[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] presenta compatibilidad total con la codificación de caracteres UTF-8 ampliamente utilizada como codificación de importación o exportación, y como intercalación de columna o base de datos para los datos de cadena. UTF-8 se permite en los tipos de datos **char** y **varchar**, y se habilita al crear o cambiar la intercalación de un objeto a una intercalación con un sufijo *UTF8*. Un ejemplo es cambiar **LATIN1_GENERAL_100_CI_AS_SC** a **LATIN1_GENERAL_100_CI_AS_SC_UTF8**. 

UTF-8 solo está disponible para las intercalaciones de Windows que admiten caracteres adicionales, como se presentó en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Los tipos de datos **nchar** y **nvarchar** solo permiten la codificación UCS-2 o UTF-16, y permanecen sin cambios.

### <a name="storage-differences-between-utf-8-and-utf-16"></a><a name="storage_differences"></a> Diferencias de almacenamiento entre UTF-8 y UTF-16
Unicode Consortium asigna a cada carácter un punto de código único, que es un valor en el intervalo comprendido entre 000000 y 10FFFF. Con [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], ambas codificaciones, UTF-8 y UTF-16, están disponibles para representar el intervalo completo:    
-  Con la codificación UTF-8, los caracteres del intervalo ASCII (entre 000000 y 00007F) requieren 1 byte, los puntos de código entre 000080 y 0007FF requieren 2 bytes, los puntos de código entre 000800 y 00FFFF requieren 3 bytes y los puntos de código entre 0010000 y 0010FFFF requieren 4 bytes. 
-  Con la codificación UTF-16, los puntos de código entre 000000 y 00FFFF requieren 2 bytes y los puntos de código entre 0010000 y 0010FFFF requieren 4 bytes. 

En la tabla siguiente se describen los bytes de almacenamiento de la codificación para cada intervalo de caracteres y tipo de codificación:

|Intervalo de códigos (hexadecimal)|Intervalo de códigos (decimal)|Bytes de almacenamiento <sup>1</sup> con UTF-8|Bytes de almacenamiento <sup>1</sup> con UTF-16|    
|---------------------------------|---------------------------------|--------------------------|-----------------------------|   
|000000–00007F|0–127|1|2|
|000080–00009F<br />0000A0–0003FF<br />000400–0007FF|128–159<br />160–1.023<br />1\.024–2.047|2|2|
|000800–003FFF<br />004000–00FFFF|2\.048–16.383<br />16.384–65.535|3|2|
|010000–03FFFF<sup>2</sup><br /><br />040000–10FFFF<sup>2</sup>|65.536–262.143<sup>2</sup><br /><br />262.144–1.114.111<sup>2</sup>|4|4|

<sup>1</sup> Los *bytes de almacenamiento* hacen referencia a la longitud de bytes codificados, no al tamaño en disco del tipo de datos. Para obtener más información acerca de los tamaños de almacenamiento en disco, consulte [nchar y nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) y [char y varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md).

<sup>2</sup> El intervalo de puntos de código para [caracteres adicionales](#Supplementary_Characters).

> [!TIP]   
> Es habitual pensar que en [CHAR(*n*) y VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), o en [NCHAR(*n*) y NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), la *n* define el número de caracteres. Esto se debe a que en el ejemplo de una columna CHAR(10), se pueden almacenar 10 caracteres ASCII en el intervalo 0-127 mediante una intercalación como **Latin1_General_100_CI_AI**, porque cada carácter de este intervalo solo usa 1 byte.
>    
> Pero en [CHAR(*n*) y VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), *n* define el tamaño de la cadena en *bytes* (0-8.000), mientras que en [NCHAR(*n*) y NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)*n* define el tamaño de la cadena en *pares de bytes* (0-4.000). *n* nunca define números de caracteres que se pueden almacenar.

Como acaba de ver, elegir la codificación Unicode y el tipo de datos adecuado puede proporcionar ahorros significativos de almacenamiento o aumentar la superficie de memoria, según el juego de caracteres en uso. Por ejemplo, al usar una intercalación Latina habilitada para UTF-8, como **Latin1_General_100_CI_AI_SC_UTF8**, una columna `CHAR(10)` almacena 10 bytes y puede contener 10 caracteres ASCII en el intervalo 0-127. Pero solo puede contener 5 caracteres en el intervalo 128-2047 y 3 caracteres en el intervalo 2048-65535. Por comparación, como una columna `NCHAR(10)` almacena 10 pares de bytes (20 bytes), puede contener 10 caracteres en el intervalo 0-65535.  

Antes de decidir si usar la codificación UTF-8 o UTF-16 para una base de datos o una columna, tenga en cuenta la distribución de datos de cadena que se almacenarán:
-  Si se encuentra principalmente en el intervalo ASCII 0-127 (por ejemplo, inglés), cada carácter requiere 1 byte con UTF-8 y 2 bytes con UTF-16. El uso de UTF-8 ofrece ventajas de almacenamiento. Si se cambia un tipo de datos de columna existente con caracteres ASCII en el intervalo 0-127 de `NCHAR(10)` a `CHAR(10)` mediante una intercalación habilitada para UTF-8, esto se traduce en una reducción del 50 % de los requisitos de almacenamiento. Esta reducción se debe a que `NCHAR(10)` requiere 20 bytes para el almacenamiento, mientras que `CHAR(10)` necesita 10 bytes para representar la misma cadena Unicode.    
-  Por encima del intervalo ASCII, casi todos los scripts basados en el alfabeto latino y también griego, cirílico, copto, armenio, hebreo, árabe, sitio, Tāna y n'ko requerirán 2 bytes por carácter tanto en UTF-8 como en UTF-16. En estos casos no existen diferencias de almacenamiento importantes con tipos de datos comparables (por ejemplo, entre el uso de **char** o **nchar**).
-  Si los scripts son principalmente de idiomas de Este de Asia (por ejemplo, coreano, chino y japonés), cada carácter requiere 3 bytes con UTF-8 y 2 bytes con UTF-16. El uso de UTF-16 ofrece ventajas de almacenamiento. 
-  Los caracteres del intervalo comprendido entre 010000 y 10FFFF requieren 4 bytes en UTF-8 y UTF-16. En estos casos, no existen diferencias en el almacenamiento con tipos de datos comparables (por ejemplo, entre usar **char** o **nchar**).

Para otras consideraciones, consulte [Escribir instrucciones Transact-SQL internacionales](../../relational-databases/collations/write-international-transact-sql-statements.md).

### <a name="converting-to-utf-8"></a><a name="converting"></a> Conversión a UTF-8
Dado que en [CHAR(*n*) y VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), o en [NCHAR(*n*) y NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), *n* define el tamaño de almacenamiento en bytes y no el número de caracteres permitidos, es importante determinar el tamaño del tipo de datos al que realizar la conversión para evitar que los datos se trunquen. 

Por ejemplo, considere una columna definida como **NVARCHAR(100)** que almacena 180 bytes de caracteres japoneses. En este ejemplo, los datos de la columna están codificados actualmente mediante UCS-2 o UTF-16, que utiliza 2 bytes por carácter. Para evitar el truncamiento de datos no basta con convertir el tipo de columna en **VARCHAR(200)** , ya que el nuevo tipo de datos solo puede almacenar 200 bytes, pero los caracteres japoneses requieren 3 bytes cuando están codificados en UTF-8. Por lo que, para evitar la pérdida de datos a través del truncamiento de datos, la columna debe definirse como **VARCHAR(270)** .

Por lo tanto, es necesario saber de antemano cuál es el tamaño de bytes previsto para la definición de columna antes de convertir los datos existentes a UTF-8, y ajustar el nuevo tamaño del tipo de datos como corresponda. Consulte el script de [!INCLUDE[tsql](../../includes/tsql-md.md)] o el bloc de notas de SQL en el [GitHub de ejemplos de datos](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/unicode), que usan la función [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) y la instrucción [COLLATE](../../t-sql/statements/collations.md) para determinar los requisitos de longitud de datos correctos para las operaciones de conversión UTF-8 en una base de datos existente.

Para cambiar la intercalación de columna y el tipo de datos en una tabla existente, use uno de los métodos descritos en [Establecer o cambiar la intercalación de columnas](../../relational-databases/collations/set-or-change-the-column-collation.md).

Para cambiar la intercalación de bases de datos y permitir que los nuevos objetos hereden la intercalación de las bases de datos de forma predeterminada, o para cambiar la intercalación del servidor y permitir que las bases de datos nuevas hereden la intercalación del sistema de forma predeterminada, consulte la sección [Tareas relacionadas](#Related_Tasks) de este artículo. 

##  <a name="related-tasks"></a><a name="Related_Tasks"></a> Related tasks    
    
|Tarea|Tema|    
|----------|-----------|    
|Describe cómo establecer o cambiar la intercalación de la instancia de SQL Server. Fíjese que, al cambiar la intercalación de servidor, no se cambia la de las bases de datos existentes.|[Configurar o cambiar la intercalación del servidor](../../relational-databases/collations/set-or-change-the-server-collation.md)|    
|Describe cómo establecer o cambiar la intercalación de una base de datos de usuario. Fíjese que, al cambiar la intercalación de una base de datos, no se cambia la de las columnas de tabla existentes.|[Establecer o cambiar la intercalación de base de datos](../../relational-databases/collations/set-or-change-the-database-collation.md)|    
|Se describe cómo establecer o cambiar la intercalación de una columna de la base de datos.|[Establecer o cambiar la intercalación de columnas](../../relational-databases/collations/set-or-change-the-column-collation.md)|    
|Se describe cómo devolver información de intercalación en el nivel de servidor, base de datos o columna.|[Ver información de intercalación](../../relational-databases/collations/view-collation-information.md)|    
|Se describe cómo escribir instrucciones Transact-SQL que sean más portátiles de un idioma a otro, o bien que admitan varios idiomas más fácilmente.|[Escribir instrucciones Transact-SQL internacionales](../../relational-databases/collations/write-international-transact-sql-statements.md)|    
|Se describe cómo cambiar el idioma de los mensajes de error y las preferencias sobre cómo usar y mostrar los datos de fecha, hora y moneda.|[Establecer un idioma de la sesión](../../relational-databases/collations/set-a-session-language.md)|    
    
##  <a name="related-content"></a><a name="Related_Content"></a> Related content    
Para más información, vea el contenido relacionado siguiente:
* [Prácticas recomendadas para cambiar las intercalaciones en SQL Server](https://go.microsoft.com/fwlink/?LinkId=113891)  
* [Uso del formato de caracteres Unicode para importar o exportar datos (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
* [Escribir instrucciones Transact-SQL internacionales](../../relational-databases/collations/write-international-transact-sql-statements.md)  
* [Migración de los procedimientos recomendados de SQL Server a Unicode](https://go.microsoft.com/fwlink/?LinkId=113890) (ya no se mantiene)   
* [Unicode Consortium](https://go.microsoft.com/fwlink/?LinkId=48619)  
* [Unicode estándar](http://www.unicode.org/standard/standard.html)  
* [Compatibilidad de UTF-8 con el controlador OLE DB para SQL Server](../../connect/oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
* [Nombre de intercalación de SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)  
* [Nombre de intercalación de Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)  
* [Introducing UTF-8 support for SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/Introducing-UTF-8-support-for-SQL-Server/ba-p/734928) (Presentación de la compatibilidad de UTF-8 con SQL Server)      
* [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md)      
* [Prioridad de intercalación](../../t-sql/statements/collation-precedence-transact-sql.md)    
    
## <a name="see-also"></a>Consulte también    
[Intercalaciones de bases de datos independientes](../../relational-databases/databases/contained-database-collations.md)     
[Elegir un idioma al crear un índice de texto completo](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)     
[sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)       
[Juegos de caracteres de un solo byte y de varios bytes](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)
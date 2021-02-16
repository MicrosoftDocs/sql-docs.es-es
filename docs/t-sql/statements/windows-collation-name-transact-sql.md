---
description: Nombre de intercalación de Windows (Transact-SQL)
title: Nombre de intercalación de Windows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Windows collations [SQL Server]
- names [SQL Server], collations
- collations [SQL Server], Windows collations
- Collation Designator
ms.assetid: acceef84-2c68-46e2-a021-be019b7ab14e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2fa047e5ef018109352972f3ae139c6eff2c1b93
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100349955"
---
# <a name="windows-collation-name-transact-sql"></a>Nombre de intercalación de Windows (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Especifica el nombre de intercalación de Windows en la cláusula COLLATE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El nombre de intercalación de Windows se compone del designador de intercalación y los estilos de comparación.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
<Windows_collation_name> :: =
<CollationDesignator>_<ComparisonStyle>

<ComparisonStyle> :: =
{ <CaseSensitivity>_<AccentSensitivity> [ _<KanatypeSensitive> ] [ _<WidthSensitive> ] [ _<VariationSelectorSensitive> ] 
}
| { _UTF8 }
| { _BIN | _BIN2 }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*CollationDesignator*   
Especifica las reglas de intercalación base utilizadas por la intercalación de Windows. Las reglas de intercalación base abarcan lo siguiente:

- Las reglas de ordenación y comparación que se aplican cuando se especifica la ordenación del diccionario. Las reglas de ordenación se basan en el alfabeto o en el idioma.
- La página de códigos que se usa para almacenar datos **varchar**.

Ejemplos:

- Latin1\_General o francés: ambos usan la página de códigos 1252.
- Turco: utiliza la página de códigos 1254.

*CaseSensitivity*  
**CI** especifica que no se diferencia mayúsculas de minúsculas, **CS** especifica que se diferencia mayúsculas de minúsculas.

*AccentSensitivity*  
**AI** especifica que no se distinguen acentos, **AS** especifica que se distinguen acentos.

*KanatypeSensitive*  
Al omitir esta opción se especifica que no se distinguen tipos de kana, **KS** especifica que se distinguen tipos de kana.

*WidthSensitivity*  
Al omitir esta opción se especifica que no se distinguen tipos de ancho, **KS** especifica que se distinguen tipos de ancho.

*VariationSelectorSensitivity*  
- **Se aplica a**: A partir de [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 

- Al omitir esta opción se especifica que no se distingue el selector de variación, **VSS** especifica que se distingue el selector de variación.

**UTF8**  
- **Se aplica a**: A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]   

- Especifica la codificación UTF-8 que se va a usar para los tipos de datos válidos. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).

**BIN**  
Especifica el criterio de ordenación binario compatible con versiones anteriores que se va a utilizar.

**BIN2**  
Especifica el criterio de ordenación binario que utiliza la semántica de comparación de punto de código.

## <a name="remarks"></a>Observaciones
Según cuál sea la versión de la intercalación, puede que algunos puntos de código no tengan definida ninguna prioridad de ordenación y/o asignación de mayúsculas o minúsculas. Por ejemplo, compare el resultado de la función `LOWER` cuando se indica en ella el mismo carácter, pero con diferentes versiones de la misma intercalación:

```sql
SELECT NCHAR(504) COLLATE Latin1_General_CI_AS AS [Uppercase],
       NCHAR(505) COLLATE Latin1_General_CI_AS AS [Lowercase];
-- Ǹ    ǹ


SELECT LOWER(NCHAR(504) COLLATE Latin1_General_CI_AS) AS [Version80Collation],
       LOWER(NCHAR(504) COLLATE Latin1_General_100_CI_AS) AS [Version100Collation];
-- Ǹ    ǹ
```

La primera instrucción muestra formas tanto en mayúsculas como en minúsculas de este carácter en la intercalación más antigua (la intercalación no afecta a la disponibilidad de los caracteres cuando se trabaja con datos Unicode). Sin embargo, la segunda instrucción pone de manifiesto que se devuelve un carácter en mayúsculas cuando la intercalación es Latin1\_General\_CI\_AS, porque este punto de código no tiene definida una asignación de minúsculas en esa intercalación.

Cuando se trabaja con algunos idiomas, puede resultar crucial evitar intercalaciones antiguas. Por ejemplo, esto es así para telegu.

En algunos casos, las intercalaciones de Windows y de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden generar planes de consulta distintos para la misma consulta.

## <a name="examples"></a>Ejemplos

Estos son algunos ejemplos de nombres de intercalación de Windows:

- **Latin1\_General\_100\_CI\_AS**

  La intercalación utiliza las reglas de ordenación del diccionario Latin1 General y se asigna a la página de códigos 1252. Se trata de una versión de la intercalación \_100 que no distingue mayúsculas y minúsculas (CI) y distingue caracteres acentuados (AS).

- **Estonian\_CS\_AS**

  La intercalación usa las reglas de ordenación del diccionario Estonian y se asigna a la página de códigos 1257. Se trata de una versión de la intercalación \_80 (se deduce porque no hay un número de versión en el nombre) que distingue mayúsculas y minúsculas (CI) y los caracteres acentuados (AS).

- **Japanese\_Bushu\_Kakusu\_140\_BIN2**

  La intercalación usa reglas de ordenación de punto de código binario y se asigna a la página de códigos 932. Se trata de una versión de la intercalación \_140 y las reglas de ordenación del diccionario Japanese Bushu Kakusu se omiten.

## <a name="windows-collations"></a>Intercalaciones de Windows

Para enumerar las intercalaciones de Windows admitidas por la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute la consulta siguiente.

```sql
SELECT * FROM sys.fn_helpcollations() WHERE [name] NOT LIKE N'SQL%';
```

En la tabla siguiente se muestran todas las intercalaciones de Windows admitidas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

|Configuración regional de Windows|Versión de intercalación 100|Versión de intercalación 90|
|--------------------|---------------------------|--------------------------|
|Alsaciano (Francia)|Latin1_General_100_|No disponible|
|Amárico (Etiopía)|Latin1_General_100_|No disponible|
|Armenio (Armenia)|Cyrillic_General_100_|No disponible|
|Asamés (India)|Assamese_100_ <sup>1</sup>|No disponible|
|Bengalí (Bangladesh)|Bengali_100_<sup>1</sup>|No disponible|
|Baskir (Rusia)|Bashkir_100_|No disponible|
|Vasco (España)|Latin1_General_100_|No disponible|
|Bengali (India)|Bengali_100_<sup>1</sup>|No disponible|
|Bosnio (cirílico, Bosnia-Herzegovina)|Bosnian_Cyrillic_100_|No disponible|
|Bosnio (latino, Bosnia-Herzegovina)|Bosnian_Latin_100_|No disponible|
|Bretón (Francia)|Breton_100_|No disponible|
|Chinese (Macao SAR)|Chinese_Traditional_Pinyin_100_|No disponible|
|Chinese (Macao SAR)|Chinese_Traditional_Stroke_Order_100_|No disponible|
|Chinese (Singapore)|Chinese_Simplified_Stroke_Order_100_|No disponible|
|Corso (Francia)|Corsican_100_|No disponible|
|Croata (Bosnia y Herzegovina, latino)|Croatian_100_|No disponible|
|Dari (Afganistán)|Dari_100_|No disponible|
|Inglés (India)|Latin1_General_100_|No disponible|
|Inglés (Malasia)|Latin1_General_100_|No disponible|
|Inglés (Singapur)|Latin1_General_100_|No disponible|
|Filipino (Filipinas)|Latin1_General_100_|No disponible|
|Frisón (Países Bajos)|Frisian_100_|No disponible|
|Georgiano (Georgia)|Cyrillic_General_100_|No disponible|
|Groenlandés (Groenlandia)|Danish_Greenlandic_100_|No disponible|
|Gujarati (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Hausa (Nigeria, latino)|Latin1_General_100_|No disponible|
|Hindi (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Igbo (Nigeria)|Latin1_General_100_|No disponible|
|Inuktitut (Canadá, latino)|Latin1_General_100_|No disponible|
|Inuktitut (silábico, Canadá)|Latin1_General_100_|No disponible|
|Irlandés (Irlanda)|Latin1_General_100_|No disponible|
|Japonés (Japón XJIS)|Japanese_XJIS_100_|Japanese_90_, Japanese_|
|Japonés (Japón)|Japanese_Bushu_Kakusu_100_|No disponible|
|Canarés (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Jemer (Camboya)|Khmer_100_<sup>1</sup>|No disponible|
|Quiché (Guatemala)|Modern_Spanish_100_|No disponible|
|Kinyarwanda (Ruanda)|Latin1_General_100_|No disponible|
|Konkani (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Lao (R.D.P. de Laos)|Lao_100_<sup>1</sup>|No disponible|
|Bajo sorbio (Alemania)|Latin1_General_100_|No disponible|
|Luxemburgués (Luxemburgo)|Latin1_General_100_|No disponible|
|Malayalam (India)|Indic_General_100_<sup>1</sup>|No disponible|
|Maltés (Malta)|Maltese_100_|No disponible|
|Maorí (Nueva Zelanda)|Maori_100_|No disponible|
|Mapuche (Chile)|Mapudungan_100_|No disponible|
|Maratí (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Mohawk (Canadá)|Mohawk_100_|No disponible|
|Mongol (RPC)|Cyrillic_General_100_|No disponible|
|Nepalí (Nepal)|Nepali_100_<sup>1</sup>|No disponible|
|Noruego (Bokmål, Noruega)|Norwegian_100_|No disponible|
|Noruego (nynorsk, Noruega)|Norwegian_100_|No disponible|
|Occitano (Francia)|French_100_|No disponible|
|Odia (India)|Indic_General_100_<sup>1</sup>|No disponible|
|Pastún (Afganistán)|Pashto_100_<sup>1</sup>|No disponible|
|Persa (Irán)|Persian_100_|No disponible|
|Punjabí (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Quechua (Bolivia)|Latin1_General_100_|No disponible|
|Quechua (Ecuador)|Latin1_General_100_|No disponible|
|Quechua (Perú)|Latin1_General_100_|No disponible|
|Romanche (Suiza)|Romansh_100_|No disponible|
|Sami (inari, Finlandia)|Sami_Sweden_Finland_100_|No disponible|
|Sami (Lule, Noruega)|Sami_Norway_100_|No disponible|
|Sami (lule, Suecia)|Sami_Sweden_Finland_100_|No disponible|
|Sami (septentrional, Finlandia)|Sami_Sweden_Finland_100_|No disponible|
|Sami (septentrional, Noruega)|Sami_Norway_100_|No disponible|
|Sami (septentrional, Suecia)|Sami_Sweden_Finland_100_|No disponible|
|Sami (skolt, Finlandia)|Sami_Sweden_Finland_100_|No disponible|
|Sami (meridional, Noruega)|Sami_Norway_100_|No disponible|
|Sami (meridional, Suecia)|Sami_Sweden_Finland_100_|No disponible|
|Sánscrito (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Serbio (cirílico, Bosnia-Herzegovina)|Serbian_Cyrillic_100_|No disponible|
|Serbio (latino, Bosnia-Herzegovina)|Serbian_Latin_100_|No disponible|
|Serbio (cirílico, Serbia)|Serbian_Cyrillic_100_|No disponible|
|Serbio (latino, Serbia)|Serbian_Latin_100_|No disponible|
|Sesotho sa Leboa/sotho septentrional (Sudáfrica)|Latin1_General_100_|No disponible|
|Setswana/tswana (Sudáfrica)|Latin1_General_100_|No disponible|
|Cingalés (Sri Lanka)|Indic_General_100_<sup>1</sup>|No disponible|
|Swahili (Kenia)|Latin1_General_100_|No disponible|
|Sirio (Siria)|Syriac_100_<sup>1</sup>|Syriac_90_|
|Tayiko (Tayikistán)|Cyrillic_General_100_|No disponible|
|Tamazight (latino, Argelia)|Tamazight_100_|No disponible|
|Tamil (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Telugu (India)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Tibetano (RPC)|Tibetan_100_<sup>1</sup>|No disponible|
|Turcomano (Turkmenistán)|Turkmen_100_|No disponible|
|Uigur (RPC)|Uighur_100_|No disponible|
|Alto sorbio (Alemania)|Upper_Sorbian_100_|No disponible|
|Urdú (Pakistán)|Urdu_100_|No disponible|
|Galés (Reino Unido)|Welsh_100_|No disponible|
|Wolof (Senegal)|French_100_|No disponible|
|Xhosa/isiXhosa (Sudáfrica)|Latin1_General_100_|No disponible|
|Sakha (Rusia)|Yakut_100_|No disponible|
|Yi (RPC)|Latin1_General_100_|No disponible|
|Yoruba (Nigeria)|Latin1_General_100_|No disponible|
|Zulú/isiZulu (Sudáfrica)|Latin1_General_100_|No disponible|
|Desusado, no disponible en el nivel de servidor en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versiones posteriores|Hindi|Hindi|
|Desusado, no disponible en el nivel de servidor en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versiones posteriores|Korean_Wansung_Unicode|Korean_Wansung_Unicode|
|Desusado, no disponible en el nivel de servidor en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versiones posteriores|Lithuanian_Classic|Lithuanian_Classic|
|Desusado, no disponible en el nivel de servidor en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versiones posteriores|Macedonio|Macedonio|
||||

<sup>1</sup> Las intercalaciones exclusivas de Unicode de Windows solo se pueden aplicar a datos de nivel de columna o de nivel de expresión. No se pueden utilizar como intercalaciones de base de datos o de servidor.

<sup>2</sup>Al igual que la intercalación Chino (Taiwán), Chino (Macao) usa las reglas de Chino simplificado; a diferencia de Chino (Taiwán), usa la página de códigos 950.

## <a name="see-also"></a>Consulte también

- [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Constantes](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [table](../../t-sql/data-types/table-transact-sql.md)
- [sys.fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)

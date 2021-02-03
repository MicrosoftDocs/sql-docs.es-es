---
description: 'Funciones de intercalación: COLLATIONPROPERTY (Transact-SQL)'
title: COLLATIONPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLLATIONPROPERTY_TSQL
- COLLATIONPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], properties
- COLLATIONPROPERTY function
ms.assetid: f5029e74-a1db-4f69-b0f5-5ee920c3311d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8425248d0517268573d789fb7eacfa44ef485dc6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211507"
---
# <a name="collation-functions---collationproperty-transact-sql"></a>Funciones de intercalación: COLLATIONPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve la propiedad solicitada de una intercalación especificada.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
COLLATIONPROPERTY( collation_name , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*collation_name*  
El nombre de la intercalación. El argumento *nombre_de_la_intercalación* tiene un tipo de datos **nvarchar (128)**, sin valor predeterminado.
  
*property*  
La propiedad de intercalación. El argumento *propiedad* tiene un tipo de datos **varchar (128)** y puede tener uno de los valores siguientes:
  
|Nombre de propiedad|Descripción|  
|---|---|
|**CodePage**|La página de códigos no Unicode de la intercalación. Se trata del juego de caracteres usado con los datos **varchar**. Vea [Appendix G DBCS/Unicode Mapping Tables](/previous-versions/cc194886(v=msdn.10)) (Apéndice G: tablas de asignaciones DBCS/Unicode) y [Appendix H Code Pages](/previous-versions/cc195051(v=msdn.10)) (Apéndice H: páginas de código) para traducir estos valores y ver sus asignaciones de caracteres.<br /><br />Tipo de datos base: **int**|  
|**LCID**|Identificador de configuración regional de Windows de la intercalación. Se trata de la referencia cultural usada en las reglas de ordenación y comparación. Vea [LCID Structure](/openspecs/windows_protocols/ms-lcid/63d3d639-7fd2-4afb-abbe-0d5b5551eef8) (Estructura de LCID) para traducir estos valores (primero hay que convertirlos a **varbinary**).<br /><br />Tipo de datos base: **int**|  
|**ComparisonStyle**|Estilo de comparación de Windows de la intercalación. Devuelve 0 en las intercalaciones binarias, tanto (\_BIN) como (\_BIN2), así como cuando todas las propiedades distinguen mayúsculas y minúsculas, como (\_CS\_AS\_KS\_WS), (\_CS\_AS\_KS\_WS\_SC) y (\_CS\_AS\_KS\_WS\_VSS). Valores de máscara de bits:<br /><br /> Omitir mayúsculas y minúsculas: 1<br /><br /> Omitir acento: 2<br /><br /> Omitir Kana: 65536<br /><br /> Omitir ancho: 131 072<br /><br /> Nota: La opción de distinción de selector de variación (\_VSS) no se representa en este valor, aunque afecta al comportamiento de las comparaciones.<br /><br />Tipo de datos base: **int**|  
|**Versión**|La versión de la intercalación. Devuelve un valor entre 0 y 3.<br /><br /> Las intercalaciones con "140" en el nombre devuelven 3.<br /><br /> Las intercalaciones con "100" en el nombre devuelven 2.<br /><br /> Las intercalaciones con "90" en el nombre devuelven 1.<br /><br /> Todas las demás intercalaciones devuelven 0.<br /><br />Tipo de datos base: **tinyint**|  
  
## <a name="return-types"></a>Tipos de valores devueltos
**sql_variant**
  
## <a name="examples"></a>Ejemplos  
  
```sql
SELECT COLLATIONPROPERTY('Traditional_Spanish_CS_AS_KS_WS', 'CodePage');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1252   
```  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
```sql
SELECT COLLATIONPROPERTY('Traditional_Spanish_CS_AS_KS_WS', 'CodePage')  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1252   
```  
  
## <a name="see-also"></a>Vea también
[sys.fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
  

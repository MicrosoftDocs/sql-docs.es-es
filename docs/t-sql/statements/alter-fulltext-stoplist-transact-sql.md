---
description: ALTER FULLTEXT STOPLIST (Transact-SQL)
title: ALTER FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER FULLTEXT STOPLIST
- ALTER_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- ALTER FULLTEXT STOPLIST statement
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: f6ad87d5-6a34-435a-8456-8244947c5c83
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 969de54c394e5ec68b2f8acd6a092f8c3b5ab1f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204204"
---
# <a name="alter-fulltext-stoplist-transact-sql"></a>ALTER FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Inserta o elimina una palabra irrelevante en la lista de palabras irrelevantes de texto completo predeterminada de la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER FULLTEXT STOPLIST stoplist_name  
{   
        ADD [N] 'stopword' LANGUAGE language_term    
  | DROP   
    {  
        'stopword' LANGUAGE language_term   
      | ALL LANGUAGE language_term   
      | ALL  
     }  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *stoplist_name*  
 Es el nombre de la lista de palabras irrelevantes que se está modificando. *stoplist_name* puede tener un máximo de 128 caracteres.  
  
 **'** *stopword* **'**  
 Es una cadena que podría ser una palabra con significado lingüístico en el idioma especificado o un token que no tiene un significado lingüístico. *stopword* tiene como límite la longitud máxima del token (64 caracteres). Una palabra irrelevante se puede especificar en forma de cadena Unicode.  
  
 LANGUAGE *language_term*  
 Especifica el idioma que se va a asociar al parámetro *stopword* que se va a agregar o quitar.  
  
 *language_term* se puede especificar como una cadena, un entero o un valor hexadecimal correspondiente al identificador de configuración regional (LCID) de un idioma, tal y como se muestra aquí:  
  
|Formato|Descripción|  
|------------|-----------------|  
|String|*language_term* corresponde al valor de columna **alias** en la vista de compatibilidad [sys.syslanguages (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). La cadena debe estar delimitada con comillas sencillas, como en **"** _language_term_*_"_*.|  
|Entero|*language_term* es la configuración regional (LCID) del idioma.|  
|Hexadecimal|*language_term* es 0x seguido del valor hexadecimal de LCID. El valor hexadecimal no puede superar los ocho dígitos, incluidos los ceros a la izquierda. Si el valor está en formato de juego de caracteres de doble byte (DBCS), SQL Server lo convertirá a Unicode.|  
  
 ADD **"** _stopword_*_"_* LANGUAGE *language_term*  
 Agrega una palabra irrelevante a una lista de palabras irrelevantes del idioma especificado por LANGUAGE *language_term*.  
  
 Si la combinación especificada de la palabra clave y del valor LCID del idioma no es única en la lista de palabras irrelevantes, se devuelve un error.  Si el valor LCID no corresponde a un idioma registrado, se genera un error.  
  
 DROP { **"** _stopword_*_"_* LANGUAGE *language_term* | ALL LANGUAGE *language_term* | ALL }  
 Quita una palabra de la lista de palabras irrelevantes.  
  
 **'** *stopword* **'** LANGUAGE *language_term*  
 Quita la palabra irrelevante especificada del idioma especificado por *language_term*.  
  
 ALL LANGUAGE *language_term*  
 Quita todas las palabras irrelevantes del idioma especificado por *language_term*.  
  
 ALL  
 Quita todas las palabras irrelevantes de la lista de palabras irrelevantes.  
  
## <a name="remarks"></a>Comentarios  
 CREATE FULLTEXT STOPLIST solo se admite para el nivel de compatibilidad 100 y posterior. Para los niveles de compatibilidad 80 y 90, la lista de palabras irrelevantes del sistema siempre se asigna a la base de datos.  
  
## <a name="permissions"></a>Permisos  
 Para designar una lista de palabras irrelevantes como la lista predeterminada de la base de datos, se requiere el permiso ALTER DATABASE. Para modificar la lista de palabras irrelevantes de cualquier otra forma, se requiere ser el propietario de la lista de palabras irrelevantes o pertenecer a los roles fijos de base de datos **db_owner** o **db_ddladmin**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se modifica la lista de palabras irrelevantes `CombinedFunctionWordList`, agregando la palabra 'en', primero para español y, a continuación, para francés.  
  
```sql  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'Spanish';  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'French';  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [Configurar y administrar palabras irrelevantes y listas de palabras irrelevantes para la búsqueda de texto completo](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [Configurar y administrar palabras irrelevantes y listas de palabras irrelevantes para la búsqueda de texto completo](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  

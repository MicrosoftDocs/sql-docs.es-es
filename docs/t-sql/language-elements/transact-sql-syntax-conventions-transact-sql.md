---
description: Convenciones de sintaxis de Transact-SQL (Transact-SQL)
title: Convenciones de sintaxis de Transact-SQL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql13.TSQLExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- conventions [SQL Server]
- Applies to section in Transact-SQL topics
- code example conventions [SQL Server]
- objects [SQL Server], names
- code [SQL Server], conventions
- multipart names [SQL Server]
- Transact-SQL syntax conventions
- syntax conventions [SQL Server]
- code [SQL Server]
- Transact-SQL
- naming conventions [SQL Server]
- syntax [SQL Server], Transact-SQL
ms.assetid: 35fbcf7f-8b55-46cd-a957-9b8c7b311241
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed6fee3cc0fae70b0258bc427322a66a32f39eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340630"
---
# <a name="transact-sql-syntax-conventions-transact-sql"></a>Convenciones de sintaxis de Transact-SQL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En la siguiente tabla se incluyen y describen las convenciones utilizadas en los diagramas de sintaxis de la referencia de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
|Convención|Se usa para|  
|----------------|--------------|  
|MAYÚSCULAS|Palabras clave de [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|_cursiva_|Parámetros proporcionados por el usuario para la sintaxis de [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|**Negrita**|Escriba los nombres de bases de datos, tablas, columnas e índices, procedimientos almacenados, utilidades, nombres de tipos de datos y texto exactamente como se muestra.|  
|subrayado|Indica el valor predeterminado que se aplica cuando la cláusula que contiene el valor subrayado se omite en la instrucción.|  
|&#124; (barra vertical)|Separa los elementos de sintaxis escritos entre corchetes o llaves. Solo puede utilizar uno de los elementos.|  
|`[ ]` (corchetes)|Elementos opcionales de sintaxis. No escriba los corchetes.|  
|{} (llaves)|Elementos obligatorios de sintaxis. No escriba las llaves.|  
|[ **,** …_n_]|Indica que el elemento anterior puede repetirse _n_ veces. Los elementos se separan por comas.|  
|[..._n_]|Indica que el elemento anterior puede repetirse _n_ veces. Cada repetición se separa del siguiente con un espacio en blanco.|  
|;|Terminador de instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Aunque el punto y coma no es necesario en la mayoría de las instrucciones de esta versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se exigirá en una versión futura.|  
|\<label> ::=|Nombre de un bloque de sintaxis. Use esta convención para agrupar y etiquetar secciones de sintaxis extensas o una unidad de sintaxis que puede usar en varias ubicaciones dentro de una instrucción. Cada ubicación en la que se podría usar el bloque de sintaxis se indica con la etiqueta entre comillas angulares: \<label>.<br /><br /> Un conjunto es una colección de expresiones, por ejemplo, \<grouping set>; una lista es una colección de conjuntos, por ejemplo, \<composite element list>.|  
  
## <a name="multipart-names"></a>Nombres de varias partes  
A menos que se especifique lo contrario, todas las referencias de [!INCLUDE[tsql](../../includes/tsql-md.md)] al nombre de un objeto de base de datos pueden ser un nombre de cuatro partes con el formato siguiente:  
  
_server\_name_.[_database\_name_].[_schema\_name_]._object\_name_  
  
| _database\_name_.[_schema\_name_]._object\_name_  
 
| _schema\_name_._object\_name_  
  
| _object\_name_  
  
_server\_name_  
Especifica un nombre de servidor vinculado o un nombre de servidor remoto.  
  
_database\_name_  
Especifica el nombre de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si el objeto reside en una instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cuando el objeto está en un servidor vinculado, *database_name* especifica un catálogo de OLE DB.  
  
_schema\_name_  
Especifica el nombre del esquema que contiene el objeto si dicho objeto se encuentra en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el objeto se encuentra en un servidor vinculado, *schema_name* especifica un nombre de esquema OLE DB.  
  
_object\_name_  
Hace referencia al nombre del objeto.  
  
Cuando se hace referencia a un objeto específico, no siempre hay que especificar el servidor, la base de datos y el esquema del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] para identificar el objeto. No obstante, si no se encuentra el objeto, se muestra un error.  
  
> [!NOTE]  
> Para evitar los errores de resolución de nombres, se recomienda especificar el nombre de esquema siempre que se especifique un objeto del ámbito del esquema.  
  
Para omitir los nodos intermedios, utilice puntos para indicar estas posiciones. En la siguiente tabla se muestran los formatos válidos para los nombres de objetos.  
  
|Formato de referencia a objetos|Descripción|  
|-----------------------------|-----------------|  
|_server_._database_._schema_._object_|Nombre de cuatro partes.|  
|_server_._database_.._object_|Se omite el nombre del esquema.|  
|_server_.._schema_._object_|Se omite el nombre de la base de datos.|  
|_server_..._object_|Se omiten los nombres de la base de datos y el esquema.|  
|_database_._schema_._object_|Se omite el nombre del servidor.|  
|_database_.._object_|Se omiten los nombres del servidor y el esquema.|  
|_schema_._object_|Se omiten los nombres del servidor y la base de datos.|  
|_object_|Se omiten los nombres del servidor, la base de datos y el esquema.|  
  
## <a name="code-example-conventions"></a>Convenciones de los ejemplos de código  
A menos que se indique lo contrario, los ejemplos proporcionados en la referencia de [!INCLUDE[tsql](../../includes/tsql-md.md)] se han probado con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y los valores predeterminados para las siguientes opciones:  
  
-   ANSI_NULLS  
-   ANSI_NULL_DFLT_ON  
-   ANSI_PADDING  
-   ANSI_WARNINGS  
-   CONCAT_NULL_YIELDS_NULL  
-   QUOTED_IDENTIFIER  
  
La mayoría de los ejemplos de código de la referencia de [!INCLUDE[tsql](../../includes/tsql-md.md)] se han comprobado en servidores que se ejecutan en un orden que distingue entre mayúsculas y minúsculas. Normalmente, los servidores de prueba ejecutaban la página de códigos ANSI/ISO 1252.  
  
Muchos ejemplos de código agregan como prefijo a las constantes de cadenas de caracteres Unicode la letra **N**. Sin el prefijo **N**, la cadena se convierte a la página de códigos predeterminada de la base de datos. Esta página de códigos predeterminada puede no reconocer determinados caracteres.  
  
## <a name="applies-to-references"></a>Referencias de "Se aplica a"  
La referencia de [!INCLUDE[tsql](../../includes/tsql-md.md)] incluye temas relacionados con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)].   

Hay una sección en la parte superior de cada artículo que indica qué productos son compatibles con el tema del artículo. Si se omite un producto, la característica descrita en el artículo no estará disponible en ese producto. Por ejemplo, los grupos de disponibilidad se introdujeron en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. En el artículo **CREATE AVAILABILITY GROUP** se indica que se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores) porque no se aplica a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Se puede usar el tema general del artículo en un producto, pero no se admiten todos los argumentos en algunos casos. Por ejemplo, los usuarios de bases de datos independientes se introdujeron en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Use la instrucción **CREATE USER** en cualquier producto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; en cambio, la sintaxis **WITH PASSWORD** no se puede usar con versiones anteriores. Las secciones **Se aplica a** adicionales se insertan en las descripciones del argumento correspondiente en el cuerpo del artículo.  
  
## <a name="see-also"></a>Consulte también  
[Referencia de Transact-SQL &#40;motor de base de datos&#41;](../language-reference.md)    
[Palabras clave reservadas &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)      
[Problemas de diseño de Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd193411(v=vs.100))    
[Problemas de nomenclatura de Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd193246(v=vs.100))        
[Problemas de rendimiento de Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd172117(v=vs.100))
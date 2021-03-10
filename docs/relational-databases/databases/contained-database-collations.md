---
title: Intercalaciones de bases de datos independientes | Microsoft Docs
description: Obtenga información sobre el funcionamiento de la intercalación en bases de datos dependientes e independientes. Vea las incidencias que pueden surgir cuando se cruzan sesiones entre contextos dependientes e independientes.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database, collations
ms.assetid: 4b44f6b9-2359-452f-8bb1-5520f2528483
author: stevestein
ms.author: sstein
ms.openlocfilehash: 260c4afa90b28aa8efc22a15fbda13cfa7f35417
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532327"
---
# <a name="contained-database-collations"></a>Intercalaciones de bases de datos independientes
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Varias propiedades afectan a la semántica de igualdad y al criterio de ordenación de los datos de texto, como son la distinción entre mayúsculas y minúsculas y de los acentos, y el idioma básico que se usa. Estas cualidades se expresan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de la opción de intercalación de los datos. Para obtener una explicación más detallada de las intercalaciones, vea [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
 Las intercalaciones se aplican no solo a los datos almacenados en las tablas de usuario, sino a todo el texto que se administra en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como son los metadatos, los objetos temporales, los nombres de variables, etc. Su tratamiento difiere en las bases de datos independientes y en las dependientes. Este cambio no afectará a muchos usuarios, pero ayuda a proporcionar independencia de la instancia y uniformidad. Pero esto también puede generar confusión, así como problemas en las sesiones que acceden tanto a bases de datos independientes como a bases de datos dependientes.  
  
 Este tema clarifica el contenido del cambio y examina áreas en las que puede ocasionar problemas.  
  
## <a name="non-contained-databases"></a>Bases de datos dependientes  
 Todas las bases de datos tienen una intercalación predeterminada (que se puede establecer al crear o modificar una base de datos). Esta intercalación se usa para todos los metadatos de la base de datos, así como el valor predeterminado para todas las columnas de cadena dentro de la base de datos. Los usuarios pueden elegir una intercalación diferente para una columna en particular utilizando la cláusula **COLLATE** .  
  
### <a name="example-1"></a>Ejemplo 1  
 Por ejemplo, si estuviéramos trabajando en Beijing, podríamos utilizar una intercalación china:  
  
```sql  
ALTER DATABASE MyDB COLLATE Chinese_Simplified_Pinyin_100_CI_AS;  
```  
  
 Ahora, si creamos una columna, su intercalación predeterminada será esta intercalación china, pero podemos elegir otra si lo deseamos:  
  
```sql  
CREATE TABLE MyTable  
      (mycolumn1 nvarchar,  
      mycolumn2 nvarchar COLLATE Frisian_100_CS_AS);  
GO  
SELECT name, collation_name  
FROM sys.columns  
WHERE name LIKE 'mycolumn%' ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```sql  
name            collation_name  
--------------- ----------------------------------  
mycolumn1       Chinese_Simplified_Pinyin_100_CI_AS  
mycolumn2       Frisian_100_CS_AS  
```  
  
 Esto parece relativamente sencillo, pero surgen varios problemas. Dado que la intercalación de una columna depende de la base de datos en la que se cree la tabla, surgen problemas con el uso de las tablas temporales almacenadas en **tempdb**. La intercalación de **tempdb** normalmente coincide con la de la instancia, que no tiene que coincidir con la intercalación de la base de datos.  
  
### <a name="example-2"></a>Ejemplo 2  
 Por ejemplo, considere la base de datos (chino) anterior cuando se usa en una instancia con una intercalación **Latin1_General** :  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max)) ;  
GO  
```  
  
 A primera vista, estas dos tablas parecen como si tuvieran el mismo esquema, pero dado que las intercalaciones de las bases de datos difieren, los valores son en realidad incompatibles:  
  
```  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Mensaje 468, Nivel 16, Estado 9, Línea 2  
  
 No puede resolver el conflicto de la intercalación entre "Latin1_General_100_CI_AS_KS_WS_SC" y Chinese_Simplified_Pinyin_100_CI_AS" en la operación igual que.  
  
 Podemos corregir esto intercalando la tabla temporal explícitamente. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] facilita esto proporcionando la palabra clave **DATABASE_DEFAULT** para la cláusula **COLLATE** .  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max) COLLATE DATABASE_DEFAULT);  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 Ahora esto se ejecuta sin error.  
  
 También podemos ver el comportamiento dependiente de la intercalación con variables. Considere la función siguiente:  
  
```  
CREATE FUNCTION f(@x INT) RETURNS INT  
AS BEGIN   
      DECLARE @I INT = 1  
      DECLARE @İ INT = 2  
      RETURN @x * @i  
END;  
```  
  
 Esta es una función bastante peculiar. En una intercalación con distinción entre mayúsculas y minúsculas, el @i de la cláusula de devolución no puede enlazar a @I o @İ. En una intercalación Latin1_General sin distinción entre mayúsculas y minúsculas, @i enlaza a @I y la función devuelve 1. Pero en una intercalación turca sin distinción entre mayúsculas y minúsculas, @i enlaza a@İ, y la función devuelve 2. Esto puede causar confusión en una base de datos que se mueva entre instancias con intercalaciones diferentes.  
  
## <a name="contained-databases"></a>Bases de datos independientes  
 Dado que uno de los objetivos de diseño de las bases de datos independientes es hacer que sean autodependientes, la dependencia de las intercalaciones de **tempdb** e instancia debe romperse. Para ello, las bases de datos independientes presentan el concepto de intercalación de catálogo. La intercalación de catálogo se utiliza para los objetos transitorios y los metadatos del sistema. Abajo se proporcionan los detalles.  
  
 En una base de datos independiente, la intercalación de catálogo es **Latin1_General_100_CI_AS_WS_KS_SC**. Esta intercalación es la misma para todas las bases de datos independientes en todas las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y no se puede cambiar.  
  
 La intercalación de la base de datos se conserva, pero solo se usa como intercalación predeterminada para los datos del usuario. De forma predeterminada, la intercalación de la base de datos es igual que la de la base de datos modelo, pero el usuario puede cambiarla a través de un comando **CREATE** o **ALTER DATABASE** igual que en las bases de datos dependientes.  
  
 Una nueva palabra clave, **CATALOG_DEFAULT**, está disponible en la cláusula **COLLATE** . Esto se utiliza como acceso directo a la intercalación actual de los metadatos tanto en las bases de datos independientes como en las dependientes. Es decir, en una base de datos dependiente, **CATALOG_DEFAULT** devolverá la intercalación de la base de datos actual, dado que los metadatos se intercalan en la intercalación de la base de datos. En una base de datos independiente, estos dos valores pueden ser diferentes, porque el usuario puede cambiar la intercalación de la base de datos para que no coincida con la del catálogo.  
  
 El comportamiento de varios objetos tanto en las bases de datos independientes como en las dependientes se resume en esta tabla:  
  
|Elemento|Base de datos dependiente|Base de datos independiente|  
|-|-|-|  
|Datos del usuario (predeterminado)|DATABASE_DEFAULT|DATABASE_DEFAULT|  
|Datos de Temp (predeterminado)|Intercalación de TempDB|DATABASE_DEFAULT|  
|Metadatos|DATABASE_DEFAULT / CATALOG_DEFAULT|CATALOG_DEFAULT|  
|Metadatos temporales|Intercalación de TempDB|CATALOG_DEFAULT|  
|variables|Intercalación de instancia|CATALOG_DEFAULT|  
|Etiquetas Goto|Intercalación de instancia|CATALOG_DEFAULT|  
|Nombres de cursor|Intercalación de instancia|CATALOG_DEFAULT|  
  
 En el ejemplo de la tabla temporal descrito previamente, podemos ver que este comportamiento de la intercalación elimina la necesidad de una cláusula **COLLATE** explícita en la mayor parte de los usos de las tablas temporales. En una base de datos independiente, este código se ejecuta ahora sin error, aun cuando las intercalaciones de instancia y base de datos difieren:  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max));  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 Esto funciona porque `T1_txt` y `T2_txt` se intercalan en la intercalación de base de datos de la base de datos independiente.  
  
## <a name="crossing-between-contained-and-uncontained-contexts"></a>Cruzar los contextos contenidos y no contenidos  
 Siempre que una sesión de una base de datos independiente sigue siendo contenida, debe permanecer dentro de la base de datos a la que se conectó. En este caso el comportamiento es muy sencillo. Pero si una sesión cruza de un contexto contenido a uno no contenido, el comportamiento se vuelve más complejo, porque los dos conjuntos de reglas deben unirse. Esto puede ocurrir en una base de datos parcialmente independiente, ya que un usuario puede usar **USE** en otra base de datos. El siguiente principio administra la diferencia en las reglas de intercalación en este caso.  
  
-   La base de datos en la que el lote comienza determina el comportamiento de la intercalación de un lote.  
  
 Observe que esta decisión se toma antes de que se ejecute ningún comando, incluso un **USE** inicial. Es decir, si un lote comienza en una base de datos independiente, pero el primer comando es **USE** para una base de datos dependiente, el comportamiento de la intercalación contenida se seguirá usando para el lote. Por tanto, una referencia a una variable, por ejemplo, puede tener varios resultados posibles:  
  
-   La referencia puede encontrar una coincidencia exactamente. En este caso, la referencia funcionará sin generar un error.  
  
-   La referencia puede no encontrar una coincidencia en la intercalación actual donde hubiera otra antes. Esto producirá un error que indica que la variable no existe, aunque en apariencia se creara.  
  
-   La referencia puede encontrar varias coincidencias que eran distintas originalmente. Esto también producirá un error.  
  
 Mostraremos esto con varios ejemplos. Para ello, suponemos que hay una base de datos independiente parcialmente denominada `MyCDB` con su intercalación de la base de datos establecida en la intercalación predeterminada, **Latin1_General_100_CI_AS_WS_KS_SC**. Se supone que la intercalación de la instancia es **Latin1_General_100_CS_AS_WS_KS_SC**. Las dos intercalaciones solo difieren en la distinción de mayúsculas y minúsculas.  
  
### <a name="example-1"></a>Ejemplo 1  
 El siguiente ejemplo ilustra el caso en el que la referencia encuentra exactamente una coincidencia.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #a VALUES(1);  
GO  
  
USE master;  
GO  
  
SELECT * FROM #a;  
GO  
  
Results:  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
x  
-----------  
1  
```  
  
 En este caso, el #a identificado enlaza en la intercalación del catálogo sin distinción entre mayúsculas y minúsculas y la intercalación de la instancia con distinción entre mayúsculas y minúsculas, y el código se ejecuta bien.  
  
### <a name="example-2"></a>Ejemplo 2  
 En el siguiente ejemplo se ilustra el caso en el que la referencia no encuentra una coincidencia en la intercalación actual donde antes había una.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #A VALUES(1);  
GO  
```  
  
 Aquí, #A enlaza a #a en la intercalación predeterminada sin distinción entre mayúsculas y minúsculas, y la inserción funciona,  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
```  
  
 Pero si continuamos el script...  
  
```  
USE master;  
GO  
  
SELECT * FROM #A;  
GO  
```  
  
 Obtenemos un error al intentar enlazar #A en la intercalación de la instancia con distinción entre mayúsculas y minúsculas;  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Mensaje 208, Nivel 16, Estado 0, Línea 2  
  
 El nombre de objeto '#A' no es válido.  
  
### <a name="example-3"></a>Ejemplo 3  
 El siguiente ejemplo ilustra el caso en el que la referencia encuentra varias coincidencias que eran distintas originalmente. Primero, comenzamos en **tempdb** (que tiene la misma intercalación con distinción entre mayúsculas y minúsculas que nuestra instancia) y ejecutamos las siguientes instrucciones.  
  
```  
USE tempdb;  
GO  
  
CREATE TABLE #a(x int);  
GO  
CREATE TABLE #A(x int);  
GO  
INSERT INTO #a VALUES(1);  
GO  
INSERT INTO #A VALUES(2);  
GO  
```  
  
 Se ejecuta correctamente, dado que las tablas son distintas en esta intercalación:  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
(1 row(s) affected)  
```  
  
 Sin embargo, si pasamos a nuestra base de datos independiente, encontramos que ya no podemos enlazar a estas tablas.  
  
```  
USE MyCDB;  
GO  
SELECT * FROM #a;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Mensaje 12800, Nivel 16, Estado 1, Línea 2  
  
 La referencia al nombre de la tabla temporal '#a' es ambiguo y no se puede resolver. Los posibles candidatos son '#a' y '#A'.  
  
## <a name="conclusion"></a>Conclusión  
 El comportamiento de la intercalación de bases de datos independientes difiere sutilmente del comportamiento en las bases de datos dependientes. Este comportamiento suele ser beneficioso y proporciona independencia de la instancia y simplicidad. Algunos usuarios pueden tener problemas, en concreto cuando una sesión accede tanto a bases de datos independientes como a dependientes.  
  
## <a name="see-also"></a>Consulte también  
 [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)  
  
  

---
title: Asignación de MySQL y SQL Server juego de caracteres (MySQLToSQL) | Microsoft Docs
description: Obtenga información sobre cómo especificar un juego de caracteres para los tipos de datos de caracteres de MySQL, expresiones y literales que se van a usar durante la conversión de tipos de datos de caracteres mediante SSMA para MySQL.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 20b3f22e-16a2-4a87-b4eb-c277be6bf5c8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 13b2accd6b4dc2ba2bbd8c9c1e6db312650d2d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077626"
---
# <a name="mapping-mysql-and-sql-server-character-set-mysqltosql"></a>Asignación de juegos de caracteres de MySQL y de SQL Server (MySQLToSQL)
El juego de caracteres (charset) se puede especificar para tipos de datos de caracteres de MySQL, expresiones y literales.  
  
## <a name="charset-mapping"></a>Asignación de charset  
La asignación de charset se define para cada conjunto de caracteres de MySQL y se usa durante la conversión de tipos de datos de caracteres. Especifica cómo convertir los tipos de datos de cadena de caracteres de un juego de caracteres determinado:  
  
-   A los tipos de caracteres SQL Server nacionales (NCHAR/NVARCHAR) o  
  
-   A tipos de caracteres SQL Server regulares (CHAR/VARCHAR)  
  
1.  los tipos de datos de caracteres de la base de datos de destino **nacional** son:  
  
    1.  **nchar**  
  
    2.  **nvarchar**  
  
2.  los tipos de datos de caracteres de la base de datos de destino **normal** son:  
  
    1.  **char**  
  
    2.  **varchar**  
  
3.  La asignación de tipos solo permite la asignación a tipos de datos de caracteres **nacionales** . Una vez convertido el tipo de datos de caracteres de MySQL según la asignación de tipos, se aplica la asignación de juegos de caracteres.  
  
> [!NOTE]  
> La asignación de charset se puede definir en cada nivel de nodo del explorador de objetos de metadatos y representar todos los juegos de caracteres leídos de MySQL.  
  
## <a name="charset-mapping-on-different-node-levels"></a>Asignación de juegos de caracteres en diferentes niveles de nodo  
La asignación de juegos de caracteres varía en diferentes niveles de nodo, es decir:  
  
1.  En el nivel de nodo de metadatos raíz  
  
2.  En el nivel de nodo de base de datos, categoría y objeto  
  
> [!NOTE]  
> La pestaña seleccionada para editar la asignación de juegos de caracteres contiene tres botones, independientemente de la asignación en los distintos niveles de nodo.  
>   
> Son las siguientes:  
>   
> 1.  **Aplicar:** Aplica los cambios realizados por el usuario, habilitado solo cuando se edita la asignación de juegos de caracteres y aún no se guarda.  
> 2.  **Cancelar:** Cancela los cambios realizados por el usuario. El botón se habilita cuando se edita la asignación de juegos de caracteres pero no se guarda.  
> 3.  **Restablecer valores predeterminados:** Restablece todas las asignaciones a los valores predeterminados.  
  
1.  **En el nivel de nodo de metadatos raíz:**  La cuadrícula de asignación de juegos de caracteres contiene cuadrícula de juegos de caracteres con una columna independiente para cada juego de caracteres. Las columnas de la cuadrícula son:  
  
    1.  La primera columna de la cuadrícula denominada **CharSet Name** contiene charset Name.  
  
    2.  La segunda Descripción de **CharSet** contiene la descripción del juego de caracteres.  
  
    3.  La tercera columna titulada, **tipo de juego de caracteres de destino** contiene la configuración de la asignación para un juego de caracteres determinado. Los valores de esta columna son:  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
    > [!IMPORTANT]  
    > Los valores predeterminados para un juego de caracteres determinado tienen el prefijo ' (default) ' después de CHAR/VARCHAR o NCHAR/NVARCHAR.  
  
    A continuación se muestra la asignación de juegos de caracteres entre la base de datos MySQL y la base de datos de destino en el nivel de nodo raíz:  
  
    |Nombre del juego de caracteres|Descripción de charset|Tipo de conjunto de caracteres de destino (valor predeterminado)|  
    |-|-|-|  
    |big5|Big5 chino tradicional|NCHAR/NVARCHAR (valor predeterminado)|  
    |dec8|Oeste de Europa occidental|CHAR/VARCHAR (valor predeterminado)|  
    |CP850|DOS en Europa occidental|CHAR/VARCHAR (valor predeterminado)|  
    |hp8|HP West Europea|CHAR/VARCHAR (valor predeterminado)|  
    |koi8r|KOI8-R Relcom Ruso|CHAR/VARCHAR (valor predeterminado)|  
    |Latin 1|Europa occidental de CP1252|CHAR/VARCHAR (valor predeterminado)|  
    |latin2|ISO 8859-2 central europea|CHAR/VARCHAR (valor predeterminado)|  
    |swe7|7bit sueco|CHAR/VARCHAR (valor predeterminado)|  
    |ascii|ASCII (EE. UU.)|CHAR/VARCHAR (valor predeterminado)|  
    |ujis|EUC-JP japonés|NCHAR/NVARCHAR (valor predeterminado)|  
    |sjis|Shift-JIS japonés|NCHAR/NVARCHAR (valor predeterminado)|  
    |Hebreo|ISO 8859-8 Hebreo|CHAR/VARCHAR (valor predeterminado)|  
    |tis620|TIS620 tailandés|CHAR/VARCHAR (valor predeterminado)|  
    |euckr|EUC: Coreano de Corea del sur|NCHAR/NVARCHAR (valor predeterminado)|  
    |koi8u|KOI8-U Ucraniano|CHAR/VARCHAR (valor predeterminado)|  
    |GB2312|GB2312 chino simplificado|NCHAR/NVARCHAR (valor predeterminado)|  
    |griego|ISO 8859-7 Griego|CHAR/VARCHAR (valor predeterminado)|  
    |CP 1250|Europa central de Windows|CHAR/VARCHAR (valor predeterminado)|  
    |simplificado GBK|Chino simplificado de simplificado GBK|NCHAR/NVARCHAR (valor predeterminado)|  
    |latin5|ISO 8859-9 Turco|CHAR/VARCHAR (valor predeterminado)|  
    |armscii8|ARMSCII-8 armenio|CHAR/VARCHAR (valor predeterminado)|  
    |utf8|Unicode UTF-8|NCHAR/NVARCHAR (valor predeterminado)|  
    |ucs2|UCS-2 Unicode|NCHAR/NVARCHAR (valor predeterminado)|  
    |cp866|DOS Ruso|CHAR/VARCHAR (valor predeterminado)|  
    |keybcs2|Czech-Slovak de DOS Kamenicky|CHAR/VARCHAR (valor predeterminado)|  
    |macce|Europa central de Mac|CHAR/VARCHAR (valor predeterminado)|  
    |macro|Mac occidental europeo|CHAR/VARCHAR (valor predeterminado)|  
    |cp852|Europa central de DOS|CHAR/VARCHAR (valor predeterminado)|  
    |latin7|ISO 8859-13 Báltico|CHAR/VARCHAR (valor predeterminado)|  
    |CP 1251|Windows cirílico|CHAR/VARCHAR (valor predeterminado)|  
    |CP 1256|Windows Árabe|CHAR/VARCHAR (valor predeterminado)|  
    |CP 1257|Windows Báltico|CHAR/VARCHAR (valor predeterminado)|  
    |binary|Subjuego de caracteres binario|CHAR/VARCHAR (valor predeterminado)|  
    |geostd8|GEOSTD8 georgiano|CHAR/VARCHAR (valor predeterminado)|  
    |cp932|SJIS para japonés de Windows|NCHAR/NVARCHAR (valor predeterminado)|  
    |eucjpms|UJIS para japonés de Windows|NCHAR/NVARCHAR (valor predeterminado)|  
  
2.  **En los niveles de nodo de base de datos, categoría u objeto:** En el nivel de nodos de objeto, categoría o base de datos, la cuadrícula de asignación de juegos de caracteres contiene las mismas filas que el nivel de nodo de metadatos raíz, es decir,:  
  
    1.  La primera columna de la cuadrícula titulada, **nombre** del juego de caracteres contiene el nombre del juego de caracteres.  
  
    2.  La segunda columna titulada, **Descripción** del juego de caracteres contiene la descripción del juego de caracteres.  
  
    3.  La única diferencia son los valores de la tercera columna de la cuadrícula. La tercera columna titulada, **tipo de datos de destino** , contiene la configuración de asignación para un juego de caracteres determinado. Los valores de la columna son:  
  
        -   Heredado (CHAR/VARCHAR o NCHAR/NVARCHAR)  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
> [!IMPORTANT]  
> -   En la asignación de juegos de caracteres entre la base de datos MySQL y la base de datos de destino en los niveles de base de datos, categoría y nodo de objeto, los valores predeterminados para un juego de caracteres determinado en cada nivel distinto de raíz para el **tipo de datos de destino** de columna deben ser ' heredado  
> -   En la cuadrícula, el valor **heredado** tiene como sufijo ' (CHAR/VARCHAR) ' o ' (NCHAR/NVARCHAR) ', dependiendo del valor que se haya heredado del elemento primario por este juego de caracteres determinado.  
  

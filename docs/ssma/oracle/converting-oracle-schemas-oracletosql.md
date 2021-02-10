---
title: Conversión de esquemas de Oracle (OracleToSQL) | Microsoft Docs
description: Obtenga información sobre cómo convertir objetos de base de datos de Oracle en objetos de base de datos de SQL Server con SSMA para Oracle, después de establecer las opciones y conectarse a Oracle y SQL Server.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Conversion Results
ms.assetid: e021182d-31da-443d-b110-937f5db27272
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8e2ee101ec916a920d4b72d924456a52850e8bc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058760"
---
# <a name="converting-oracle-schemas-oracletosql"></a>Conversión de esquemas de Oracle (OracleToSQL)
Después de conectarse a Oracle, conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y establecer las opciones de asignación de datos y de proyecto, puede convertir objetos de base de datos de Oracle en objetos de base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="the-conversion-process"></a>Proceso de conversión  
La conversión de objetos de base de datos toma las definiciones de objetos de Oracle, las convierte en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos similares y, a continuación, carga esta información en los metadatos de SSMA. No carga la información en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . A continuación, puede ver los objetos y sus propiedades mediante el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos.  
  
Durante la conversión, SSMA imprime los mensajes de salida en el panel de salida y los mensajes de error en el panel de Lista de errores. Utilice la información de salida y de error para determinar si tiene que modificar las bases de datos de Oracle o el proceso de conversión para obtener los resultados de la conversión deseada.  
  
## <a name="setting-conversion-options"></a>Establecer opciones de conversión  
Antes de convertir objetos, revise las opciones de conversión de proyectos en el cuadro de diálogo **configuración del proyecto** . Mediante este cuadro de diálogo, puede establecer cómo SSMA convierte las funciones y las variables globales. Para obtener más información, vea [configuración del proyecto &#40;conversión&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-conversion-oracletosql.md).  
  
## <a name="conversion-results"></a>Resultados de la conversión  
En la tabla siguiente se muestran los objetos de Oracle convertidos y los objetos resultantes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objetos de Oracle|Objetos SQL Server resultantes|  
|-|-|  
|Functions|Si la función se puede convertir directamente en [!INCLUDE[tsql](../../includes/tsql-md.md)] , SSMA crea una función.<br /><br />En algunos casos, la función se debe convertir en un procedimiento almacenado. En este caso, SSMA crea un procedimiento almacenado y una función que llama al procedimiento almacenado.|  
|Procedimientos|Si el procedimiento se puede convertir directamente en [!INCLUDE[tsql](../../includes/tsql-md.md)] , SSMA crea un procedimiento almacenado.<br /><br />En algunos casos, se debe llamar a un procedimiento almacenado en una transacción autónoma. En este caso, SSMA crea dos procedimientos almacenados: uno que implementa el procedimiento y otro que se utiliza para llamar al procedimiento almacenado de implementación.|  
|Paquetes|SSMA crea un conjunto de procedimientos almacenados y funciones que se unifican mediante nombres de objeto similares.|  
|Secuencias|SSMA crea objetos de secuencia (SQL Server 2012 o SQL Server 2014) o emula secuencias de Oracle.|  
|Tablas con objetos dependientes como índices y desencadenadores|SSMA crea tablas con objetos dependientes.|  
|Vista con objetos dependientes, como desencadenadores|SSMA crea vistas con objetos dependientes.|  
|Vistas materializadas|**SSMA crea vistas indizadas en SQL Server con algunas excepciones. Se producirá un error en la conversión si la vista materializada incluye una o varias de las construcciones siguientes:**<br /><br />Función definida por el usuario<br /><br />Campo/función/expresión no determinista en las cláusulas SELECT, WHERE o GROUP BY<br /><br />Uso de la columna Float en las cláusulas SELECT *, WHERE o GROUP BY (caso especial del problema anterior)<br /><br />Tipo de datos personalizado (incluidas las tablas anidadas)<br /><br />COUNT (&lt;campo&gt; distinto)<br /><br />FETCH<br /><br />Combinaciones externas (LEFT, RIGHT o FULL)<br /><br />Subconsulta, otra vista<br /><br />OVER, RANK, LEAD, LOG<br /><br />MIN, MAX<br /><br />UNION, MINUS, INTERSECT<br /><br />HAVING|  
|Desencadenador|**SSMA crea desencadenadores basados en las siguientes reglas:**<br /><br />Los desencadenadores BEFORE se convierten en desencadenadores INSTEAD OF.<br /><br />Los desencadenadores AFTER se convierten en desencadenadores AFTER.<br /><br />Los desencadenadores INSTEAD OF se convierten en desencadenadores INSTEAD OF. Varios desencadenadores INSTEAD OF definidos en la misma operación se combinan en un solo desencadenador.<br /><br />Los desencadenadores de nivel de fila se emulan mediante cursores.<br /><br />Los desencadenadores en cascada se convierten en varios desencadenadores individuales.|  
|Sinónimos|**Los sinónimos se crean para los siguientes tipos de objeto:**<br /><br />Tablas y tablas de objetos<br /><br />Vistas y vistas de objetos<br /><br />Procedimientos almacenados<br /><br />Functions<br /><br />**Los sinónimos de los siguientes objetos se resuelven y se reemplazan por referencias de objeto directas:**<br /><br />Secuencias<br /><br />Paquetes<br /><br />Objetos de esquema de clase Java<br /><br />Tipos de objetos definidos por el usuario<br /><br />Los sinónimos de otro sinónimo no se pueden migrar y se marcarán como errores.<br /><br />No se crean sinónimos para vistas materializadas.|  
|Tipos definidos por el usuario|**SSMA no proporciona compatibilidad para la conversión de tipos definidos por el usuario. Los tipos definidos por el usuario, incluido su uso en programas PL/SQL, se marcan con errores de conversión especiales guiados por las siguientes reglas:**<br /><br />La columna de tabla de un tipo definido por el usuario se convierte en VARCHAR (8000).<br /><br />El argumento del tipo definido por el usuario para un procedimiento almacenado o una función se convierte en VARCHAR (8000).<br /><br />La variable de tipo definido por el usuario en el bloque PL/SQL se convierte en VARCHAR (8000).<br /><br />La tabla de objetos se convierte en una tabla estándar.<br /><br />La vista de objetos se convierte en una vista estándar.|  
  
## <a name="converting-oracle-database-objects"></a>Convertir objetos Oracle Database  
Para convertir objetos de base de datos de Oracle, primero debe seleccionar los objetos que desea convertir y, a continuación, usar SSMA para realizar la conversión. Para ver los mensajes de salida durante la conversión, en el menú **Ver** , seleccione **salida**.  
  
**Para convertir objetos de Oracle en SQL Server sintaxis**  
  
1.  En el explorador de metadatos de Oracle, expanda el servidor de Oracle y, a continuación, expanda **esquemas**.  
  
2.  Seleccionar los objetos que se van a convertir:  
  
    -   Para convertir todos los esquemas, active la casilla situada junto a **esquemas**.  
  
    -   Para convertir u omitir una base de datos, active la casilla situada junto al nombre del esquema.  
  
    -   Para convertir u omitir una categoría de objetos, expanda un esquema y, a continuación, Active o desactive la casilla situada junto a la categoría.  
  
    -   Para convertir u omitir objetos individuales, expanda la carpeta categoría y, a continuación, Active o desactive la casilla situada junto al objeto.  
  
3.  Para convertir todos los objetos seleccionados, haga clic con el botón derecho en **esquemas** y seleccione **convertir esquema**.  
  
    También puede convertir objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta primaria y, a continuación, seleccione **convertir esquema**.  
  
## <a name="viewing-conversion-problems"></a>Ver problemas de conversión  
Es posible que algunos objetos de Oracle no se conviertan. Puede determinar las tasas de éxito de la conversión viendo el informe de conversión de resumen.  
  
**Para ver un informe de Resumen**  
  
1.  En el explorador de metadatos de Oracle, seleccione **esquemas**.  
  
2.  En el panel derecho, seleccione la pestaña **Informe** .  
  
    Este informe muestra el informe de evaluación de resumen para todos los objetos de base de datos que se han evaluado o convertido. También puede ver un informe de resumen para objetos individuales:  
  
    -   Para ver el informe de un esquema individual, seleccione el esquema en el explorador de metadatos de Oracle.  
  
    -   Para ver el informe de un objeto individual, seleccione el objeto en el explorador de metadatos de Oracle. Los objetos que tienen problemas de conversión tienen un icono de error rojo.  
  
En el caso de los objetos que no se pudieron convertir, puede ver la sintaxis que produjo el error de conversión.  
  
**Para ver los problemas de conversión individuales**  
  
1.  En el explorador de metadatos de Oracle, expanda **esquemas**.  
  
2.  Expanda el esquema que muestra un icono de error rojo.  
  
3.  En el esquema, expanda una carpeta que tenga un icono de error rojo.  
  
4.  Seleccione el objeto que tiene un icono de error rojo.  
  
5.  En el panel derecho, haga clic en la pestaña **Informe** .  
  
6.  En la parte superior de la pestaña **Informe** está una lista desplegable. Si la lista muestra **estadísticas**, cambie la selección a **origen**.  
  
    SSMA mostrará el código fuente y varios botones justo encima del código.  
  
7.  Haga clic en el botón **siguiente problema** . Se trata de un icono de error rojo con una flecha que apunta a la derecha.  
  
    SSMA resaltará el primer código fuente problemático que encuentre en el objeto actual.  
  
Para cada elemento que no se pueda convertir, debe determinar qué desea hacer con ese objeto:  
  
-   Puede modificar el código fuente de los procedimientos en la pestaña **SQL** .  
  
-   Puede modificar el objeto en la base de datos de Oracle para quitar o revisar el código problemático. Para cargar el código actualizado en SSMA, tendrá que actualizar los metadatos. Para obtener más información, vea [conectarse a Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md).  
  
-   Puede excluir el objeto de la migración. En el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos y en el explorador de metadatos de Oracle, desactive la casilla situada junto al elemento antes de cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y migrar datos de Oracle.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [cargar los objetos convertidos en SQL Server](loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  

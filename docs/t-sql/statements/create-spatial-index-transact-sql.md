---
description: CREATE SPATIAL INDEX (Transact-SQL)
title: CREATE SPATIAL INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SPATIAL INDEX
- CREATE SPATIAL INDEX
- CREATE_SPATIAL_INDEX_TSQL
- SPATIAL_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], creating
- index creation [SQL Server], spatial indexes
- CREATE SPATIAL INDEX statement
- CREATE INDEX statement
ms.assetid: ee6b9116-a7ff-463a-a9f0-b360804d8678
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 07f7bdead063167b789bd465fac6f66b653a0cbb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191032"
---
# <a name="create-spatial-index-transact-sql"></a>CREATE SPATIAL INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un índice espacial en la tabla y la columna especificadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se puede crear un índice antes de que la tabla posea datos. Los índices se pueden crear en tablas o vistas de otra base de datos especificando un nombre completo de base de datos. Los índices espaciales requieren que la tabla tenga una clave principal clúster. Los índices espaciales no se pueden especificar en vistas indizadas. Para más información sobre los índices espaciales, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE SPATIAL INDEX index_name
  ON <object> ( spatial_column_name )  
    {  
       <geometry_tessellation> | <geography_tessellation>  
    }
  [ ON { filegroup_name | "default" } ]  
[;]
  
<object> ::=  
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<geometry_tessellation> ::=  
{
  <geometry_automatic_grid_tessellation>
| <geometry_manual_grid_tessellation>
}  
  
<geometry_automatic_grid_tessellation> ::=  
{  
    [ USING GEOMETRY_AUTO_GRID ]  
          WITH  (  
        <bounding_box>  
            [ [,] <tessellation_cells_per_object> [ ,...n] ]  
            [ [,] <spatial_index_option> [ ,...n] ]  
                 )  
}  
  
<geometry_manual_grid_tessellation> ::=  
{  
       [ USING GEOMETRY_GRID ]  
         WITH (  
                    <bounding_box>  
                        [ [,]<tessellation_grid> [ ,...n] ]  
                        [ [,]<tessellation_cells_per_object> [ ,...n] ]  
                        [ [,]<spatial_index_option> [ ,...n] ]  
   )  
}
  
<geography_tessellation> ::=  
{  
      <geography_automatic_grid_tessellation> | <geography_manual_grid_tessellation>  
}  
  
<geography_automatic_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_AUTO_GRID ]  
    [ WITH (  
        [ [,] <tessellation_cells_per_object> [ ,...n] ]  
        [ [,] <spatial_index_option> ]  
     ) ]  
}  
  
<geography_manual_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_GRID ]  
    [ WITH (  
                [ <tessellation_grid> [ ,...n] ]  
                [ [,] <tessellation_cells_per_object> [ ,...n] ]  
                [ [,] <spatial_index_option> [ ,...n] ]  
                ) ]  
}  
  
<bounding_box> ::=  
{  
      BOUNDING_BOX = ( {  
       xmin, ymin, xmax, ymax
       | <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>
  } )  
}  
  
<named_bb_coordinate> ::= { XMIN = xmin | YMIN = ymin | XMAX = xmax | YMAX=ymax }  
  
<tessellation_grid> ::=  
{
    GRIDS = ( { <grid_level> [ ,...n ] | <grid_size>, <grid_size>, <grid_size>, <grid_size>  }
        )  
}  
<tessellation_cells_per_object> ::=  
{
   CELLS_PER_OBJECT = n
}  
  
<grid_level> ::=  
{  
     LEVEL_1 = <grid_size>
  |  LEVEL_2 = <grid_size>
  |  LEVEL_3 = <grid_size>
  |  LEVEL_4 = <grid_size>
}  
  
<grid_size> ::= { LOW | MEDIUM | HIGH }  
  
<spatial_index_option> ::=  
{  
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE }  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

 *index_name*     
 Es el nombre del índice. Los nombres de índice deben ser únicos en una tabla, pero no es necesario que sean únicos en una base de datos. Los nombres de índice deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 ON \<object> ( *spatial_column_name* )     
 Especifica el objeto (base de datos, esquema o tabla) en el que va a crearse el índice y el nombre de columna espacial.  
  
 *spatial_column_name* especifica la columna espacial en la que se basa el índice. Solamente puede especificarse una columna espacial en una definición de índice espacial único; pero pueden crearse varios índices espaciales en una columna de tipo **geometry** o **geography**.  
  
 USING      
 Indica el esquema de teselación del índice espacial. Este parámetro usa el valor específico del tipo, tal y como aparece en la siguiente tabla:  
  
|Tipo de datos de la columna|Esquema de teselación|  
|-------------------------|-------------------------|  
|**geometry**|GEOMETRY_GRID|  
|**geometry**|GEOMETRY_AUTO_GRID|  
|**geography**|GEOGRAPHY_GRID|  
|**geography**|GEOGRAPHY_AUTO_GRID|  
  
 Un índice espacial solamente puede crearse en una columna de tipo **geometría** o **geografía**, en caso contrario se produce un error. Se produce un error si se pasa un parámetro no válido para un tipo determinado.  
  
 Para conocer mejor el modo en que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa la teselación, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ON *filegroup_name*      
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Crea el índice especificado en el grupo de archivos especificado. Si no se especifica ninguna ubicación y la tabla no tiene particiones, el índice utiliza el mismo grupo de archivos que la tabla subyacente. El grupo de archivos debe existir previamente.  
  
 ON "default"     
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Crea el índice especificado en el grupo de archivos predeterminado.  
  
 El término predeterminado (default), en este contexto, no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en ON"default" o en ON [default]. Si se especifica "default", la opción QUOTED_IDENTIFIER debe tener el valor ON para la sesión actual. Esta es la configuración predeterminada. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **\<object>::=**     
 Es el objeto completo o no completo que va a indizarse.  
  
 *database_name*      
 Es el nombre de la base de datos.  
  
 *schema_name*     
 Es el nombre del esquema al que pertenece la tabla.  
  
 *table_name*      
 Es el nombre de la tabla que va a indizarse.  
  
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] admite el formato de nombre de tres partes database_name.[schema_name].object_name cuando database_name es la base de datos actual o database_name es tempdb y object_name comienza por #.  
  
### <a name="using-options"></a>Opciones de USING

 GEOMETRY_GRID      
 Especifica el esquema de teselación de cuadrícula de **geometry** que se está usando. GEOMETRY_GRID puede especificarse solamente en una columna del tipo de datos **geometry**.  GEOMETRY_GRID permite que se ajuste manualmente el esquema de teselación.  
  
 GEOMETRY_AUTO_GRID      
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Puede especificarse solamente en una columna del tipo de datos . Se trata del valor predeterminado para este tipo de datos y no es necesario especificarlo.  
  
 GEOGRAPHY_GRID      
 Especifica el esquema de teselación de cuadrícula de geografía. GRAPHY_GRID puede especificarse solamente en una columna del tipo de datos **geography**.  
  
 GEOGRAPHY_AUTO_GRID      
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Solamente puede especificarse en una columna del tipo de datos Geography.  Se trata del valor predeterminado para este tipo de datos y no es necesario especificarlo.  
  
### <a name="with-options"></a>Opciones de WITH

BOUNDING_BOX      
Especifica una tupla numérica de cuatro elementos que define las cuatro coordenadas del cuadro de límite: las coordenadas X mínima e Y mínima de la esquina inferior izquierda y las coordenadas X máxima e Y máxima de la esquina superior derecha.  
  
 *xmin*      
 Especifica la coordenada X de la esquina inferior izquierda del cuadro de límite.  
  
 *ymin*      
 Especifica la coordenada Y de la esquina inferior izquierda del cuadro de límite.  
  
 *xmax*     
 Especifica la coordenada X de la esquina superior derecha del cuadro de límite.  
  
 *ymax*     
 Especifica la coordenada Y de la esquina superior derecha del cuadro de límite.  
  
 XMIN = *xmin*     
 Especifica el nombre y el valor de la propiedad para la coordenada X de la esquina inferior izquierda del cuadro de límite.  
  
 YMIN =*ymin*      
 Especifica el nombre y el valor de la propiedad para la coordenada Y de la esquina inferior izquierda del cuadro de límite.  
  
 XMAX =*xmax*      
 Especifica el nombre y el valor de la propiedad para la coordenada X de la esquina superior derecha del cuadro de límite.  
  
 YMAX =*ymax*     
 Especifica el nombre y el valor de la propiedad para la coordenada Y de la esquina superior derecha del cuadro de límite.  
  
 > [!NOTE]
 > Las coordenadas del cuadro de límite solamente se aplican dentro de una cláusula USING GEOMETRY_GRID.  
 >
 > *xmax* debe ser mayor que *xmin* e *ymax* debe ser mayor que *ymin*. Puede especificar cualquier representación válida del valor [float](../../t-sql/data-types/float-and-real-transact-sql.md) suponiendo que: *xmax* > *xmin* e *ymax* > *ymin*. De lo contrario, se producen los errores correspondientes.  
 >
 > No hay valores predeterminados.  
 >
 > En los nombres de las propiedades del cuadro de límite no se distinguen mayúsculas de minúsculas, independientemente de la intercalación de la base de datos.  
  
 Cada nombre de propiedad debe especificarse una sola vez. Pueden especificarse en cualquier orden. Por ejemplo, las cláusulas siguientes son equivalentes:  
  
- BOUNDING_BOX =( XMIN =*xmin*, YMIN =*ymin*, XMAX =*xmax*, YMAX =*ymax* )  
  
- BOUNDING_BOX =( XMIN =*xmin*, XMAX =*xmax*, YMIN =*ymin*, YMAX =*ymax*)  
  
GRIDS      
Define la densidad de la cuadrícula en cada nivel del esquema de teselación. Cuando GEOMETRY_AUTO_GRID y GEOGRAPHY_AUTO_GRID están seleccionados, esta opción está deshabilitada.  
  
 Para más información sobre la teselación, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 GRIDS admite los parámetros siguientes:  
  
 LEVEL_1     
 Especifica la cuadrícula del primer nivel (superior).  
  
 LEVEL_2    
 Especifica la cuadrícula del segundo nivel.  
  
 LEVEL_3    
 Especifica la cuadrícula del tercer nivel.  
  
 LEVEL_4    
 Especifica la cuadrícula del cuarto nivel.  
  
 LOW    
 Especifica la mínima densidad posible de la cuadrícula en un nivel determinado. LOW equivale a 16 celdas (una cuadrícula de 4x4).  
  
 **MEDIUM**     
 Especifica la densidad media de la cuadrícula en un nivel determinado. MEDIUM equivale a 64 celdas (una cuadrícula de 8x8).  
  
 HIGH     
 Especifica la máxima densidad posible de la cuadrícula en un nivel determinado. HIGH equivale a 256 celdas (una cuadrícula de 16x16).  
  
> [!NOTE]
> El uso de nombres para los niveles permite especificar los niveles en cualquier orden y omitirlos. Si utiliza el nombre para un nivel, debe utilizar el nombre para cualquier otro nivel que especifique. Si omite un nivel, su densidad adopta el valor predeterminado, MEDIUM.  

> [!WARNING]
> Si se especifica una densidad no válida, se produce un error.  
  
CELLS_PER_OBJECT =*n*     
Especifica el número de celdas por objeto de teselación que el proceso de teselación puede usar para un único objeto espacial en el índice. *n* puede ser cualquier número entero comprendido entre 1 y 8192, ambos incluidos. Si se pasa un número que no es válido o si el número supera el máximo de celdas para la teselación especificada, se produce un error.  
  
 CELLS_PER_OBJECT tiene los siguientes valores predeterminados:  
  
|Opción USING|Celdas predeterminadas por objeto|  
|------------------|------------------------------|  
|GEOMETRY_GRID|**16**|  
|GEOMETRY_AUTO_GRID|**8**|  
|GEOGRAPHY_GRID|**16**|  
|GEOGRAPHY_AUTO_GRID|**12**|  
  
En el nivel superior, si un objeto abarca más celdas de las especificadas mediante *n*, la indexación usa tantas celdas como sean necesarias para proporcionar una teselación de nivel superior completa. En tales casos, un objeto podría recibir más celdas de las especificadas. En este caso, el número máximo es la cantidad de celdas generadas por la cuadrícula de nivel superior, que depende de la densidad.  
  
La regla de teselación de celdas por objeto utiliza el valor CELLS_PER_OBJECT. Para más información sobre las reglas de teselación, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
PAD_INDEX = { ON | **OFF** }     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
Especifica el relleno del índice. El valor predeterminado es OFF.  
  
ACTIVAR     
Indica que el porcentaje de espacio disponible especificado por *fillfactor* se aplica a las páginas de nivel intermedio del índice.  
  
No se especifica OFF ni *fillfactor*.     
Indica que las páginas de nivel intermedio se llenan casi al máximo de su capacidad y dejan espacio suficiente para al menos una fila del tamaño máximo que puede tener el índice, considerando el conjunto de claves incluidas en las páginas de nivel intermedio.  
  
La opción PAD_INDEX solamente resulta útil si también se especifica FILLFACTOR, porque PAD_INDEX utiliza el mismo porcentaje especificado por FILLFACTOR. Si el porcentaje especificado para FILLFACTOR no es lo suficientemente grande como para admitir una fila, [!INCLUDE[ssDE](../../includes/ssde-md.md)] invalida internamente el porcentaje para permitir el valor mínimo. El número de filas de una página de nivel intermedio del índice no es nunca inferior a dos, independientemente de lo bajo que sea el valor de *fillfactor*.  
  
FILLFACTOR =*fillfactor*     
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Especifica un porcentaje que indica cuánto debe llenar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] el nivel hoja de cada página de índice durante la creación o nueva generación de los índices. *fillfactor* debe ser un valor entero comprendido entre 1 y 100. El valor predeterminado es 0. Si *fillfactor* es 100 o 0, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea índices con las páginas hoja llenas al máximo de su capacidad.  
  
> [!NOTE]  
> Los valores de fill factor 0 y 100 son idénticos.
  
 La configuración de FILLFACTOR solo se aplica cuando se crea o se vuelve a generar el índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] no mantiene dinámicamente el porcentaje especificado de espacio disponible de las páginas. Para ver la configuración del factor de relleno, use la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
> [!IMPORTANT]  
> La creación de un índice clúster con un valor de FILLFACTOR menor que 100 afecta a la cantidad de espacio de almacenamiento que ocupan los datos, porque [!INCLUDE[ssDE](../../includes/ssde-md.md)] vuelve a distribuir los datos cuando crea el índice clúster.  
  
 Para obtener más información, vea [Especificar el factor de relleno para un índice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
SORT_IN_TEMPDB = { ON | **OFF** }       
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Indica si los resultados de ordenación temporales deben almacenarse en tempdb. El valor predeterminado es OFF.  
  
 ACTIVAR     
 Los resultados de ordenación intermedios utilizados para generar el índice se almacenan en tempdb. Esto puede reducir el tiempo necesario para crear un índice si tempdb y la base de datos de usuarios están en conjuntos de discos distintos. Sin embargo, esto aumenta la cantidad de espacio en disco utilizado durante la generación del índice.  
  
 Apagado     
 Los resultados de orden intermedios se almacenan en la misma base de datos que el índice.  
  
 Además del espacio necesario en la base de datos del usuario para crear el índice, tempdb debe tener la misma cantidad de espacio adicional para almacenar los resultados de orden intermedio. Para más información, vea [Opción SORT_IN_TEMPDB para índices](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
IGNORE_DUP_KEY =**OFF**     
No tiene ningún efecto sobre los índices espaciales porque el tipo de índice nunca es único. No establezca esta opción en ON porque, de lo contrario, se producirá un error.  
  
STATISTICS_NORECOMPUTE = { ON | **OFF**}     
Especifica si se vuelven a calcular las estadísticas de distribución. El valor predeterminado es OFF.  
  
 ACTIVAR    
 Las estadísticas obsoletas no se vuelven a calcular automáticamente.  
  
 Apagado    
 Se habilita la actualización automática de las estadísticas.  
  
 Para restaurar la actualización automática de estadísticas, establezca STATISTICS_NORECOMPUTE en OFF o ejecute UPDATE STATISTICS sin la cláusula NORECOMPUTE.  
  
> [!IMPORTANT]  
> Deshabilitar el cálculo automático de estadísticas de distribución puede impedir que el optimizador de consultas elija los planes de ejecución óptimos de las consultas relativas a la tabla.  
  
DROP_EXISTING = { ON | **OFF** }     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Especifica que se quite y se vuelva a generar el índice espacial con nombre preexistente. El valor predeterminado es OFF.  
  
 ACTIVAR     
 El índice existente se quita y se vuelve a generar. El nombre de índice especificado debe ser el mismo que el de un índice actualmente existente; sin embargo, la definición se puede modificar. Por ejemplo, puede especificar columnas, criterio de ordenación, esquema de particionamiento u opciones de índice diferentes.  
  
 Apagado     
 Se muestra un error si ya existe el nombre de índice especificado.  
  
 El tipo de índice no puede cambiarse utilizando DROP_EXISTING.  
  
ONLINE =**OFF**      
Especifica que las tablas subyacentes y los índices asociados no están disponibles para la realización de consultas y modificaciones de datos durante la operación del índice. En esta versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], no se admiten generaciones de índices en línea para los índices espaciales. Si esta opción se establece en ON para un índice espacial, se produce un error. Omita la opción ONLINE o establezca ONLINE en OFF.  
  
 Una operación de índice sin conexión para crear, volver a crear o quitar un índice espacial adquiere un bloqueo de modificación del esquema (Sch-M) de la tabla. Esto evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación.  
  
> [!NOTE]  
> Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
ALLOW_ROW_LOCKS = { **ON** | OFF }     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Especifica si se permiten los bloqueos de fila. El valor predeterminado es ON.  
  
 ACTIVAR     
 Los bloqueos de fila se admiten al obtener acceso al índice. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina cuándo se usan los bloqueos de fila.  
  
 Apagado     
 No se usan los bloqueos de fila.  
  
ALLOW_PAGE_LOCKS = { **ON** | OFF }     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Especifica si se permiten bloqueos de página. El valor predeterminado es ON.  
  
 ACTIVAR    
 Los bloqueos de página se permiten al obtener acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina el momento en que se usan los bloqueos de página.  
  
 Apagado     
 No se utilizan bloqueos de página.  
  
MAXDOP =*max_degree_of_parallelism*      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Invalida la opción de configuración `max degree of parallelism` mientras se prolongue la operación del índice. Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
> [!IMPORTANT]  
> Aunque la opción MAXDOP se admite desde el punto de vista sintáctico, actualmente CREATE SPATIAL INDEX siempre utiliza un solo procesador.  
  
 *max_degree_of_parallelism* puede tener estos valores:  
  
 1     
 Suprime la generación de planes paralelos.  
  
 \>1    
 Restringe el número máximo de procesadores utilizados en una operación de índice paralelo al número especificado o a un número inferior, en función de la actual carga de trabajo del sistema.  
  
 0 (predeterminado)    
 Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
 Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
DATA_COMPRESSION = {NONE | ROW | PAGE}     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Determina el nivel de compresión de datos usado por el índice.  
  
 Ninguno     
 El índice no usa ninguna compresión de los datos.  
  
 ROW    
 El índice no usa compresión de filas.  
  
 PAGE    
 El índice no usa compresión de páginas.  
  
## <a name="remarks"></a>Observaciones
Cada opción solamente puede especificarse una vez para cada instrucción CREATE SPATIAL INDEX. Si se especifica dos veces cualquier opción, se produce un error.  
  
Puede crear un máximo de 249 índices espaciales en cada columna espacial de una tabla. Crear más de un índice espacial en una columna espacial concreta puede resultar útil, por ejemplo, para indizar parámetros de teselación diferentes en una única columna.  
  
> [!IMPORTANT]  
> Existen otras restricciones para la creación de índices espaciales. Para más información, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
Una generación de índices no puede usar el paralelismo de procesos disponible.  
  
## <a name="methods-supported-on-spatial-indexes"></a>Métodos que se admiten en los índices espaciales
Bajo ciertas condiciones, los índices espaciales admiten determinados métodos de geometría orientados a conjuntos. Para más información, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="spatial-indexes-and-partitioning"></a>Los índices espaciales y la creación de particiones
De forma predeterminada, si se crea un índice espacial en una tabla con particiones, el índice se particiona según el esquema de partición de la tabla. Esto garantiza que los datos del índice y la fila relacionada se almacenen en la misma partición.  
  
En este caso, para modificar el esquema de partición de la tabla base, habría que quitar el índice espacial antes de volver a particionar la tabla base. Para evitar esta restricción, puede especificar la opción "ON filegroup" durante la creación de un índice espacial. Para obtener más información, vea "Los índices espaciales y los grupos de archivos", más adelante en este tema.  
  
## <a name="spatial-indexes-and-filegroups"></a>Los índices espaciales y los grupos de archivos
De forma predeterminada los índices espaciales se particionan en los mismos grupos de archivos que la tabla en la que se especifica el índice. Esto puede invalidarse utilizando la especificación del grupo de archivos:  
  
[ ON { *filegroup_name* | "default" } ]     
Si especifica un grupo de archivos para un índice espacial, el índice se sitúa en ese grupo de archivos, independientemente del esquema de partición de la tabla.  
  
## <a name="catalog-views-for-spatial-indexes"></a>Vistas de catálogo para los índices espaciales

 Las vistas de catálogo siguientes son específicas de los índices espaciales:  
  
 [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)  
 Representa la información del índice principal de los índices espaciales.  
  
 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)  
 Representa la información acerca del esquema de teselación y los parámetros de cada uno de los índices espaciales.  
  
## <a name="additional-remarks-about-creating-indexes"></a>Comentarios adicionales sobre la creación de índices

 Para más información sobre la creación de índices, vea la sección "Comentarios" de [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="permissions"></a>Permisos
El usuario debe disponer del permiso `ALTER` en la tabla o vista o bien ser miembro del rol de servidor fijo sysadmin o de los roles fijos de bases de datos `db_ddladmin` y `db_owner`.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-spatial-index-on-a-geometry-column"></a>A. Crear un índice espacial en una columna de geometría
En este ejemplo se crea una tabla denominada `SpatialTable` que contiene una columna de tipo **geometry**, denominada `geometry_col`. A continuación, se crea un índice espacial, `SIndx_SpatialTable_geometry_col1`, en la columna `geometry_col`. El ejemplo utiliza el esquema de teselación predeterminado y especifica el cuadro de límite.  
  
```sql  
CREATE TABLE SpatialTable(id int primary key, geometry_col geometry);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col1
   ON SpatialTable(geometry_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ) );  
```  
  
### <a name="b-creating-a-spatial-index-on-a-geometry-column"></a>B. Crear un índice espacial en una columna de geometría
En el ejemplo siguiente se crea un segundo índice espacial, `SIndx_SpatialTable_geometry_col2`, en la columna `geometry_col` de la tabla `SpatialTable`. En el ejemplo se especifica `GEOMETRY_GRID` como esquema de teselación. También especifica el cuadro de límite, densidades diferentes para los distintos niveles de la cuadrícula y 64 celdas por objeto. También establece el relleno del índice en `ON`.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col2  
   ON SpatialTable(geometry_col)  
   USING GEOMETRY_GRID  
   WITH (  
    BOUNDING_BOX = ( xmin=0, ymin=0, xmax=500, ymax=200 ),  
    GRIDS = (LOW, LOW, MEDIUM, HIGH),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="c-creating-a-spatial-index-on-a-geometry-column"></a>C. Crear un índice espacial en una columna de geometría
El ejemplo siguiente crea un tercer índice espacial, `SIndx_SpatialTable_geometry_col3`, en la columna `geometry_col` de la tabla `SpatialTable`. El ejemplo utiliza el esquema de teselación predeterminado. También especifica el cuadro de límite y utiliza densidades de celda diferentes en los niveles tercero y cuarto, y utiliza el número predeterminado de celdas por objeto.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col3  
   ON SpatialTable(geometry_col)  
   WITH (  
    BOUNDING_BOX = ( 0, 0, 500, 200 ),  
    GRIDS = ( LEVEL_4 = HIGH, LEVEL_3 = MEDIUM ) );  
```  
  
### <a name="d-changing-an-option-that-is-specific-to-spatial-indexes"></a>D. Cambiar una opción específica de los índices espaciales
El ejemplo siguiente vuelve a generar el índice espacial creado en el ejemplo anterior, `SIndx_SpatialTable_geography_col3`, especificando una nueva densidad `LEVEL_3` con DROP_EXISTING = ON.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable(geography_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ),  
        GRIDS = ( LEVEL_3 = LOW ),  
        DROP_EXISTING = ON );  
```  
  
### <a name="e-creating-a-spatial-index-on-a-geography-column"></a>E. Crear un índice espacial en una columna de geografía
En este ejemplo se crea una tabla denominada `SpatialTable2` que contiene una columna de tipo **geography**, denominada `geography_col`. A continuación, se crea un índice espacial, `SIndx_SpatialTable_geography_col1`, en la columna `geography_col`. En el ejemplo se usan los valores predeterminados de los parámetros del esquema de teselación GEOGRAPHY_AUTO_GRID.  
  
```sql  
CREATE TABLE SpatialTable2(id int primary key, object GEOGRAPHY);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col1
   ON SpatialTable2(object);  
```  
  
> [!NOTE]  
> No es posible especificar un cuadro de límite para los índices de cuadrícula de geografía.
  
### <a name="f-creating-a-spatial-index-on-a-geography-column"></a>F. Crear un índice espacial en una columna de geografía
En el ejemplo siguiente se crea un segundo índice espacial, `SIndx_SpatialTable_geography_col2`, en la columna `geography_col` de la tabla `SpatialTable2`. En el ejemplo se especifica `GEOGRAPHY_GRID` como esquema de teselación. También especifica densidades de la cuadrícula diferentes para los distintos niveles y 64 celdas por objeto. También establece el relleno del índice en `ON`.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col2  
   ON SpatialTable2(object)  
   USING GEOGRAPHY_GRID  
   WITH (  
    GRIDS = (MEDIUM, LOW, MEDIUM, HIGH ),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="g-creating-a-spatial-index-on-a-geography-column"></a>G. Crear un índice espacial en una columna de geografía
A continuación, el ejemplo crea un tercer índice espacial, `SIndx_SpatialTable_geography_col3`, en la columna `geography_col` de la tabla `SpatialTable2`. Utiliza el esquema de teselación predeterminado, GEOGRAPHY_GRID, y el valor predeterminado de CELLS_PER_OBJECT (16).  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable2(object)  
   WITH ( GRIDS = ( LEVEL_3 = HIGH, LEVEL_2 = HIGH ) );  
```  
  
## <a name="see-also"></a>Consulte también
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)       
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)       
[CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)       
[CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)       
[CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)       
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)       
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)       
[DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)       
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)       
[EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)       
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)       
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)       
[sys.spatial_index_tessellations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)       
[sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)       
[Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md)       

---
description: DBCC CHECKTABLE (Transact-SQL)
title: DBCC CHECKTABLE (Transact-SQL) | Microsoft Docs
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKTABLE_TSQL
- DBCC_CHECKTABLE_TSQL
- DBCC CHECKTABLE
- CHECKTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], DBCC CHECKTABLE
- page integrity checks [SQL Server]
- consistency [SQL Server], tables
- consistency [SQL Server], indexed views
- DBCC CHECKTABLE statement
- integrity [SQL Server]
- low overhead checks
- table integrity checks [SQL Server]
ms.assetid: 0d6cb620-eb58-4745-8587-4133a1b16994
author: pmasl
ms.author: umajay
ms.openlocfilehash: adf908dbd20532903ba8bcc40251419d0bb956d6
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233231"
---
# <a name="dbcc-checktable-transact-sql"></a>DBCC CHECKTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Comprueba la integridad de todas las páginas y estructuras que constituyen la tabla o la vista indizada.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
    
## <a name="syntax"></a>Sintaxis    
    
```syntaxsql
DBCC CHECKTABLE     
(    
    table_name | view_name    
    [ , { NOINDEX | index_id }    
     |, { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD }     
    ]     
)    
    [ WITH     
        { [ ALL_ERRORMSGS ]    
          [ , EXTENDED_LOGICAL_CHECKS ]     
          [ , NO_INFOMSGS ]    
          [ , TABLOCK ]     
          [ , ESTIMATEONLY ]     
          [ , { PHYSICAL_ONLY | DATA_PURITY } ]     
          [ , MAXDOP = number_of_processors ]    
        }    
    ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *table_name* | *view_name*  
 Es la tabla o la vista indizada para la que se ejecutan las comprobaciones de integridad. Los nombres de tablas y vistas deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
    
NOINDEX  
 Especifica que no se deben realizar comprobaciones intensivas de índices no agrupado para las tablas de usuario. Esto reduce el tiempo total de ejecución. NOINDEX no afecta a las tablas del sistema porque las comprobaciones de integridad siempre se ejecutan en todos los índices de las tablas del sistema.  
    
 *id_de_índice*  
 Es el número de identificación (Id.) del índice para el que se van a ejecutar las comprobaciones de integridad. Si se especifica *id_de_índice*, DBCC CHECKTABLE ejecuta las comprobaciones de integridad solo en ese índice, junto con el índice de montón o el índice agrupado.  
    
REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD  
 Especifica que DBCC CHECKTABLE repare los errores que encuentre. Para utilizar una opción de reparación, la base de datos debe estar en modo de usuario único.  
    
REPAIR_ALLOW_DATA_LOSS  
 Intenta reparar todos los errores indicados. Estas reparaciones pueden ocasionar alguna pérdida de datos.  
    
REPAIR_FAST  
 La sintaxis solo se mantiene por razones de compatibilidad con versiones anteriores. No se realizan acciones de reparación.  
    
REPAIR_REBUILD  
 Realiza reparaciones que no tienen ninguna posibilidad de pérdida de datos. Pueden ser reparaciones rápidas, como la reparación de las filas que faltan en índices no agrupados, y reparaciones que consumen más tiempo, como regenerar un índice.  
 Este argumento no repara errores relacionados con datos de FILESTREAM.  
    
 > [!NOTE]  
 > Utilice las opciones REPAIR solo como último recurso. Para reparar errores, se recomienda restaurar a partir de una copia de seguridad. Las operaciones de reparación no tienen en cuenta ninguna de las restricciones que puede haber en las tablas o entre ellas. Si la tabla especificada está implicada en una o más restricciones, se recomienda ejecutar `DBCC CHECKCONSTRAINTS` tras una operación de reparación.
 > Si tiene que usar REPAIR, ejecute `DBCC CHECKTABLE` sin una opción de reparación para localizar el nivel de reparación que se va a usar. Si se va a usar el nivel REPAIR_ALLOW_DATA_LOSS, se recomienda realizar una copia de seguridad de la base de datos antes de ejecutar `DBCC CHECKTABLE` con esta opción.  
    
ALL_ERRORMSGS  
 Muestra un número ilimitado de errores. De forma predeterminada, se muestran todos los mensajes de error. Especificar u omitir esta opción no tiene ningún efecto.  
    
EXTENDED_LOGICAL_CHECKS  
 Si el nivel de compatibilidad es 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) o superior, realiza comprobaciones de coherencia lógica en una vista indexada, en índices XML y en índices espaciales, en caso de que los haya.  
 Para obtener más información, vea *Realizar comprobaciones de coherencia lógica en índices* en la sección [Comentarios](#remarks) más adelante en este tema.  
    
NO_INFOMSGS  
 Suprime todos los mensajes de información.  
    
TABLOCK  
 Hace que DBCC CHECKTABLE reciba un bloqueo de tabla compartido en vez de utilizar una instantánea de base de datos interna. TABLOCK hará que DBCC CHECKTABLE se ejecute más rápido en una tabla con mucha carga, pero disminuirá la simultaneidad disponible sobre la tabla mientras DBCC CHECKTABLE está ejecutándose.  
    
ESTIMATEONLY  
 Muestra la cantidad de espacio estimado de la base de datos tempdb necesario para ejecutar DBCC CHECKTABLE con todas las demás opciones especificadas.  
    
PHYSICAL_ONLY  
 Limita la comprobación de la integridad a la estructura física de la página, los encabezados de registro y la estructura física de árboles b. Se ha diseñado para proporcionar una pequeña comprobación de sobrecarga de la coherencia física de la tabla; esta comprobación también puede detectar páginas rasgadas y errores de hardware comunes que pueden comprometer los datos. Una ejecución completa de DBCC CHECKTABLE puede tardar mucho más tiempo que en versiones anteriores. Este comportamiento se debe a las razones siguientes:  
 -   Las comprobaciones lógicas son más exhaustivas.  
 -   Algunas de las estructuras subyacentes que hay que comprobar son más complejas.  
 -   Se han agregado muchas comprobaciones nuevas para incluir las nuevas características.  
   
Por tanto, el uso de la opción PHYSICAL_ONLY puede llevar mucho menos tiempo para DBCC CHECKTABLE en tablas grandes y, por ello, se recomienda para su uso frecuente en sistemas de producción. Aun así, se recomienda realizar periódicamente una ejecución completa DBCC CHECKTABLE. La frecuencia de estas ejecuciones depende de factores específicos de cada empresa y de los entornos de producción. PHYSICAL_ONLY siempre implica NO_INFOMSGS y no se permite con ninguna de las opciones de reparación.  
    
 > [!NOTE]  
 > Si se especifica PHYSICAL_ONLY, DBCC CHECKTABLE omite todas las comprobaciones de datos de FILESTREAM.  
    
DATA_PURITY  
 Hace que DBCC CHECKTABLE compruebe si la tabla contiene valores de columna que no son válidos o están fuera del intervalo correcto. Por ejemplo, DBCC CHECKTABLE detecta las columnas cuyos valores de fecha y hora son superiores o inferiores al intervalo de valores válido para el tipo de datos **datetime**, o bien las columnas del tipo de datos **decimal** o numérico aproximado con valores de escala o precisión que no son válidos.  
 Las comprobaciones de integridad de valores de columna están habilitadas de manera predeterminada y no requieren la opción DATA_PURITY. En las bases de datos actualizadas desde versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede usar DBCC CHECKTABLE WITH DATA_PURITY para buscar y corregir errores en una tabla concreta; sin embargo, la comprobación de los valores de columnas de la tabla no se habilitan de forma predeterminada hasta que se ejecute DBCC CHECKDB WITH DATA_PURITY sin errores en la base de datos. Después, DBCC CHECKDB y DBCC CHECKTABLE comprueban la integridad de los valores de columnas de forma predeterminada.  
 Los errores de validación de los que informe esta opción no se pueden corregir con las opciones de reparación de DBCC. Para obtener información acerca de cómo corregir manualmente estos errores, consulte el artículo 923247 de Knowledge Base: [Resolución del error DBCC 2570 en SQL Server 2005 y versiones posteriores](https://support.microsoft.com/kb/923247).  
 Si se especifica PHYSICAL_ONLY, no se realizan comprobaciones de integridad de columna.  
    
MAXDOP  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 y versiones posteriores)  
 
 Invalida la opción de configuración de **grado máximo de paralelismo** de **sp_configure** para la instrucción. MAXDOP puede superar el valor configurado con sp_configure. Si MAXDOP supera el valor configurado con Resource Governor, el motor de base de datos usa el valor MAXDOP de Resource Governor, descrito en ALTER WORKLOAD GROUP (Transact-SQL). Se pueden aplicar todas las reglas semánticas utilizadas con la opción de configuración max degree of parallelism cuando se utiliza la sugerencia de consulta MAXDOP. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
    
 > [!NOTE]  
 > Si MAXDOP se establece en cero, el servidor elige el grado máximo de paralelismo.  
    
## <a name="remarks"></a>Observaciones    
    
> [!NOTE]    
> Para ejecutar DBCC CHECKTABLE en todas las tablas de la base de datos, use [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).    
    
En la tabla especificada, DBCC CHECKTABLE comprueba lo siguiente:
-   Las páginas de índice, consecutivas, de objetos grandes (LOB) y de datos de desbordamiento de fila están vinculadas correctamente.    
-   Los índices se encuentran en el orden correcto.    
-   Los punteros son coherentes.    
-   Los datos de cada página son razonables, incluidas las columnas calculadas.    
-   Los desplazamientos de página son razonables.    
-   Cada fila de la tabla base tiene una fila correspondiente en cada índice no agrupado y viceversa.    
-   Todas las filas de un índice o tabla con particiones están en la partición correcta.    
-   Coherencia de nivel de vínculo entre la tabla y el sistema de archivos cuando se almacenan datos **varbinary(max)** en el sistema de archivos mediante FILESTREAM.    
    
## <a name="performing-logical-consistency-checks-on-indexes"></a>Realizar comprobaciones de coherencia lógica en índices    
La comprobación de coherencia lógica en índices varía según el nivel de compatibilidad de la base de datos, tal como se indica a continuación:
-   Si el nivel de compatibilidad es 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) o superior:    
    -   A menos que se especifique NOINDEX, DBCC CHECKTABLE realiza comprobaciones de coherencia física y lógica en una sola tabla y en todos sus índices no agrupado. Sin embargo, en los índices XML, índices espaciales y vistas indizadas solo se realizan comprobaciones de coherencia física de forma predeterminada.    
    -   Si se especifica WITH EXTENDED_LOGICAL_CHECKS, se realizarán comprobaciones lógicas en una vista indizada, en índices XML e índices espaciales, en caso de que los hubiese. De forma predeterminada, las comprobaciones de coherencia física se realizan antes que las comprobaciones de coherencia lógica. Si también se especifica NOINDEX, solamente se realizarán las comprobaciones lógicas.    
         Estas comprobaciones de coherencia lógica realizan una comprobación cruzada de la tabla de índices interna del objeto de índice con la tabla de usuario a la que hace referencia. Para buscar las filas periféricas, se crea una consulta interna que lleve a cabo una intersección completa de las tablas internas y del usuario. La ejecución de esta consulta puede afectar mucho al rendimiento y no se puede realizar el seguimiento de su progreso. Por consiguiente, se recomienda especificar únicamente WITH EXTENDED_LOGICAL_CHECKS si cree que existen problemas del índice que no estén relacionados con daños físicos, o si las sumas de comprobación del nivel de página se han desactivado y sospecha que puedan existir daños de hardware de nivel de columna.    
    -   Si el índice es un índice filtrado, DBCC CHECKDB realizará las comprobaciones de coherencia para comprobar que las entradas de índice satisfacen el predicado de filtro.   
      
- A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], no se ejecutarán de forma predeterminada comprobaciones adicionales en las columnas calculadas persistentes, las columnas UDT y los índices filtrados para evitar evaluaciones de expresiones costosas. Este cambio reduce considerablemente la duración de CHECKDB en bases de datos que contienen estos objetos. A pesar de ello, las comprobaciones de coherencia física de estos objetos siempre se completan. Solo cuando se especifica la opción EXTENDED_LOGICAL_CHECKS se realizarán las evaluaciones de expresiones además de las comprobaciones lógicas ya presentes (vista indexada, índices XML e índices espaciales) como parte de la opción EXTENDED_LOGICAL_CHECKS.
-  Si el nivel de compatibilidad es 90 ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]) o inferior, a menos que se especifique NOINDEX, DBCC CHECKTABLE realizará las comprobaciones de coherencia física y lógica en una única tabla o vista indexada, y en todos sus índices XML y no agrupados. Los índices espaciales no se admiten.
    
 **Conocer el nivel de compatibilidad de una base de datos**    
[Ver o cambiar el nivel de compatibilidad de una base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)    
    
## <a name="internal-database-snapshot"></a>Instantánea de base de datos interna    
DBCC CHECKTABLE utiliza una instantánea de base de datos interna para proporcionar la coherencia transaccional que debe tener para realizar estas comprobaciones. Para más información, vea [Ver el tamaño del archivo disperso de una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) y la sección "Uso de comandos DBCC en instantáneas internas de la base de datos" de [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md).
Si no se puede crear una instantánea, o se ha especificado TABLOCK, DBCC CHECKTABLE adquiere un bloqueo de tabla compartido para lograr la coherencia requerida.
    
> [!NOTE]    
> Si DBCC CHECKTABLE se ejecuta en tempdb, debe adquirir un bloqueo de tabla compartido. Esto es debido a que, por motivos de rendimiento, las instantáneas de base de datos no están disponibles en tempdb. Eso significa que no es posible obtener la coherencia transaccional necesaria.    
    
## <a name="checking-and-repairing-filestream-data"></a>Comprobar y reparar datos de FILESTREAM    
Cuando FILESTREAM está habilitado para una base de datos y una tabla, puede almacenar opcionalmente los objetos binarios grandes (BLOB) **varbinary(max)** en el sistema de archivos. Al utilizar DBCC CHECKTABLE en una tabla que almacena objetos BLOB en el sistema de archivos, DBCC comprueba la coherencia de nivel de vínculo entre el sistema de archivos y la base de datos.
Por ejemplo, si una tabla contiene una columna **varbinary(max)** en la que se usa el atributo FILESTREAM, DBCC CHECKTABLE comprobará que existe una asignación uno a uno entre los directorios del sistema de archivos y los archivos y filas de tabla, las columnas, y los valores de columna. DBCC CHECKTABLE puede reparar el daño producido si especifica la opción de REPAIR_ALLOW_DATA_LOSS. Para reparar el daño de FILESTREAM, DBCC quitará todas las filas de tabla a las que les falten datos del sistema de archivos, así como todos los directorios y archivos no asignados a una fila de tabla, columna o valor de columna.
    
## <a name="checking-objects-in-parallel"></a>Comprobar objetos en paralelo    
De forma predeterminada, DBCC CHECKTABLE realiza comprobaciones paralelas de los objetos. El grado de paralelismo se determina automáticamente mediante el procesador de consultas. El grado de paralelismo máximo se configura de la misma forma que el de las consultas paralelas. Para restringir el número máximo de procesadores disponibles para las comprobaciones DBCC, use [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
La comprobación del paralelismo se puede deshabilitar utilizando el marcador de seguimiento 2528. Para obtener más información, vea [Marcas de seguimiento &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
    
> [!NOTE]    
> Durante una operación DBCC CHECKTABLE, los bytes almacenados en una columna de tipo definido por el usuario ordenada por bytes deben ser iguales a la serialización calculada del valor del tipo definido por el usuario. Si no es así, la rutina DBCC CHECKTABLE indicará un error de coherencia.    
    
## <a name="understanding-dbcc-error-messages"></a>Descripción de los mensajes de error de DBCC    
Cuando finaliza el comando DBCC CHECKTABLE, se escribe un mensaje en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el comando DBCC se ejecuta correctamente, el mensaje lo indica, así como el tiempo de ejecución del comando. Si el comando DBCC se detiene antes de finalizar la comprobación debido a un error, el mensaje indica que el comando se ha cancelado, un valor de estado y el tiempo de ejecución del comando. En la tabla siguiente se muestran y describen los valores de estado que pueden aparecer en el mensaje.
    
|State|Descripción|    
|-----------|-----------------|    
|0|Se ha generado el error número 8930. Indica un daño en los metadatos que provoca la cancelación del comando DBCC.|    
|1|Se ha generado el error número 8967. Error DBCC interno.|    
|2|Error durante una reparación de base de datos en modo de emergencia.|    
|3|Indica un daño en los metadatos que provoca la cancelación del comando DBCC.|    
|4|Se ha detectado una infracción de acceso o aserción.|    
|5|Error desconocido que cancela el comando DBCC.|    
    
## <a name="error-reporting"></a>Informes de errores    
Un archivo de minivolcado (`SQLDUMP*nnnn*.txt`) se crea en el directorio LOG de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cada vez que DBCC CHECKTABLE detecta un error relacionado con datos dañados. Si las características de recopilación de datos de *Uso de la característica* e *Informes de errores* están habilitadas para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el archivo se reenvía automáticamente a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Los datos recopilados se utilizan para mejorar la funcionalidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
El archivo de volcado contiene los resultados del comando DBCC CHECKTABLE y salida de diagnóstico adicional. El archivo tiene listas de control de acceso discrecional (DACL) restringidas. El acceso está limitado a la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y a los miembros del rol sysadmin. De forma predeterminada, el rol sysadmin contiene todos los miembros del grupo BUILTIN\Administradores de Windows y el grupo de administradores local. El comando DBCC no producirá error en caso de que se produzca un error en el proceso de recopilación de datos.
    
## <a name="resolving-errors"></a>Resolver errores    
 Si DBCC CHECKTABLE informa de errores, se recomienda restaurar la base de datos de la copia de seguridad de base de datos en vez de ejecutar REPAIR con una de las opciones de REPAIR. Si no existe una copia de seguridad, la ejecución de REPAIR puede corregir los errores indicados. La opción REPAIR que se debe utilizar se especifica al final de la lista de errores indicados. No obstante, la corrección de errores mediante la opción REPAIR_ALLOW_DATA_LOSS puede eliminar algunas páginas, y por tanto, también algunos datos.    
La reparación se puede realizar en una transacción de usuario para permitirle revertir los cambios que se han realizado. Si se revierten las reparaciones, la base de datos aún contendrá errores por lo que se debe restaurar a partir de una copia de seguridad. Una vez finalizadas todas las reparaciones, realice una copia de seguridad de la base de datos.
    
## <a name="result-sets"></a>Conjuntos de resultados    
DBCC CHECKTABLE devuelve el siguiente conjunto de resultados. Si se especifica solo el nombre de tabla o alguna de las opciones se devuelve el mismo conjunto de resultados.
    
```
DBCC results for 'HumanResources.Employee'.    
There are 288 rows in 13 pages for object 'Employee'.    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
Si se especifica la opción ESTIMATEONLY, DBCC CHECKTABLE devuelve el siguiente conjunto de resultados:
    
```
Estimated TEMPDB space needed for CHECKTABLES (KB)     
--------------------------------------------------     
21    
(1 row(s) affected)    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
## <a name="permissions"></a>Permisos    
El usuario debe ser propietario de la tabla, o bien un miembro del rol fijo de servidor sysadmin o de los roles fijos de base de datos db_owner o db_ddladmin.    
    
## <a name="examples"></a>Ejemplos    
    
### <a name="a-checking-a-specific-table"></a>A. Comprobar una tabla específica    
En el siguiente ejemplo se comprueba la integridad de la página de datos de la tabla `HumanResources.Employee` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee');    
GO    
```    
    
### <a name="b-performing-a-low-overhead-check-of-the-table"></a>B. Ejecutar una comprobación de carga baja de la tabla    
 En el ejemplo siguiente se realiza una comprobación de carga baja de la tabla `Employee` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].    
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee') WITH PHYSICAL_ONLY;    
GO    
```    
    
### <a name="c-checking-a-specific-index"></a>C. Comprobar un índice específico    
 En el siguiente ejemplo se comprueba un índice específico, obtenido mediante un acceso a `sys.indexes`.    
    
```sql    
DECLARE @indid int;    
SET @indid = (SELECT index_id     
              FROM sys.indexes    
              WHERE object_id = OBJECT_ID('Production.Product')    
                    AND name = 'AK_Product_Name');    
DBCC CHECKTABLE ('Production.Product',@indid);    
```    
    
## <a name="see-also"></a>Consulte también    
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)     
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)    
    
  

---
description: DBCC CHECKFILEGROUP (Transact-SQL)
title: DBCC CHECKFILEGROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKFILEGROUP_TSQL
- DBCC_CHECKFILEGROUP_TSQL
- DBCC CHECKFILEGROUP
- CHECKFILEGROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKFILEGROUP statement
- database objects [SQL Server], checking
- allocation checks
- integrity [SQL Server], database objects
- filegroups [SQL Server], consistency checks
- table integrity checks [SQL Server]
- checking database objects
ms.assetid: 8c70bf34-7570-4eb6-877a-e35064a1380a
author: pmasl
ms.author: umajay
ms.openlocfilehash: f95e730dac286be230c27a8e0fc14aa52d07cfee
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813028"
---
# <a name="dbcc-checkfilegroup-transact-sql"></a>DBCC CHECKFILEGROUP (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Comprueba la asignación y la integridad estructural de todas las tablas y vistas indizadas del grupo de archivos especificado de la base de datos actual.
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DBCC CHECKFILEGROUP   
[  
    [ ( { filegroup_name | filegroup_id | 0 }   
        [ , NOINDEX ]   
  ) ]  
    [ WITH   
        {   
            [ ALL_ERRORMSGS | NO_INFOMSGS ]   
            [ , TABLOCK ]   
            [ , ESTIMATEONLY ]  
            [ , PHYSICAL_ONLY ]    
            [ , MAXDOP  = number_of_processors ]  
        }   
    ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *filegroup_name*  
 Es el nombre del grupo de archivos de la base de datos actual para el que se debe comprobar la asignación de tablas y la integridad estructural. Si no se especifica o se especifica 0, el valor predeterminado es el grupo de archivos principal. Los nombres de los grupos de archivos deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
 *filegroup_name* no puede ser un grupo de archivos FILESTREAM.  
  
 *filegroup_id*  
 Es el número de identificación (identificador) del grupo de archivos de la base de datos actual para el que se debe comprobar la asignación de tablas y la integridad estructural.  
  
 NOINDEX  
 Especifica que no se deben realizar comprobaciones intensivas de índices no agrupado para las tablas de usuario. Esto reduce el tiempo total de ejecución. La opción NOINDEX no afecta a las tablas del sistema, ya que DBCC CHECKFILEGROUP siempre comprueba todos los índices de las tablas del sistema.  
  
 ALL_ERRORMSGS  
 Muestra un número ilimitado de errores por objeto. De forma predeterminada, se muestran todos los mensajes de error. Especificar u omitir esta opción no tiene ningún efecto.  
  
 NO_INFOMSGS  
 Suprime todos los mensajes de información.  
  
 TABLOCK  
 Hace que DBCC CHECKFILEGROUP obtenga bloqueos en lugar de utilizar una instantánea de base de datos interna.  
  
 ESTIMATE ONLY  
 Muestra la cantidad de espacio para la base de datos tempdb que se calcula que es necesario para ejecutar DBCC CHECKFILEGROUP con todas las demás opciones especificadas.  
  
 PHYSICAL_ONLY  
 Limita la comprobación de la integridad a la estructura física de la página, los encabezados de registro y la estructura física de árboles b. Se ha diseñado para proporcionar una pequeña comprobación de la sobrecarga de la coherencia física del grupo de archivos; esta comprobación también puede detectar páginas rasgadas y errores de hardware comunes que pueden comprometer los datos. Una ejecución completa de DBCC CHECKFILEGROUP puede tardar mucho más tiempo que en versiones anteriores. Este comportamiento se debe a las razones siguientes:  
 -   Las comprobaciones lógicas son más exhaustivas.  
 -   Algunas de las estructuras subyacentes que hay que comprobar son más complejas.  
 -   Se han agregado muchas comprobaciones nuevas para incluir las nuevas características.  
 Por tanto, el uso de la opción PHYSICAL_ONLY puede llevar mucho menos tiempo para DBCC CHECKFILEGROUP en grupos de archivos grandes y, por ello, se recomienda si se usa frecuentemente en sistemas de producción. Aun así, se recomienda realizar periódicamente una ejecución completa de DBCC CHECKFILEGROUP. La frecuencia de estas ejecuciones depende de factores específicos de cada empresa y de los entornos de producción. PHYSICAL_ONLY siempre implica NO_INFOMSGS y no se permite con ninguna de las opciones de reparación.  
  
> [!NOTE]  
>  Si se especifica PHYSICAL_ONLY, DBCC CHECKFILEGROUP omite todas las comprobaciones de datos de FILESTREAM.  
  
 MAXDOP  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014 SP2 a la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level).  
  
 Invalida la opción de configuración de **grado máximo de paralelismo** de **sp_configure** para la instrucción. MAXDOP puede superar el valor configurado con sp_configure. Si MAXDOP supera el valor configurado con Resource Governor, el motor de base de datos usa el valor MAXDOP de Resource Governor, descrito en ALTER WORKLOAD GROUP (Transact-SQL). Se pueden aplicar todas las reglas semánticas utilizadas con la opción de configuración max degree of parallelism cuando se utiliza la sugerencia de consulta MAXDOP. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!CAUTION]  
>  Si MAXDOP se establece en cero, el servidor elige el grado máximo de paralelismo.  
  
## <a name="remarks"></a>Comentarios  
DBCC CHECKFILEGROUP y DBCC CHECKDB son comandos DBCC similares. La diferencia principal es que DBCC CHECKFILEGROUP se limita al grupo de archivos especificado y a las tablas necesarias.
DBCC CHECKFILEGROUP ejecuta los siguientes comandos:
-   [DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md) del grupo de archivos.  
-   [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) de cada tabla y cada vista indexada del grupo de archivos.  
  
No es necesario ejecutar DBCC CHECKALLOC o DBCC CHECKTABLE independientemente de DBCC CHECKFILEGROUP.
  
## <a name="internal-database-snapshot"></a>Instantánea de base de datos interna  
DBCC CHECKFILEGROUP utiliza una instantánea de base de datos interna para proporcionar la coherencia transaccional que debe tener para realizar estas comprobaciones. Para más información, vea [Ver el tamaño del archivo disperso de una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) y la sección "Uso de comandos DBCC en instantáneas internas de la base de datos" de [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md).
Si no se puede crear una instantánea o se especifica la opción TABLOCK, DBCC CHECKFILEGROUP adquiere bloqueos para obtener la coherencia necesaria. En este caso, se requiere un bloqueo exclusivo de base de datos para realizar las comprobaciones de asignación y se requieren bloqueos compartidos de tabla para realizar las comprobaciones de tabla. TABLOCK hace que DBCC CHECKFILEGROUP se ejecute más rápido en una base de datos con mucha carga, pero disminuye la simultaneidad disponible en la base de datos mientras DBCC CHECKFILEGROUP está en ejecución.
  
> [!NOTE]  
>  Al ejecutar DBCC CHECKFILEGROUP en tempdb no se realiza ninguna comprobación de asignación y debe adquirir bloqueos de tabla compartidos para realizar las comprobaciones de tablas. Esto es debido a que, por motivos de rendimiento, las instantáneas de base de datos no están disponibles en tempdb. Eso significa que no es posible obtener la coherencia transaccional necesaria.  
  
## <a name="checking-objects-in-parallel"></a>Comprobar objetos en paralelo  
De forma predeterminada, DBCC CHECKFILEGROUP realiza comprobaciones paralelas de los objetos. El grado de paralelismo se determina automáticamente mediante el procesador de consultas. El grado máximo de paralelismo se configura igual que las consultas paralelas. Para restringir el número máximo de procesadores disponibles para las comprobaciones DBCC, use [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
La comprobación del paralelismo se puede deshabilitar utilizando el marcador de seguimiento 2528. Para obtener más información, vea [Marcas de seguimiento &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
  
## <a name="nonclustered-indexes-on-separate-filegroups"></a>Índices no clúster en grupos de archivos independientes  
Si un índice no clúster del grupo de archivos especificado está asociado a una tabla de otro grupo de archivos, no se comprueba el índice, ya que la tabla base no está disponible para la validación.
Si una tabla del grupo de archivos especificado tiene un índice no clúster en otro grupo de archivos, dicho índice no se comprueba porque:
-   La estructura de la tabla base no depende de la estructura de un índice no clúster. Los índices no clúster no tienen que ser examinados para validar la tabla base.  
-   El comando DBCC CHECKFILEGROUP solo valida los objetos del grupo de archivos especificado.  
Un índice clúster y una tabla no pueden estar en grupos de archivos diferentes; por lo tanto, las consideraciones anteriores solo se aplican a los índices no clúster.
  
## <a name="partitioned-tables-on-separate-filegroups"></a>Tablas con particiones en grupos de archivos independientes  
Cuando una tabla con particiones existe en varios grupos de archivos, DBCC CHECKFILEGROUP comprueba los conjuntos de filas de las particiones que existen en el grupo de archivos especificado y omite los de los demás. El mensaje informativo 2594 indica las particiones que no se comprobaron. Los índices no clúster que no residan en el grupo de archivos especificado no se comprueban.
  
## <a name="understanding-dbcc-error-messages"></a>Descripción de los mensajes de error de DBCC  
Cuando finaliza el comando DBCC CHECKFILEGROUP, se escribe un mensaje en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el comando DBCC se ejecuta correctamente, el mensaje lo indica, así como el tiempo de ejecución del comando. Si el comando DBCC se detiene antes de finalizar la comprobación debido a un error, el mensaje indica que el comando se ha cancelado, un valor de estado y el tiempo de ejecución del comando. En la tabla siguiente se muestran y describen los valores de estado que pueden aparecer en el mensaje.
  
|State|Descripción|  
|-----------|-----------------|  
|0|Se ha generado el error número 8930. Indica un daño en los metadatos que provoca la cancelación del comando DBCC.|  
|1|Se ha generado el error número 8967. Error DBCC interno.|  
|2|Error durante una reparación de base de datos en modo de emergencia.|  
|3|Indica un daño en los metadatos que provoca la cancelación del comando DBCC.|  
|4|Se ha detectado una infracción de acceso o aserción.|  
|5|Error desconocido que cancela el comando DBCC.|  
  
## <a name="error-reporting"></a>Informes de errores  
Se crea un archivo de minivolcado (SQLDUMP *nnnn*.txt) en el directorio LOG de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siempre que DBCC CHECKFILEGROUP detecta un error relacionado con datos dañados. Si la recopilación de datos de uso de características y la creación informes de errores están habilitadas para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el archivo se reenvía automáticamente a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Los datos recopilados se utilizan para mejorar la funcionalidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
El archivo de volcado contiene los resultados del comando DBCC CHECKFILEGROUP y la salida de diagnóstico adicional. El archivo tiene listas de control de acceso discrecional (DACL) restringidas. El acceso está limitado a la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y a los miembros del rol **sysadmin**. De forma predeterminada, el rol **sysadmin** contiene todos los miembros del grupo BUILTIN\Administradores de Windows y el grupo de administradores local. El comando DBCC no producirá error en caso de que se produzca un error en el proceso de recopilación de datos.
  
## <a name="resolving-errors"></a>Resolver errores  
Si DBCC CHECKFILEGROUP indica algún error, se recomienda restaurar la base de datos a partir de la copia de seguridad. Tenga en cuenta que no se pueden especificar opciones de reparación para DBCC CHECKFILEGROUP.
Si no existe una copia de seguridad, es posible corregir los errores ejecutando DBCC CHECKDB con una opción de reparación especificada. La opción de reparación que se utilizará se especifica al final de la lista de errores. La corrección de errores con la opción REPAIR_ALLOW_DATA_LOSS puede ocasionar la eliminación de algunas páginas y, en consecuencia, de datos.
  
## <a name="result-sets"></a>Conjuntos de resultados  
DBCC CHECKFILEGROUP devuelve el siguiente conjunto de resultados (los valores pueden variar):
-   Excepto cuando se especifica ESTIMATEONLY o NO_INFOMSGS.  
-   Para la base de datos actual, si no se especifica ninguna base de datos, independientemente de si hay o no opciones especificadas (excepto NOINDEX).  
  
```
DBCC results for 'master'.  
DBCC results for 'sys.sysrowsetcolumns'.  
There are 630 rows in 7 pages for object 'sys.sysrowsetcolumns'.  
DBCC results for 'sys.sysrowsets'.  
There are 97 rows in 1 pages for object 'sys.sysrowsets'.  
DBCC results for 'sysallocunits'.  
There are 195 rows in 3 pages for object 'sysallocunits'.  
  
There are 2340 rows in 16 pages for object 'spt_values'.  
DBCC results for 'MSreplication_options'.  
There are 2 rows in 1 pages for object 'MSreplication_options'.  
CHECKFILEGROUP found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
Si se especifica NO_INFOMSGS, DBCC CHECKFILEGROUP devuelve:
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
 Si se especifica ESTIMATEONLY, DBCC CHECKFILEGROUP devuelve (los valores pueden variar):  

```
Estimated TEMPDB space needed for CHECKALLOC (KB)
-------------------------------------------------   
15  
  
(1 row(s) affected)  
  
Estimated TEMPDB space needed for CHECKTABLES (KB)   
--------------------------------------------------   
207  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Permisos  
Debe pertenecer al rol fijo de servidor **sysadmin** o al rol fijo de base de datos **db_owner** .
  
## <a name="examples"></a>Ejemplos  
### <a name="a-checking-the-primary-filegroup-in-the-a-database"></a>A. Comprobar el grupo de archivos principal (PRIMARY) en la base de datos  
En este ejemplo, se comprueba el grupo de archivos principal de la base de datos actual.
  
```sql
DBCC CHECKFILEGROUP;  
GO  
```  
  
### <a name="b-checking-the-adventureworks-primary-filegroup-without-nonclustered-indexes"></a>B. Comprobar el grupo de archivos principal (PRIMARY) de AdventureWorks sin índices no clúster  
En el ejemplo siguiente, se comprueba el grupo de archivos principal de la base de datos `AdventureWorks2012` (excluidos los índices no agrupados). Para ello, se especifica el número de identificación del grupo de archivos principal y la opción `NOINDEX`.
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKFILEGROUP (1, NOINDEX);  
GO  
```  
  
### <a name="c-checking-the-primary-filegroup-with-options"></a>C. Comprobar el grupo de archivos principal (PRIMARY) con opciones  
En el ejemplo siguiente se comprueba el grupo de archivos principal de la base de datos `master` y se especifica la opción `ESTIMATEONLY`.
  
```sql
USE master;  
GO  
DBCC CHECKFILEGROUP (1)  
WITH ESTIMATEONLY;  
```  
  
## <a name="see-also"></a>Consulte también  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[FILEGROUP_ID &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-id-transact-sql.md)  
[sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)  
[sp_helpfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)  
[sys.sysfilegroups &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysfilegroups-transact-sql.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKALLOC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)
  

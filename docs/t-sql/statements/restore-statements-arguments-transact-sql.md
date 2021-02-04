---
description: 'Instrucciones RESTORE: argumentos (Transact-SQL)'
title: RESTORE (argumentos, Transact-SQL)
ms.custom: ''
ms.date: 08/08/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE statement, arguments
- RESTORE statement
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c07e6ba84a511d81e1fb17718552a0ca2738271a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236237"
---
# <a name="restore-statements---arguments-transact-sql"></a>Instrucciones RESTORE: argumentos (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se documentan los argumentos descritos en las secciones "Sintaxis" de la instrucción RESTORE {DATABASE|LOG} y del conjunto de instrucciones auxiliares asociado: RESTORE FILELISTONLY, RESTORE HEADERONLY, RESTORE LABELONLY, RESTORE REWINDONLY y RESTORE VERIFYONLY. Solo un subconjunto de estas seis instrucciones admite la mayoría de los argumentos. En la descripción del argumento se indica si éste está admitido.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
 Para comprobar la sintaxis, vea los siguientes temas:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 DATABASE  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica la base de datos de destino. Si se especifica una lista de archivos o de grupos de archivos, solo se restauran esos archivos y grupos de archivos.  
  
 En el caso de una base de datos que utilice el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere en la mayoría de los casos que realice una copia de seguridad de registros después del error antes de restaurar la base de datos. Restaurar una base de datos sin hacer primero una copia del final del registro produce un error, a menos que la instrucción RESTORE DATABASE contenga una cláusula WITH REPLACE o WITH STOPAT, que deben especificar un tiempo o una transacción producidos después de finalizar la copia de seguridad de los datos. Para obtener más información sobre las copias del final del registro, vea [Copias del final del registro &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 REGISTRO  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica que se va a aplicar una copia de seguridad de registros de transacciones a esta base de datos. Los registros de transacciones deben aplicarse en orden secuencial. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprueba el registro de transacciones del que se ha realizado la copia de seguridad para asegurarse de que las transacciones se cargan en la base de datos y en la secuencia correctas. Para aplicar varios registros de transacciones, utilice la opción NORECOVERY en todas las operaciones de restauración, excepto en la última.  
  
> [!NOTE]  
>  Normalmente, el último registro que se restaura es el que corresponde a la copia del final del registro. Una *copia del final del registro* es una copia de seguridad de registros que se realiza justo antes de restaurar una base de datos, normalmente después de que se haya producido un error en ella. Al realizar una copia del final del registro de una base de datos posiblemente dañada se evita la pérdida de trabajo, ya que se captura el registro que todavía no se había incluido en la copia de seguridad (el final del registro). Para obtener más información, vea [Copias del final del registro &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 Para obtener más información, vea [Aplicar copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
 { _database\_name_ |  **@** _database\_name\_var_}  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Es la base de datos en la que se restaura el registro o la base de datos completa. Si se proporciona como una variable ( **@** _database\_name\_var_), este nombre se puede especificar como una constante de cadena ( **@** _database\_name\_var_ = *database*\_*name*) o como una variable de tipo de datos de cadena de caracteres, excepto para los tipos de datos **ntext** o **text**.  
  
 \<file_or_filegroup_or_page> [ **,** ...*n* ]  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica el nombre de un archivo, un grupo de archivos o una página lógicos que se va a incluir en una instrucción RESTORE DATABASE o RESTORE LOG. Puede especificar una lista de archivos o grupos de archivos.  
  
 En una base de datos que usa el modelo de recuperación simple, las opciones FILE y FILEGROUP solo se admiten si los archivos o grupos de archivos de destino son de solo lectura, o bien si se trata de una restauración de tipo PARTIAL (el resultado es un [grupo de archivos inactivo](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)).  
  
 Para una base de datos que utilice el modelo de recuperación completa o de recuperación optimizado para cargas masivas de registros, después de utilizar RESTORE DATABASE para restaurar uno o más archivos, grupos de archivos o páginas, normalmente se debe aplicar el registro de transacciones a los archivos que contienen los datos restaurados; al aplicarles el registro, esos archivos guardarán coherencia con el resto de la base de datos. Existen las siguientes excepciones:  
  
-   Si los archivos que se restauran eran de solo lectura antes de su última copia de seguridad, no debe aplicarse un registro de transacciones, y la instrucción RESTORE le informará de esta situación.  
  
-   Si la copia de seguridad contiene el grupo de archivos principal y se va a realizar una restauración parcial. En este caso, el registro de restauración no es necesario, porque se restaura automáticamente desde el conjunto de copia de seguridad.  
  
FILE **=** { *logical_file_name_in_backup*|  **@** _logical\_file\_name\_in\_backup\_var_}  
 Especifica el nombre de un archivo que se va a incluir en la restauración de la base de datos.  
  
FILEGROUP **=** { *logical_filegroup_name* |  **@** _logical\_filegroup\_name\_var_ }  
 Especifica el nombre de un grupo de archivos que se va a incluir en la restauración de la base de datos.  
  
 **Nota:** FILEGROUP solo se admite en el modelo de recuperación simple si el grupo de archivos especificado es de solo lectura y se trata de una restauración parcial, es decir, si se usa WITH PARTIAL. Los grupos de archivos de lectura y escritura sin restaurar se marcan como inactivos y, en consecuencia, no se podrán restaurar en la base de datos resultante.  
  
READ_WRITE_FILEGROUPS  
 Selecciona todos los grupos de archivos de lectura y escritura. Esta opción es especialmente útil cuando se desean restaurar grupos de archivos de solo lectura después de los grupos de archivos de lectura y escritura anteriores a los grupos de archivos de solo lectura.  
  
PAGE = **'** _file_ **:** _page* [ **,** ...*n* ] **'**  
 Especifica una lista de una o varias páginas para una restauración de página (que solo se admite en bases de datos que utilizan los modelos de recuperación completa o de recuperación optimizado para cargas masivas de registros). Éstos son sus valores:  
  
PAGE  
 Indica una lista de uno o varios archivos y páginas.  
  
 *file*  
 Es el Id. del archivo que contiene la página específica que se va a restaurar.  
  
 *page*  
 Es el Id. de la página que se va a restaurar en el archivo.  
  
 *n*  
 Es un marcador de posición que indica que se pueden especificar varias páginas.  
  
 El número máximo de páginas que se pueden restaurar en cualquier archivo único en una secuencia de restauración es 1000. Sin embargo, si la cantidad de páginas dañadas en un archivo no es pequeña, plantéese restaurar el archivo entero en lugar de las páginas.  
  
> [!NOTE]  
>  Las restauraciones de página no se recuperan nunca.  
  
 Para más información sobre la restauración de páginas, vea [Restaurar páginas &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).  
  
 [ **,** ...*n* ]  
 Es un marcador de posición que indica que se pueden especificar varios archivos y grupos de archivos y páginas en una lista separada por comas. El número es ilimitado.  
  
FROM { \<backup_device> [ **,** ...*n* ]| \<database_snapshot> } Normalmente, especifica los dispositivos de copia de seguridad desde los que se restaura la copia de seguridad. Alternativamente, en una instrucción RESTORE DATABASE, la cláusula FROM puede especificar el nombre de una instantánea de base de datos a la que va a revertir la base de datos, en cuyo caso no se admite ninguna cláusula WITH.  
  
 Si se omite la cláusula FROM, no se produce la restauración de la copia de seguridad. En su lugar, se recupera la base de datos. Esto permite recuperar una base de datos restaurada con la opción NORECOVERY o cambiar a un servidor en espera. Si se omite la cláusula FROM, se debe especificar NORECOVERY, RECOVERY o STANDBY en la cláusula WITH.  
  
 \<backup_device> [ **,** ...*n* ] Especifica los dispositivos de copia de seguridad físicos o lógicos que se usan para la operación de restauración.  
  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md), [RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 \<backup_device>::= Especifica un dispositivo de copia de seguridad físico o lógico que se va a usar para la operación de copia de seguridad de la manera siguiente:  
  
 { _logical\_backup\_device\_name_ |  **@** _logical\_backup\_device\_name\_var_ }  
 Es el nombre lógico, que debe seguir las reglas de los identificadores, de los dispositivos de copia de seguridad creados por **sp_addumpdevice** desde los que se restaura la base de datos. Si se proporciona como una variable ( **@** _logical\_backup\_device\_name\_var_), el nombre del dispositivo de copia de seguridad se puede especificar como una constante de cadena ( **@** _logical\_backup\_device\_name\_var_ = _logical\_backup\_device\_name_) o como una variable de tipo de datos de cadena de caracteres, excepto para los tipos de datos **ntext** o **text**.  
  
 {DISK | TAPE } **=** { **'** _physical\_backup\_device\_name_ **'**  |  **@** _physical\_backup\_device\_name\_var_ }  
 Permite restaurar las copias de seguridad guardadas en el dispositivo de disco o cinta con nombre. Los tipos de dispositivo de disco y cinta deben especificarse con el nombre real (por ejemplo, nombre de archivo y ruta de acceso completa) del dispositivo: `DISK ='Z:\SQLServerBackups\AdventureWorks.bak'` o `TAPE ='\\\\.\TAPE0'`. Si se especifica como una variable ( **@** _physical\_backup\_device\_name\_var_), el nombre del dispositivo se puede especificar como una constante de cadena ( **@** _physical\_backup\_device\_name\_var_ = "*physical_backup_device_name*") o como una variable de tipo de datos de cadena de caracteres, excepto para los tipos de datos **ntext** o **text**.  
  
 Si utiliza un servidor de red con un nombre UNC (que debe contener el nombre del equipo), especifique un tipo de dispositivo de disco. Para más información sobre cómo usar los nombres UNC, vea [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 La cuenta con la que está ejecutando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener acceso de lectura al equipo remoto o servidor de red para poder realizar la operación RESTORE.  
  
 *n*  
 Es un marcador de posición que indica que se pueden especificar hasta 64 dispositivos de copia de seguridad en una lista separada por comas.  
  
 El hecho de que una secuencia de restauración requiera tantos dispositivos de copia de seguridad como los utilizados para crear el conjunto de medios al que pertenecen las copias de seguridad depende de si la restauración se realiza sin conexión o en línea de la forma siguiente.  
  
-   La restauración sin conexión permite restaurar una copia de seguridad con menos dispositivos que los utilizados para crear la copia de seguridad.  
  
-   La restauración en línea necesita todos los dispositivos de copia de seguridad para la copia de seguridad. Si se intenta la restauración con menos dispositivos, se obtiene un error.  
  
 Por ejemplo, suponga que se realiza la copia de seguridad de una base de datos en cuatro unidades de cinta conectadas al servidor. Una restauración en línea requiere que las cuatro unidades estén conectadas al servidor, mientras que una restauración sin conexión permite restaurar la copia de seguridad si hay menos de cuatro unidades en el equipo.  
  
> [!NOTE]  
>  Si se restaura una copia de seguridad desde un conjunto de medios reflejado, solo puede especificar un reflejo para cada familia de medios. No obstante, si hay errores, el hecho de tener otros reflejos habilita la resolución de algunos problemas de restauración rápidamente. Puede sustituir un volumen de medios dañado con el volumen correspondiente de otro reflejo. Tenga en cuenta que para las restauraciones sin conexión, puede restaurar desde menos dispositivos que familias de medios, pero cada familia se procesa solo una vez.  
  
\<database_snapshot>::=  
**Compatible con:**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
DATABASE_SNAPSHOT **=** _database\_snapshot\_name_  
 Revierte la base de datos a la instantánea de base de datos especificada por *database_snapshot_name*. La opción DATABASE_SNAPSHOT solo está disponible para una restauración de base de datos completa. En una operación de reversión, la instantánea de base de datos ocupa el lugar de una copia de seguridad de base de datos completa.  
  
 En una operación de reversión se requiere que la instantánea de base de datos especificada sea la única en la base de datos. Durante la operación de reversión, la instantánea de base de datos y la base de datos de destino se marcan como `In restore`. Para más información, vea la sección "Comentarios" de [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
### <a name="with-options"></a>Opciones de WITH  
 Especifica las opciones que han de utilizarse en una operación de restauración. Para obtener un resumen de las instrucciones que utilizan cada opción, vea "Resumen de compatibilidad para las opciones de WITH", más adelante en este tema.  
  
> [!NOTE]  
>  Las opciones de WITH se organizan aquí en el mismo orden que en la sección "Sintaxis" de [RESTORE {DATABASE|LOG}](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 PARTIAL  
 **Compatible con:**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica una operación de restauración parcial que solo restaura el grupo de archivos principal y cualquiera de los grupos de archivos secundarios especificados. La opción PARTIAL selecciona implícitamente el grupo de archivos principal; por tanto, no es necesario especificar FILEGROUP = 'PRIMARY'. Para restaurar un grupo de archivos secundario, debe especificarlo de forma explícita mediante la opción FILE o FILEGROUP.  
  
 La opción PARTIAL no se permite en las instrucciones RESTORE LOG.  
  
 La opción PARTIAL abre la fase inicial de una restauración por etapas, que permite restaurar los grupos de archivos restantes más adelante. Para obtener más información, vea [Restauraciones por etapas &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
 [ **RECOVERY** | NORECOVERY | STANDBY ]  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 **RECOVERY**  
 Indica a la operación de restauración que revierta las transacciones no confirmadas. Después del proceso de recuperación, la base de datos está preparada para ser utilizada. Si no se especifica NORECOVERY, RECOVERY o STANDBY, la opción predeterminada es RECOVERY.  
  
 Si las siguientes operaciones RESTORE (RESTORE LOG o RESTORE DATABASE a partir de una copia de seguridad diferencial) están planeadas, se debe especificar en su lugar NORECOVERY o STANDBY.  
  
 Al restaurar los conjuntos de copia de seguridad de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede ser necesaria una actualización de la base de datos. Esta actualización se realiza automáticamente al especificar WITH RECOVERY. Para obtener más información, vea [Aplicar copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
> [!NOTE]  
>  Si se omite la cláusula FROM, se debe especificar NORECOVERY, RECOVERY o STANDBY en la cláusula WITH.  
  
 NORECOVERY  
 Indica a la operación de restauración que no revierta las transacciones no confirmadas. Si debe aplicarse otro registro de transacciones más adelante, especifique la opción NORECOVERY o STANDBY. Si no se especifica NORECOVERY, RECOVERY o STANDBY, la opción predeterminada es RECOVERY. Si se utiliza la opción NORECOVERY durante una operación de restauración sin conexión, la base de datos no puede utilizarse.  
  
 Para restaurar una copia de seguridad de base de datos y uno o varios registros de transacciones, o siempre que sean necesarias varias instrucciones RESTORE (por ejemplo, para restaurar una copia de seguridad de base de datos completa seguida de una copia de seguridad de base de datos diferencial), se requiere la opción WITH NORECOVERY en todas las instrucciones RESTORE, menos en la última. Se recomienda utilizar WITH NORECOVERY en todas las instrucciones de una secuencia de restauración de varios pasos hasta que se llegue al punto de recuperación deseado y, después, utilizar una instrucción RESTORE WITH RECOVERY aparte solo para la restauración.  
  
 Cuando se utiliza en la operación de restauración de un archivo o grupo de archivos, NORECOVERY obliga a la base de datos a permanecer en estado de restauración después de la operación de restauración. Esto resulta útil en cualquiera de las situaciones siguientes:  
  
-   Se está ejecutando un script de restauración y se aplica el registro en todo momento.  
  
-   Se está utilizando una secuencia de restauraciones de archivos y no está previsto que la base de datos se pueda utilizar entre dos de las operaciones de restauración.  
  
 En algunos casos, RESTORE WITH NORECOVERY actualiza el conjunto de puestas al día lo suficiente para que sea coherente con la base de datos. En esos casos, no se produce la reversión y los datos permanecen sin conexión, como cabe esperar con esta opción. No obstante, [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un mensaje informativo donde indica que el conjunto de puestas al día se puede recuperar mediante la opción RECOVERY.  
  
STANDBY **=** _standby\_file\_name_  
 Especifica un archivo en espera que permite deshacer los efectos de la recuperación. La opción STANDBY se puede utilizar en operaciones de restauración sin conexión (incluida la restauración parcial). Esta opción no se permite en operaciones de restauración en línea. Si se intenta especificar la opción STANDBY para una operación de restauración en línea, se producirá un error en la operación. STANDBY tampoco se permite cuando es necesario actualizar una base de datos.  
  
 El archivo en espera se utiliza para mantener una imagen previa de "copia en escritura" de las páginas modificadas durante la fase de deshacer de una operación RESTORE WITH STANDBY. El archivo en espera permite abrir una base de datos para el acceso de solo lectura entre las restauraciones del registro de transacciones, y utilizarla cuando haya un servidor en espera semiactiva o en situaciones de recuperación especiales en las que resulte útil inspeccionar la base de datos entre las restauraciones del registro. Después de una operación RESTORE WITH STANDBY, el archivo para deshacer se elimina automáticamente en la siguiente operación RESTORE. Si elimina el archivo en espera de forma manual antes de la siguiente operación RESTORE, deberá volver a restaurar la base de datos completa. Mientras la base de datos está en estado STANDBY, debe tratar el archivo en espera con el mismo cuidado que cualquier otro archivo de base de datos. A diferencia de lo que sucede con los demás archivos de base de datos, [!INCLUDE[ssDE](../../includes/ssde-md.md)] mantiene abierto este archivo solo durante las operaciones de restauración activas.  
  
 *standby_file_name* especifica un archivo en espera cuya ubicación se almacena en el registro de la base de datos. Si hay otro archivo con el mismo nombre, se sobrescribe; en caso contrario, [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea el archivo.  
  
 Los requisitos de tamaño de un archivo en espera determinado dependen del volumen de las acciones de deshacer resultantes de las transacciones no confirmadas durante la operación de restauración.  
  
> [!IMPORTANT]  
>  Si la unidad que contiene el archivo en espera especificado se queda sin espacio de disco, la operación de restauración se detiene.  
  
 Para obtener una comparación de RECOVERY y NORECOVERY, vea la sección "Comentarios" de [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
LOADHISTORY  
 **Compatible con:**  [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Especifica que la operación de restauración cargará la información en las tablas de historial de **msdb**. La opción LOADHISTORY carga en las tablas de historial de copias de seguridad y restauración de la base de datos **msdb** la información sobre las copias de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] almacenadas en el conjunto de medios para el conjunto de copia de seguridad que se comprueba. Para más información sobre las tablas de historial, vea [System Tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md) (Tablas del sistema [Transact-SQL]).  
  
#### <a name="general_with_options--n-"></a>\<general_WITH_options> [ ,...n ]  
 Las opciones generales de WITH son todas compatibles con las instrucciones RESTORE DATABASE y RESTORE LOG. Algunas de estas opciones también son compatibles con una o varias instrucciones auxiliares, tal y como se indica aquí.  
  
##### <a name="restore-operation-options"></a>Opciones de la operación de restauración  
 Estas opciones afectan al comportamiento de la operación de restauración.  
  
MOVE **'** _logical\_file\_name\_in\_backup_ **'** TO **'** _operating\_system\_file\_name_ **'** [ ...*n* ]  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Especifica que el archivo de datos o de registro cuyo nombre lógico está especificado por *logical_file_name_in_backup* debe moverse restaurándolo en la ubicación especificada por *operating_system_file_name*. El nombre de archivo lógico de un archivo de datos o de registro de un conjunto de copia de seguridad coincide con el nombre lógico que tenía en la base de datos cuando se creó el conjunto de copia de seguridad.  
  
*n* es un marcador de posición que indica que puede especificar instrucciones MOVE adicionales. Especifique una instrucción MOVE para cada archivo lógico del conjunto de copia de seguridad que desee restaurar en otra ubicación. El archivo *logical_file_name_in_backup* se restaura de forma predeterminada en su ubicación original.  
  
> [!NOTE]  
>  Utilice RESTORE FILELISTONLY para obtener una lista de los archivos lógicos del conjunto de copia de seguridad.  
  
 Si se utiliza la instrucción RESTORE para reubicar una base de datos en el mismo servidor o para copiarla en uno diferente, puede que sea necesario utilizar la opción MOVE para volver a ubicar los archivos de base de datos, con el fin de evitar conflictos con los archivos existentes.  
  
 Cuando se utiliza con RESTORE LOG, la opción MOVE se puede utilizar solo para reubicar los archivos agregados durante la restauración del registro. Por ejemplo, si la copia de seguridad de registros contiene una operación para agregar el archivo `file23`, este archivo se puede reubicar utilizando la opción MOVE en RESTORE LOG.  
  
 Cuando se usa con la copia de seguridad de instantáneade [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la opción MOVE solo se puede usar para reubicar archivos en un blob de Azure dentro de la misma cuenta de almacenamiento que el blob original. La opción MOVE no se puede usar para restaurar la copia de seguridad de instantánea en un archivo local o en otra cuenta de almacenamiento.  
  
 Si se utiliza la instrucción RESTORE VERIFYONLY cuando se tiene previsto reubicar una base de datos en el mismo servidor o copiarla en uno diferente, puede que sea necesario utilizar la opción MOVE para comprobar si hay espacio suficiente disponible en el destino y para identificar los posibles conflictos con los archivos existentes.  
  
 Para obtener más información, vea [Copiar bases de datos con Copias de seguridad y restauración](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
CREDENTIAL  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 y versiones posteriores
  
 Se usa solo al restaurar una copia de seguridad desde el servicio Microsoft Azure Blob Storage.  
  
> [!NOTE]  
>  Con [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 y hasta [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], las restauraciones solo se pueden realizar desde un único dispositivo al restaurar desde una dirección URL. Para realizar una restauración a partir de varios dispositivos, al hacerlo de una URL, debe usar [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], o una versión posterior, y tokens de firma de acceso compartido (SAS). Para más información, vea [Habilitar la copia de seguridad administrada de SQL Server en Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md) y [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Simplificación de la creación de credenciales de SQL con tokens de firmas de acceso compartido [SAS] en Almacenamiento de Azure con PowerShell).  
  
 REPLACE  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe crear la base de datos especificada y sus archivos relacionados aunque ya exista otra base de datos con el mismo nombre. En ese caso, se elimina la base de datos existente. Si no se especifica la opción REPLACE, se realiza una comprobación de seguridad. Así se evita sobrescribir por accidente una base de datos distinta. La comprobación de seguridad garantiza que la instrucción RESTORE DATABASE no restaurará la base de datos en el servidor actual si se dan las dos condiciones siguientes:  
  
-   La base de datos nombrada en la instrucción RESTORE ya existe en el servidor actual.  
  
-   El nombre de la base de datos es diferente del nombre de la base de datos registrado en el conjunto de copia de seguridad.  
  
 REPLACE también permite que RESTORE sobrescriba un archivo existente cuando no se puede comprobar si pertenece a la base de datos que se está restaurando. Normalmente, RESTORE no sobrescribe los archivos existentes. WITH REPLACE también se puede utilizar de la misma forma para la opción RESTORE LOG.  
  
 REPLACE elimina el requisito de que se realice una copia del final del registro antes de restaurar la base de datos.  
  
 Para más información sobre el impacto de usar la opción REPLACE, vea [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
RESTART  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe reiniciar una operación de restauración que se ha interrumpido. RESTART reinicia la operación de restauración en el punto en que se interrumpió.  
  
RESTRICTED_USER  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 Restringe el acceso a la base de datos recién restaurada a los miembros de los roles **db_owner**, **dbcreator** o **sysadmin**.  RESTRICTED_USER sustituye a la opción DBO_ONLY. DBO_ONLY no está incluida en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 Utilícela con la opción RECOVERY.  
  
##### <a name="backup-set-options"></a>Opciones de conjunto de copia de seguridad  
 Estas opciones funcionan en el conjunto de copia de seguridad que contiene la copia de seguridad que se va a restaurar.  
  
FILE **=** { *backup_set_file_number* |  **@** _backup\_set\_file\_number_ }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Identifica el conjunto de copia de seguridad que se va a restaurar. Por ejemplo, si *backup_set_file_number* es **1** , indica el primer conjunto de copia de seguridad del medio de copia, y si *backup_set_file_number* es **2** , indica el segundo conjunto de copia de seguridad. Puede obtener el valor *backup_set_file_number* de un conjunto de copia de seguridad mediante la instrucción [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
 Cuando no se especifica, el valor predeterminado es **1**, excepto en el caso de RESTORE HEADERONLY, que procesa todos los conjuntos de copia de seguridad de los medios. Para obtener más información, vea "Especificar un conjunto de copia de seguridad" más adelante en este tema.  
  
> [!IMPORTANT]  
>  Esta opción FILE no está relacionada con la opción FILE para especificar un archivo de base de datos, FILE **=** { *logical_file_name_in_backup* |  **@** _logical\_file\_name\_in\_backup\_var_ }.  
  
 PASSWORD  **=** { *password* |  **@** _password\_variable_ }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Proporciona la contraseña del conjunto de copia de seguridad. Una contraseña de conjunto de copia de seguridad es una cadena de caracteres.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Si se especificó una contraseña al crear el conjunto de copia de seguridad, ésta es necesaria para realizar operaciones de restauración desde ese conjunto de copia de seguridad. Es un error especificar una contraseña incorrecta o especificar una contraseña si el conjunto de copia de seguridad no tiene ninguna.  
  
> [!IMPORTANT]  
>  Esta contraseña proporciona un nivel de protección bajo para el conjunto de medios. Para obtener más información, vea la sección Permisos de la instrucción correspondiente.  
  
##### <a name="media-set-options"></a>Opciones de conjuntos de medios  
 Estas opciones funcionan para todo el conjunto de medios.  
  
 MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_}  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Especifica el nombre de los medios. Si se proporciona, el nombre de los medios debe coincidir con el nombre de los medios de los volúmenes de copia de seguridad; en caso contrario, la operación de restauración finaliza. Si no se proporciona el nombre de los medios en la instrucción RESTORE, no se comprueba que éste coincida con el nombre de los medios de los volúmenes de copia de seguridad.  
  
> [!IMPORTANT]  
>  La utilización coherente de nombres de medios en las operaciones de copias de seguridad y restauración proporciona una comprobación adicional de seguridad del medio seleccionado para la operación de restauración.  
  
 MEDIAPASSWORD **=** { *mediapassword* |  **@** _mediapassword\_variable_ }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Proporciona la contraseña del conjunto de medios. Una contraseña de conjunto de medios es una cadena de caracteres.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Si se proporcionó una contraseña al dar formato al conjunto de medios, ésta será necesaria para tener acceso a cualquier conjunto de copia de seguridad de ese conjunto de medios. Es un error especificar una contraseña incorrecta o especificar una contraseña si el conjunto de medios no tiene ninguna.  
  
> [!IMPORTANT]  
>  Esta contraseña proporciona un nivel de protección bajo para el conjunto de medios. Para más información, vea la sección "Permisos" de la instrucción correspondiente.  
  
 BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica el tamaño de bloque físico, en bytes. Los tamaños admitidos son 512, 1024, 2048, 4096, 8192, 16384, 32768 y 65536 (64 KB) bytes. El valor predeterminado es 65536 para dispositivos de cinta y 512 para otros dispositivos. Normalmente, esta opción no es necesaria, ya que RESTORE selecciona automáticamente un tamaño de bloque apropiado para el dispositivo. La especificación explícita de un tamaño de bloque invalida la selección automática del tamaño de bloque.  
  
 Si va a restaurar una copia de seguridad desde un CD-ROM, especifique BLOCKSIZE=2048.  
  
> [!NOTE]  
>  Normalmente, esta opción solo afecta al rendimiento al leer desde dispositivos de cinta.  
  
##### <a name="data-transfer-options"></a>Opciones de transferencia de datos  
 Las opciones le permiten optimizar la transferencia de datos desde el dispositivo de copia de seguridad.  
  
 BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica el número total de búferes de E/S que se van a utilizar para la operación de restauración. Puede especificar cualquier entero positivo; no obstante, un número de búferes demasiado grande podría provocar errores de "memoria insuficiente" a causa de un espacio de direcciones virtuales inadecuado en el proceso Sqlservr.exe.  
  
 El espacio total usado por los búferes viene determinado por: _buffercount_* *\** _maxtransfersize_.  
  
 MAXTRANSFERSIZE **=** { _maxtransfersize_ |  **@** _maxtransfersize\_variable_ }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Especifica la unidad de transferencia más grande (en bytes) que se debe utilizar entre el medio de copia de seguridad y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los valores posibles son múltiplos de 65536 bytes (64 KB), hasta un máximo de 4194304 bytes (4 MB).  
> [!NOTE]  
>  Cuando la base de datos tiene FILESTREAM configurado o incluye grupos de archivos con optimización para memoria, el valor de `MAXTRANSFERSIZE` en el momento de una restauración debe ser mayor o igual que el valor que se usó cuando se creó la copia de seguridad.  
  
##### <a name="error-management-options"></a>Operaciones de administración de errores  
 Estas opciones permiten determinar si se habilitarán las sumas de comprobación de copia de seguridad para la operación de restauración y si ésta se detiene al encontrar un error.    
  
 { CHECKSUM | NO_CHECKSUM }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 El comportamiento predeterminado es comprobar las sumas de comprobación, si están presentes, y continuar sin esta comprobación si no lo están.  
  
 CHECKSUM  
 Especifica que se comprueben las sumas de comprobación de copia de seguridad y que, si la copia de seguridad no dispone de ellas, la operación de restauración genere un error, con un mensaje que indique que no hay sumas de comprobación presentes.  
  
> [!NOTE]  
>  Las sumas de comprobación de la página solo son pertinentes para las operaciones de copia de seguridad si se utilizan sumas de comprobación de copia de seguridad.  
  
 De manera predeterminada, al encontrar una suma de comprobación no válida, RESTORE informa de un error de suma de comprobación y se detiene. Sin embargo, si se especifica CONTINUE_AFTER_ERROR, RESTORE continuará después de devolver el error de suma de comprobación y el número de la página que contiene la suma de comprobación que no es válida, si el daño lo permite.  
  
 Para más información sobre cómo trabajar con sumas de comprobación de copia de seguridad, vea [Errores posibles de medios durante copia de seguridad y restauración &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
 NO_CHECKSUM  
 Deshabilita explícitamente la validación de las sumas de comprobación en la operación de restauración.  
  
 { **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 STOP_ON_ERROR  
 Especifica que la operación de restauración debe detenerse con el primer error encontrado. Es el comportamiento predeterminado de RESTORE, excepto en el caso de VERIFYONLY, cuyo comportamiento predeterminado es CONTINUE_AFTER_ERROR.  
  
 CONTINUE_AFTER_ERROR  
 Especifica que la operación de restauración debe continuar después de encontrar un error.  
  
 Si una copia de seguridad contiene páginas dañadas, se recomienda repetir la operación de restauración usando una copia de seguridad alternativa que no contenga esos errores (por ejemplo, una copia de seguridad realizada antes de que se produjeran daños en las páginas). Sin embargo, como último recurso, puede restaurar la copia de seguridad dañada con la opción CONTINUE_AFTER_ERROR de la instrucción de restauración e intentar recuperar los datos.  
  
##### <a name="filestream-options"></a>Opciones de FILESTREAM  
 FILESTREAM ( DIRECTORY_NAME =*directory_name* )  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
**Válido para** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.
  
 Un nombre de directorio compatible con Windows. Este nombre debe ser único entre todos los nombres de directorio de FILESTREAM de base de datos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comparación de unicidad se realiza sin distinción entre mayúsculas y minúsculas, independientemente de la configuración de intercalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##### <a name="monitoring-options"></a>Opciones de supervisión  
 Estas opciones le permiten supervisar la transferencia de datos desde el dispositivo de copia de seguridad.  
  
 STATS [ **=** _percentage_ ]  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Muestra un mensaje cada vez que se completa otro porcentaje; se utiliza para indicar el progreso. Si *percentage* se omite, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muestra un mensaje después de completar cada 10 por ciento (aproximadamente).  
  
 La opción STATS informa del porcentaje completado desde el umbral para informar del próximo intervalo. Esto sucede más o menos en el porcentaje especificado. Por ejemplo, con STATS=10, [!INCLUDE[ssDE](../../includes/ssde-md.md)] informa aproximadamente en ese intervalo; en lugar de mostrar exactamente el 40%, la opción podría mostrar el 43%. En el caso de los conjuntos de copia de seguridad de gran tamaño, esto no representa ningún problema porque el porcentaje completado se mueve muy lentamente entre las llamadas de E/S.  
  
##### <a name="tape-options"></a>Opciones de cinta  
 Estas opciones solo se utilizan para dispositivos de cinta. Se omitirán si se utiliza otro tipo de dispositivo.  
  
 { **REWIND** | NOREWIND }  
 Estas opciones solo se utilizan para dispositivos de cinta. Se omitirán si se usa otro tipo de dispositivo.  
  
 REWIND  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Especifica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] liberará y rebobinará la cinta. REWIND es la opción predeterminada.  
  
 NOREWIND  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Si se especifica NOREWIND en alguna otra instrucción de restauración, se produce un error.  
  
 Especifica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantendrá la cinta abierta tras la operación de copia de seguridad. Puede utilizar esta opción para mejorar el rendimiento al realizar varias operaciones de copia de seguridad en una cinta.  
  
 NOREWIND implica NOUNLOAD, y estas opciones son incompatibles en una sola instrucción RESTORE.  
  
> [!NOTE]  
>  Si usa NOREWIND, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserva la propiedad de la unidad de cinta hasta que una instrucción BACKUP o RESTORE que se ejecuta en el mismo proceso usa la opción REWIND o UNLOAD, o bien se cierra la instancia del servidor. Mantener abierta la cinta evita que otros procesos obtengan acceso a la misma. Para más información sobre cómo mostrar una lista de cintas abiertas y cómo cerrar una cinta abierta, vea [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 { **UNLOAD** | NOUNLOAD }  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md), [RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md) y [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Estas opciones solo se utilizan para dispositivos de cinta. Se omitirán si se usa otro tipo de dispositivo.  
  
> [!NOTE]  
>  UNLOAD/NOUNLOAD es una configuración de sesión que persiste mientras dure la sesión o hasta que se restablezca especificando la alternativa.  
  
 UNLOAD  
 Especifica que la cinta se rebobina y descarga automáticamente al terminar la copia de seguridad. UNLOAD es el valor predeterminado cuando se inicia una sesión.  
  
 NOUNLOAD  
 Especifica que, tras la operación RESTORE, la cinta permanece cargada en la unidad de cinta.  
  
#### <a name="replication_with_option"></a><replication_WITH_option>  
 Esta opción solo es pertinente si la base de datos se replicó cuando se creó la copia de seguridad.  
  
 KEEP_REPLICATION  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
Use KEEP_REPLICATION al configurar la replicación para que funcione con el trasvase de registros. Evita que se quite la configuración de replicación al restaurar una copia de seguridad de base de datos o de registros en un servidor en espera semiactiva, y la base de datos se recupera. No se permite especificar esta opción para restaurar una copia de seguridad con la opción NORECOVERY. Para garantizar que la replicación funciona correctamente después de la restauración:  
  
-   Las bases de datos **msdb** y **maestra** del servidor en espera semiactiva deben estar sincronizadas con las bases de datos **msdb** y **maestra** del servidor principal.  
  
-   El nombre del servidor en espera semiactiva debe cambiarse de manera que sea igual que el del servidor principal.  
  
#### <a name="change_data_capture_with_option"></a><change_data_capture_WITH_option>  
 Esta opción solo es pertinente si la base de datos se habilitó para la captura de datos modificados cuando se creó la copia de seguridad.  
  
 KEEP_CDC  
 **Compatible con:**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 KEEP_CDC se debe usar para evitar que la configuración de la captura de datos modificados se quite cuando una copia de seguridad de bases de datos o de registros se restaura en otro servidor y se recupera la base de datos. No se permite especificar esta opción para restaurar una copia de seguridad con la opción NORECOVERY.  
  
 Al restaurar la base de datos con KEEP_CDC, no se crean los trabajos de la captura de datos modificados. Para extraer los cambios del registro después de restaurar la base de datos, deberá volver a crear el trabajo del proceso de captura y el trabajo de limpieza para la base de datos restaurada. Para más información, vea [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).  
  
 Para más información sobre cómo usar la captura de datos modificados con la creación de reflejo de la base de datos, vea [Captura de datos modificados y otras características de SQL Server](../../relational-databases/track-changes/change-data-capture-and-other-sql-server-features.md).  
  
#### \<service_broker_WITH_options>  
 Activa o desactiva la entrega de mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)] o establece un nuevo identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Esta opción solo es pertinente si [!INCLUDE[ssSB](../../includes/sssb-md.md)] se habilitó (activó) para la base de datos cuando se creó la copia de seguridad.  
  
 { ENABLE_BROKER  | ERROR_BROKER_CONVERSATIONS  | NEW_BROKER }  
 **Compatible con:**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 ENABLE_BROKER  
 Especifica que la entrega de mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)] está habilitada al final de la restauración para que los mensajes se puedan enviar inmediatamente. De forma predeterminada, la entrega de mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)] está deshabilitada durante una restauración. La base de datos conserva el identificador de Service Broker existente.  
  
 ERROR_BROKER_CONVERSATIONS  
 Finaliza todas las conversaciones con un error que indica que la base de datos está adjunta o restaurada. Esto permite que las aplicaciones realicen una limpieza regular de las conversaciones existentes. La entrega de mensajes de Service Broker está deshabilitada hasta que se completa esta operación y, después, se habilita. La base de datos conserva el identificador de Service Broker existente.  
  
 NEW_BROKER  
 Especifica que se asigne a la base de datos un nuevo identificador de Service Broker. Dado que la base de datos se considera como un nuevo Service Broker, todas las conversaciones existentes en la base de datos se quitan inmediatamente sin generar mensajes de fin de diálogo. Cualquier ruta que haga referencia al identificador de Service Broker anterior debe volverse a crear con el nuevo identificador.  
  
#### \<point_in_time_WITH_options>  
 **Compatible con:**  [RESTORE {DATABASE|LOG}](../../t-sql/statements/restore-statements-transact-sql.md) y solo en los modelos de recuperación completa o recuperación optimizada para cargas masivas de registros.  
  
 Puede restaurar una base de datos hasta un momento concreto o transacción especificando el punto de recuperación de destino en una cláusula STOPAT, STOPBEFOREMARK o STOPATMARK. Un momento o transacción especificada siempre se restaura a partir de una copia de seguridad de registros. En cada instrucción RESTORE LOG de la secuencia de restauración, debe especificar el momento o la transacción de destino en una cláusula STOPAT, STOPBEFOREMARK o STOPATMARK idéntica.  
  
 Como requisito previo para realizar una restauración a un momento dado, primero debe restaurar una copia de seguridad total de la base de datos cuyo final sea anterior al punto de recuperación de destino. Para ayudarle a identificar qué copia de seguridad de la base de datos restaurar, si lo desea puede especificar la cláusula STOPAT, STOPBEFOREMARK o STOPATMARK en una instrucción RESTORE DATABASE para generar un error si una copia de seguridad de los datos es demasiado reciente para el momento de destino especificado. Pero la copia de seguridad completa de los datos se restaura siempre, aunque contenga el momento de destino.  
  
> [!NOTE]  
>  Las opciones RESTORE_DATABASE y RESTORE_LOG a un momento dado de WITH son parecidas, pero solo RESTORE LOG admite el argumento *mark_name*.  
  
 { STOPAT | STOPATMARK | STOPBEFOREMARK }   
 
 STOPAT **=** { **'** _datetime_ **'**  |  **@** _datetime\_var* }  
 Especifica que se va restaurar la base de datos al estado en que estaba en la fecha y hora especificadas por el parámetro *datetime* o **@** _datetime\_var_. Para más información sobre cómo especificar una fecha y una hora, vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Si se usa una variable para STOPAT, la variable debe ser del tipo de datos **varchar**, **char**, **smalldatetime** o **datetime**. Solo se aplican a la base de datos los datos del registro de transacciones escritos antes de la fecha y hora especificadas.  
  
> [!NOTE]  
>  Si la hora de STOPAT es posterior a la última copia de seguridad de LOG, la base de datos se deja en estado no recuperado, como si se hubiera ejecutado RESTORE LOG con la opción NORECOVERY.  
  
 Para obtener más información, vea [Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
 STOPATMARK **=** { **'** _mark\_name_ **'**  |  **'** lsn:_lsn\_number_ **'** } [ AFTER **'** _datetime_ **'** ]  
 Especifica que la recuperación se realice en un punto de recuperación especificado. La transacción especificada se incluye en la recuperación, pero solo se confirma si estaba confirmada inicialmente cuando se generó.  
  
 Tanto RESTORE DATABASE como RESTORE LOG admiten el parámetro *lsn_number*. Este parámetro especifica un número de secuencia de registro.  
  
 El parámetro *mark_name* solo es compatible con la instrucción RESTORE LOG. Este parámetro identifica una marca de transacción en la copia de seguridad de registros.  
  
 En una instrucción RESTORE LOG, si se omite AFTER *datetime*, la recuperación se detiene en la primera marca con el nombre especificado. Si se especifica AFTER *datetime*, la recuperación se detiene en la primera marca que tenga el nombre especificado, ya coincida con *datetime* o ya sea posterior.  
  
> [!NOTE]  
>  Si la marca especificada, LSN, o la hora es posterior a la última copia de seguridad de LOG, la base de datos se deja en estado no recuperado, como si se hubiera ejecutado RESTORE LOG con la opción NORECOVERY.  
  
 Para más información, vea [Usar transacciones marcadas para recuperar bases de datos relacionadas sistemáticamente &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md) y [Recuperar a un número de secuencia de registro &#40;SQL Server&#41;](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md).  
  
 STOPBEFOREMARK **=** { **'** _mark\_name_ **'**  |  **'** lsn:_lsn\_number_ **'** } [ AFTER **'** _datetime_ **'** ]  
 Especifica que la recuperación se realice en un punto de recuperación especificado. La transacción especificada no se incluye en la recuperación y se revertirá cuando se utilice WITH RECOVERY.  
  
 Tanto RESTORE DATABASE como RESTORE LOG admiten el parámetro *lsn_number*. Este parámetro especifica un número de secuencia de registro.  
  
 El parámetro *mark_name* solo es compatible con la instrucción RESTORE LOG. Este parámetro identifica una marca de transacción en la copia de seguridad de registros.  
  
 En una instrucción RESTORE LOG, si se omite AFTER *datetime*, la recuperación se detiene justo antes de la primera marca con el nombre especificado. Si se especifica AFTER *datetime*, la recuperación se detiene justo antes de la primera marca que tenga el nombre especificado, ya coincida con *datetime* o ya sea posterior.  
  
> [!IMPORTANT]  
>  Si una secuencia de restauración parcial excluye cualquier grupo de archivos FILESTREAM, no se admite la restauración a un momento dado. Puede forzarse la continuación de la secuencia de restauración. Sin embargo, no se podrán restaurar los grupos de archivos FILESTREAM omitidos en la instrucción RESTORE. Para forzar una restauración a un momento dado, especifique la opción CONTINUE_AFTER_ERROR junto con la opción STOPAT, STOPATMARK o STOPBEFOREMARK. Si se especifica CONTINUE_AFTER_ERROR, la secuencia de restauración parcial será correcta y el grupo de archivos FILESTREAM no será recuperable.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Para obtener los conjuntos de resultados, vea los siguientes temas:  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
## <a name="remarks"></a>Observaciones  
 Para comprobar las notas adicionales, vea los siguientes temas:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
## <a name="specifying-a-backup-set"></a>Especificar un conjunto de copia de seguridad  
 Un *conjunto de copia de seguridad* contiene la copia de seguridad de una sola operación de copia de seguridad realizada correctamente. Las instrucciones RESTORE, RESTORE FILELISTONLY, RESTORE HEADERONLY y RESTORE VERIFYONLY actúan sobre un solo conjunto de copia de seguridad en el conjunto de medios de los dispositivos de copia de seguridad especificados. Debe especificar la copia de seguridad que necesita del conjunto de medios. Puede obtener el valor *backup_set_file_number* de un conjunto de copia de seguridad mediante la instrucción [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
 La opción para especificar el conjunto de copia de seguridad que se desea restaurar es:  
  
 FILE **=** { *backup_set_file_number* |  **@** _backup\_set\_file\_number_ }  
  
 Donde *backup_set_file_number* indica la posición de la copia de seguridad en el conjunto de medios. Un valor de *backup_set_file_number* de 1 (FILE = 1) indica el primer conjunto de copia de seguridad del medio de copia, y si *backup_set_file_number* es 2 (FILE = 2), indica el segundo conjunto de copia de seguridad, y así sucesivamente.  
  
 El comportamiento de esta opción varía en función de la instrucción, como se indica en la siguiente tabla:  
  
|.|Comportamiento de la opción FILE de conjunto de copia de seguridad|  
|---------------|-----------------------------------------|  
|RESTORE|El número predeterminado del conjunto de copia de seguridad es 1. En una instrucción RESTORE solo se permite una opción FILE de conjunto de copia de seguridad. Es importante especificar los conjuntos de copia de seguridad por orden.|  
|RESTORE FILELISTONLY|El número predeterminado del conjunto de copia de seguridad es 1.|  
|RESTORE HEADERONLY|De forma predeterminada, se procesan todos los conjuntos de copia de seguridad del conjunto de medios. El conjunto de resultados de RESTORE HEADERONLY devuelve información sobre cada conjunto de copia de seguridad, incluida su **posición** en el conjunto de medios. Para devolver información sobre un conjunto de copia de seguridad determinado, use su número de posición como valor de *backup_set_file_number* en la opción FILE.<br /><br /> Nota: Para cintas, RESTORE HEADER solo procesa los conjuntos de copia de seguridad de la cinta cargada.|  
|RESTORE VERIFYONLY|El valor predeterminado de *backup_set_file_number* es 1.|  
  
> [!NOTE]  
>  La opción FILE para especificar un conjunto de copia de seguridad no está relacionada con la opción FILE para especificar un archivo de base de datos, FILE **=** { *logical_file_name_in_backup* |  **@** _logical\_file\_name\_in\_backup\_var_ }.  
  
## <a name="summary-of-support-for-with-options"></a>Resumen de compatibilidad para las opciones de WITH  
 Las opciones de WITH siguientes solo son compatibles con la instrucción RESTORE: BLOCKSIZE, BUFFERCOUNT, MAXTRANSFERSIZE, PARTIAL, KEEP_REPLICATION, { RECOVERY | NORECOVERY | STANDBY }, REPLACE, RESTART, RESTRICTED_USER y { STOPAT | STOPATMARK | STOPBEFOREMARK }  
  
> [!NOTE]  
>  La opción PARTIAL solo se admite en RESTORE DATABASE.  
  
 En la tabla siguiente se enumeran las opciones de WITH que se utilizan en una o más instrucciones, y se indica qué instrucciones admiten cada opción. Una marca de verificación (√) indica que la opción se admite; el guion (-) indica que la opción no se admite.  
  
|Opción de WITH|RESTORE|RESTORE FILELISTONLY|RESTORE HEADERONLY|RESTORE LABELONLY|RESTORE REWINDONLY|RESTORE VERIFYONLY|  
|-----------------|-------------|--------------------------|------------------------|-----------------------|------------------------|------------------------|  
|{ CHECKSUM<br /><br /> &#124; NO_CHECKSUM }|√|√|√|√|-|√|  
|{ CONTINUE_AFTER_ERROR<br /><br /> &#124; STOP_ON_ERROR }|√|√|√|√|-|√|  
|FILE<sup>1</sup>|√|√|√|-|-|√|  
|LOADHISTORY|-|-|-|-|-|√|  
|MEDIANAME|√|√|√|√|-|√|  
|MEDIAPASSWORD|√|√|√|√|-|√|  
|MOVE|√|-|-|-|-|√|  
|PASSWORD|√|√|√|-|-|√|  
|{ REWIND &#124; NOREWIND }|√|Solo REWIND|Solo REWIND|Solo REWIND|-|√|  
|STATS|√|-|-|-|-|√|  
|{ UNLOAD &#124; NOUNLOAD }|√|√|√|√|√|√|  
  
 <sup>1</sup> FILE **=** _backup\_set\_file\_number_, que es distinto de {FILE | FILEGROUP}.  
  
## <a name="permissions"></a>Permisos  
 Para comprobar los permisos, vea los siguientes temas:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
## <a name="examples"></a>Ejemplos  
 Para obtener ejemplos, vea los siguientes temas:  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [Realizar copias de seguridad y restaurar bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)  

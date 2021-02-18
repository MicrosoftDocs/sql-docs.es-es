---
title: sqlmaint, utilidad
description: En SQL Server, use sqlmaint para ejecutar comprobaciones de DBCC, realizar una copia de seguridad de una base de datos y su registro de transacciones, actualizar estadísticas y volver a generar índices.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- database maintenance plans [SQL Server]
- sqlmaint utility
- maintaining databases [SQL Server]
- backups [SQL Server], sqlmaint utility
- command prompt utilities [SQL Server], sqlmaint
- maintenance plans [SQL Server], command prompt
- backing up [SQL Server], sqlmaint utility
ms.assetid: 937a9932-4aed-464b-b97a-a5acfe6a50de
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c822bd2acbd92bcfd9b3008c78c3ccef244f3c88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338556"
---
# <a name="sqlmaint-utility"></a>sqlmaint, utilidad
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  La utilidad **sqlmaint** realiza un conjunto especificado de operaciones de mantenimiento en una o varias bases de datos. Use **sqlmaint** para ejecutar comprobaciones de DBCC, realizar una copia de seguridad de una base de datos y su registro de transacciones, actualizar estadísticas y volver a generar índices. Todas las actividades de mantenimiento de bases de datos generan un informe que se puede enviar a un archivo de texto designado, un archivo HTML o una cuenta de correo electrónico. **sqlmaint** ejecuta los planes de mantenimiento de bases de datos creados con versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Para ejecutar los planes de mantenimiento de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde el símbolo del sistema, emplee la [utilidad dtexec](../integration-services/packages/dtexec-utility.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../includes/ssnotedepnextavoid-md.md)] Use en su lugar la característica de plan de mantenimiento de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Para obtener más información sobre los planes de mantenimiento, vea [Planes de mantenimiento](../relational-databases/maintenance-plans/maintenance-plans.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlmaint   
[-?] |  
[  
     [-S server_name[\instance_name]]  
     [-U login_ID [-P password]]  
     {  
          [-D database_name | -PlanName name | -PlanID guid ]  
          [-Rpt text_file]  
          [-To operator_name]  
          [-HtmlRpt html_file [-DelHtmlRpt <time_period>] ]  
          [-RmUnusedSpace threshold_percentfree_percent]  
          [-CkDB | -CkDBNoIdx]  
          [-CkAl | -CkAlNoIdx]  
          [-CkCat]  
          [-UpdOptiStats sample_percent]  
          [-RebldIdx free_space]  
          [-SupportComputedColumn]  
          [-WriteHistory]  
          [  
               {-BkUpDB [backup_path] | -BkUpLog [backup_path] }  
               {-BkUpMedia  
                    {DISK [  
                           [-DelBkUps <time_period>]   
                           [-CrBkSubDir ]   
                           [-UseDefDir ]   
                          ]  
                     | TAPE   
                    }  
               }  
               [-BkUpOnlyIfClean]  
               [-VrfyBackup]  
          ]  
     }  
]  
<time_period> ::=  
number[minutes | hours | days | weeks | months]  
```  
  
## <a name="arguments"></a>Argumentos  
 Los parámetros y sus valores deben estar separados por un espacio. Por ejemplo, debe haber un espacio en blanco entre **-S** y *server_name*.  
  
 **-?**  
 Especifica que debe devolverse el diagrama de sintaxis para **sqlmaint** . Este parámetro debe utilizarse solo.  
  
 **-S** _server_name_[ **\\** _instance\_name_]  
 Especifica la instancia de destino de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Especifique _server\_name_ para conectar con la instancia predeterminada de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] en ese servidor. Especifique _nombre\_servidor_ **\\** _nombre\_instancia_ para conectar una instancia con nombre de [!INCLUDE[ssDE](../includes/ssde-md.md)] en ese servidor. Si no se especifica ningún servidor, **sqlmaint** se conecta a la instancia predeterminada de [!INCLUDE[ssDE](../includes/ssde-md.md)] en el equipo local.  
  
 **-U** _login_ID_  
 Especifica el Id. de inicio de sesión que va a utilizarse para conectar al servidor. Si no se especifica, **sqlmaint** intenta utilizar la autenticación de [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows. Si *login_ID* contiene caracteres especiales, debe incluirse entre comillas dobles ("); de lo contrario, las comillas dobles son opcionales.  
  
> [!IMPORTANT]  
>  Siempre que sea posible, utilice la autenticación de Windows.  
  
 **-P** _password_  
 Especifica la contraseña para el identificador de inicio de sesión. Solo tiene validez si también se especifica el parámetro **-U** . Si *password* contiene caracteres especiales, debe incluirse entre comillas dobles; de lo contrario, las comillas dobles son opcionales.  
  
> [!IMPORTANT]  
>  La contraseña no se enmascara. Siempre que sea posible, utilice la autenticación de Windows.  
  
 **-D** _database_name_  
 Especifica el nombre de la base de datos en que se va a realizar la operación de mantenimiento. Si *database_name* contiene caracteres especiales, debe incluirse entre comillas dobles; de lo contrario, las comillas dobles son opcionales.  
  
 **-PlanName** _name_  
 Especifica el nombre de un plan de mantenimiento de base de datos definido mediante el Asistente para planes de mantenimiento de bases de datos. La única información del plan que **sqlmaint** usa es la lista de las bases de datos incluidas en él. Todas las actividades de mantenimiento que se especifiquen en los demás parámetros de **sqlmaint** se aplicarán a esta lista de bases de datos.  
  
 **-PlanID** _guid_  
 Especifica el identificador exclusivo global (GUID) de un plan de mantenimiento de base de datos definido mediante el Asistente para planes de mantenimiento de bases de datos. La única información del plan que **sqlmaint** usa es la lista de las bases de datos incluidas en él. Todas las actividades de mantenimiento que se especifiquen en los demás parámetros de **sqlmaint** se aplicarán a esta lista de bases de datos. Debe coincidir con un valor de plan_id en msdb.dbo.sysdbmaintplans.  
  
 **-Rpt** _text_file_  
 Especifica la ruta de acceso completa y el nombre del archivo en que se va a generar el informe. También se genera el informe en la pantalla. El informe mantiene información de las versiones al agregar una fecha al nombre de archivo. La fecha se genera de la siguiente manera: al final del nombre de archivo pero antes del punto, con el formato _ *yyyyMMddhhmm*. *yyyy* = año, *MM* = mes, *dd* = día, *hh* = hora, *mm* = minuto.  
  
 Si ejecuta la utilidad a las 10:23 a.m. el 1 de diciembre de 1996 y este es el valor de *text_file* :  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.rpt  
```  
  
 El nombre del archivo generado será:  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint_199612011023.rpt  
```  
  
 Se requiere el nombre de archivo UNC (Convención de nomenclatura universal) para *text_file* cuando **sqlmaint** tiene acceso a un servidor remoto.  
  
 **-To**  _operator_name_  
 Especifica el operador al que se envía el informe generado a través de SQL Mail.  
  
 **-HtmlRpt** _html_file_  
 Especifica la ruta de acceso completa y el nombre del archivo en que se va a generar un informe HTML. **sqlmaint** genera el nombre de archivo anexando una cadena con el formato _ *yyyyMMddhhmm* al nombre de archivo, al igual que en el caso del parámetro **-Rpt** .  
  
 Se requiere el nombre UNC completo del archivo para *html_file* cuando **sqlmaint** tiene acceso a un servidor remoto.  
  
 **-DelHtmlRpt** \<*time_period*>  
 Especifica que debe eliminarse cualquier informe HTML del directorio de informes si el intervalo posterior a la creación del archivo de informe supera el valor de \<*time_period*>. **-DelHtmlRpt** busca archivos cuyo nombre se ajuste al patrón generado a partir del parámetro *html_file*. Si *html_file* es C:\Archivos de programa\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.htm, **-DelHtmlRpt** hará que **sqlmaint** elimine todos los archivos cuyos nombres coincidan con el patrón C:\Archivos de programa\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint\*.htm y que sean más antiguos que el valor de \<*time_period*> especificado.  
  
 **-RmUnusedSpace** _threshold_percent free_percent_  
 Especifica que se quite el espacio no usado de la base de datos especificada en **-D**. Esta opción solo resulta útil para aquellas bases de datos definidas para crecer automáticamente. *Threshold_percent* especifica en megabytes el tamaño que debe alcanzar la base de datos antes de que **sqlmaint** intente quitar el espacio de datos no usado. Si la base de datos es menor que el valor de *threshold_percent*, no se realiza ninguna acción. *Free_percent* especifica la cantidad de espacio no usado que debe conservarse en la base de datos, especificado como porcentaje del tamaño final de la base de datos. Por ejemplo, si una base de datos de 200 MB contiene 100 MB de datos y se especifica el valor 10 para *free_percent* , el tamaño final de la base de datos será de 110 MB. Tenga en cuenta que una base de datos no se ampliará si ocupa menos espacio que la suma de *free_percent* más el espacio ocupado por los datos de la base de datos. Por ejemplo, si una base de datos de 108 MB tiene 100 MB de datos y se especifica 10 para *free_percent* , la base de datos no se ampliará a 110 MB; se mantendrá con un espacio de 108 MB.  
  
 **-CkDB** |  **-CkDBNoIdx**  
 Especifica que se ejecute una instrucción DBCC CHECKDB o una instrucción DBCC CHECKDB con la opción NOINDEX en la base de datos especificada en **-D**. Para obtener más información, vea DBCC CHECKDB.  
  
 Se escribe una advertencia en *text_file* si la base de datos está en uso cuando **sqlmaint** se ejecuta.  
  
 **-CkAl** |  **-CkAlNoIdx**  
 Especifica que se ejecute una instrucción DBCC CHECKALLOC con la opción NOINDEX en la base de datos especificada en **-D**. Para obtener más información, vea [DBCC CHECKALLOC &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md).  
  
 **-CkCat**  
 Especifica que se ejecute una instrucción DBCC CHECKCATALOG (Transact-SQL) en la base de datos especificada en **-D**. Para obtener más información, vea [DBCC CHECKCATALOG &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkcatalog-transact-sql.md).  
  
 **-UpdOptiStats** _sample_percent_  
 Especifica que se ejecute la instrucción siguiente en cada una de las tablas de la base de datos:  
  
```  
UPDATE STATISTICS table WITH SAMPLE sample_percent PERCENT;  
```  
  
 Si las tablas contienen columnas calculadas, también debe especificar el argumento **-SupportedComputedColumn** al usar **-UpdOptiStats**.  
  
 Para obtener más información, vea [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md).  
  
 **-RebldIdx** _free_space_  
 Especifica que se deben volver a generar los índices de las tablas de la base de datos de destino mediante el valor porcentual de *free_space* en proporción inversa al factor de relleno. Por ejemplo, si el porcentaje de *free_space* es 30, el factor de relleno empleado será 70. Si se especifica un valor de 100 como porcentaje de *free_space* , se volverán a generar los índices con el valor de factor de relleno original.  
  
 Si los índices están en columnas calculadas, también debe especificar el argumento **-SupportComputedColumn** al usar **-RebldIdx**.  
  
 **-SupportComputedColumn**  
 Debe especificarse para ejecutar los comandos de mantenimiento de DBCC con **sqlmaint** en columnas calculadas.  
  
 **-WriteHistory**  
 Especifica que se escriba una entrada en msdb.dbo.sysdbmaintplan_history para cada acción de mantenimiento que **sqlmaint** realice. Si se especifica **-PlanName** o **-PlanID** , las entradas de sysdbmaintplan_history usarán el identificador del plan especificado. Si se especifica **-D** , las entradas de sysdbmaintplan_history para el identificador de plan estarán compuestas de ceros.  
  
 **-BkUpDB** [ *backup_path*] |  **-BkUpLog** [ *backup_path* ]  
 Especifica una operación de copia de seguridad. **-BkUpDb** hace una copia de seguridad de toda la base de datos. **-BkUpLog** hace una copia de seguridad solo del registro de transacciones.  
  
 *backup_path* especifica el directorio de la copia de seguridad. *backup_path* no es necesario si también se especifica **-UseDefDir** . Si se especifican ambos, se reemplaza por **-UseDefDir** . La copia de seguridad se puede colocar en un directorio o en una dirección de dispositivo de cinta (por ejemplo, \\\\.\TAPE0). El nombre de archivo de una copia de seguridad de base de datos se genera automáticamente del modo siguiente:  
  
```  
dbname_db_yyyyMMddhhmm.BAK  
  
```  
  
 , donde  
  
-   *dbname* es el nombre de la base de datos de la que se realiza una copia de seguridad.  
  
-   *aaaaMMddhhmm* es la hora de la operación de copia de seguridad, siendo *aaaa* = año, *MM* = mes, *dd* = día, *hh* = hora y *mm* = minuto.  
  
 El nombre del archivo para una copia de seguridad de transacciones se genera automáticamente con un formato similar:  
  
```  
dbname_log_yyyymmddhhmm.BAK  
  
```  
  
 Si se usa el parámetro **-BkUpDB** , también deberá especificar el medio por medio del parámetro **-BkUpMedia** .  
  
 **-BkUpMedia**  
 Especifica el tipo de medio para la copia de seguridad (DISK o TAPE).  
  
 **DISK**  
 Especifica que el medio de la copia de seguridad es un disco.  
  
 **-DelBkUps**< *time_period* >  
 Para copias de seguridad en disco, especifica que deben eliminarse todos los archivos de copia de seguridad del directorio de copia de seguridad si el intervalo posterior a la creación de la copia de seguridad supera el valor de \<*time_period*>.  
  
 **-CrBkSubDir**  
 Para copias de seguridad en disco, especifica que se cree un subdirectorio en el directorio [*backup_path*] o en el directorio de copia de seguridad predeterminado, si también se especifica **-UseDefDir** . El nombre del subdirectorio se genera a partir del nombre de la base de datos especificado en **-D**. **-CrBkSubDir** ofrece una sencilla forma de poner todas las copias de seguridad de diferentes bases de datos en subdirectorios separados sin tener que cambiar el parámetro *backup_path* .  
  
 **-UseDefDir**  
 Para copias de seguridad en disco, especifica que se cree el archivo de copia de seguridad en el directorio de copia de seguridad predeterminado. **UseDefDir** invalida *backup_path* si se especifican ambos parámetros. El directorio predeterminado de copia de seguridad en una instalación predeterminada de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es C:\Archivos de programa\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\Backup.  
  
 **TAPE**  
 Especifica que el medio de copia de seguridad es una cinta.  
  
 **-BkUpOnlyIfClean**  
 Indica que solo se realice la copia de seguridad si las comprobaciones **-Ck** especificadas no han detectado problemas en los datos. Las acciones de mantenimiento se ejecutan en la misma secuencia en que aparecen en el símbolo del sistema. Especifique los parámetros **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** o **-CkCat** antes de los parámetros **-BkUpDB**/ **-BkUpLog** si también va a especificar **-BkUpOnlyIfClean**; de lo contrario, la copia de seguridad se llevará a cabo con independencia de que la comprobación indique la existencia de problemas o no.  
  
 **-VrfyBackup**  
 Especifica que solo se ejecute RESTORE VERIFYONLY en la copia de seguridad cuando finalice.  
  
 *number*[**minutes**\| **hours**\| **day**\| **weeks**\| **months**]  
 Especifica el intervalo de tiempo que se utiliza para determinar si un informe o un archivo de copia de seguridad son suficientemente antiguos para poder eliminarlos. *number* es un número entero seguido (sin espacio) de una unidad de tiempo. Ejemplos válidos:  
  
-   **12weeks**  
  
-   **3months**  
  
-   **15days**  
  
 Si solo se especifica *number* , la parte de fecha predeterminada es **weeks**.  
  
## <a name="remarks"></a>Observaciones  
 La utilidad **sqlmaint** realiza operaciones de mantenimiento en una o varias bases de datos. Si se especifica **-D** , las operaciones especificadas en los modificadores restantes solo se realizan en la base de datos indicada. Si se especifica **-PlanName** o **-PlanID** , la única información que **sqlmaint** recupera del plan de mantenimiento indicado es la lista de bases de datos del plan. Todas las operaciones especificadas en los parámetros **sqlmaint** restantes se aplican en cada base de datos de la lista obtenida del plan. La utilidad **sqlmaint** no aplica ninguna de las actividades de mantenimiento definidas en el propio plan.  
  
 La utilidad **sqlmaint** devuelve 0 si se ejecuta correctamente o 1 si se produce algún error. Se informa del error:  
  
-   Si no se produce alguna de las operaciones de mantenimiento.  
  
-   Si las comprobaciones **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** o **-CkCat** detectan algún problema en los datos.  
  
-   Si se detecta un error general.  
  
## <a name="permissions"></a>Permisos  
 La utilidad **sqlmaint** puede ejecutarla cualquier usuario de Windows con permiso de **lectura y ejecución** para `sqlmaint.exe`, que está almacenado de manera predeterminada en la carpeta `x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER1\MSSQL\Binn` . Además, el inicio de sesión de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] especificado con **-login_ID** debe tener los permisos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] necesarios para realizar la acción indicada. Si la conexión con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utiliza la autenticación de Windows, el inicio de sesión de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] asignado al usuario de Windows autenticado debe tener los permisos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] necesarios para realizar la acción especificada.  
  
 Por ejemplo, para usar **-BkUpDB** , es necesario tener permiso para ejecutar la instrucción BACKUP. Para usar el argumento **-UpdOptiStats** , es necesario tener permiso para ejecutar la instrucción UPDATE STATISTICS. Para obtener más información, vea las secciones sobre permisos en los temas correspondientes de los Libros en pantalla.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-performing-dbcc-checks-on-a-database"></a>A. Realizar comprobaciones DBCC en una base de datos  
  
```  
sqlmaint -S MyServer -D AdventureWorks2012 -CkDB -CkAl -CkCat -Rpt C:\MyReports\AdvWks_chk.rpt  
```  
  
### <a name="b-updating-statistics-using-a-15-sample-in-all-databases-in-a-plan-also-shrink-any-of-the-database-that-have-reached-110-mb-to-having-only-10-free-space"></a>B. Actualizar estadísticas mediante una muestra del 15% en todas las bases de datos de un plan. Además, reducir todas las bases de datos que hayan alcanzado los 110 MB para que tengan solo el 10 % de espacio disponible  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -UpdOptiStats 15 -RmUnusedSpace 110 10  
```  
  
### <a name="c-backing-up-all-the-databases-in-a-plan-to-their-individual-subdirectories-in-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory-also-delete-any-backups-older-than-2-weeks"></a>C. Realizar una copia de seguridad de todas las bases de datos de un plan en los subdirectorios individuales del directorio predeterminado x:\Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup. Además, eliminar las copias de seguridad con una antigüedad superior a 2 semanas  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -BkUpDB -BkUpMedia DISK -UseDefDir -CrBkSubDir -DelBkUps 2weeks  
```  
  
### <a name="d-backing-up-a-database-to-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory"></a>D. Realizar una copia de seguridad de una base de datos en el directorio predeterminado x:\Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup.  
  
```  
sqlmaint -S MyServer -BkUpDB -BkUpMedia DISK -UseDefDir  
```  
  
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md)  
  
  

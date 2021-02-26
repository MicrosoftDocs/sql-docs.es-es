---
description: 'Tutorial: Usar el servicio Azure Blob Storage con SQL Server 2016'
title: 'Tutorial: Usar el servicio Azure Blob Storage con SQL Server 2016'
ms.custom: seo-dt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: e69be67d-da1c-41ae-8c9a-6b12c8c2fb61
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5ed027db3b2749997a12674cb657c448a26b9732
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "100646393"
---
# <a name="tutorial-use-azure-blob-storage-service-with-sql-server-2016"></a>Tutorial: Usar el servicio Azure Blob Storage con SQL Server 2016

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Bienvenido al tutorial del servicio Trabajar con SQL Server 2016 en Microsoft Azure Blob Storage. Este tutorial le ayudará a saber cómo usar el servicio Microsoft Azure Blob Storage para archivos de datos de SQL Server y copias de seguridad de SQL Server.  
  
La compatibilidad de integración de SQL Server para el servicio Microsoft Azure Blob Storage comenzó como una mejora de SQL Server 2012 Service Pack 1 CU2 y se ha mejorado aún más con SQL Server 2014 y SQL Server 2016. Para obtener información general sobre la funcionalidad y las ventajas de usar esta característica, consulte [Archivos de datos de SQL Server en Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). Para obtener una demostración en vivo, consulte [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(Demostración de restauración a un momento dado).  

En este tutorial se incluyen varias secciones en las que aprenderá a trabajar con archivos de datos de SQL Server en el servicio Microsoft Azure Blob Storage. Cada sección se centra en una tarea específica, y se deben completar por orden. En primer lugar, aprenderá a crear un nuevo contenedor en Blob Storage con una directiva de acceso almacenada y una firma de acceso compartido. Después, aprenderá a crear una credencial de SQL Server para integrar SQL Server con Azure Blob Storage. Luego, realizará una copia de seguridad de una base de datos en Blob Storage y la restaurará en una máquina virtual de Azure. Después usará la copia de seguridad del registro de transacciones de instantáneas de archivos de SQL Server 2016 para restaurar a un momento dado y a una nueva base de datos. Por último, en el tutorial se muestra el uso de funciones y procedimientos almacenados del sistema de metadatos para ayudarle a comprender y trabajar con copias de seguridad de instantáneas de archivos.
  
## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe estar familiarizado con los conceptos de copias de seguridad y restauración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y con la sintaxis de T-SQL. Para usar este tutorial, necesita una cuenta de Azure Storage, SQL Server Management Studio (SSMS), acceso a una instancia de SQL Server local, acceso a una máquina virtual de Azure (VM) que ejecute SQL Server 2016 y una base de datos AdventureWorks2016. Además, la cuenta que se usa para emitir comandos BACKUP o RESTORE debe tener el rol de base de datos **db_backupoperator** con permisos **Modificar cualquier credencial**. 

- Obtenga una [cuenta de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) gratis.
- Cree una [cuenta de almacenamiento de Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Aprovisione una [máquina virtual de Azure que ejecute SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/).
- Instale [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Descargue las [bases de datos de ejemplo de AdventureWorks2016](../samples/adventureworks-install-configure.md).
- Asigne la cuenta de usuario al rol [db_backupoperator](./security/authentication-access/database-level-roles.md) y conceda permisos [Modificar cualquier credencial](../t-sql/statements/alter-credential-transact-sql.md). 

> [!IMPORTANT]
> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no admite [Azure Data Lake Storage](/azure/storage/blobs/data-lake-storage-introduction); asegúrese de que el [espacio de nombres jerárquico](/azure/storage/blobs/data-lake-storage-namespace) no esté habilitado en la cuenta de almacenamiento que se emplea en este tutorial.

## <a name="1---create-stored-access-policy-and-shared-access-storage"></a>1. Crear una directiva de acceso almacenada y un almacenamiento de acceso compartido

En esta sección, usará un script de [Azure PowerShell](/powershell/azure/) para crear una firma de acceso compartido en un contenedor de blobs de Azure mediante una directiva de acceso almacenada.  
  
> [!NOTE]  
> Este script se escribió con Azure PowerShell 5.0.10586.  
  
Una firma de acceso compartido es un URI que concede derechos de acceso restringidos a los contenedores, los blobs, las colas o las tablas. Una directiva de acceso almacenada proporciona un nivel de control adicional sobre las firmas de acceso compartidas en el servidor, incluida la revocación, expiración o ampliación del acceso. Al usar esta nueva mejora, debe crear una directiva en un contenedor con derechos de lectura, escritura y enumeración como mínimo.  
  
Puede crear una directiva de acceso y una firma de acceso compartido mediante Azure PowerShell, el SDK Azure Storage, la API de REST de Azure o una utilidad de terceros. Este tutorial muestra cómo usar un script de PowerShell de Azure para completar esta tarea. El script usa el modelo de implementación del Administrador de recursos y crea los siguientes  recursos nuevos  
  
-   Resource group   
-   Cuenta de almacenamiento  
-   Contenedor de blobs de Azure   
-   Directiva SAS    

Este script comienza con la declaración de una serie de variables para especificar los nombres para los recursos anteriores y los nombres para los siguientes valores de entrada necesarios:  
  
-   El nombre de prefijo usado en la nomenclatura de otros objetos de recursos    
-   Nombre de suscripción    
-   Ubicación del centro de datos  

  
El script termina con la generación de la instrucción CREATE CREDENTIAL correspondiente que se va a usar en la sección [2. Crear una credencial de SQL Server con una firma de acceso compartido](#2---create-a-sql-server-credential-using-a-shared-access-signature). Esta instrucción se copia en el Portapapeles y se reproduce en la consola para que la pueda ver.  
  
Para crear una directiva en el contenedor y generar una clave de firma de acceso compartido (SAS), haga lo siguiente:  
  
1.  Abra la ventana de PowerShell o Windows PowerShell ISE (consulte los requisitos de versión anteriores).  
  
2.  Edite y ejecute el script siguiente:  
  
    ```powershell
    # Define global variables for the script  
    $prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
    $subscriptionID = '<your subscription ID>'   # the ID  of subscription name you will use  
    $locationName = '<a data center location>'  # the data center region you will use  
    $storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
    $containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = $prefixName + 'policy' # the name of the SAS policy 

    # Set a variable for the name of the resource group you will create or use  
    $resourceGroupName=$prefixName + 'rg'   
  
    # Add an authenticated Azure account for use in the session   
    Connect-AzAccount    
  
    # Set the tenant, subscription and environment for use in the rest of   
    Set-AzContext -SubscriptionId $subscriptionID   
  
    # Create a new resource group - comment out this line to use an existing resource group  
    New-AzResourceGroup -Name $resourceGroupName -Location $locationName   
  
    # Create a new Azure Resource Manager storage account - comment out this line to use an existing Azure Resource Manager storage account  
    New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   
  
    # Get the access keys for the Azure Resource Manager storage account  
    $accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  
  
    # Create a new storage account context using an Azure Resource Manager storage account  
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].Value

    # Creates a new container in blob storage  
    $container = New-AzStorageContainer -Context $storageContext -Name $containerName  
  
    # Sets up a Stored Access Policy and a Shared Access Signature for the new container  
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -StartTime $(Get-Date).ToUniversalTime().AddMinutes(-5) -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission rwld

    # Gets the Shared Access Signature for the policy  
    $sas = New-AzStorageContainerSASToken -name $containerName -Policy $policyName -Context $storageContext
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

    # Sets the variables for the new container you just created
    $container = Get-AzStorageContainer -Context $storageContext -Name $containerName
    $cbc = $container.CloudBlobContainer 
  
    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
    $tSql | clip  
    Write-Host $tSql 

    # Once you're done with the tutorial, remove the resource group to clean up the resources. 
    # Remove-AzResourceGroup -Name $resourceGroupName  
    ```  

3.  Cuando finalice el script, la instrucción CREATE CREDENTIAL estará en el Portapapeles para poder usarla en la siguiente sección.  


## <a name="2---create-a-sql-server-credential-using-a-shared-access-signature"></a>2. Crear una credencial de SQL Server con una firma de acceso compartido

En esta sección, creará una credencial para almacenar la información de seguridad que SQL Server usará para escribir y leer desde el contenedor de Azure que ha creado en el paso anterior.  
  
Una credencial de SQL Server es un objeto que se usa para almacenar la información de autenticación necesaria para conectarse a un recurso fuera de SQL Server. La credencial almacena la ruta de acceso URI de la firma de acceso compartido y del contenedor de almacenamiento de este contenedor.  

Para crear una credencial de SQL Server, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.  
2.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server del motor de base de datos en su entorno local.  
  
3.  En la nueva ventana de consulta, pegue la instrucción CREATE CREDENTIAL con la firma de acceso compartido de la sección 1 y ejecute el script.  
  
    El script tendrá un aspecto similar al código siguiente.  
  
    ```sql   
    /* Example:
    USE master  
    CREATE CREDENTIAL [https://msfttutorial.blob.core.windows.net/containername] 
    WITH IDENTITY='SHARED ACCESS SIGNATURE'   
    , SECRET = 'sharedaccesssignature' 
    GO */
    
    USE master  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      -- this name must match the container path, start with https and must not contain a forward slash at the end
    WITH IDENTITY='SHARED ACCESS SIGNATURE' 
      -- this is a mandatory string and should not be changed   
     , SECRET = 'sharedaccesssignature' 
       -- this is the shared access signature key that you obtained in section 1.   
    GO    
    ```  
  
4.  Para ver todas las credenciales disponibles, puede ejecutar la siguiente instrucción en una ventana de consulta conectada a su instancia:  
  
    ```sql  
    SELECT * from sys.credentials  
    ```  
  
5.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server del motor de base de datos de la máquina virtual de Azure.  
  
6.  En la nueva ventana de consulta, pegue la instrucción CREATE CREDENTIAL con la firma de acceso compartido de la sección 1 y ejecute el script.  
  
7.  Repita los pasos 5 y 6 para cualquier instancia de SQL Server adicional que quiere que tenga acceso al contenedor de Azure.  

## <a name="3---database-backup-to-url"></a>3. Realizar una copia de seguridad de base de datos en la dirección URL

En esta sección, hará una copia la base de datos AdventureWorks2016 en su instancia de SQL Server 2016 local en el contenedor de Azure que creó en la [sección 1](#1---create-stored-access-policy-and-shared-access-storage).
  
> [!NOTE]  
> Si quiere realizar una copia de seguridad de una base de datos de SQL Server 2012 SP1 CU2 o posterior o de una base de datos de SQL Server 2014 a este contenedor de Azure, puede usar la sintaxis en desuso que aparece [aquí](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2014&preserve-view=true) para realizar una copia de seguridad en la dirección URL mediante la sintaxis WITH CREDENTIAL.  
  
Para realizar una copia de seguridad de una base de datos en Blob Storage, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.  
2.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server 2016 del motor de base de datos de la máquina virtual de Azure.  
3.  Copie y pegue el siguiente script de Transact-SQL en la ventana de consulta. Modifique la dirección URL correctamente para su nombre de cuenta de almacenamiento y el contenedor que ha especificado en la sección 1 y, después, ejecute este script.  
  
    ```sql  
  
    -- To permit log backups, before the full database backup, modify the database to use the full recovery model.  
    USE master;  
    ALTER DATABASE AdventureWorks2016  
       SET RECOVERY FULL;  
  
    -- Back up the full AdventureWorks2016 database to the container that you created in section 1  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'  
  
    ```  
  
4.  Abra el Explorador de objetos y conéctese a Azure Storage con su cuenta de almacenamiento y la clave de la cuenta. 
    1.  Expanda Contenedores, expanda el contenedor creado en la sección 1 y compruebe que la copia de seguridad del paso 3 aparece en este contenedor.  
  
   ![Conexión a la cuenta de Azure Storage](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/connect-to-azure-storage.png)


## <a name="4----restore-database-to-virtual-machine-from-url"></a>4. Restaurar la base de datos a la máquina virtual desde la dirección URL

En esta sección, restaurará la base de datos AdventureWorks2016 en la instancia de SQL Server 2016 de la máquina virtual de Azure.
  
> [!NOTE]  
> Para simplificar este tutorial, usamos el mismo contenedor para los archivos de datos y de registros que se usan para la copia de seguridad de la base de datos. En un entorno de producción, usaría varios contenedores y, con frecuencia, varios archivos de datos. Con SQL Server 2016, también puede considerar la posibilidad de crear bandas de la copia de seguridad en varios blobs para aumentar el rendimiento de la copia de seguridad al realizar la copia de seguridad de una base de datos de gran tamaño.  
  
Para restaurar la base de datos de AdventureWorks2016 desde Azure Blob Storage en la instancia de SQL Server 2016 de la máquina virtual de Azure, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.   
2.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server 2016 del motor de base de datos de la máquina virtual de Azure.   
3.  Copie y pegue el siguiente script de Transact-SQL en la ventana de consulta. Modifique la dirección URL correctamente para su nombre de cuenta de almacenamiento y el contenedor que ha especificado en la sección 1 y, después, ejecute este script.  
  
    ```sql  
    -- Restore AdventureWorks2016 from URL to SQL Server instance using Azure blob storage for database files  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'   
       WITH  
          MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Data.mdf'  
         ,MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Log.ldf'  
    --, REPLACE  
  
    ```  
  
4.  Abra el Explorador de objetos y conéctese a la instancia de SQL Server 2016 de Azure.   
5.  En el Explorador de objetos, expanda el nodo Bases de datos y compruebe que se ha restaurado la base de datos AdventureWorks2016 (actualice el nodo según sea necesario).    
    1. Haga clic con el botón derecho en AdventureWorks2016 y seleccione Propiedades.
    1. Seleccione Archivos y compruebe que las rutas de acceso a los dos archivos de base de datos son direcciones URL que apuntan a los blobs del contenedor de blobs de Azure (seleccione Cancelar cuando haya terminado).
    
     ![Base de datos de AdventureWorks en la máquina virtual de Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/adventureworks-on-azure-vm.png)

    
8.  En el Explorador de objetos, conéctese a Azure Storage.
    1. Expanda Contenedores, expanda el contenedor que ha creado en la sección 1 y compruebe que AdventureWorks2016_Data.mdf y AdventureWorks2016_Log.ldf del paso 3 anterior aparecen en este contenedor, junto con el archivo de copia de seguridad de la sección 3 (actualice el nodo según sea necesario).  
  
   ![Archivos de datos dentro del contenedor en Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/data-files-in-container.png)

## <a name="5---backup-database-using-file-snapshot-backup"></a>5. Realizar una copia de seguridad de la base de datos mediante la copia de seguridad de instantáneas de archivos

En esta sección, realizará una copia de seguridad de la base de datos AdventureWorks2016 en la máquina virtual de Azure mediante la copia de seguridad de instantáneas de archivos para realizar una copia de seguridad casi inmediata mediante instantáneas de Azure. Para obtener más información sobre las copias de seguridad de instantáneas de archivos, consulte [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
  
Para realizar una copia de seguridad de la base de datos AdventureWorks2016 mediante la copia de seguridad de instantáneas de archivos, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.   
2.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server 2016 del motor de base de datos de la máquina virtual de Azure.  
3.  Copie, pegue y ejecute el siguiente script de Transact-SQL en la ventana de consulta (no cierre esta ventana de consulta: ejecutará este script nuevo en el paso 5). Este procedimiento almacenado del sistema permite ver las copias de seguridad de instantáneas de archivos existentes para cada archivo que forma parte de una base de datos especificada. Observará que no hay copias de seguridad de instantáneas de archivos para esta base de datos.  
  
    ```sql  
    -- Verify that no file snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
    ```  
  
4.  Copie y pegue el siguiente script de Transact-SQL en la ventana de consulta. Modifique la dirección URL correctamente para su nombre de cuenta de almacenamiento y el contenedor que ha especificado en la sección 1 y, después, ejecute este script. Observe la rapidez con la que se crea esta copia de seguridad.  
  
    ```sql  
    -- Backup the AdventureWorks2016 database with FILE_SNAPSHOT  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Azure.bak'   
       WITH FILE_SNAPSHOT;    
    ```  
  
5.  Después de comprobar que el script del paso 4 se ha ejecutado correctamente, ejecute el siguiente script de nuevo. Observe que la operación de copia de seguridad de instantáneas de archivos en el paso 4 genera instantáneas de archivo de los datos y del archivo de registro.  
  
    ```sql  
    -- Verify that two file-snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
  
    ```  
  
    ![Resultados de fn_db_backup_file_snapshots mostrando instantáneas](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-showing-snapshot.png) 
  
6.  En el Explorador de objetos, en la instancia de SQL Server 2016 de la máquina virtual de Azure, expanda el nodo Bases de datos y compruebe que se ha restaurado la base de datos AdventureWorks2016 a esta instancia (actualice el nodo según sea necesario).  
7.  En el Explorador de objetos, conéctese a Azure Storage.   
8.  Expanda Contenedores, expanda el contenedor que creó en la sección 1 y compruebe que la AdventureWorks2016_Azure.bak del paso 4 anterior aparece en este contenedor, junto con el archivo de copia de seguridad de la sección 3 y los archivos de base de datos de la sección 4 (actualice el nodo según sea necesario).  
  
    ![Copia de seguridad de instantáneas en Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-backup-on-azure.PNG)

## <a name="6----generate-activity-and-backup-log-using-file-snapshot-backup"></a>6. Generar el registro de actividades y copias de seguridad mediante la copia de seguridad de instantáneas de archivos

En esta sección, va a generar actividad en la base de datos AdventureWorks2016 y, periódicamente, va a crear copias de seguridad del registro de transacciones mediante copias de seguridad de instantáneas de archivos. Para obtener más información sobre las copias de seguridad de instantáneas de archivos, vea [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Para generar actividad en la base de datos AdventureWorks2016 y crear periódicamente copias de seguridad del registro de transacciones mediante copias de seguridad de instantáneas de archivos, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.   
2.  Abra dos nuevas ventanas de consulta y conecte cada una de ellas a la instancia de SQL Server 2016 del motor de base de datos de la máquina virtual de Azure.   
3.  Copie, pegue y ejecute el siguiente script de Transact-SQL en una de las ventanas de consulta. Tenga en cuenta que la tabla Production.Location tiene 14 filas antes de agregar nuevas filas en el paso 4.  
  
    ```sql 
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location;    
    ```  
  
4.  Copie y pegue los dos siguientes scripts de Transact-SQL en las dos ventanas de consulta independientes. Modifique la dirección URL correctamente para su nombre de cuenta de almacenamiento y el contenedor que ha especificado en la sección 1 y, después, ejecute estos scripts simultáneamente en ventanas de consulta independientes. Estos scripts tardarán unos siete minutos en completarse.  
  
    ```sql  
    -- Insert 30,000 new rows into the Production.Location table in the AdventureWorks2014 database in batches of 75  
    DECLARE @count INT=1, @inner INT;  
    WHILE @count < 400  
       BEGIN  
          BEGIN TRAN;  
             SET @inner =1;  
                WHILE @inner <= 75  
                   BEGIN;  
                      INSERT INTO AdventureWorks2016.Production.Location    
                         (Name, CostRate, Availability, ModifiedDate)   
                            VALUES (NEWID(), .5, 5.2, GETDATE());  
                      SET @inner = @inner + 1;  
                   END;  
          COMMIT;  
       WAITFOR DELAY '00:00:01';   
       SET @count = @count + 1;  
       END;  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;    
    ```  
  
    ```sql  
    --take 7 transaction log backups with FILE_SNAPSHOT, one per minute, and include the row count and the execution time in the backup file name   
    DECLARE @count INT=1, @device NVARCHAR(120), @numrows INT;  
    WHILE @count <= 7  
       BEGIN  
             SET @numrows = (SELECT COUNT (*) FROM AdventureWorks2016.Production.Location);  
             SET @device = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-' + CONVERT (varchar(10),@numrows) + '-' + FORMAT(GETDATE(), 'yyyyMMddHHmmss') + '.bak';  
             BACKUP LOG AdventureWorks2016 TO URL = @device WITH FILE_SNAPSHOT;  
             SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
          WAITFOR DELAY '00:1:00';   
             SET @count = @count + 1;  
       END;  
    ```  
  
5.  Revise el resultado del primer script y tenga en cuenta que el recuento final de filas ahora es de 29 939.  
  
    ![Se muestra el recuento de filas de 29 939](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png)
  
6.  Revise el resultado del segundo script y tenga en cuenta que cada vez que la instrucción BACKUP LOG se ejecuta, se crean dos nuevas instantáneas de archivos (una instantánea de archivo del archivo de registro y otra del archivo de datos) para un total de dos instantáneas de archivos para cada archivo de base de datos. Después de que se complete el segundo script, tenga en cuenta que ahora existen un total de 16 instantáneas de archivo, 8 para cada archivo de base de datos (uno de la instrucción BACKUP DATABASE y otro de cada ejecución de la instrucción BACKUP LOG).  
  
   ![Resultados de la instantánea de copia de seguridad](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-back-results.png)
  
7.  En el Explorador de objetos, conéctese a Azure Storage.  
  
8.  Expanda Contenedores, expanda el contenedor que ha creado en la sección 1 y compruebe que aparecen siete nuevos archivos de copia de seguridad junto con los archivos de datos de las secciones anteriores (actualice el nodo según sea necesario).  
  
    ![Varias instantáneas en el contenedor de Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/tutorial-snapshots-in-container.png)

## <a name="7---restore-a-database-to-a-point-in-time"></a>7. Restaurar una base de datos a un momento dado

En esta sección, restaurará la base de datos AdventureWorks2016 a un momento dado entre dos de las copias de seguridad del registro de transacciones.  
  
Con las copias de seguridad tradicionales, para lograr una restauración a un momento dado, necesitará usar la copia de seguridad de la base de datos completa, quizás una copia de seguridad diferencial y todos los archivos de registro de transacciones hasta el momento dado y justo después del momento dado al que quiere restaurar. Con las copias de seguridad de instantáneas de archivos, solo necesita los dos archivos de copia de seguridad de registros adyacentes que proporcionan los objetivos que enmarcan el momento al que quiere restaurar. Solo necesita dos conjuntos de copia de seguridad de registros de instantáneas de archivos, porque cada copia de seguridad de registros crea una instantánea de archivo de cada archivo de base de datos (cada archivo de datos y el archivo de registro).  
  
Para restaurar una base de datos a un momento dado a partir de conjuntos de copia de seguridad de instantáneas de archivos, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.  
2.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server 2016 del motor de base de datos de la máquina virtual de Azure.   
3.  Copie, pegue y ejecute el siguiente script de Transact-SQL en la ventana de consulta. Compruebe que la tabla Production.Location tiene 29 939 filas antes de restaurarla a un momento dado en el que tiene menos filas en el paso 5.  
  
    ```sql
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location   
    ```  
  
    ![Se muestra el recuento de filas de 29 939](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png) 
  
4.  Copie y pegue el siguiente script de Transact-SQL en la ventana de consulta. Seleccione dos archivos de copia de seguridad de registros adyacentes y convierta el nombre de archivo en la fecha y hora que necesitará para este script. Modifique la dirección URL correctamente para su nombre de cuenta de almacenamiento y el contenedor que ha especificado en la sección 1, proporcione el primer y segundo nombre de archivo de copia de seguridad, proporcione el momento STOPAT en el formato "26 de junio, 2018 01:48 p. m." y, después, ejecute este script. Este script tardará unos minutos en completarse  
  
    ```sql  
    -- restore and recover to a point in time between the times of two transaction log backups, and then verify the row count  
    ALTER DATABASE AdventureWorks2016 SET SINGLE_USER WITH ROLLBACK IMMEDIATE;  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<firstbackupfile>.bak'   
       WITH NORECOVERY,REPLACE;  
    RESTORE LOG AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<secondbackupfile>.bak'    
       WITH RECOVERY, STOPAT = 'June 26, 2018 01:48 PM';  
    ALTER DATABASE AdventureWorks2016 set multi_user;  
    -- get new count  
    SELECT COUNT (*) FROM AdventureWorks2016.Production.Location ;
    ```  
  
5.  Revise el resultado. Observe que, después de la restauración, el recuento de filas es 18 389, que es un número de recuento de filas entre la copia de seguridad de registros 5 y 6 (su recuento de filas puede variar).  
  
    ![18-thousand-rows.JPG](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/18-thousand-rows.png)

## <a name="8----restore-as-new-database-from-log-backup"></a>8. Restaurar como una base de datos nueva desde una copia de seguridad de registros

En esta sección, restaurará la base de datos AdventureWorks2016 como una base de datos nueva desde una copia de seguridad del registro de transacciones de instantáneas de archivos.  
  
En este escenario, realizará una restauración a una instancia de SQL Server en una máquina virtual diferente a efectos de análisis de negocio e informes. Al restaurar en una instancia diferente en una máquina virtual diferente, se descarga la carga de trabajo en una máquina virtual dedicada y con un tamaño para este propósito, quitando los requisitos del recurso del sistema transaccional.  
  
La restauración desde una copia de seguridad del registro de transacciones con una copia de seguridad de instantáneas de archivos es muy rápida, considerablemente más rápido que con las copias de seguridad de streaming tradicionales. Con las copias de seguridad de streaming tradicionales, usaba la copia de seguridad completa de la base de datos, quizás una copia de seguridad diferencial y algunas o todas las copias de seguridad del registro de transacciones (o una copia de seguridad completa de la base de datos nueva). En cambio, con las copias de seguridad de registros de instantáneas de archivos, solo necesita la copia de seguridad de registros más reciente (o cualquier otra copia de seguridad de registros o dos copias de seguridad de registros adyacentes para la restauración a un momento dado entre dos copias de seguridad de registros). Para que quede claro, solo necesita un conjunto de copia de seguridad de registros de instantáneas de archivos, porque cada copia de seguridad de registros de instantáneas de archivos crea una instantánea de archivo de cada archivo de base de datos (cada archivo de datos y el archivo de registro).  
  
Para restaurar una base de datos a una base de datos nueva desde una copia de seguridad del registro de transacciones mediante la copia de seguridad de instantánea de archivo, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.   
2.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server 2016 del motor de base de datos de una máquina virtual de Azure.  
  
    > [!NOTE]  
    > Si se trata de una máquina virtual de Azure diferente de la que ha usado para las secciones anteriores, asegúrese de que ha seguido los pasos descritos en [2. Crear una credencial de SQL Server con una firma de acceso compartido](#2---create-a-sql-server-credential-using-a-shared-access-signature). Si desea restaurar a un contenedor diferente, siga los pasos descritos en [1. Crear una directiva de acceso almacenada y un almacenamiento de acceso compartido](#1---create-stored-access-policy-and-shared-access-storage) para el nuevo contenedor.  
  
3.  Copie y pegue el siguiente script de Transact-SQL en la ventana de consulta. Seleccione el archivo de copia de seguridad de registros que quiera usar. Modifique la dirección URL correctamente para su nombre de cuenta de almacenamiento y el contenedor que ha especificado en la sección 1, proporcione el nombre de archivo de la copia de seguridad de registros y ejecute este script.  
  
    ```sql  
    -- restore as a new database from a transaction log backup file  
    RESTORE DATABASE AdventureWorks2016_EOM   
        FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<logbackupfile.bak>'    
        WITH MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Data.mdf'  
       , MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Log.ldf'  
       , RECOVERY  
    --, REPLACE   
    ```  
  
4.  Revise el resultado para comprobar que la restauración se ha realizado correctamente.   
5.  En el Explorador de objetos, conéctese a Azure Storage.    
6.  Expanda Contenedores, expanda el contenedor que ha creado en la sección 1 (actualícelo si es necesario) y compruebe que aparecen los nuevos archivos de datos y de registro en el contenedor, junto con los blobs de las secciones anteriores.  
  
    ![Contenedor de Azure que muestra los archivos de datos y de registro para la nueva base de datos](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/new-db-in-azure-container.png)

## <a name="9---manage-backup-sets-and-file-snapshot-backups"></a>9. Administrar conjuntos de copia de seguridad y copias de seguridad de instantáneas de archivos

En esta lección, eliminará un conjunto de copia de seguridad mediante el procedimiento almacenado del sistema [sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md). Este procedimiento almacenado del sistema elimina el archivo de copia de seguridad y el archivo de instantánea de cada archivo de base de datos asociado a este conjunto de copia de seguridad.  
  
> [!NOTE]  
> Si intenta eliminar un conjunto de copia de seguridad simplemente eliminando el archivo de copia de seguridad del contenedor de blobs de Azure, solo eliminará el archivo de copia de seguridad en sí y se conservarán las instantáneas de archivo asociadas. Si se encuentra en esta situación, use la función del sistema [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) para identificar la dirección URL de las instantáneas de archivos huérfanas y use el procedimiento almacenado del sistema [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) para eliminar cada instantánea de archivos huérfanas. Para obtener más información, vea  [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Para eliminar un conjunto de copia de seguridad de instantánea de archivos, siga estos pasos:  
  
1.  Conéctese a SQL Server Management Studio.  
2.  Abra una nueva ventana de consulta y conéctese a la instancia de SQL Server 2016 del motor de base de datos de la máquina virtual de Azure (o a cualquier instancia de SQL Server 2016 con permisos de lectura y escritura en este contenedor).   
3.  Copie y pegue el siguiente script de Transact-SQL en la ventana de consulta. Seleccione la copia de seguridad de registros que quiere eliminar junto con sus instantáneas de archivos asociadas. Modifique la dirección URL correctamente para su nombre de cuenta de almacenamiento y el contenedor que ha especificado en la sección 1, proporcione el nombre de archivo de la copia de seguridad de registros y ejecute este script.  
  
  ```sql
  sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-21764-20181003205236.bak';  
  ```  
  
4.  En el Explorador de objetos, conéctese a Azure Storage.  
5.  Expanda Contenedores, expanda el contenedor creado en la sección 1 y compruebe que el archivo de copia de seguridad usado en el paso 3 ya no aparece en este contenedor (actualice el nodo si es necesario).  
  
    ![Contenedor de Azure que muestra la eliminación del blob de copia de seguridad del registro](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/deleted-backup-snapshot.png)
  
6.  Copie, pegue y ejecute el siguiente script de Transact-SQL en la ventana de consulta para comprobar que se han eliminado dos instantáneas de archivos.  
  
    ```sql  
    -- verify that two file snapshots have been removed  
    SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');   
    ```  
    ![Panel de resultados que muestra 2 instantáneas de archivos eliminadas](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-of-two-deleted-snapshot-files.png)

## <a name="10---remove-resources"></a>10. Quitar recursos

Una vez que haya terminado con este tutorial, y para ahorrar recursos, asegúrese de eliminar el grupo de recursos creado con este fin. 

Para eliminar el grupo de recursos, ejecute el siguiente código de PowerShell:

  ```powershell
  # Define global variables for the script  
  $prefixName = '<prefix name>'  # should be the same as the beginning of the tutorial
  
  # Set a variable for the name of the resource group you will create or use  
  $resourceGroupName=$prefixName + 'rg'   
  
  # Adds an authenticated Azure account for use in the session   
  Connect-AzAccount    
  
  # Set the tenant, subscription and environment for use in the rest of   
  Set-AzContext -SubscriptionId $subscriptionID    
    
  # Remove the resource group
  Remove-AzResourceGroup -Name $resourceGroupName   
  ```


  
## <a name="see-also"></a>Consulte también

[Archivos de datos de SQL Server en Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
[Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[Copia de seguridad en URL de SQL Server](../relational-databases/backup-restore/sql-server-backup-to-url.md) 
[Firmas de acceso compartido, Parte 1: Descripción del modelo SAS](/azure/storage/common/storage-sas-overview)  
[Creación de un contenedor](/rest/api/storageservices/Create-Container)  
[Establecer lista de control de acceso de contenedor](/rest/api/storageservices/Set-Container-ACL)  
[Get Container ACL](/rest/api/storageservices/Get-Container-ACL) (Obtención de ACL de contenedor)
[Credenciales &#40;motor de base de datos&#41;](../relational-databases/security/authentication-access/credentials-database-engine.md)  
[CREATE CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-credential-transact-sql.md)  
[sys.credentials &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
[sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
[sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)  
[sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)

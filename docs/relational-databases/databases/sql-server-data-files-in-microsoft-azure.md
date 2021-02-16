---
title: Archivos de datos de SQL Server en Microsoft Azure | Microsoft Docs
description: Descubra conceptos y consideraciones fundamentales para almacenar archivos de datos de SQL Server en Microsoft Azure Storage y algunas ventajas del uso de Azure Storage.
ms.custom: ''
ms.date: 12/04/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: 38ffd9c2-18a5-43d2-b674-e425addec4e4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8e9138ff27909f9236a608fd0c66f9dca8d5addf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351457"
---
# <a name="sql-server-data-files-in-microsoft-azure"></a>Archivos de datos de SQL Server en Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  ![Archivos de datos en Azure](../../relational-databases/databases/media/data-files-on-azure.png "Archivos de datos en Azure")  
  
La característica Archivos de datos de SQL Server en Microsoft Azure habilita la compatibilidad nativa para los archivos de base de datos de SQL Server almacenados como blobs. Permite crear una base de datos de SQL Server que se ejecuta en el entorno local o en una máquina virtual de Microsoft Azure con una ubicación de almacenamiento dedicada para los datos de Microsoft Azure Blob Storage. También simplifica el proceso de mover bases de datos entre equipos. Puede desconectar las bases de datos de un equipo y conectarlas a otro. Además, proporciona una ubicación de almacenamiento alternativa para los archivos de copia de seguridad de la base de datos, lo que permite realizar la restauración desde o hasta Almacenamiento de Microsoft Azure. Por tanto, habilita diversas soluciones híbridas al aportar varias ventajas en cuanto a virtualización de datos, movimiento de datos, seguridad y disponibilidad, así como costos y mantenimiento reducidos para lograr escalado flexible y alta disponibilidad.
 
> [!IMPORTANT]  
>  El almacenamiento de bases de datos del sistema en Azure Blob Storage no es recomendable y no se admite. 

 En este tema se describen los conceptos y los factores básicos a la hora de almacenar archivos de datos de SQL Server en el Servicio de Azure Storage.  
  
 Para experimentar de manera práctica cómo usar esta nueva característica, consulte el [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
## <a name="why-use-sql-server-data-files-in-microsoft-azure"></a>¿Por qué usar archivos de datos de SQL Server en Microsoft Azure? 

- **Ventajas de una migración rápida y sencilla:** Esta característica simplifica el proceso de migración, para lo que mueve una base de datos cada vez entre equipos locales así como entre entornos locales y en la nube sin cambios en la aplicación. Por tanto, admite una migración incremental a la vez que conserva la infraestructura local existente. Además, tener acceso a un almacenamiento de datos centralizado simplifica la lógica de la aplicación en los casos en los que una aplicación deba ejecutarse en varias ubicaciones en un entorno local. En algunos casos, es posible que necesite configurar rápidamente centros informáticos en ubicaciones geográficamente dispersas que recopilen datos de muchas fuentes diferentes. Con esta nueva mejora, en lugar de mover datos de una ubicación a otra, puede almacenar muchas bases de datos como blobs de Microsoft Azure y después ejecutar scripts Transact-SQL para crear bases de datos en equipos locales o máquinas virtuales.

- **Ventajas económicas y de espacio de almacenamiento ilimitado:** esta característica le permite disponer de almacenamiento externo ilimitado en Microsoft Azure mientras aprovecha los recursos informáticos locales. Si usa Microsoft Azure como ubicación de almacenamiento, puede centrarse tranquilamente en la lógica de la aplicación sin la sobrecarga de administración de hardware. Si pierde un nodo de proceso local, puede configurar uno nuevo sin tener que mover datos.

- **Ventajas de la alta disponibilidad y recuperación ante desastres:** si usa la característica de archivos de datos de SQL Server en Microsoft Azure, puede simplificar las soluciones de alta disponibilidad y recuperación ante desastres. Por ejemplo, si se bloquea una máquina virtual en Microsoft Azure o una instancia de SQL Server, puede volver a crear las bases de datos en una instancia nueva de SQL Server simplemente restableciendo los vínculos a los blobs.

- **Ventajas de seguridad:** esta nueva mejora permite separar una instancia de proceso de una instancia de almacenamiento. Puede tener una base de datos completamente cifrada donde el descifrado solo se produce en una instancia de proceso, pero no en una instancia de almacenamiento. Es decir, con esta nueva mejora, puede cifrar todos los datos de la nube pública mediante certificados de cifrado de datos transparente (TDE), que están separados físicamente de los datos. Las claves TDE se pueden almacenar en la base de datos maestra, la cual se almacena localmente en el equipo local protegido físicamente y con copia de seguridad local. Puede usar estas claves locales para cifrar los datos que se encuentran en Almacenamiento de Microsoft Azure. Si roban las credenciales de la cuenta de almacenamiento en la nube, los datos seguirán estando protegidos ya que los certificados TDE siempre se encuentran en el entorno local.

- **Copia de seguridad de instantáneas:**  esta característica le permite usar las instantáneas de Azure para proporcionar copias de seguridad prácticamente instantáneas y restauraciones más rápidas de los archivos de base de datos almacenados mediante el servicio Azure Blob Storage. Esta capacidad le permite simplificar las directivas de copia de seguridad y restauración. Para obtener más información, vea [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  

## <a name="concepts-and-requirements"></a>Conceptos y requisitos  
  
Los discos de Azure son compatibles con las soluciones de continuidad empresarial y recuperación ante desastres de toda la empresa. Si almacena las bases de datos directamente en blobs, o en archivos premium de Azure, los datos no se asocian automáticamente a la máquina virtual para la infraestructura, administración y supervisión.

La colocación de archivos de bases de datos en blobs en páginas es una característica más avanzada que el uso de discos de Azure, que son sencillos y fáciles de usar.

La instrucción básica es usar discos de Azure, a menos que en su caso realmente necesite evitar la creación de una copia de los datos para copias de seguridad o la restauración como una operación de tamaño de datos. Para lograr una alta disponibilidad y recuperación ante desastres, también es mucho más útil usar copias de seguridad periódicas para la dirección URL o copias de seguridad administradas en Blob Storage, en lugar de copias de seguridad de instantáneas de archivos, ya que de ese modo logra la administración del ciclo de vida, la compatibilidad con varias regiones, la eliminación temporal y todas las demás características del almacenamiento de blobs de las copias de seguridad.

### <a name="azure-storage-concepts"></a>Conceptos de Azure Storage  
Cuando use la característica Archivos de datos de SQL Server en Azure, debe crear una cuenta de almacenamiento y un contenedor en Azure. A continuación, debe crear una credencial de SQL Server, que incluye información sobre la directiva del contenedor así como una firma de acceso compartido, la cual resulta necesaria para tener acceso al contenedor.  

En [Microsoft Azure](https://azure.microsoft.com), una cuenta de [Azure Storage](https://azure.microsoft.com/services/storage/) representa el nivel superior del espacio de nombres para tener acceso a los blobs. Una cuenta de almacenamiento puede contener un número ilimitado de contenedores, siempre que su tamaño total sea inferior a los límites de almacenamiento. Para obtener la información más reciente acerca de los límites de almacenamiento, vea [Suscripciones de Azure y límites de servicio, cuotas y restricciones](/azure/azure-subscription-service-limits). Un contenedor proporciona una agrupación de un conjunto de [blobs](/azure/storage/common/storage-introduction#blob-storage). Todos los blobs deben estar en un contenedor. Una cuenta puede contener un número ilimitado de contenedores. De forma similar, un contenedor puede almacenar un número ilimitado de blobs. Existen dos tipos de blobs que pueden almacenarse en Azure Storage: blobs en páginas y en bloques. Esta nueva característica utiliza blobs en páginas, que son más eficaces cuando los intervalos de bytes en el archivo se modifican con frecuencia. Puede obtener acceso a blobs mediante el siguiente formato de dirección URL: `https://storageaccount.blob.core.windows.net/<container>/<blob>`.  

### <a name="azure-billing-considerations"></a>Consideraciones sobre la facturación de Azure  

 Estimar el costo de usar los servicios de Azure es una cuestión importante a la hora de tomar decisiones y en el proceso de planeamiento. Al almacenar archivos de datos de SQL Server en Azure Storage, deberá correr con los gastos asociados al almacenamiento y las transacciones. Además, la implementación de la característica Archivos de datos de SQL Server en Azure Storage requiere una renovación de la concesión de blobs cada 45-60 segundos de forma implícita. Esto también ocasiona costos de transacción por archivo de base de datos, como .mdf y .ldf. Use la información de la página [Precios de Azure](https://azure.microsoft.com/pricing/) como ayuda para estimar los costos mensuales asociados al uso de Azure Storage y Azure Virtual Machines.  
  
### <a name="sql-server-concepts"></a>Conceptos de SQL Server  

Cuando use esta nueva mejora, deberá hacer lo siguiente:

- Cree una directiva en un contenedor y genere una clave de firma de acceso compartido (SAS).

- Cree una credencial de SQL Server cuyo nombre coincida con la ruta de acceso del contenedor para cada uno de los que emplee un archivo de datos o de registro.

- Almacene la información sobre el contenedor de Azure Storage, su nombre de directiva asociado y la clave SAS en el almacén de credenciales de SQL Server.

En el ejemplo siguiente se da por supuesto que se ha creado un contenedor de almacenamiento de Azure, así como una directiva con derechos de lectura, escritura y enumeración. Cuando se crea una directiva en un contenedor, se genera una clave SAS que podrá guardar en la memoria de forma segura sin cifrar y que SQL Server necesita para acceder a los archivos de blob en el contenedor. 

En el siguiente fragmento de código, reemplace `'<your SAS key>'` por la clave SAS. La clave SAS tendrá el siguiente aspecto: `'sr=c&si=<MYPOLICYNAME>&sig=<THESHAREDACCESSSIGNATURE>'`.

```sql
CREATE CREDENTIAL [https://testdb.blob.core.windows.net/data]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '<your SAS key>'  
  
CREATE DATABASE testdb   
ON  
( NAME = testdb_dat,  
    FILENAME = 'https://testdb.blob.core.windows.net/data/TestData.mdf' )  
 LOG ON  
( NAME = testdb_log,  
    FILENAME =  'https://testdb.blob.core.windows.net/data/TestLog.ldf')  
```  

>[!IMPORTANT]
>Si hay referencias activas a archivos de datos en un contenedor, cualquier intento de eliminar la credencial correspondiente de SQL Server generará un error.

Para obtener más información, consulte [Manage anonymous read access to containers and blobs (Administración del acceso de lectura anónimo a contenedores y blobs)](/azure/storage/blobs/storage-manage-access-to-resources).  

### <a name="security"></a>Seguridad  
 A continuación se describen las consideraciones y los requisitos relativos a la seguridad cuando se almacenan archivos de datos de SQL Server en Azure Storage.

- Al crear un contenedor para el servicio Blob Storage de Azure, se recomienda establecer el acceso en privado. Al establecer el acceso en privado, solo el propietario de la cuenta de Azure puede leer el contenedor y los datos de blob.

- Cuando se almacenan archivos de la base de datos de SQL Server en Azure Storage, debe usar una firma de acceso compartido, un URI que concede derechos de acceso restringido a contenedores, blobs, colas y tablas. El uso de una firma de acceso compartido permite que SQL Server obtenga acceso a recursos de su cuenta de almacenamiento sin compartir la clave de su cuenta de Azure Storage.

- Por otra parte, se recomienda que siga implementando los procedimientos habituales de seguridad locales para las bases de datos.  
  
### <a name="installation-prerequisites"></a>Requisitos previos de instalación  
 A continuación se describen los requisitos previos de instalación cuando se almacenan archivos de datos de SQL Server en Azure.

- **SQL Server local:** SQL Server 2016 y versiones posterior incluyen esta característica. Para obtener información sobre cómo descargar la última versión de SQL Server, vea [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads).

- SQL Server en una máquina virtual de Azure: si va a instalar [SQL Server en una máquina virtual de Azure](https://azuremarketplace.microsoft.com/marketplace/apps?search=sql%20server&page=1), instale SQL Server 2016 o actualice la instancia existente. Del mismo modo, también puede crear una nueva máquina virtual de Azure mediante una imagen de plataforma de SQL Server 2016.

  
###  <a name="limitations"></a><a name="bkmk_Limitations"></a> Limitaciones  
  
- En la versión actual de esta característica, no se admite almacenar datos **FileStream** en Azure Storage. Puede almacenar datos de **FileStream** en una base de datos que también contenga archivos de datos almacenados en Azure Storage, pero todos los archivos de datos de FileStream se deben almacenar en el almacenamiento local.  Puesto que los datos de FileStream deben residir en el almacenamiento local, no se pueden mover entre máquinas con Azure Storage; por lo tanto, se recomienda usar las [técnicas tradicionales](../../relational-databases/blob/move-a-filestream-enabled-database.md) para mover los datos asociados con FileStream entre las distintas máquinas.  
  
- Actualmente, esta nueva mejora no admite que más de una instancia de SQL Server tenga acceso a los mismos archivos de base de datos de Azure Storage al mismo tiempo. Si el Servidor A está en línea con un archivo de base de datos activo y el Servidor B se inicia accidentalmente y tiene también una base de datos que apunta al mismo archivo de datos, el segundo servidor no podrá iniciar la base de datos y se generará un error con un código **5120 No se puede abrir el archivo físico "%.\*ls". Error del sistema operativo %d: "%ls"** .  
  
- Solo los archivos .mdf, .ldf y .ndf se pueden almacenar en Azure Storage con la característica Archivos de datos de SQL Server en Azure.  
  
- Cuando se utiliza la característica Archivos de datos de SQL Server en Azure, no se admite la replicación geográfica para la cuenta de almacenamiento. Si se realiza la replicación geográfica en la cuenta de almacenamiento y se produce una conmutación por error geográfica, se dañará la base de datos.  
  
- Para conocer las limitaciones de capacidad, vea [Introducción a Blob Storage](/azure/storage/blobs/storage-blobs-introduction).  
  
- No es posible almacenar datos de OLTP en memoria en Blob Storage con la característica Archivos de datos de SQL Server en Azure Storage. Esto se debe a que OLTP en memoria tiene una dependencia de **FileStream** y, en la versión actual de esta característica, no se admite almacenar datos **FileStream** en Azure Storage.  
  
- Cuando se utiliza la característica Archivos de datos de SQL Server en Azure, SQL Server realiza todas las comparaciones de direcciones URL o de rutas de acceso de archivo mediante la intercalación establecida en la base de datos **maestra**.  
  
- Se admiten **Grupos de disponibilidad AlwaysOn** siempre y cuando no agregue nuevos archivos de base de datos a la base de datos principal. Si una operación de base de datos requiere que se cree un nuevo archivo en la base de datos principal, primero deshabilite los grupos de disponibilidad en el nodo secundario. Posteriormente, realice la operación de base de datos en la base de datos principal y haga una copia de seguridad de la base de datos en el nodo principal. Después, restaure la base de datos en el nodo secundario. Cuando termine, vuelva a habilitar los grupos de disponibilidad AlwaysOn en el nodo secundario. 

   >[!NOTE]
   >Las instancias de clúster de conmutación por error de AlwaysOn no se admiten al usar Archivos de datos de SQL Server en Azure.
  
- Durante el funcionamiento normal, SQL Server utiliza concesiones temporales con el fin de reservar y almacenar blobs con una renovación de cada concesión de blob cada 45-60 segundos. Si se bloquea un servidor y se inicia otra instancia de SQL Server configurada para usar los mismos blobs, la instancia nueva esperará hasta 60 segundos para que expire la concesión existente del blob. Si quiere conectar la base de datos a otra instancia y no puede esperar 60 segundos a que expire la concesión, puede liberar explícitamente la concesión en el blob.
  
## <a name="tools-and-programming-reference-support"></a>Herramientas y compatibilidad con referencia de programación  
 En esta sección se describen las herramientas y las bibliotecas de referencia de programación que se pueden utilizar al almacenar archivos de datos de SQL Server en Azure Storage.  
  
### <a name="powershell-support"></a>Compatibilidad con PowerShell  
 Use cmdlets de PowerShell para almacenar archivos de datos de SQL Server en el servicio de Blob Storage; para ello, se debe hacer referencia a una ruta de acceso de dirección URL de Blob Storage en lugar de a una ruta de acceso de archivos. Obtenga acceso a blobs mediante el siguiente formato de dirección URL: `https://storageaccount.blob.core.windows.net/<container>/<blob>`.  
  
### <a name="sql-server-object-and-performance-counters-support"></a>Objetos de SQL Server y compatibilidad con contadores de rendimiento  
 A partir de SQL Server 2014, se ha agregado un nuevo objeto de SQL Server que se usará con la característica Archivos de datos de SQL Server en Azure Storage. El nuevo objeto de SQL Server se denomina [SQL Server, HTTP_STORAGE_OBJECT](../../relational-databases/performance-monitor/sql-server-http-storage-object.md) y lo puede usar el Monitor de sistema para supervisar la actividad cuando se ejecuta SQL Server con Azure Storage.  
  
### <a name="sql-server-management-studio-support"></a>Compatibilidad con SQL Server Management Studio  
 SQL Server Management Studio le permite usar esta característica a través de varias ventanas de diálogo. Por ejemplo, `https://teststorageaccnt.blob.core.windows.net/testcontainer/` representa la ruta de acceso de la dirección URL de un contenedor de almacenamiento.
 
 como una **Ruta de acceso** en varias ventanas de diálogo, como **Nueva base de datos**, **Adjuntar base de datos** y **Restaurar base de datos**. Para más información, consulte el [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
### <a name="sql-server-management-objects-smo-support"></a>Compatibilidad con Objetos de administración de SQL Server (SMO)  
 Cuando se usa la característica Archivos de datos de SQL Server en Azure, se admiten todos los Objetos de administración de SQL Server (SMO). Si un objeto SMO requiere una ruta de acceso, use el formato de dirección URL BLOB en lugar de una ruta de acceso de archivos local, como `https://teststorageaccnt.blob.core.windows.net/testcontainer/`. Para obtener más información sobre Objetos de administración de SQL Server (SMO), vea [Guía de programación para objetos de administración de SQL Server &#40;SMO&#41;](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)en los Libros en pantalla de SQL Server.  
  
### <a name="transact-sql-support"></a>Compatibilidad con Transact-SQL  
 Esta nueva característica presenta el siguiente cambio en el área expuesta de Transact-SQL:

- Una nueva columna **int** , **credential_id**, en la vista del sistema **sys.master_files** . La columna **credential_id** se usa para permitir la referencia cruzada de los archivos de datos de Azure Storage en `sys.credentials` para las credenciales creadas para ellos. Puede usarla para solucionar problemas, como que una credencial no se puede eliminar cuando un archivo de base de datos la está usando.  
  
##  <a name="troubleshooting-for-sql-server-data-files-in-microsoft-azure"></a><a name="bkmk_Troubleshooting"></a> Solución de problemas de Archivos de datos de SQL Server en Microsoft Azure  
 Para evitar errores por funciones no admitidas o limitaciones, primero consulte [Limitations](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md#bkmk_Limitations).  
  
 La lista de errores que puede obtener cuando se usa la característica Archivos de datos de SQL Server en Azure Storage es la siguiente:  
  
 **Errores de autenticación**  
  
- *No se puede quitar la credencial '%.\*ls' porque la usa un archivo de base de datos activo.*    
    Resolución: puede ver este error si intenta quitar una credencial que aún se use con un archivo de base de datos activo en Azure Storage. Para quitar la credencial, en primer lugar, debe eliminar el blob asociado que tiene este archivo de base de datos. Para eliminar un blob que tiene una concesión activa, debe liberar primero la concesión.  
  
- *No se ha creado correctamente la firma de acceso compartido en el contenedor.*    
     Resolución: asegúrese de haber creado correctamente una firma de acceso compartido en el contenedor. Consulte las instrucciones de la lección 2 del [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature).  
  
- *La credencial de SQL Server no se ha creado correctamente.*    
    Resolución: asegúrese de que ha usado "Firma de acceso compartido" en el campo **Identidad** y que ha creado un secreto correctamente. Consulte las instrucciones de la lección 3 del [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#3---database-backup-to-url).  
  
 **Errores de concesión de blobs:**  
  
- Error al intentar iniciar SQL Server después de que otra instancia que usa los mismos archivos de blob se haya bloqueado. Resolución: Durante el funcionamiento normal, SQL Server utiliza concesiones temporales con el fin de reservar y almacenar blobs con una renovación de cada concesión de blob cada 45-60 segundos. Si se bloquea un servidor y se inicia otra instancia de SQL Server configurada para usar los mismos blobs, la instancia nueva esperará hasta 60 segundos para que expire la concesión existente del blob. Si quiere conectar la base de datos a otra instancia y no puede esperar 60 segundos a que expire la concesión, puede liberar explícitamente la concesión en el blob para evitar errores en las operaciones de conexión.  
  
 **Errores de base de datos:**  
  
**Errores al crear una base de datos** Resolución: Consulte las instrucciones de la lección 4 del [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#4----restore-database-to-virtual-machine-from-url).  
  
**Errores al ejecutar la instrucción ALTER** Solución: asegúrese de ejecutar la instrucción ALTER DATABASE cuando la base de datos esté en línea. Cuando copie archivos de datos en Azure Storage, cree siempre un blob en páginas y no un blob en bloques. De lo contrario, ALTER DATABASE no se ejecutará correctamente. Consulte las instrucciones de la lección 7 del [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
**Código de error 5120: no se puede abrir el archivo físico "%.\*ls"; Error del sistema operativo %d: "%ls"**   

Resolución: Actualmente, esta nueva mejora no admite que más de una instancia de SQL Server tenga acceso a los mismos archivos de base de datos de Azure Storage al mismo tiempo. Si el Servidor A está en línea con un archivo de base de datos activo y el Servidor B se inicia accidentalmente y tiene también una base de datos que apunta al mismo archivo de datos, el segundo servidor no podrá iniciar la base de datos y se generará un error con un código *5120 No se puede abrir el archivo físico "%.\*ls". Error del sistema operativo %d: "%ls"* .  
  
Para resolver este problema, determine en primer lugar si necesita que el Servidor A obtenga acceso al archivo de base de datos de Azure Storage o no. Si no es así, quite las conexiones entre el Servidor A y los archivos de base de datos de Azure Storage. Para ello, siga estos pasos.  

1.  Establezca la ruta de acceso de ServidorA en una carpeta local mediante la instrucción ALTER DATABASE.  

2.  Ponga la base de datos sin conexión en el ServidorA.  

3.  Luego copie los archivos de base de datos de Azure Storage en la carpeta local del Servidor A. Esto garantiza que el Servidor A siga conservando localmente una copia de la base de datos.  

4.  Ponga la base de datos en línea.

**Código de error 833: las solicitudes de E/S están tardando más de 15 segundos en completarse** 
   
   Este error indica que el sistema de almacenamiento no puede satisfacer las demandas de la carga de trabajo de SQL Server. Reduzca la actividad de E/S de la capa de aplicación, o bien aumente la capacidad de rendimiento en la capa de almacenamiento. Para obtener más información, vea el [error 833](../errors-events/mssqlserver-833-database-engine-error.md). Si los problemas de rendimiento persisten, valore la posibilidad de mover archivos a una capa de almacenamiento diferente, como Premium o UltraSSD. Con respecto a SQL Server en máquinas virtuales de Azure, consulte la [optimización el rendimiento de almacenamiento](/azure/virtual-machines/premium-storage-performance).


## <a name="next-steps"></a>Pasos siguientes  
  
[Creación de una base de datos](create-a-database.md)

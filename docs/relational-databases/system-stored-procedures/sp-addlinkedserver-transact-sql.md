---
description: sp_addlinkedserver (Transact-SQL)
title: sp_addlinkedserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addlinkedserver_TSQL
- sp_addlinkedserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedserver
ms.assetid: fed3adb0-4c15-4a1a-8acd-1b184aff558f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c894bbf5bdb3e523901fc4f1200d146d0a569449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171784"
---
# <a name="sp_addlinkedserver-transact-sql"></a>sp_addlinkedserver (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un servidor vinculado. Un servidor vinculado permite obtener acceso a consultas heterogéneas distribuidas en orígenes de datos OLE DB. Después de crear un servidor vinculado mediante **sp_addlinkedserver**, las consultas distribuidas se pueden ejecutar en este servidor. Si el servidor vinculado se define como una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se pueden ejecutar procedimientos almacenados remotos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_addlinkedserver [ @server= ] 'server' [ , [ @srvproduct= ] 'product_name' ]   
     [ , [ @provider= ] 'provider_name' ]  
     [ , [ @datasrc= ] 'data_source' ]   
     [ , [ @location= ] 'location' ]   
     [ , [ @provstr= ] 'provider_string' ]   
     [ , [ @catalog= ] 'catalog' ]   
```  
  
## <a name="arguments"></a>Argumentos  
[ @server =] *\' servidor \'*          
Es el nombre del servidor vinculado que se va a crear. *server* es de tipo **sysname** y no tiene ningún valor predeterminado.  
  
[ @srvproduct =] *\' PRODUCT_NAME \'*          
Es el nombre del producto del origen de datos OLE DB para agregarlo como servidor vinculado. *PRODUCT_NAME* es de tipo **nvarchar (** 128 **)** y su valor predeterminado es NULL. Si no es necesario especificar **SQL Server**, *provider_name*, *data_source*, *Ubicación*, *provider_string* y *Catálogo* .  
  
[ @provider =] *\' provider_name \'*          
Es el identificador de programación (PROGID) único del proveedor OLE DB que corresponde a este origen de datos. *provider_name* debe ser único para el proveedor de OLE DB especificado instalado en el equipo actual. *provider_name* es de tipo **nvarchar (128)** y su valor predeterminado es null; sin embargo, si se omite *provider_name* , se utiliza SQLNCLI. 

> [!NOTE]
> El uso de SQLNCLI redirigirá [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la versión más reciente del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client. Se espera que el proveedor OLE DB se registre con el PROGID especificado en el registro.

> [!IMPORTANT] 
> El anterior proveedor de Microsoft OLE DB para SQL Server (SQLOLEDB) y de OLE DB para SQL Server Native Client (SQLNCLI) permanece en desuso y no se recomienda utilizarlo para nuevos trabajos de desarrollo. En su lugar, use el nuevo [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), que se actualizará con las características de servidor más recientes.
  
[ @datasrc =] *\' data_source \'*          
 Es el nombre del origen de datos como lo interpreta el proveedor OLE DB. *data_source* es **nvarchar (** 4000 **)**. *data_source* se pasa como la propiedad DBPROP_INIT_DATASOURCE para inicializar el proveedor de OLE DB.  
  
[ @location =] *\' Ubicación \'*          
 Es la ubicación de la base de datos según la interpretación del proveedor OLE DB. *Location* es de tipo **nvarchar (** 4000 **)** y su valor predeterminado es NULL. *Location* se pasa como la propiedad DBPROP_INIT_LOCATION para inicializar el proveedor de OLE DB.  
  
[ @provstr =] *\' provider_string \'*          
 Es la cadena de conexión específica del proveedor OLE DB que identifica un origen de datos único. *provider_string* es de tipo **nvarchar (** 4000 **)** y su valor predeterminado es NULL. *provstr* se pasa a IDataInitialize o se establece como la propiedad DBPROP_INIT_PROVIDERSTRING para inicializar el proveedor de OLE DB.  
  
 Cuando el servidor vinculado se crea en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client, la instancia puede especificarse mediante la palabra clave Server como Server =*ServerName* \\ *InstanceName* para especificar una instancia específica de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *ServerName* es el nombre del equipo en el que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando e *nombreDeInstancia* es el nombre de la instancia específica de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que se conectará el usuario.  
  
> [!NOTE]
> Para tener acceso a una base de datos reflejada, una cadena de conexión debe contener el nombre de la base de datos. Este nombre es necesario para que el proveedor de acceso a datos pueda intentar la conmutación por error. La base de datos se puede especificar en el parámetro **\@ provstr** o **\@ Catalog** . Opcionalmente, la cadena de conexión también puede proporcionar un nombre de asociado de conmutación por error.  
  
[ @catalog =] *\' Catálogo \'*       
 Es el catálogo que se va a utilizar cuando se establece una conexión con el proveedor de OLE DB. *Catalog* es de **tipo sysname y su** valor predeterminado es NULL. *Catalog* se pasa como la propiedad DBPROP_INIT_CATALOG para inicializar el proveedor de OLE DB. Cuando se define el servidor vinculado en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], catalog se refiere a la base de datos predeterminada a la que se asigna el servidor vinculado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Ninguno.  
  
## <a name="remarks"></a>Observaciones  
 En la tabla siguiente se muestran las distintas maneras de configurar un servidor vinculado para los orígenes de datos a los que se puede tener acceso a través de OLE DB. Existen varias formas de configurar un servidor vinculado para un origen de datos determinado; puede haber más de una fila por tipo de origen de datos. En esta tabla también se muestran los valores de parámetro **sp_addlinkedserver** que se van a usar para configurar el servidor vinculado.  
  
|Origen de datos remotos de OLE DB|Proveedor OLE DB|product_name|provider_name|data_source|ubicación|provider_string|catalog|  
|-------------------------------|---------------------|-------------------|--------------------|------------------|--------------|----------------------|-------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Proveedor de OLE DB de Native Client|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<sup>1</sup> (valor predeterminado)||||||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Proveedor de OLE DB de Native Client||**SQLNCLI**|Nombre de red de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (para la instancia predeterminada)|||Nombre de la base de datos (opcional)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Proveedor de OLE DB de Native Client||**SQLNCLI**|*nombreDeServidor* \\ *nombreDeInstancia* (para la instancia específica)|||Nombre de la base de datos (opcional)|  
|Oracle, versión 8 y posterior|Proveedor Oracle para OLE DB|Any|**OraOLEDB.Oracle**|Alias para base de datos Oracle||||  
|Access/Jet|Proveedor Microsoft OLE DB para Jet|Any|**Microsoft.Jet.OLEDB.4.0**|Ruta de acceso completa del archivo de base de datos Jet||||  
|Origen de datos ODBC|Proveedor Microsoft OLE DB para ODBC|Any|**MSDASQL**|DSN del sistema del origen de datos ODBC||||  
|Origen de datos ODBC|Proveedor [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB para ODBC|Any|**MSDASQL**|||Cadena de conexión ODBC||  
|Sistema de archivos|Proveedor [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB para Servicios de Index Server|Any|**MSIDXS**|Nombre de catálogo de Servicios de Index Server||||  
|Hojas de cálculo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel|Proveedor [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB para Jet|Any|**Microsoft.Jet.OLEDB.4.0**|Ruta de acceso completa del archivo Excel||Excel 5,0||  
|Base de datos IBM DB2|Proveedor OLE DB de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para DB2|Any|**DB2OLEDB**|||Consulte la [!INCLUDE[msCoName](../../includes/msconame-md.md)] documentación del proveedor de OLE DB para DB2.|Nombre del catálogo de la base de datos DB2|  
  
 <sup>1</sup> esta forma de configurar un servidor vinculado fuerza que el nombre del servidor vinculado sea el mismo que el nombre de red de la instancia remota de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Use *data_source* para especificar el servidor.  
  
 <sup>2</sup> "any" indica que el nombre del producto puede ser cualquier cosa.  
  
 El [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client es el proveedor que se utiliza con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si no se especifica ningún nombre de proveedor o si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se especifica como el nombre del producto. Incluso si especifica el nombre de proveedor antiguo, SQLOLEDB, éste se cambiará a SQLNCLI cuando se mantenga en el catálogo.  
  
 Los parámetros *data_source*, *Location*, *provider_string* y *Catalog* identifican la base de datos o las bases de datos a las que apunta el servidor vinculado. Si alguno de estos parámetros es NULL, no se establecerá la propiedad de inicialización de OLE DB correspondiente.  
  
 En un entorno en clúster, al especificar nombres de archivo para apuntar a orígenes de datos OLE DB, utilice el nombre UNC (convención de nomenclatura universal) o una unidad compartida para especificar la ubicación.  
  
 **sp_addlinkedserver** no se puede ejecutar en una transacción definida por el usuario.  
  
> [!IMPORTANT]
> Cuando se crea un servidor vinculado mediante **sp_addlinkedserver**, se agrega una autoasignación predeterminada para todos los inicios de sesión locales. En el caso de los proveedores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es posible que los inicios de sesión autenticados puedan obtener acceso al proveedor en la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio. Los administradores deben tener en cuenta el uso de `sp_droplinkedsrvlogin <linkedserver_name>, NULL` para quitar la asignación global.  
  
## <a name="permissions"></a>Permisos  
 La `sp_addlinkedserver` instrucción requiere el `ALTER ANY LINKED SERVER` permiso. (El [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] El cuadro de diálogo **nuevo servidor vinculado** se implementa de forma que requiere la pertenencia al `sysadmin` rol fijo de servidor.)  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-microsoft-sql-server-ole-db-provider"></a>A. Usar el proveedor de OLE DB de Microsoft SQL Server  
 En el siguiente ejemplo se crea un servidor vinculado denominado `SEATTLESales`. El nombre del producto es `SQL Server` y no se utiliza ningún nombre de proveedor.  
  
```sql  
USE master;  
GO  
EXEC sp_addlinkedserver   
   N'SEATTLESales',  
   N'SQL Server';  
GO  
```  

 En el ejemplo siguiente se crea un servidor vinculado `S1_instance1` en una instancia de mediante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controlador OLE DB.  

```sql  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'MSOLEDBSQL',   
   @datasrc=N'S1\instance1';  
```  

 En el ejemplo siguiente se crea un servidor vinculado `S1_instance1` en una instancia de mediante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client.  
 
> [!IMPORTANT] 
> El proveedor OLE DB de SQL Server Native Client (SQLNCLI) permanece en desuso y no se recomienda utilizarlo para nuevos trabajos de desarrollo. En su lugar, use el nuevo [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), que se actualizará con las características de servidor más recientes.
  
```sql  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'SQLNCLI',   
   @datasrc=N'S1\instance1';  
```  
  
### <a name="b-using-the-microsoft-ole-db-provider-for-microsoft-access"></a>B. Usar el proveedor Microsoft OLE DB para Microsoft Access  
 El proveedor de Microsoft.Jet.OLEDB.4.0 conecta a las bases de datos de Microsoft Access que usan el formato 2002-2003. En el siguiente ejemplo se crea un servidor vinculado denominado `SEATTLE Mktg`.  
  
> [!NOTE]  
> En este ejemplo se da por supuesto que tanto el [!INCLUDE[msCoName](../../includes/msconame-md.md)] acceso como la base de datos de ejemplo **Northwind** están instalados y que la base de datos **Northwind** reside en C:\Msoffice\Access\Samples.  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.Jet.OLEDB.4.0',   
   @srvproduct = N'OLE DB Provider for Jet',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.mdb';  
GO  
```  
  
 El proveedor de Microsoft.ACE.OLEDB.12.0 conecta a las bases de datos de Microsoft Access que usan el formato 2007. En el siguiente ejemplo se crea un servidor vinculado denominado `SEATTLE Mktg`.  
  
> [!NOTE]  
> En este ejemplo se da por supuesto que tanto el [!INCLUDE[msCoName](../../includes/msconame-md.md)] acceso como la base de datos de ejemplo **Northwind** están instalados y que la base de datos **Northwind** reside en C:\Msoffice\Access\Samples.  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.ACE.OLEDB.12.0',   
   @srvproduct = N'OLE DB Provider for ACE',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.accdb';  
GO  
```  
  
### <a name="c-using-the-microsoft-ole-db-provider-for-odbc-with-the-data_source-parameter"></a>C. Usar el proveedor Microsoft OLE DB para ODBC con el parámetro data_source  
 En el ejemplo siguiente se crea un servidor vinculado denominado `SEATTLE Payroll` que utiliza el [!INCLUDE[msCoName](../../includes/msconame-md.md)] proveedor de OLE DB para ODBC ( `MSDASQL` ) y el parámetro *data_source* .  
  
> [!NOTE]  
> El nombre del origen de datos ODBC especificado debe definirse como DSN del sistema en el servidor antes de utilizar el servidor vinculado.  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Payroll',   
   @srvproduct = N'',  
   @provider = N'MSDASQL',   
   @datasrc = N'LocalServer';  
GO  
```  
  
### <a name="d-using-the-microsoft-ole-db-provider-for-excel-spreadsheet"></a>D. Usar el proveedor Microsoft OLE DB para una hoja de cálculo de Excel  
 Para crear una definición de servidor vinculado mediante el [!INCLUDE[msCoName](../../includes/msconame-md.md)] proveedor de OLE DB para Jet para tener acceso a una hoja de cálculo de Excel con el formato 1997-2003, primero debe crear un rango con nombre en Excel especificando las columnas y filas de la hoja de cálculo de Excel que se van a seleccionar. Entonces, podrá hacerse referencia al nombre del intervalo como un nombre de tabla en una consulta distribuida.  
  
```sql  
EXEC sp_addlinkedserver 'ExcelSource',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   'c:\MyData\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
GO  
```  
  
 Para obtener acceso a los datos de una hoja de cálculo Excel, asocie un nombre a un intervalo de celdas. La consulta siguiente se puede utilizar para obtener acceso al intervalo denominado `SalesData` especificado como una tabla utilizando el servidor vinculado que se creó anteriormente.  
  
```sql  
SELECT *  
   FROM ExcelSource...SalesData;  
GO  
```  
  
 Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta en una cuenta de dominio que tiene acceso a un recurso compartido remoto, se puede utilizar una ruta de acceso UNC en lugar de una unidad asignada.  
  
```sql  
EXEC sp_addlinkedserver 'ExcelShare',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   '\\MyServer\MyShare\Spreadsheets\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
```  
  
 Para conectarse a una hoja de cálculo de Excel en formato de Excel 2007, use el proveedor ACE.  
  
```sql  
EXEC sp_addlinkedserver @server = N'ExcelDataSource',   
@srvproduct=N'ExcelData', @provider=N'Microsoft.ACE.OLEDB.12.0',   
@datasrc=N'C:\DataFolder\People.xlsx',  
@provstr=N'EXCEL 12.0' ;  
  
```  
  
### <a name="e-using-the-microsoft-ole-db-provider-for-jet-to-access-a-text-file"></a>E. Usar el proveedor Microsoft OLE DB para Jet para tener acceso a un archivo de texto  
 En este ejemplo se crea un servidor vinculado para obtener acceso directamente a archivos de texto, sin necesidad de vincular los archivos como tablas en un archivo .mdb de Access. El proveedor es `Microsoft.Jet.OLEDB.4.0` y la cadena de proveedor es `Text`.  
  
 El origen de datos es la ruta de acceso completa del directorio que contiene los archivos de texto. En el mismo directorio que los archivos de texto, debe existir un archivo schema.ini, que describe la estructura de dichos archivos. Para obtener más información sobre cómo crear un archivo schema.ini, vea la documentación relativa al motor de bases de datos Jet.  
  
```sql  
--Create a linked server.  
EXEC sp_addlinkedserver txtsrv, N'Jet 4.0',   
   N'Microsoft.Jet.OLEDB.4.0',  
   N'c:\data\distqry',  
   NULL,  
   N'Text';  
GO  
  
--Set up login mappings.  
EXEC sp_addlinkedsrvlogin txtsrv, FALSE, Admin, NULL;  
GO  
  
--List the tables in the linked server.  
EXEC sp_tables_ex txtsrv;  
GO  
  
--Query one of the tables: file1#txt  
--using a four-part name.   
SELECT *   
FROM txtsrv...[file1#txt];  
```  
  
### <a name="f-using-the-microsoft-ole-db-provider-for-db2"></a>F. Usar el proveedor Microsoft OLE DB para DB2  
 En el siguiente ejemplo se crea un servidor vinculado llamado `DB2` que utiliza el `Microsoft OLE DB Provider for DB2`.  
  
```sql  
EXEC sp_addlinkedserver  
   @server=N'DB2',  
   @srvproduct=N'Microsoft OLE DB Provider for DB2',  
   @catalog=N'DB2',  
   @provider=N'DB2OLEDB',  
   @provstr=N'Initial Catalog=PUBS;  
       Data Source=DB2;  
       HostCCSID=1252;  
       Network Address=XYZ;  
       Network Port=50000;  
       Package Collection=admin;  
       Default Schema=admin;';  
```  
  
### <a name="g-add-a-sssdsfull-as-a-linked-server-for-use-with-distributed-queries-on-cloud-and-on-premises-databases"></a>G. Agregar un [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] como servidor vinculado para su uso con consultas distribuidas en bases de datos locales y en la nube  
 Puede agregar una [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] como un servidor vinculado y después utilizarlo con consultas distribuidas que abarcan las bases de datos locales y en la nube. Se trata de un componente para las soluciones híbridas de base de datos que abarcan redes corporativas locales y la nube de Azure.  
  
 El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] producto de Box contiene la característica de consulta distribuida, que permite escribir consultas para combinar datos de orígenes de datos locales y datos de orígenes remotos (incluidos los datos de orígenes que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] datos) definidos como servidores vinculados. Cada [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (excepto la base de datos maestra virtual) se puede agregar como un servidor vinculado individual y utilizar después directamente en las aplicaciones de base de datos como cualquier otra base de datos.  
  
 Entre las ventajas de utilizar [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] se incluyen facilidad de uso, alta disponibilidad, escalabilidad, trabajo con un modelo de desarrollo conocido y un modelo de datos relacional. Los requisitos de la aplicación de base de datos determinan cómo utilizar [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] en la nube. Puede mover todos los datos a la vez a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] o mover progresivamente algunos de los datos y conservar los datos restantes de forma local. En el caso de una aplicación de base de datos híbrida, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ahora se puede agregar como servidores vinculados y la aplicación de base de datos puede emitir consultas distribuidas para combinar datos de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] orígenes de datos locales y.  
  
 Este es un ejemplo sencillo en el que se explica cómo conectarse a un [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] mediante consultas distribuidas:  
  
```sql  
-- Configure the linked server  
-- Add one Azure SQL Database as Linked Server  
EXEC sp_addlinkedserver  
  @server='myLinkedServer', -- here you can specify the name of the linked server  
  @srvproduct='',       
  @provider='sqlncli', -- using SQL Server Native Client  
  @datasrc='myServer.database.windows.net',   -- add here your server name  
  @location='',  
  @provstr='',  
  @catalog='myDatabase'  -- add here your database name as initial catalog (you cannot connect to the master database)  

-- Add credentials and options to this linked server  
EXEC sp_addlinkedsrvlogin  
  @rmtsrvname = 'myLinkedServer',  
  @useself = 'false',  
  @rmtuser = 'myLogin',             -- add here your login on Azure DB  
  @rmtpassword = 'myPassword' -- add here your password on Azure DB  

EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;  

-- Now you can use the linked server to execute 4-part queries  
-- You can create a new table in the Azure DB  
EXEC ('CREATE TABLE t1tutut2(col1 int not null CONSTRAINT PK_col1 PRIMARY KEY CLUSTERED (col1) )') at myLinkedServer  
-- Insert data from your local SQL Server  
EXEC ('INSERT INTO t1tutut2 VALUES(1),(2),(3)') at myLinkedServer  
  
-- Query the data using 4-part names  
SELECT * FROM myLinkedServer.myDatabase.dbo.myTable  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de consultas distribuidas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [sp_setnetname &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  

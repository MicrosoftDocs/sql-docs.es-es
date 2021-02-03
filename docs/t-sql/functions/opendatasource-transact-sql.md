---
description: OPENDATASOURCE (Transact-SQL)
title: OPENDATASOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENDATASOURCE
- OPENDATASOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENDATASOURCE function
- remote data access [SQL Server], OPENDATASOURCE
- ad hoc distributed queries
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: 5510b846-9cde-4687-8798-be9a273aad31
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 57b832a82329e3eb38d7d2d332d0eaa9c210c8be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189834"
---
# <a name="opendatasource-transact-sql"></a>OPENDATASOURCE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Proporciona información de conexión ad hoc como parte de un nombre de objeto de cuatro partes sin utilizar un nombre de servidor vinculado.  

 ![icono de vínculo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
OPENDATASOURCE ( 'provider_name', 'init_string' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 '*provider_name*'  
 Es el nombre registrado como PROGID del proveedor OLE DB utilizado para obtener acceso al origen de datos. *provider_name* es un tipo de datos **char** que carece de valor predeterminado.  

 > [!IMPORTANT]
 > El anterior proveedor de Microsoft OLE DB para SQL Server (SQLOLEDB) y de OLE DB para SQL Server Native Client (SQLNCLI) permanece en desuso y no se recomienda utilizarlo para nuevos trabajos de desarrollo. En su lugar, use el nuevo [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), que se actualizará con las características de servidor más recientes.
 
 "*init_string*"  
 Es la cadena de conexión que se pasa a la interfaz IDataInitialize del proveedor de destino. La sintaxis de cadena del proveedor se basa en pares palabra clave-valor separados por puntos y coma, como: **'** _palabra clave1_=_valor_ **;** _palabra clave2_=_valor_ **'** .  
  
 Para conocer los pares de palabra clave y valor admitidos, vea [!INCLUDE[msCoName](../../includes/msconame-md.md)] Data Access SDK. En esta documentación se define la sintaxis básica. En la siguiente tabla se muestran las palabras clave más utilizadas en el argumento *init_string*.  
  
|Palabra clave|Propiedad OLE DB|Valores válidos y descripción|  
|-------------|---------------------|----------------------------------|  
|Origen de datos|DBPROP_INIT_DATASOURCE|Nombre del origen de datos al que se va a conectar. Distintos proveedores lo interpretan de formas distintas. Para un proveedor OLE DB de SQL Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , indica el nombre del servidor. Para un proveedor OLE DB para Jet, indica la ruta de acceso completa al archivo .mdb o al archivo .xls.|  
|Location|DBPROP_INIT_LOCATION|Ubicación de la base de datos a la que se va a conectar.|  
|Propiedades extendidas|DBPROP_INIT_PROVIDERSTRING|Cadena de conexión específica del proveedor.|  
|Tiempo de espera de conexión|DBPROP_INIT_TIMEOUT|Valor de tiempo de espera después del cual se produce un error en el intento de conexión.|  
|Id. de usuario|DBPROP_AUTH_USERID|Id. de usuario que se va a utilizar para la conexión.|  
|Contraseña|DBPROP_AUTH_PASSWORD|Contraseña que se va a utilizar para la conexión.|  
|Catálogo|DBPROP_INIT_CATALOG|Nombre del catálogo inicial o predeterminado al conectarse al origen de datos.|  
|Seguridad integrada|DBPROP_AUTH_INTEGRATED|SSPI, para especificar la autenticación de Windows|  
  
## <a name="remarks"></a>Comentarios  
`OPENROWSET` siempre hereda la intercalación de la instancia, independientemente de la intercalación establecida para las columnas.

`OPENDATASOURCE` se puede usar para tener acceso a datos remotos desde orígenes de datos de OLE DB solo cuando la opción de registro DisallowAdhocAccess está establecida explícitamente en 0 para el proveedor especificado y la opción de configuración avanzada Ad Hoc Distributed Queries está habilitada. Cuando no se establecen estas opciones, el comportamiento predeterminado no permite el acceso ad hoc.  
  
Es posible utilizar la función `OPENDATASOURCE` en las mismas ubicaciones de la sintaxis [!INCLUDE[tsql](../../includes/tsql-md.md)] como un nombre del servidor vinculado. Por tanto, se puede utilizar `OPENDATASOURCE` como la primera parte de un nombre de cuatro partes que hace referencia a un nombre de tabla o vista en una instrucción SELECT, INSERT, UPDATE o DELETE, o a un procedimiento almacenado remoto en una instrucción EXECUTE. Cuando se ejecutan procedimientos almacenados remotos, `OPENDATASOURCE` debe hacer referencia a otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. OPENDATASOURCE no acepta variables como argumentos.  
  
Al igual que la función `OPENROWSET`, `OPENDATASOURCE` solo debe hacer referencia a orígenes de datos OLE DB a los que se tiene acceso con poca frecuencia. Defina un servidor vinculado para los orígenes de datos a los que tiene acceso varias veces. Ni OPENDATASOURCE ni OPENROWSET proporcionan toda la funcionalidad de definiciones de servidores vinculados, como la administración de seguridad y la capacidad de consultar información de catálogos. Toda la información de conexión, incluidas las contraseñas, se debe proporcionar siempre que se llama OPENDATASOURCE.  
  
> [!IMPORTANT]  
> La autenticación de Windows es mucho más segura que la de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Siempre que sea posible, debe utilizar la Autenticación de Windows. `OPENDATASOURCE` no se debe utilizar con contraseñas explícitas en la cadena de conexión.  
  
Los requisitos de conexión de cada proveedor son similares a los requisitos de esos parámetros cuando se crean servidores vinculados. En el artículo [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) se incluye información detallada sobre muchos proveedores comunes.  
  
Las llamadas a `OPENDATASOURCE`, `OPENQUERY` o `OPENROWSET` en la cláusula `FROM` se evalúan por separado y de forma independiente de otras llamadas a estas funciones usadas como destino de la actualización, incluso si se han suministrado argumentos idénticos a las dos llamadas. En particular, las condiciones de filtro o combinación aplicadas en el resultado de una de esas llamadas no tienen ningún efecto en los resultados de la otra llamada.  
  
## <a name="permissions"></a>Permisos  
 Todos los usuarios pueden ejecutar OPENDATASOURCE. Los permisos que se utilizan para conectarse al servidor remoto se determinan a partir de la cadena de conexión.  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-using-opendatasource-with-select-and-the-sql-server-ole-db-driver"></a>A. Uso de OPENDATASOURCE con SELECT y OLE DB Driver de SQL Server  
 En el siguiente ejemplo se usa [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) con el fin de tener acceso a la tabla `HumanResources.Department` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en el servidor remoto `Seattle1`. Se utiliza una instrucción `SELECT` para definir el conjunto de filas devuelto. La cadena de proveedor contiene las palabras clave `Server` y `Trusted_Connection`. OLE DB Driver de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconoce estas palabras clave.  
  
```sql  
SELECT GroupName, Name, DepartmentID  
FROM OPENDATASOURCE('MSOLEDBSQL', 'Server=Seattle1;Database=AdventureWorks2016;TrustServerCertificate=Yes;Trusted_Connection=Yes;').HumanResources.Department  
ORDER BY GroupName, Name;  
``` 

### <a name="b-using-opendatasource-with-select-and-the-sql-server-ole-db-provider"></a>B. Uso de OPENDATASOURCE con SELECT y el proveedor OLE DB de SQL Server  
En el ejemplo siguiente se crea una conexión ad hoc a la instancia de `Payroll` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servidor `London`, y se consulta la tabla `AdventureWorks2012.HumanResources.Employee`. 

> [!NOTE] 
> El uso de SQLNCLI redirigirá [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la última versión del proveedor OLE DB de SQL Server Native Client. Se espera que el proveedor OLE DB se registre con el PROGID especificado en el registro. 

> [!IMPORTANT]  
> El proveedor OLE DB de SQL Server Native Client (SQLNCLI) permanece en desuso y no se recomienda utilizarlo para nuevos trabajos de desarrollo. En su lugar, use el nuevo [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), que se actualizará con las características de servidor más recientes.
  
```sql  
SELECT *  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=London\Payroll;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Employee;  
```  

### <a name="c-using-the-microsoft-ole-db-provider-for-jet"></a>C. Usar el proveedor Microsoft OLE DB para Jet   
 En el ejemplo siguiente se crea una conexión ad hoc a una hoja de cálculo de Excel en el formato 1997 - 2003.  
  
```sql  
SELECT * FROM OPENDATASOURCE('Microsoft.Jet.OLEDB.4.0',  
    'Data Source=C:\DataFolder\Documents\TestExcel.xls;Extended Properties=EXCEL 5.0')...[Sheet1$] ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)  
  

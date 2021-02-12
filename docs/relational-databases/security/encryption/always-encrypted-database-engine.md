---
title: Always Encrypted | Microsoft Docs
description: Información general de Always Encrypted que admite el cifrado de cliente transparente y la informática confidencial en SQL Server y Azure SQL Database
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], Always Encrypted
- Always Encrypted
- TCE Always Encrypted
- Always Encrypted, about
- SQL13.SWB.COLUMNMASTERKEY.CLEANUP.F1
ms.assetid: 54757c91-615b-468f-814b-87e5376a960f
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 523cd4e58ad340dfaa8751868eccb5e593c67a74
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536386"
---
# <a name="always-encrypted"></a>Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  ![Always Encrypted](../../../relational-databases/security/encryption/media/always-encrypted.png "Always Encrypted")  
  
 Always Encrypted es una característica diseñada para proteger la información confidencial, como números de tarjeta de crédito o números de identificación nacional (por ejemplo, los números de seguridad social de EE. UU.), almacenada en bases de datos [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Always Encrypted permite a los clientes cifrar información confidencial en aplicaciones cliente y nunca revelar las claves de cifrado en [!INCLUDE[ssDE](../../../includes/ssde-md.md)] ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]). Como resultado, Always Encrypted proporciona una separación entre aquellos que poseen los datos y pueden verlos, y aquellos que los administran, pero que no deberían tener acceso. Al asegurar que los administradores de base de datos local, los operadores de base de datos en la nube y otros con usuarios con privilegios elevados no autorizados no pueden obtener acceso a los datos cifrados, Always Encrypted permite a los clientes almacenar información confidencial de forma segura fuera de su control directo. Esto permite a las organizaciones almacenar sus datos en Azure, y permitir la delegación de la administración de la base de datos local a terceros, o reducir los requisitos de autorización de seguridad para su propio personal de administración de bases de datos.

 Always Encrypted proporciona capacidades de computación confidencial al permitir que [!INCLUDE[ssDE](../../../includes/ssde-md.md)] procese algunas consultas en datos cifrados y, al mismo tiempo, preserva la confidencialidad de los datos y brinda las ventajas de seguridad mencionadas anteriormente. En [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], [!INCLUDE[sssSQLv14](../../../includes/sssql17-md.md)] y [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)], Always Encrypted admite la comparación de igualdad mediante el cifrado determinista. Vea [Selección del cifrado determinista o aleatorio](#selecting--deterministic-or-randomized-encryption). 

  > [!NOTE] 
  > En [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], los enclaves seguros amplían considerablemente las capacidades de computación confidencial de Always Encrypted con coincidencia de patrones, otros operadores de comparación y el cifrado en contexto. Vea [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md).

 Always Encrypted realiza cifrado transparente en las aplicaciones. Un controlador habilitado para Always Encrypted instalado en el equipo cliente consigue esto al cifrar y descifrar automáticamente la información confidencial en la aplicación cliente. El controlador cifra los datos en columnas confidenciales antes de pasar los datos a [!INCLUDE[ssDE](../../../includes/ssde-md.md)]y vuelve a escribir las consultas automáticamente para que se conserve la semántica de la aplicación. De forma similar, el controlador descifra los datos de forma transparente, almacenados en columnas de bases de datos cifradas, incluidas en los resultados de la consulta.  
  
 Always Encrypted está disponible en todas las ediciones de [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)], a partir de [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], y en todos los niveles de servicio de [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. (Antes de [!INCLUDE[ssSQL15_md](../../../includes/sssql16-md.md)] SP1, Always Encrypted estaba limitado a Enterprise Edition). Si desea ver una presentación de Channel 9 que incluye Always Encrypted, vea [Keeping Sensitive Data Secure with Always Encrypted](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted)(Mantener protegida la información confidencial con Always Encrypted).  

  
## <a name="typical-scenarios"></a>Escenarios típicos  
  
### <a name="client-and-data-on-premises"></a>Cliente y datos locales  
 Un cliente tiene una aplicación cliente y [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , que se ejecutan localmente, en su ubicación de la empresa. El cliente desea contratar a un proveedor externo para administrar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para proteger la información confidencial almacenada en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el cliente utiliza Always Encrypted para garantizar la separación de obligaciones entre los administradores de base de datos y los de aplicación. El cliente almacena valores de texto no cifrado de claves de Always Encrypted en un almacén de claves de confianza al que puede acceder la aplicación cliente. Los administradores de[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no tienen acceso a las claves y, por lo tanto, no pueden descifrar la información confidencial almacenada en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
### <a name="client-on-premises-with-data-in-azure"></a>Cliente local con datos de Azure  
 Un cliente tiene una aplicación cliente local en su ubicación de la empresa. La aplicación trabaja sobre la información confidencial almacenada en una base de datos hospedada en Azure ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se ejecutan en una máquina virtual en Microsoft Azure). El cliente usa Always Encrypted y almacena las claves de Always Encrypted en un almacén de claves de confianza hospedado en local, para garantizar que los administradores de nube de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] no tengan acceso a la información confidencial.  
  
### <a name="client-and-data-in-azure"></a>Cliente y datos en Azure  
 Un cliente tiene una aplicación cliente hospedada en Microsoft Azure (por ejemplo, en un rol de trabajo o un rol web) que trabaja sobre la información confidencial almacenada en una base de datos hospedada en Azure (SQL Database o SQL Server se ejecutan en una máquina virtual en Microsoft Azure). Aunque Always Encrypted no proporciona un aislamiento completo de los datos ante los administradores de la nube, dado que tanto los datos como las claves están expuestos a los administradores de la nube de la plataforma que hospeda el nivel de cliente, el cliente se sigue beneficiando de la reducción del área expuesta a ataques de seguridad (los datos siempre se cifran en la base de datos).  
 
## <a name="how-it-works"></a>Funcionamiento

Puede configurar Always Encrypted de modo que las columnas individuales de la base de datos contengan la información confidencial. Al configurar el cifrado de una columna, especifique la información sobre el algoritmo de cifrado y las claves criptográficas usados para proteger los datos de la columna. Always Encrypted usa claves de dos tipos: claves maestras de columna y claves de cifrado de columna. Una clave de cifrado de columna se usa para cifrar los datos de una columna cifrada. Una clave maestra de columna es una clave de protección de claves que cifra una o varias claves de cifrado de columna. 

El motor de base de datos almacena la configuración de cifrado de cada columna en los metadatos de la base de datos. Pero tenga en cuenta que el motor de base de datos nunca almacena ni usa las claves de cualquier tipo de texto no cifrado. Solo almacena valores cifrados de claves de cifrado de columna y la información sobre la ubicación de claves maestras de columna, que se almacenan en almacenes de claves de confianza externos, como el almacén de claves de Azure, el almacén de certificados de Windows en un equipo cliente o un módulo de seguridad de hardware.

Para acceder a los datos almacenados en una columna cifrada en texto no cifrado, una aplicación debe usar un controlador de cliente habilitado para Always Encrypted. Cuando una aplicación emite una consulta con parámetros, el controlador colabora de forma transparente con el motor de base de datos para determinar qué parámetros se dirigen a columnas cifradas y, por lo tanto, se deben cifrar. De cada parámetro que se tiene que cifrar, el controlador obtiene la información sobre el algoritmo de cifrado y el valor cifrado de la clave de cifrado de columna de la columna, los destinos del parámetro y la ubicación de su clave maestra de columna correspondiente.

Después, el controlador se pone en contacto con el almacén de claves, que contiene la clave maestra de columna, para descifrar el valor de clave de cifrado de columna cifrado y, luego, usa la clave de cifrado de columna de texto no cifrado para cifrar el parámetro. La clave de cifrado de columna de texto no cifrado resultante se almacena en la memoria caché para reducir el número de viajes de ida y vuelta al almacén de claves en usos posteriores de la misma clave de cifrado de columna. El controlador sustituye los valores de texto no cifrado de los parámetros que se dirigen a columnas cifradas por sus valores cifrados y envía la consulta al servidor para su procesamiento.

El servidor calcula el conjunto de resultados y, para cualquier columna cifrada incluida en el conjunto de resultados, el controlador adjunta los metadatos de cifrado de la columna, incluida la información sobre el algoritmo de cifrado y las claves correspondientes. El controlador primero intenta buscar la clave de cifrado de columna de texto no cifrado en la memoria caché local y, si no la encuentra, solo realiza una ronda en la clave maestra de columna. Luego, el controlador descifra los resultados y devuelve valores de texto no cifrado a la aplicación.

 Un controlador cliente interactúa con un almacén de claves, que contiene una clave maestra de columna, mediante un proveedor de almacén de claves maestras de columna, que es un componente de software de cliente que encapsula un almacén de claves que contiene la clave maestra de columna. Los proveedores de los tipos comunes de almacenes de claves están disponibles en bibliotecas de controladores de cliente de Microsoft o como descargas independientes. También puede implementar su propio proveedor. Las funciones de Always Encrypted, incluidos los proveedores integrados de almacenes de claves maestras de columna, varían según la biblioteca de controladores y su versión. 

Para obtener información sobre cómo desarrollar aplicaciones mediante Always Encrypted con controladores de cliente determinados, vea [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md).

## <a name="remarks"></a>Observaciones

El cifrado y descifrado se produce a través del controlador de cliente. Esto significa que algunas acciones que se producen solo en el lado servidor no funcionarán cuando se use Always Encrypted. Estas acciones incluyen, entre otras: 
- Copiar datos de una columna a otra mediante una instrucción UPDATE, BULK INSERT (T-SQL), SELECT INTO o INSERT SELECT. 
- Desencadenadores, tablas temporales, columnas dispersas, texto completo, OLTP en memoria y captura de datos modificados (CDC). 


Este es un ejemplo de una instrucción UPDATE que intenta mover datos de una columna cifrada a una columna sin cifrar sin devolver al cliente un conjunto de resultados: 

```sql
update dbo.Patients set testssn = SSN
```

Si SSN es una columna cifrada mediante Always Encrypted, la instrucción de actualización anterior producirá un error similar a:

```
Msg 206, Level 16, State 2, Line 89
Operand type clash: char(11) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_1', column_encryption_key_database_name = 'ssn') collation_name = 'Latin1_General_BIN2' is incompatible with char
```

Para actualizar correctamente la columna, haga lo siguiente:

1. SELECCIONE los datos de fuera de la columna SSN y almacénelos como un conjunto de resultados en la aplicación. Esto permitirá que la aplicación (*controlador* del cliente) descifre la columna.
2. INSERTE los datos desde el conjunto de resultados en SQL Server. 

 >[!IMPORTANT]
 > En este escenario, los datos se descifrarán cuando se envíen al servidor, ya que la columna de destino es varchar normal que no acepta datos cifrados. 
  
## <a name="selecting-deterministic-or-randomized-encryption"></a><a name="selecting--deterministic-or-randomized-encryption"></a> Selección del cifrado determinista o aleatorio  
 El motor de base de datos nunca funciona en los datos de texto no cifrado almacenados en columnas cifradas, pero sigue admitiendo algunas consultas en datos cifrados, según el tipo de cifrado de la columna. Always Encrypted admite dos tipos de cifrado: el cifrado aleatorio y el determinista.  
  
- El cifrado determinista genera siempre el mismo valor cifrado para cualquier valor de texto no cifrado concreto. El empleo del cifrado determinista permite búsquedas de puntos, combinaciones de igualdad, agrupaciones e indexación en columnas cifradas. Pero también puede permitir que usuarios no autorizados adivinen información sobre los valores cifrados al examinar los patrones de la columna cifrada, especialmente si hay un pequeño conjunto de posibles valores cifrados, como verdadero o falso, o como la región norte, sur, este u oeste. El cifrado determinista debe usar una intercalación de columna con un criterio de ordenación binario 2 para columnas de caracteres.

- El cifrado aleatorio usa un método que cifra los datos de una manera menos predecible. El cifrado aleatorio es más seguro, pero evita las búsquedas, la agrupación, la indexación y la combinación en columnas cifradas.

Use el cifrado determinista para las columnas que se usarán como parámetros de búsqueda o agrupación. Por ejemplo, un número de identificación gubernamental. Use el cifrado aleatorio para aquellos datos como comentarios de investigación confidenciales que no están agrupados con otros registros y no se usan para combinar tablas.
Para obtener detalles sobre los algoritmos criptográficos de Always Encrypted, vea [Criptografía de Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-cryptography.md).

## <a name="configuring-always-encrypted"></a>Configuración de Always Encrypted

 La configuración inicial de Always Encrypted en una base de datos implica la generación de claves de Always Encrypted, la creación de metadatos de clave, la configuración de propiedades de cifrado de columnas seleccionadas de la base de datos o el cifrado de los datos que puedan existir en las columnas que deban cifrarse. Tenga en cuenta que algunas de estas tareas no se admiten en Transact-SQL y exigen el uso de herramientas de cliente. Dado que las claves de Always Encrypted y la información confidencial protegida nunca se revelan en texto no cifrado al servidor, el motor de base de datos no puede estar implicado en el aprovisionamiento de claves ni realizar operaciones de cifrado o descifrado de datos. Puede usar SQL Server Management Studio o PowerShell para realizar esas tareas. 

|Tarea|SSMS|PowerShell|T-SQL|
|:---|:---|:---|:---
|Aprovisionamiento de claves maestras de columna, claves de cifrado de columna y claves de cifrado de columna cifrada con sus claves maestras de columna correspondientes.|Sí|Sí|No|
|Creación de metadatos de clave en la base de datos.|Sí|Sí|Sí|
|Creación de nuevas tablas con columnas cifradas|Sí|Sí|Sí|
|Cifrado de los datos existentes en las columnas seleccionadas de la base de datos|Sí|Sí|No|

> [!NOTE]
> [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md), introducido en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], admite el cifrado de datos existentes con Transact-SQL. También elimina la necesidad de sacar los datos de la base de datos para realizar las operaciones criptográficas.

> [!NOTE]
> Asegúrese de ejecutar las herramientas de aprovisionamiento de claves o de cifrado de datos en un entorno seguro, en un equipo que no sea el que hospeda la base de datos. De lo contrario, podría filtrarse información confidencial o las claves al entorno de servidor, lo que reduciría las ventajas del empleo de Always Encrypted.  

Para obtener más información sobre la configuración de Always Encrypted, vea:
- [Configurar Always Encrypted con SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurar Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

## <a name="getting-started-with-always-encrypted"></a>Introducción a Always Encrypted

Use el [Asistente para Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md) para comenzar a usar este software rápidamente. El Asistente aprovisionará las claves necesarias y configurará el cifrado de las columnas seleccionadas. Si las columnas para las que está configurando el cifrado ya contienen algunos datos, el asistente los cifrará. En el ejemplo siguiente se muestra el proceso para cifrar una columna.

> [!NOTE]  
>  Para ver un vídeo que incluye el uso del asistente, vea [Getting Started with Always Encrypted with SSMS (Introducción a Always Encrypted con SSMS)](https://channel9.msdn.com/Shows/Data-Exposed/Getting-Started-with-Always-Encrypted-with-SSMS).

1.  Conéctese a una base de datos existente que contenga tablas con columnas que quiera cifrar mediante el **Explorador de objetos** de Management Studio, o cree una nueva base de datos, cree una o más tablas con columnas para cifrar y conéctese a ella.
2.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas** y, luego, haga clic en **Cifrar columnas** para abrir el **Asistente de Always Encrypted**.
3.  Revise la página **Introduction** y haga clic en **Next**.
4.  En la página **Column Selection** , expanda las tablas y seleccione las columnas que desea cifrar.
5.  En cada columna seleccionada para cifrado, establezca **Tipo de cifrado** en *Determinista* o *Aleatorio*.
6.  Para cada columna seleccionada para cifrado, seleccione una **clave de cifrado**. Si aún no ha creado claves de cifrado para esta base de datos, seleccione la opción predeterminada de una nueva clave generada automáticamente y, después, haga clic en **Siguiente**.
7.  En la página **Configuración de la clave maestra** , seleccione una ubicación para almacenar la clave nueva y seleccione un origen de clave maestra. Después, haga clic en **Siguiente**.
8.  En la página **Validation** , elija si desea ejecutar inmediatamente el script o crear un script de PowerShell y, después, haga clic en **Next**.
9.  En la página **Resumen**, revise las opciones que ha seleccionado y haga clic en **Finalizar**. Cierre al asistente cuando finalice.

  
## <a name="feature-details"></a>Detalles de las características  
  
-   Las consultas realizan la comparación de igualdad en aquellas columnas cifradas mediante el cifrado determinista, pero ninguna otra operación (por ejemplo, mayor o menor que, coincidencia de patrones con el operador LIKE u operaciones matemáticas).  
  
-   Las consultas en columnas cifradas mediante el cifrado aleatorio no pueden realizar operaciones en ninguna de esas columnas. No se admite la indexación de columnas cifradas mediante el cifrado aleatorio.  
 > [!NOTE] 
 > [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md), introducido en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], aborda la limitación anterior al habilitar la coincidencia de patrones, los operadores de comparación y la indexación en columnas mediante el cifrado aleatorio.

-   Una clave de cifrado de columna puede tener hasta dos valores cifrados diferentes, cada uno de ellos cifrado con una clave maestra de columna diferente. Esto facilita la rotación de claves maestras de columna.  
  
-   El cifrado determinista requiere que una columna tenga una de las [intercalaciones *binary2*](../../../relational-databases/collations/collation-and-unicode-support.md).  

-   Después de cambiar la definición de un objeto cifrado, ejecute [sp_refresh_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) para actualizar los metadatos de Always Encrypted para el objeto.
  
Always Encrypted no se admite para las columnas con las características siguientes. Por ejemplo, si se aplica alguna de las siguientes condiciones a la columna, no se puede usar la cláusula `ENCRYPTED WITH` en `CREATE TABLE/ALTER TABLE` para una columna:  
  
-   Columnas que usan uno de los siguientes tipos de datos: `xml`, `timestamp`/`rowversion`, `image`, `ntext`, `text`, `sql_variant`, `hierarchyid`, `geography`, `geometry`, alias o tipos definidos por el usuario.  
- Columnas `FILESTREAM`.  
- Columnas con la propiedad `IDENTITY`.  
- Columnas con la propiedad `ROWGUIDCOL`.  
- Columnas de cadena (`varchar`, `char`, etc.) con intercalaciones que no son BIN2.  
- Columnas que son claves para índices agrupados y no agrupados al usar el cifrado aleatorio (se admite el cifrado determinista).
- Se incluyen columnas en los índices de texto completo (Always Encrypted no admite [Búsqueda de texto completo](../../../relational-databases/search/full-text-search.md)).  
- Columnas calculadas
- Columnas que hacen referencia a columnas calculadas (cuando la expresión realiza operaciones no admitidas para Always Encrypted).  
- Un conjunto de columnas dispersas.  
- Columnas a las que hacen referencia las estadísticas mediante el cifrado aleatorio (se admite el cifrado determinista).  
- Columnas con tipos de alias.  
- Columnas de partición.  
- Columnas con restricciones predeterminadas.  
- Columnas a las que hacen referencia las restricciones únicas mediante el cifrado aleatorio (se admite el cifrado determinista).  
- Columnas de clave principal al usar cifrado aleatorio (se admite el cifrado determinista).  
- Columnas de referencia en las restricciones de clave externa cuando se usa el cifrado aleatorio o el cifrado determinista, si las columnas de referencia y las que se hace referencia usan diferentes claves o algoritmos.  
- Columnas de referencia por restricciones de comprobación.  
- Columnas capturadas/con seguimiento mediante captura de datos modificados.  
- Columnas de clave principal en tablas que tienen seguimiento de cambios.  
- Columnas que están enmascaradas (con Enmascaramiento dinámico de datos).  
- Columnas de tablas de Stretch Database. (Las tablas con columnas cifradas con Always Encrypted pueden habilitarse para Stretch).  
- Columnas de tablas externas (PolyBase). (Nota: Se admite el uso de tablas externas y tablas con columnas cifradas en la misma consulta).  
- No se admiten parámetros con valores de tabla que se dirijan a columnas cifradas.  

Las cláusulas siguientes no se pueden usar en las columnas cifradas:

- `FOR XML`
- `FOR JSON PATH`

Las características siguientes no funcionan en las columnas cifradas:

- Replicación transaccional o de mezcla
- Consultas distribuidas: servidores vinculados, `OPENROWSET`(T-SQL) y `OPENDATASOURCE`(T-SQL)

Requisitos de la herramienta

- Se recomienda usar la versión 18 de SQL Server Management Studio o una versión posterior para ejecutar consultas que descifren los resultados recuperados de columnas cifradas o para insertar, actualizar o filtrar columnas cifradas. 
- Se necesita `sqlcmd` versión 13.1 (o una versión posterior), la cual está disponible en el [Centro de descarga](https://go.microsoft.com/fwlink/?LinkID=825643).

  
## <a name="database-permissions"></a>Permisos para la base de datos  
 Hay cuatro permisos para Always Encrypted:  
  
*  `ALTER ANY COLUMN MASTER KEY` (Necesario para crear y eliminar una clave maestra de columna).  
  
*  `ALTER ANY COLUMN ENCRYPTION KEY` (Necesario para crear y eliminar una clave de cifrado de columna). 
  
*  `VIEW ANY COLUMN MASTER KEY DEFINITION` (Necesario para acceder a los metadatos de las claves maestras de columna para administrar claves o consultar columnas cifradas y leerlos).  
  
*  `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` (Necesario para acceder a los metadatos de la clave de cifrado de columna para administrar claves o consultar columnas cifradas y leerlos).  
  
 En la tabla siguiente se resumen los permisos necesarios para acciones comunes.  
  
|Escenario|<code>ALTER ANY COLUMN MASTER KEY</code>|<code>ALTER ANY COLUMN ENCRYPTION KEY</code>|<code>VIEW ANY COLUMN MASTER KEY DEFINITION</code>|<code>VIEW ANY COLUMN ENCRYPTION KEY DEFINITION</code>|  
|--------------|-----------------------------------|---------------------------------------|---------------------------------------------|-------------------------------------------------|  
|Administración de claves (crear, cambiar, revisar metadatos de clave en la base de datos)|X|X|X|X|  
|Consultar columnas cifradas|||X|X|  
  
 **Notas importantes:**  
  
-   Los permisos se aplican a las acciones con [!INCLUDE[tsql](../../../includes/tsql-md.md)], [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] (cuadros de diálogo y asistente) o PowerShell.  
  
-   Los dos permisos *VIEW* son necesarios para seleccionar columnas cifradas, aunque el usuario no tenga permiso para descifrar las columnas.  
  
-   En [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], ambos permisos *VIEW* se conceden de manera predeterminada al rol fijo de base de datos `public` . Un administrador de base de datos puede optar por revocar (o denegar) los permisos *VIEW* al rol `public` y concederlos a roles o usuarios concretos para implementar un control más restringido.  
  
-   En [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], los permisos *VIEW* no se conceden de manera predeterminada al rol fijo de base de datos `public`. Esto permite que determinadas herramientas existentes heredadas (con versiones anteriores de DacFx) funcionen correctamente. Por lo tanto, para trabajar con columnas cifradas (aunque no se descifren), un administrador de base de datos debe conceder explícitamente los dos permisos *VIEW* .  

  
## <a name="example"></a>Ejemplo  
 La siguiente [!INCLUDE[tsql](../../../includes/tsql-md.md)] crea metadatos de clave maestra de columna, metadatos de clave de cifrado de columna y una tabla con columnas cifradas. Para obtener información sobre cómo crear las claves a las que se hace referencia en los metadatos, vea:
- [Configurar Always Encrypted con SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurar Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = 'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
---------------------------------------------  
CREATE COLUMN ENCRYPTION KEY MyCEK   
WITH VALUES  
(  
    COLUMN_MASTER_KEY = MyCMK,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x01700000016C006F00630061006C006D0061006300680069006E0065002F006D0079002F003200660061006600640038003100320031003400340034006500620031006100320065003000360039003300340038006100350064003400300032003300380065006600620063006300610031006300284FC4316518CF3328A6D9304F65DD2CE387B79D95D077B4156E9ED8683FC0E09FA848275C685373228762B02DF2522AFF6D661782607B4A2275F2F922A5324B392C9D498E4ECFC61B79F0553EE8FB2E5A8635C4DBC0224D5A7F1B136C182DCDE32A00451F1A7AC6B4492067FD0FAC7D3D6F4AB7FC0E86614455DBB2AB37013E0A5B8B5089B180CA36D8B06CDB15E95A7D06E25AACB645D42C85B0B7EA2962BD3080B9A7CDB805C6279FE7DD6941E7EA4C2139E0D4101D8D7891076E70D433A214E82D9030CF1F40C503103075DEEB3D64537D15D244F503C2750CF940B71967F51095BFA51A85D2F764C78704CAB6F015EA87753355367C5C9F66E465C0C66BADEDFDF76FB7E5C21A0D89A2FCCA8595471F8918B1387E055FA0B816E74201CD5C50129D29C015895CD073925B6EA87CAF4A4FAF018C06A3856F5DFB724F42807543F777D82B809232B465D983E6F19DFB572BEA7B61C50154605452A891190FB5A0C4E464862CF5EFAD5E7D91F7D65AA1A78F688E69A1EB098AB42E95C674E234173CD7E0925541AD5AE7CED9A3D12FDFE6EB8EA4F8AAD2629D4F5A18BA3DDCC9CF7F352A892D4BEBDC4A1303F9C683DACD51A237E34B045EBE579A381E26B40DCFBF49EFFA6F65D17F37C6DBA54AA99A65D5573D4EB5BA038E024910A4D36B79A1D4E3C70349DADFF08FD8B4DEE77FDB57F01CB276ED5E676F1EC973154F86  
);  
---------------------------------------------  
CREATE TABLE Customers (  
    CustName nvarchar(60)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = RANDOMIZED,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    SSN varchar(11)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = DETERMINISTIC ,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    Age int NULL  
);  
GO  
  
```  
## <a name="see-also"></a>Consulte también  
- [Configurar Always Encrypted con SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)   
- [Configurar Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md) 
- [Configuración del cifrado de columna mediante el asistente para Always Encrypted](always-encrypted-wizard.md)
- [Criptografía de Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-cryptography.md)   
- [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md)   
- [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
- [CREATE TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-table-transact-sql.md)   
- [column_definition &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
- [sys.column_encryption_keys &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)  
- [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
- [sys.column_master_keys &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)   
- [sys.columns &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
- [sp_refresh_parameter_encryption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md)   
  
  

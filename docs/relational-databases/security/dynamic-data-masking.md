---
title: Enmascaramiento de datos dinámicos | Microsoft Docs
description: Obtenga información sobre el enmascaramiento dinámico de datos, que limita la exposición de información confidencial mediante su enmascaramiento a los usuarios sin privilegios. Puede simplificar considerablemente la seguridad en SQL Server.
ms.date: 05/02/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
ms.assetid: a62f4ff9-2953-42ca-b7d8-1f8f527c4d66
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0a05c7af0bc8b8846e3b4ab5c1e3472249350e7b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344394"
---
# <a name="dynamic-data-masking"></a>Enmascaramiento dinámico de datos
[!INCLUDE [SQL Server 2016 ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

![Enmascaramiento de datos dinámicos](../../relational-databases/security/media/dynamic-data-masking.png)

El enmascaramiento dinámico de datos (DDM) limita la exposición de información confidencial al ocultarla a los usuarios sin privilegios. Se puede usar para simplificar considerablemente el diseño y la codificación de la seguridad en la aplicación.  

El enmascaramiento dinámico de datos evita el acceso no autorizado a información confidencial permitiendo que los clientes especifiquen la cantidad de información confidencial que se debe revelar, con un impacto mínimo en la capa de aplicación. DDM se puede configurar en los campos de la base de datos designada para ocultar información confidencial en los conjuntos de resultados de consultas. Con DDM no se modifican los datos en la base de datos. El enmascaramiento dinámico de datos resulta fácil de usar con las aplicaciones existentes, ya que las reglas de enmascaramiento se aplican en los resultados de la consulta. Muchas aplicaciones pueden enmascarar información confidencial sin modificar las consultas existentes.

* Una directiva de enmascaramiento de datos central actúa directamente en los campos confidenciales de la base de datos.
* Designe roles o usuarios con privilegios que tienen acceso a la información confidencial.
* DDM cuenta con funciones de enmascaramiento total y parcial, además de una máscara aleatoria para datos numéricos.
* Comandos [!INCLUDE[tsql_md](../../includes/tsql-md.md)] simples definen y administran las máscaras.

La finalidad del enmascaramiento dinámico de datos consiste en limitar la exposición de la información confidencial, con lo que se impide que los usuarios vean datos a los que no deberían poder acceder. El enmascaramiento dinámico de datos no pretende evitar que los usuarios de la base de datos se conecten directamente a ella y ejecuten consultas exhaustivas que expongan información confidencial. El enmascaramiento dinámico de datos se complementa con otras características de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (auditoría, cifrado, seguridad de nivel de fila...), y resulta muy recomendable usarlo con esas características para proteger mejor la información confidencial en la base de datos.  
  
El enmascaramiento dinámico de datos está disponible en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y en [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)], y se configura con comandos [!INCLUDE[tsql](../../includes/tsql-md.md)] . Para obtener más información sobre cómo configurar el enmascaramiento dinámico de datos con el Portal de Azure, vea [Introducción al enmascaramiento dinámico de datos de SQL Database (Portal de Azure)](/azure/azure-sql/database/dynamic-data-masking-overview).  
  
## <a name="defining-a-dynamic-data-mask"></a>Definición de una máscara dinámica de datos
 Es posible definir una regla de enmascaramiento en una columna de una tabla, con el objetivo de ofuscar los datos de esa columna. Existen cuatro tipos de máscaras.  
  
|Función|Descripción|Ejemplos|  
|--------------|-----------------|--------------|  
|Valor predeterminado|Enmascaramiento completo de acuerdo con los tipos de datos de los campos designados.<br /><br /> Para los tipos de datos String, use XXXX o un número menor de X si el tamaño del campo es inferior a 4 caracteres (**char**, **nchar**,  **varchar**, **nvarchar**, **text**, **ntext**).  <br /><br /> Para los tipos de datos numéricos, use un valor cero (**bigint**, **bit**, **decimal**, **int**, **money**, **numeric**, **smallint**, **smallmoney**, **tinyint**, **float**, **real**).<br /><br /> En el caso de los tipos de datos de fecha y hora, use 01.01.1900 00:00:00.0000000 (**date**, **datetime2**, **datetime**, **datetimeoffset**, **smalldatetime**, **time**).<br /><br />En lo que respecta a los tipos de datos binarios, use un solo byte de valor 0 de ASCII (**binary**, **varbinary**, **image**).|Ejemplo de sintaxis de definición de columna: `Phone# varchar(12) MASKED WITH (FUNCTION = 'default()') NULL`<br /><br /> Sintaxis modificada de ejemplo: `ALTER COLUMN Gender ADD MASKED WITH (FUNCTION = 'default()')`|  
|Email|Método de enmascaramiento que expone la primera letra de una dirección de correo electrónico y el sufijo constante ".com", en el formato de una dirección de correo electrónico. `aXXX@XXXX.com`.|Ejemplo de sintaxis de definición: `Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL`<br /><br /> Sintaxis modificada de ejemplo: `ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()')`|  
|Random|Una función de enmascaramiento aleatorio que se puede usar con cualquier tipo numérico a fin de enmascarar el valor original con uno aleatorio dentro de un intervalo especificado.|Ejemplo de sintaxis de definición: `Account_Number bigint MASKED WITH (FUNCTION = 'random([start range], [end range])')`<br /><br /> Sintaxis modificada de ejemplo: `ALTER COLUMN [Month] ADD MASKED WITH (FUNCTION = 'random(1, 12)')`|  
|Cadena personalizada|Método de enmascaramiento que expone la primera y última letra y agrega una cadena de relleno personalizada en el medio. `prefix,[padding],suffix`<br /><br /> Nota: Si el valor original es demasiado corto como para que se complete toda la máscara, no se expondrá parte del prefijo o sufijo.|Ejemplo de sintaxis de definición: `FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(prefix,[padding],suffix)') NULL`<br /><br /> Sintaxis modificada de ejemplo: `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)')`<br /><br /> Ejemplo adicional:<br /><br /> `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(5,"XXXXXXX",0)')`|  
  
## <a name="permissions"></a>Permisos  
 No se necesita ningún permiso especial para crear una tabla una máscara dinámica de datos, solo los permisos de esquema estándares **CREATE TABLE** y **ALTER** .  
  
 Para agregar, reemplazar o quitar la máscara de una columna, se precisan los permisos **ALTER ANY MASK** y **ALTER** (este último, en la tabla). Se recomienda otorgar el permiso **ALTER ANY MASK** a un responsable de seguridad.  
  
 Los usuarios con el permiso **SELECT** en una tabla, podrán ver los datos de esta. Las columnas que estén definidas como enmascaradas mostrarán datos enmascarados. Otorgue el permiso **UNMASK** a un usuario para permitir que recupere datos sin enmascarar de las columnas para las que se ha definido una regla de enmascaramiento.  
  
 El permiso **CONTROL** en la base de datos engloba tanto **ALTER ANY MASK** como **UNMASK** .  
  
## <a name="best-practices-and-common-use-cases"></a>Procedimientos recomendados y casos de uso habituales  
  
-   La creación de una máscara en una columna no impide que se efectúen actualizaciones en ella. De modo que, aunque los usuarios recibirán datos enmascarados cuando realicen una consulta en una columna enmascarada, ellos mismos podrán actualizar los datos si cuentan con permisos de escritura. Aun así, se debe usar una directiva de control de acceso adecuada para limitar los permisos de actualización.  
  
-   Si se utiliza `SELECT INTO` o `INSERT INTO` para copiar datos de una columna enmascarada en otra tabla, se generarán datos enmascarados en la tabla de destino.  
  
-   Se aplica el enmascaramiento dinámico de datos al ejecutar la importación y exportación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Una base de datos que contenga columnas enmascaradas dará como resultado un archivo de datos exportado con los datos enmascarados (suponiendo que lo exporte un usuario sin privilegios **UNMASK**), y la base de datos importada contendrá datos enmascarados de forma estática.  
  
## <a name="querying-for-masked-columns"></a>Realización de consultas en columnas enmascaradas  
 Use la vista **sys.masked_columns** para realizar una consulta en columnas y tablas que tengan aplicada la función de enmascaramiento. Esta vista se hereda de la vista **sys.columns** . Devuelve todas las columnas de la vista **sys.columns** , junto con las columnas **is_masked** y **masking_function** . Además, indica si estas están enmascaradas y, en caso afirmativo, qué función de enmascaramiento se ha definido. Esta vista solo muestra las columnas en las que se ha aplicado la función de enmascaramiento.  
  
```sql 
SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.[object_id] = tbl.[object_id]  
WHERE is_masked = 1;  
```  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 No se puede definir una regla de enmascaramiento para los siguientes tipos de columnas:  
  
-   Columnas cifradas (siempre cifradas)  
  
-   FILESTREAM  
  
-   COLUMN_SET o una columna dispersa que forme parte de un conjunto de columnas.  
  
-   No se puede configurar una máscara en una columna calculada, pero si esta última depende de otra con una máscara, entonces la columna calculada devolverá datos enmascarados.  
  
-   Una columna con enmascaramiento de datos no puede ser una clave para un índice FULLTEXT.  
  
 Para los usuarios sin el permiso **UNMASK** , las instrucciones en desuso **READTEXT**, **UPDATETEXT** y **WRITETEXT** no funcionan adecuadamente en una columna configurada para el enmascaramiento dinámico de datos. 
 
 Agregar una máscara dinámica de datos se implementa como un cambio de esquema en la tabla subyacente y, por tanto, no puede realizarse en una columna con dependencias. Para trabajar en torno a esta restricción, primero puede quitar la dependencia y, después, agregar la máscara dinámica de datos y volver a crear la dependencia. Por ejemplo, si la dependencia se debe a un índice que depende de esa columna, puede quitar el índice, después agregar la máscara y, luego, volver a crear el índice dependiente.
 

## <a name="security-note-bypassing-masking-using-inference-or-brute-force-techniques"></a>Nota de seguridad: Omisión del enmascaramiento con técnicas de fuerza bruta o inferencia

El enmascaramiento dinámico de datos está diseñado para simplificar el desarrollo de aplicaciones limitando la exposición de los datos en un conjunto de consultas predefinidas que la aplicación usa. A pesar de que el enmascaramiento dinámico de datos también puede ser útil para evitar la exposición accidental de información confidencial cuando se obtiene acceso directo a una base de datos de producción, es importante tener en cuenta que los usuarios sin privilegios y que tienen permisos de consulta ad hoc. Si es necesario conceder ese acceso ad hoc, se debe usar la auditoría para supervisar toda la actividad de base de datos y mitigar este escenario.
 
Por ejemplo, considere una entidad de seguridad de base de datos con los privilegios suficientes para ejecutar consultas ad hoc en la base de datos y que intenta "adivinar" los datos subyacentes y, en última instancia, inferir los valores reales. Suponga que tenemos una máscara definida en la columna `[Employee].[Salary]` , este usuario se conecta directamente a la base de datos, comienza a adivinar los valores y, a la larga, infiere el valor `[Salary]` de un conjunto de empleados:
 

```sql
SELECT ID, Name, Salary FROM Employees
WHERE Salary > 99999 and Salary < 100001;
```

>    |  Identificador | Nombre| Salario |   
>    | ----- | ---------- | ------ | 
>    |  62543 | Jane Doe | 0 | 
>    |  91245 | John Smith | 0 |  

Esto demuestra que el Enmascaramiento dinámico de datos no se debe usar como una medida aislada para proteger completamente la información confidencial de los usuarios que ejecutan consultas ad hoc en la base de datos. Es adecuado para evitar la exposición accidental de la información confidencial, pero no brindará protección contra intentos malintencionados de inferir los datos subyacentes.
 
Es importante administrar adecuadamente los permisos en la base de datos y seguir el principio de un mínimo de permisos requeridos. Además, recuerde tener habilitada la auditoría para hacer seguimiento de todas las actividades que se realizan en la base de datos.

  
## <a name="examples"></a>Ejemplos  
  
### <a name="creating-a-dynamic-data-mask"></a>Creación de una máscara dinámica de datos  
 En el siguiente ejemplo se crea una tabla con tres tipos distintos de máscaras dinámicas de datos. En el ejemplo se rellena la tabla y se selecciona que se muestre el resultado.  
  
```sql
CREATE TABLE Membership  
  (MemberID int IDENTITY PRIMARY KEY,  
   FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)') NULL,  
   LastName varchar(100) NOT NULL,  
   Phone varchar(12) MASKED WITH (FUNCTION = 'default()') NULL,  
   Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL);  
  
INSERT Membership (FirstName, LastName, Phone, Email) VALUES   
('Roberto', 'Tamburello', '555.123.4567', 'RTamburello@contoso.com'),  
('Janice', 'Galvin', '555.123.4568', 'JGalvin@contoso.com.co'),  
('Zheng', 'Mu', '555.123.4569', 'ZMu@contoso.net');  
SELECT * FROM Membership;  
```  
  
 Se crea un nuevo usuario, a quien se le otorga el permiso **SELECT** en la tabla. En las consultas ejecutadas como el usuario `TestUser` , los datos se mostrarán enmascarados.  
  
```sql 
CREATE USER TestUser WITHOUT LOGIN;  
GRANT SELECT ON Membership TO TestUser;  
  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;  
```  
  
 El resultado demuestra que las máscaras funcionan, ya que cambia los datos de esto:  
  
 `1    Roberto     Tamburello    555.123.4567    RTamburello@contoso.com`  
  
 into  
  
 `1    RXXXXXXX    Tamburello    xxxx            RXXX@XXXX.com`  
  
### <a name="adding-or-editing-a-mask-on-an-existing-column"></a>Adición o edición de una máscara en una columna existente  
 Utilice la instrucción **ALTER TABLE** para agregar una máscara a una columna existente de la tabla o a fin de editarla en dicha columna.  
En el siguiente ejemplo, se agrega la función de enmascaramiento a la columna `LastName`:  
  
```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName ADD MASKED WITH (FUNCTION = 'partial(2,"XXX",0)');  
```  
  
 En el siguiente ejemplo se cambia la función de enmascaramiento en la columna `LastName` :  

```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName varchar(100) MASKED WITH (FUNCTION = 'default()');  
```  
  
### <a name="granting-permissions-to-view-unmasked-data"></a>Concesión de permisos para ver datos sin enmascarar  
 Si se otorga el permiso **UNMASK** , `TestUser` podrá ver los datos sin enmascarar.  
  
```sql
GRANT UNMASK TO TestUser;  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;   
  
-- Removing the UNMASK permission  
REVOKE UNMASK TO TestUser;  
```  
  
### <a name="dropping-a-dynamic-data-mask"></a>Anulación de una máscara dinámica de datos  
 La siguiente instrucción anula la máscara de la columna `LastName` creada en el ejemplo anterior:  
  
```sql  
ALTER TABLE Membership   
ALTER COLUMN LastName DROP MASKED;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
 [sys.masked_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-masked-columns-transact-sql.md)   
 [Introducción al Enmascaramiento dinámico de datos de SQL Database (Portal de Azure)](/azure/azure-sql/database/dynamic-data-masking-overview)

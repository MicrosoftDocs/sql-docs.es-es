---
title: Configuración y uso de Always Encrypted con enclaves seguros
description: Ejecución de instrucciones de lenguaje de definición de datos (DDL) para configurar el cifrado de la columna o administrar índices en columnas cifradas y consultar columnas cifradas
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: bc92b0af972236b588369869afc5b023735ae699
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237132"
---
# <a name="run-transact-sql-statements-using-secure-enclaves"></a>Configuración y uso de Always Encrypted con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) permite que algunas instrucciones de Transact-SQL (T-SQL) realicen cálculos confidenciales sobre columnas de base de datos cifradas en un enclave seguro del lado servidor.

## <a name="statements-using-secure-enclaves"></a>Instrucciones con enclaves seguros

Los siguientes tipos de instrucciones T-SQL usan enclaves seguros.

### <a name="ddl-statements-using-secure-enclaves"></a>Instrucciones de DDL con enclaves seguros

Para los siguientes tipos de instrucciones de [Lenguaje de definición de datos (DDL)](../../../t-sql/statements/statements.md#data-definition-language) se necesitan enclaves seguros.

- Las instrucciones [ALTER TABLE definición_de_columna (Transact-SQL)](../../../t-sql/statements/alter-table-column-definition-transact-sql.md) que desencadenan operaciones criptográficas en contexto mediante claves habilitadas para el enclave. Para obtener más información, vea [Configuración del cifrado de columnas en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md).
- Las instrucciones [CREATE INDEX (Transact-SQL)](../../../t-sql/statements/create-index-transact-sql.md) y [ALTER INDEX (Transact-SQL)](../../../t-sql/statements/alter-index-transact-sql.md) que crean o modifican índices en columnas habilitadas para el enclave mediante el cifrado aleatorio. Para más información, vea [Creación y uso de índices en columnas de Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md).
  
### <a name="dml-statements-using-secure-enclaves"></a>Instrucciones de DML con enclaves seguros

En las siguientes instrucciones del [Lenguaje de manipulación de datos (DML)](../../../t-sql/statements/statements.md#data-manipulation-language) o las consultas realizadas en columnas habilitadas para el enclave mediante cifrado aleatorio se necesitan enclaves seguros:

- Las consultas que usan uno o varios de los siguientes operadores de Transact-SQL admitidos dentro de enclaves seguros:
  - [Operadores de comparación](../../../mdx/comparison-operators.md)
  - [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md)
  - [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md)
  - [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md)
  - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
  - Las [combinaciones](../../performance/joins.md) - [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] solo admiten combinaciones de bucle anidadas. [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] admite combinaciones de bucle anidadas, de hash y de combinación
  - [Cláusula SELECT - ORDER BY (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md). Se admite en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. No se admiten en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].
  - [Cláusula SELECT - GROUP BY (Transact-SQL)](../../../t-sql/queries/select-group-by-transact-sql.md). Se admite en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. No se admiten en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].
- Las consultas que insertan, actualizan o eliminan filas, que a su vez desencadenan la inserción o eliminación de una clave de índice en un índice de una columna habilitada para el enclave. Para más información, vea [Creación y uso de índices en columnas de Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md).

> [!NOTE]
> Las operaciones en índices y consultas DML confidenciales que usan enclaves solo se admiten en columnas habilitadas para el enclave en las que se utiliza el cifrado aleatorio. No se admite el cifrado determinista.
>
> En [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], en las consultas confidenciales que utilizan enclaves en columnas de cadena de caracteres (`char`, `nchar`) es necesario usar una intercalación de criterio de ordenación binary2 (BIN2) para la columna. En [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], es necesario utilizar intercalaciones BIN2 o UTF-8.

### <a name="dbcc-commands-using-secure-enclaves"></a>Comandos DBCC con enclaves seguros

Los comandos administrativos [DBCC (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-transact-sql.md) que implican la comprobación de la integridad de los índices también pueden necesitar enclaves seguros, si la base de datos contiene índices en columnas habilitadas para el enclave mediante cifrado aleatorio. Por ejemplo, [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) y [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).

## <a name="prerequisites-for-running-statements-using-secure-enclaves"></a>Requisitos previos para ejecutar instrucciones con enclaves seguros

El entorno debe cumplir los requisitos siguientes para admitir la ejecución de instrucciones que usan un enclave seguro.

- La instancia de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] o la base de datos y el servidor en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] deben estar configurados correctamente para admitir los enclaves y la atestación. Para obtener más información, vea [Configuración del enclave seguro y la atestación](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Debe obtener por parte del administrador del servicio de atestación una dirección URL de atestación del entorno.

  - Si usa [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), vea [Determinación y uso compartido de la dirección URL de atestación de HGS](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, vea [Determinación de la dirección URL de atestación de la directiva de atestación](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation).

- Si se va a conectar a la base de datos mediante la aplicación, debe usar un controlador cliente que admita Always Encrypted con enclaves seguros. La aplicación se debe conectar a la base de datos con Always Encrypted habilitado para la conexión de base de datos y el protocolo y la dirección URL de atestación configurados correctamente. Para obtener más información, vea [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md).
- Si usa SQL Server Management Studio (SSMS) o Azure SQL Data Studio, debe habilitar Always Encrypted y configurar el protocolo y la dirección URL de atestación al conectarse a la base de datos. Vea las secciones siguientes para obtener información.

> [!NOTE]
> La conexión a la base de datos con Always Encrypted y la atestación configurada no es necesaria para las operaciones siguientes si usa claves de cifrado de columnas en caché: Consultas DDL que crean o modifican índices, consultas DML que actualizan índices y comandos DBCC que comprueban la integridad de los índices. Para obtener más información, vea [Invocación de operaciones de indexación mediante claves de cifrado de columna almacenadas en caché](always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms"></a>Requisitos previos para ejecutar instrucciones T-SQL con enclaves en SSMS

Requisitos de versión mínima para SQL Server Management Studio:

- SSMS 18.3 cuando se usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SSMS 18.8 cuando se usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Asegúrese de ejecutar las instrucciones desde una ventana de consulta que use una conexión que tenga Always Encrypted y la dirección URL de atestación correcta configurada.

1. En el cuadro de diálogo **Conectar a servidor**, especifique el nombre del servidor, seleccione un método de autenticación e indique sus credenciales.
2. Haga clic en **Opciones >>** y seleccione la pestaña **Always Encrypted**.
3. Active la casilla **Habilitar Always Encrypted (cifrado de columna)** y especifique la dirección URL de atestación de enclave. Por ejemplo, `https://hgs.bastion.local/Attestation` o `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`.

    ![Conexión al servidor con atestación mediante SSMS](./media/always-encrypted-enclaves/column-encryption-setting.png)

4. Seleccione **Conectar**.
5. Si se le pide que habilite la parametrización para las consultas de Always Encrypted, seleccione **Habilitar** si tiene previsto ejecutar consultas DML con parámetros. Para obtener más información, vea [Parametrización de Always Encrypted](always-encrypted-query-columns-ssms.md#param).

Para obtener más información, vea [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](always-encrypted-query-columns-ssms.md#en-dis).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio"></a>Requisitos previos para ejecutar instrucciones T-SQL con enclaves en Azure Data Studio

Se recomienda la versión mínima recomendada **1.23** o superior.

Asegúrese de ejecutar las instrucciones desde una ventana de consulta que use una conexión que tenga Always Encrypted y tanto el protocolo como la dirección URL de atestación correctos configurados.

1. En el cuadro de diálogo **Conexión**, haga clic en **Avanzada...** .
2. Para habilitar Always Encrypted para la conexión, establezca el campo **Always Encrypted** en **Habilitado**.
3. Especifique el protocolo y la dirección URL de atestación.
    - Si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)], establezca **Protocolo de atestación** en **Servicio de protección de host** y escriba la dirección URL de atestación del Servicio de protección de host en el campo **Dirección URL de atestación de enclave**.
    - Si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], establezca **Protocolo de atestación** en **Azure Attestation** y escriba la dirección URL de atestación que hace referencia a la directiva de Microsoft Azure Attestation en el campo **Dirección URL de atestación de enclave**.

    ![Conexión al servidor con atestación mediante Azure Data Studio](./media/always-encrypted-enclaves/azure-data-studio-connect-with-enclaves.png)

4. Haga clic en **Aceptar** para cerrar las **Propiedades avanzadas**.

Para obtener más información, vea [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](always-encrypted-query-columns-ads.md#enabling-and-disabling-always-encrypted-for-a-database-connection).

Si tiene previsto ejecutar consultas DML con parámetros, también tendrá que habilitar [Parametrización con Always Encrypted](always-encrypted-query-columns-ads.md#parameterization-for-always-encrypted).

## <a name="examples"></a>Ejemplos

En esta sección se incluyen ejemplos de consultas DML que usan enclaves. 

En los ejemplos se usa el esquema siguiente.

```sql
CREATE SCHEMA [HR];
GO

CREATE TABLE [HR].[Jobs](
 [JobID] [int] IDENTITY(1,1) PRIMARY KEY,
 [JobTitle] [nvarchar](50) NOT NULL,
 [MinSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [MaxSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
);
GO

CREATE TABLE [HR].[Employees](
 [EmployeeID] [int] IDENTITY(1,1) PRIMARY KEY,
 [SSN] [char](11) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [FirstName] [nvarchar](50) NOT NULL,
 [LastName] [nvarchar](50) NOT NULL,
 [Salary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [JobID] [int] NULL,
 FOREIGN KEY (JobID) REFERENCES [HR].[Jobs] (JobID)
);
GO
```

### <a name="exact-match-search"></a>Búsqueda de coincidencias exactas

La siguiente consulta realiza una búsqueda de coincidencias exactas en la columna de cadena `SSN` cifrada.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="pattern-matching-search"></a>Búsqueda de coincidencia de patrones

La siguiente consulta realiza una búsqueda de coincidencia de patrones en la columna de cadena cifrada `SSN`, y busca empleados con los últimos dígitos de un número de la seguridad social.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="range-comparison"></a>Comparación de rangos

La consulta siguiente realiza una comparación de rangos en la columna `Salary` cifrada, y busca empleados con salarios dentro del rango especificado.

```sql
DECLARE @MinSalary money = 40000;
DECLARE @MaxSalary money = 45000;
SELECT * FROM [HR].[Employees] WHERE [Salary] > @MinSalary AND [Salary] < @MaxSalary;
GO
```

### <a name="joins"></a>Combinaciones

La consulta siguiente realiza una combinación entre las tablas `Employees` y `Jobs` mediante la columna `Salary` cifrada. La consulta recupera los empleados con salarios fuera de un rango de salarios para el trabajo del empleado.

```sql
SELECT * FROM [HR].[Employees] e
JOIN [HR].[Jobs] j
ON e.[JobID] = j.[JobID] AND e.[Salary] > j.[MaxSalary] OR e.[Salary] < j.[MinSalary];
GO
```

### <a name="sorting"></a>Ordenación

La consulta siguiente ordena los registros de empleados en función de la columna `Salary` cifrada y recupera los 10 empleados con los salarios más altos.
> [!NOTE]
> La ordenación de columnas cifradas se admite en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], pero no en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

```sql
SELECT TOP(10) * FROM [HR].[Employees]
ORDER BY [Salary] DESC;
GO
```

## <a name="next-steps"></a>Pasos siguientes

- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Consulte también

- [Solución de problemas comunes de Always Encrypted con enclaves seguros](always-encrypted-enclaves-troubleshooting.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md)
- [Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md)

---
title: Always Encrypted con enclaves seguros
description: Obtenga información sobre la característica Always Encrypted con enclaves seguros para SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: e84635c1f32396e033841c546dafc1796624d5ab
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237088"
---
# <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Always Encrypted con enclaves seguros amplía las funciones de computación confidencial de [Always Encrypted](always-encrypted-database-engine.md) mediante la habilitación del cifrado en contexto y consultas confidenciales más enriquecidas. Always Encrypted con enclaves seguros está disponible en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] y [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] (en versión preliminar).

Presentado en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] en 2015 y en [!INCLUDE[sssql16](../../../includes/sssql16-md.md)], Always Encrypted protege la privacidad de los datos confidenciales contra malware y usuarios *no autorizados* con privilegios elevados: administradores de bases de datos, de equipos, de nube o cualquiera con acceso legítimo a las instancias de servidor, hardware, etc., pero que no debería tener acceso a parte ni a la totalidad de los datos reales.  

Sin las mejoras que se describen en este artículo, Always Encrypted protege los datos mediante su cifrado en el lado cliente y *nunca* permite que los datos ni las claves criptográficas correspondientes aparezcan en texto no cifrado dentro de [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. Como resultado, la funcionalidad en columnas cifradas dentro de la base de datos está muy restringida. Las únicas operaciones que [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] puede realizar con datos cifrados son las comparaciones de igualdad (solo están disponibles con [cifrado determinista](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)). Todas las demás operaciones, incluidas las criptográficas (el cifrado inicial de los datos o la rotación de claves) y las consultas más completas (por ejemplo, la coincidencia de patrones) no se admiten dentro de la base de datos. Los usuarios tienen que sacar sus datos de la base de datos para llevar a cabo estas operaciones en el cliente.

Always Encrypted *con enclaves seguros* aborda estas limitaciones al permitir realizar algunos cálculos en datos de texto no cifrado dentro de un enclave seguro en el lado servidor. Un enclave seguro es una región de memoria protegida dentro del proceso del [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. El enclave seguro aparece como una caja opaca para el resto del [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] y los otros procesos en el equipo de host. No hay ninguna manera de ver los datos ni el código que se encuentran dentro del enclave desde el exterior, incluso si se cuenta con un depurador. Estas propiedades hacen que el enclave seguro sea un *entorno de ejecución de confianza* que puede acceder de forma segura a claves criptográficas y datos confidenciales en texto no cifrado sin poner en peligro la confidencialidad de los datos.

Always Encrypted usa enclaves seguros, tal como se muestra en el siguiente diagrama:

![flujo de datos](./media/always-encrypted-enclaves/ae-data-flow.png)

Al analizar una instrucción de Transact-SQL enviada por una aplicación, el [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] determina si la instrucción contiene alguna operación realizada en datos cifrados para la que es necesario usar el enclave seguro. Para este tipo de instrucciones:

- El controlador cliente envía las claves de cifrado de columna necesarias para las operaciones al enclave seguro (a través de un canal seguro) y remite la instrucción para que se ejecute.

- Al procesar la instrucción, el [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] delega en el enclave seguro los cálculos o las operaciones criptográficas sobre columnas cifradas. Si es necesario, el enclave descifra los datos y realiza cálculos en texto no cifrado.

Durante el procesamiento de la instrucción, las claves de cifrado de columna y los datos no se exponen en texto no cifrado en el [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] fuera del enclave seguro.

## <a name="supported-enclave-technologies"></a>Tecnologías de enclave admitidas

En [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], Always Encrypted con enclaves seguros usa enclaves seguros de memoria de [Seguridad basada en virtualización (VBS)](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/) (también conocidos como enclaves de Modo seguro virtual, VSM) en Windows.

En [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], Always Encrypted con enclaves seguros usa enclaves de [Software Guard Extensions de Intel (Intel SGX)](https://itpeernetwork.intel.com/microsoft-azure-confidential-computing/). Intel SGX es una tecnología de entorno de ejecución de confianza basada en hardware que se admite en las bases de datos que usan la configuración de hardware de la [serie DC](https://docs.microsoft.com/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series).

## <a name="secure-enclave-attestation"></a>Atestación de enclaves seguros

El enclave seguro dentro del [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] puede acceder a datos confidenciales y a las claves de cifrado de columna en texto no cifrado. Por tanto, antes de enviar al [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] una instrucción que implique cálculos de enclave, el controlador cliente dentro de la aplicación debe comprobar que el enclave seguro es un enclave de VBS o SGX legítimo, y que el código que se ejecuta dentro del enclave seguro es la biblioteca de Always Encrypted original que implementa algoritmos criptográficos de Always Encrypted para el cifrado en contexto y las operaciones que se admiten en consultas confidenciales.

El proceso de comprobación del enclave se denomina **atestación del enclave** e implica que un controlador cliente dentro de la aplicación y [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] se deben poner en contacto con un servicio de atestación externo. Los detalles específicos del proceso de atestación dependen del tipo de enclave (VBS o SGX) y del servicio de atestación.

El proceso de atestación para enclaves seguros de VBS en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] es la [atestación en tiempo de ejecución Protección del sistema de Windows Defender](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/), para la que se necesita el Servicio de protección de host (HGS) como servicio de atestación. 

Para la atestación de enclaves de Intel SGX en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] se necesita [Microsoft Azure Attestation](https://docs.microsoft.com/azure/attestation/overview).

> [!NOTE]
> [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] no admite Microsoft Azure Attestation. El Servicio de protección de host es la única solución de atestación que se admite para enclaves de VBS en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

## <a name="supported-client-drivers"></a>Controladores cliente admitidos

Para usar Always Encrypted con enclaves seguros, una aplicación debe usar un controlador cliente que admita la característica. Configure la aplicación y el controlador cliente para habilitar los cálculos y la atestación de enclaves. Para obtener más información, incluida la lista de controladores cliente admitidos, consulte [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md).

## <a name="terminology"></a>Terminología

### <a name="enclave-enabled-keys"></a>Claves habilitadas para el enclave

Always Encrypted con enclaves seguros presenta el concepto de las claves habilitadas para el enclave:

- **Clave maestra de columna habilitada para el enclave**: clave maestra de columna con la propiedad `ENCLAVE_COMPUTATIONS` especificada en el objeto de metadatos de clave maestra de columna dentro de la base de datos. El objeto de metadatos de clave maestra de columna también debe contener una signatura válida de las propiedades de los metadatos. Para obtener más información, vea [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md).
- **Clave de cifrado de columna habilitada para el enclave**: una clave de cifrado de columna cifrada con una clave maestra de columna habilitada para el enclave. Para los cálculos dentro del enclave seguro, solo se pueden usar claves de cifrado de columna habilitadas para el enclave. 

Para obtener más información, consulte [Administración de claves para Always Encrypted con enclaves seguros](always-encrypted-enclaves-manage-keys.md).

### <a name="enclave-enabled-columns"></a>Columnas habilitadas para el enclave

Una columna habilitada para el enclave es una columna de base de datos cifrada con una clave de cifrado de columna habilitada para el enclave.

## <a name="confidential-computing-capabilities-for-enclave-enabled-columns"></a>Funciones de computación confidencial para las columnas habilitadas para el enclave

Las dos ventajas principales de Always Encrypted con enclaves seguros son el cifrado en contexto y las consultas confidenciales enriquecidas.

### <a name="in-place-encryption"></a>Cifrado en contexto

El cifrado en contexto permite operaciones criptográficas en las columnas de base de datos dentro del enclave seguro, sin mover los datos fuera de la base de datos. El cifrado en contexto mejora el rendimiento y la confiabilidad del cifrado. Puede realizar el cifrado en contexto mediante la instrucción [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md). 

Las operaciones criptográficas que se admiten en contexto son las siguientes:

- Cifrado de una columna en texto no cifrado con una clave de cifrado de columna habilitada para el enclave.
- Nuevo cifrado de una columna cifrada habilitada para el enclave a fin de:
  - Rotar una clave de cifrado de columna: vuelva a cifrar la columna con una nueva clave de cifrado de columna habilitada para el enclave.
  - Cambiar el tipo de cifrado de una columna habilitada para el enclave, por ejemplo, de determinista a aleatorio.
- Descifrado de los datos almacenados en una columna habilitada para el enclave (conversión de la columna en una columna de texto no cifrado).

El cifrado en contexto se permite con el cifrado determinista y aleatorio, siempre que las claves de cifrado de columna implicadas en una operación criptográfica estén habilitadas para el enclave.

### <a name="confidential-queries"></a>Consultas confidenciales

Las consultas confidenciales son [consultas DML](../../../t-sql/queries/queries.md) que implican operaciones en columnas habilitadas para el enclave realizadas dentro del enclave seguro.

Las operaciones admitidas dentro de los enclaves seguros son las siguientes:

| Operación| [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] | [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] |
|:---|:---|:---|
| [Operadores de comparación](../../../mdx/comparison-operators.md) | Compatible | Compatible |
| [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md) | Compatible | Compatible |
| [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md) | Compatible | Compatible |
| [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md) | Compatible | Compatible |
| [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select) | Compatible | Compatible |
| [Combinaciones](../../performance/joins.md) | Solo se admiten combinaciones de bucle anidadas | Compatible |
| [SELECT: Cláusula ORDER BY (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md) | No compatible | Compatible |
| [SELECT: GROUP BY (Transact-SQL)](../../../t-sql/queries/select-group-by-transact-sql.md) | No compatible | Compatible |

> [!NOTE]
> Las operaciones anteriores solo se admiten en enclaves seguros en columnas habilitadas para el enclave mediante el cifrado **aleatorio**, no el determinista. La comparación de igualdad sigue siendo el único cálculo que se admite en las columnas con el cifrado determinista y se realiza comparando el texto cifrado fuera del enclave, con independencia de si la columna está habilitada o no para el enclave. El cifrado determinista admite las siguientes operaciones que implican comparaciones de igualdad: 
> - [= (Es igual a)](../../../t-sql/language-elements/equals-transact-sql.md) en búsquedas de puntos, búsquedas y combinaciones
> - [IN](../../../t-sql/language-elements/in-transact-sql.md)
> - [SELECT - GROUP BY](../../../t-sql/queries/select-group-by-transact-sql.md)
> - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
>
> En [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], en las consultas confidenciales que utilizan enclaves en una columna de cadena de caracteres (`char`, `nchar`) es necesario que la columna use una intercalación de criterio de ordenación binary2 (BIN2). En [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], en las consultas confidenciales en cadenas de caracteres se necesita una intercalación BIN2 o UTF-8. 

### <a name="indexes-on-enclave-enabled-columns"></a>Índices en columnas habilitadas para el enclave

Puede crear índices no agrupados en columnas habilitadas para el enclave mediante cifrado aleatorio con el fin de acelerar la ejecución de las consultas DML confidenciales que usen el enclave seguro.

Para asegurarse de que un índice en una columna cifrada mediante cifrado aleatorio no pierda datos confidenciales, los valores de clave de la estructura de datos de índice (árbol B) se cifran y se ordenan según sus valores de texto no cifrado. La ordenación por el valor de texto no cifrado también es útil para procesar consultas dentro del enclave. Cuando el ejecutor de consultas del [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] utiliza un índice en una columna cifrada para realizar cálculos dentro del enclave, busca el índice para consultar valores específicos almacenados en la columna. Cada búsqueda puede suponer varias comparaciones. El ejecutor de consultas delega cada comparación en el enclave, que descifra un valor almacenado en la columna y el valor del índice cifrado que se va a comparar, realiza la comparación en texto sin cifrar y devuelve el resultado de la comparación al ejecutor.

Sigue sin ser posible la creación de índices en columnas que usan cifrado aleatorio y no están habilitadas para el enclave.

Un índice en una columna que usa cifrado determinista se ordena según el texto cifrado (no texto no cifrado), con independencia de que la columna esté o no habilitada para el enclave.

Para más información, vea [Creación y uso de índices en columnas de Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md). Para obtener información general sobre el funcionamiento de la indexación en [!INCLUDE[ssde-md](../../../includes/ssde-md.md)], vea [Índices agrupados y no agrupados descritos](../../indexes/clustered-and-nonclustered-indexes-described.md).

### <a name="database-recovery"></a>Recuperación de base de datos

Si se produce un error en una instancia de SQL Server, sus bases de datos pueden quedar en un estado donde los archivos de datos pueden contener algunas modificaciones por transacciones incompletas. Cuando se inicia la instancia, se ejecuta un proceso denominado [recuperación de bases de datos](../../logs/the-transaction-log-sql-server.md#recovery-of-all-incomplete-transactions-when--is-started), que supone revertir las transacciones incompletas encontradas en el registro de transacciones para asegurarse de que se preserva la integridad de la base de datos. Si una transacción incompleta realizó algún cambio en un índice, puede que también sea necesario deshacer esos cambios. Por ejemplo, puede que haya que quitar o volver a insertar algunos valores de clave del índice.

> [!IMPORTANT]
> Microsoft recomienda encarecidamente habilitar la [recuperación de base de datos acelerada (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) para la base de datos **antes** de crear el primer índice en una columna habilitada para enclave que se ha cifrado con cifrado aleatorio. ADR está habilitada de forma predeterminada en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], pero no en [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

Con el [proceso de recuperación de base de datos tradicional](/azure/sql-database/sql-database-accelerated-database-recovery#the-current-database-recovery-process) (que sigue al modelo de recuperación [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf)), para deshacer un cambio en un índice, SQL Server debe esperar a que una aplicación proporcione la clave de cifrado de la columna al enclave, lo que puede tardar mucho tiempo. La Recuperación acelerada de la base de datos (ADR) reduce considerablemente el número de operaciones de deshacer que se deben aplazar porque una clave de cifrado de columna no está disponible en la caché dentro del enclave. Por lo tanto, aumenta considerablemente la disponibilidad de la base de datos, ya que reduce la posibilidad de que una nueva transacción se bloquee. Con ADR habilitado, SQL Server puede seguir necesitando una clave de cifrado de columna para completar la limpieza de las versiones de datos antiguas, pero se haría como una tarea en segundo plano que no afecta a la disponibilidad de la base de datos o a las transacciones del usuario. Sin embargo, puede que vea mensajes de error en el registro de errores, que indican operaciones de limpieza incorrectas debido a que falta una clave de cifrado de columna.

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Se aplican las siguientes consideraciones de seguridad a Always Encrypted con enclaves seguros.

- La seguridad de los datos en el enclave depende de un protocolo de atestación y un servicio de atestación. Por lo tanto, debe asegurarse de que el servicio de atestación y las directivas de atestación, que impone el servicio, sean administrados por un administrador de confianza. Además, los servicios de atestación suelen admitir distintas directivas y protocolos de atestación, algunos de los cuales realizan la comprobación mínima del enclave y su entorno, y están diseñados para desarrollo y pruebas. Siga atentamente las directrices específicas para el servicio de atestación a fin de asegurarse de que usa las directivas y configuraciones recomendadas para sus implementaciones en producción. 
- El cifrado de una columna mediante cifrado aleatorio con una clave de cifrado de columna habilitada para el enclave puede dar lugar a una pérdida del orden de los datos almacenados en la columna, ya que ese tipo de columnas admiten comparaciones de rangos. Por ejemplo, si una columna cifrada, que contiene los salarios de los empleados, tiene un índice, un administrador de base de datos malintencionado podría examinar el índice para buscar el valor cifrado de salario máximo e identificar a una persona con el salario máximo (suponiendo que el nombre de la persona no esté cifrado). 
- Si usa Always Encrypted para proteger datos confidenciales del acceso no autorizado por los administradores de base de datos, no comparta las claves maestras de columna ni las claves de cifrado de columna con ellos. Un administrador de base de datos puede administrar índices en columnas cifradas sin tener acceso directo a las claves, sino que aprovecha la caché de claves de cifrado de columna dentro del enclave.

## <a name="considerations-for-business-continuity-disaster-recovery-and-data-migration"></a><a name="anchorname-1-considerations-availability-groups-db-migration"></a> Consideraciones sobre continuidad empresarial, recuperación ante desastres y migración de datos

Al configurar una solución de alta disponibilidad o de recuperación ante desastres para una base de datos mediante Always Encrypted con enclaves seguros, asegúrese de que todas las réplicas de base de datos pueden usar un enclave seguro. Si hay un enclave disponible para la réplica principal, pero no para la secundaria, se producirá un error en cualquier instrucción que intente usar la funcionalidad de Always Encrypted con enclaves seguros después de la conmutación por error.

Al copiar o migrar una base de datos mediante Always Encrypted con enclaves seguros, asegúrese de que el entorno de destino siempre admite los enclaves. De lo contrario, las instrucciones que usen los enclaves no funcionarán en la copia ni en la base de datos migrada.

Estas son las consideraciones específicas que se deben tener en cuenta:

- **SQL Server**
  - Al configurar un [grupo de disponibilidad Always On](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md), asegúrese de que todas las instancias de SQL Server en las que se hospeda una base de datos del grupo de disponibilidad admitan Always Encrypted con enclaves seguros y tengan un enclave y la atestación configurados.
  - Al restaurar desde un archivo de copia de seguridad de una base de datos que usa la funcionalidad de Always Encrypted con enclaves seguros en una instancia de SQL Server que no tiene el enclave configurado, la operación de restauración se realizará correctamente y estará disponible toda la funcionalidad que no se base en el enclave. Pero se producirá un error en las instrucciones posteriores que usen la funcionalidad de enclave, y los índices de las columnas habilitadas para el enclave que usen el cifrado aleatorio dejarán de ser válidos. Lo mismo se aplica al adjuntar una base de datos que usa Always Encrypted con enclaves seguros en la instancia que no tiene configurado el enclave.
  - Si la base de datos contiene índices en columnas habilitadas para el enclave mediante cifrado aleatorio, asegúrese de habilitar [Recuperación de la base de datos acelerada (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) en la base de datos antes de crear una copia de seguridad de base de datos. ADR garantizará que la base de datos, incluidos los índices, está disponible inmediatamente después de restaurarla. Para más información, consulte [Recuperación de la base de datos](#database-recovery).
  
- **Azure SQL Database**
  - Al configurar la [replicación geográfica activa](https://docs.microsoft.com/azure/azure-sql/database/active-geo-replication-overview), asegúrese de que una base de datos secundaria admita los enclaves seguros, si la principal lo hace.

En SQL Server y Azure SQL Database, cuando migre su base de datos mediante un archivo bacpac, debe asegurarse de quitar todos los índices de las columnas habilitadas para el enclave con cifrado aleatorio antes de crear el archivo bacpac.

## <a name="known-limitations"></a>Limitaciones conocidas

Always Encrypted con enclaves seguros soluciona algunas limitaciones de Always Encrypted al admitir el cifrado en contexto y las consultas confidenciales más enriquecidas con índices, como se explica en [Funciones de computación confidencial para las columnas habilitadas para el enclave](#confidential-computing-capabilities-for-enclave-enabled-columns).

El resto de limitaciones de Always Encrypted enumeradas en [Detalles de la característica](always-encrypted-database-engine.md#feature-details) también se aplican a Always Encrypted con enclaves seguros.

Las siguientes limitaciones son específicas de Always Encrypted con enclaves seguros:

- No se pueden crear índices agrupados en columnas habilitadas para enclave que usan cifrado aleatorio.
- Las columnas habilitadas para enclave que usan cifrado aleatorio no pueden ser columnas de clave principales y no se puede hacer referencia a ellas con restricciones de clave externa o restricciones de clave única.
- En [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] (esta limitación no se aplica a [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]) solo se admiten las combinaciones de bucle anidadas (con índices, si están disponibles) en las columnas habilitadas para el enclave mediante el cifrado aleatorio. Para obtener información sobre otras diferencias entre los distintos productos, vea [Consultas confidenciales](#confidential-queries).
- Las operaciones criptográficas en contexto no se pueden combinar con ningún otro cambio de metadatos de columna, excepto los cambios de intercalación dentro de la misma página de código y nulabilidad. Por ejemplo, no puede cifrar, volver a cifrar ni descifrar una columna y cambiar un tipo de datos de la columna en una sola instrucción `ALTER TABLE`/`ALTER COLUMN` de Transact-SQL. Use dos instrucciones independientes.
- No se admite el uso de claves habilitadas para enclave en columnas de tablas en memoria.
- Las expresiones que definen columnas calculadas no pueden realizar cálculos en columnas habilitadas para el enclave mediante el cifrado aleatorio (aunque los cálculos se encuentren ente las operaciones admitidas que se enumeran en [Consultas confidenciales](#confidential-queries)).
- Los caracteres de escape no se admiten en los parámetros del operador LIKE en las columnas habilitadas para enclave mediante el cifrado aleatorio.
- Las consultas con el operador LIKE o un operador de comparación que tiene un parámetro de consulta que usa uno de los siguientes tipos de datos (que se convierten en objetos grandes después del cifrado) pasan por alto los índices y realizan recorridos de tabla.
  - `nchar[n]` y `nvarchar[n]`, si "n" es mayor que 3967.
  - `char[n]`, `varchar[n]`, `binary[n]` y `varbinary[n]`, si "n" es mayor que 7935.
- Limitaciones de las herramientas:
  - Los únicos almacenes de claves que se admiten para almacenar claves maestras de columna habilitadas para el enclave son Almacén de certificados de Windows y Azure Key Vault.
  - No se admite la importación o exportación de bases de datos que contienen claves habilitadas para enclave.
  - Para desencadenar una operación criptográfica en contexto a través de `ALTER TABLE`/`ALTER COLUMN`, debe emitir la instrucción mediante una ventana de consulta en SSMS, o bien puede escribir su propio programa que emita la instrucción. Actualmente, el cmdlet Set-SqlColumnEncryption en el módulo SqlServer de PowerShell y el asistente para Always Encrypted en SQL Server Management Studio no admiten el cifrado en contexto, sino que sacan los datos de la base de datos para realizar operaciones criptográficas, incluso si las claves de cifrado de columna que se usan en estas operaciones están habilitadas para enclave.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Configuración y uso de Always Encrypted con enclaves seguros](configure-always-encrypted-enclaves.md)

## <a name="see-also"></a>Consulte también

- [Administración de claves para Always Encrypted con enclaves seguros](always-encrypted-enclaves-manage-keys.md)
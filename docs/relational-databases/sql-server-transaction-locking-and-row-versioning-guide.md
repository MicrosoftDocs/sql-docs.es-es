---
description: Guía de versiones de fila y bloqueo de transacciones
title: Guía de versiones de fila y bloqueo de transacciones
ms.custom: seo-dt-2019
ms.date: 03/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- guide, transaction locking and row versioning
- transaction locking and row versioning guide
- lock compatibility matrix, [SQL Server]
- lock granularity and hierarchies, [SQL Server]
- deadlocks, [SQL Server]
- lock escalation, [SQL Server]
- lock partitioning, [SQL Server]
ms.assetid: 44fadbee-b5fe-40c0-af8a-11a1eecf6cb7
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47545adc6e3e620cd74585d477bdd028f1e8625f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352493"
---
# <a name="transaction-locking-and-row-versioning-guide"></a>Guía de versiones de fila y bloqueo de transacciones
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En cualquier base de datos, la falta de administración de las transacciones a menudo produce problemas de contención y de rendimiento en sistemas con muchos usuarios. A medida que aumenta el número de usuarios que obtienen acceso a los datos, adquiere importancia el que las aplicaciones utilicen las transacciones eficazmente. En esta guía se describen los mecanismos de versiones de fila y bloqueo que el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza para garantizar la integridad física de cada transacción y proporciona información acerca de cómo las aplicaciones pueden controlar las transacciones de manera eficaz.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] hasta [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], a menos que se especifique lo contrario) y [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]. 
  
##  <a name="transaction-basics"></a><a name="Basics"></a> Conceptos básicos sobre las transacciones  
 Una transacción es una secuencia de operaciones realizadas como una sola unidad lógica de trabajo. Una unidad lógica de trabajo debe exhibir cuatro propiedades, conocidas como propiedades de atomicidad, coherencia, aislamiento y durabilidad (ACID), para ser calificada como transacción.  
  
 **Atomicidad**  
 Una transacción debe ser una unidad atómica de trabajo, tanto si se realizan todas sus modificaciones en los datos, como si no se realiza ninguna de ellas.  
  
 **Coherencia**  
 Cuando finaliza, una transacción debe dejar todos los datos en un estado coherente. En una base de datos relacional, se deben aplicar todas las reglas a las modificaciones de la transacción para mantener la integridad de todos los datos. Todas las estructuras internas de datos, como índices de árbol b o listas doblemente vinculadas, deben estar correctas al final de la transacción.  
  
 **Aislamiento**  
 Las modificaciones realizadas por transacciones simultáneas se deben aislar de las modificaciones llevadas a cabo por otras transacciones simultáneas. Una transacción reconoce los datos en el estado en que estaban antes de que otra transacción simultánea los modificara o después de que la segunda transacción haya concluido, pero no reconoce un estado intermedio. Esto se conoce como seriabilidad, ya que deriva en la capacidad de volver a cargar los datos iniciales y reproducir una serie de transacciones para finalizar con los datos en el mismo estado en que estaban después de realizar las transacciones originales.  
  
 **Durabilidad**  
 Una vez concluida una transacción totalmente durable, sus efectos son permanentes en el sistema. Las modificaciones persisten aún en el caso de producirse un error del sistema. [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] y versiones posteriores habilitan las transacciones durables diferidas. Las transacciones durables diferidas se confirman antes de que la entrada de registro de transacciones se guarde en el disco. Para más información sobre la durabilidad de las transacciones diferidas, vea el tema [Durabilidad de transacciones](../relational-databases/logs/control-transaction-durability.md).  
  
 Los programadores de SQL son los responsables de iniciar y finalizar las transacciones en puntos que exijan la coherencia lógica de los datos. El programador debe definir la secuencia de modificaciones de datos que los dejan en un estado coherente en relación con las reglas de negocios de la organización. El programador incluye estas instrucciones de modificación en una sola transacción de forma que [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] puede hacer cumplir la integridad física de la misma.  
  
 Es responsabilidad de un sistema de base de datos corporativo, como una instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], proporcionar los mecanismos que aseguren la integridad física de cada transacción. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] proporciona:  
  
-   Servicios de bloqueo que preservan el aislamiento de la transacción.  
  
-   Los servicios de registro garantizan la durabilidad de la transacción. Para las transacciones totalmente durables, la entrada de registro se graba en el disco antes de la confirmación de las transacciones. Por tanto, aunque se produzca un error en el hardware del servidor, el sistema operativo o la instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], la instancia usa los registros de transacciones durante el reinicio para revertir automáticamente las transacciones incompletas al punto en que se haya producido el error del sistema. Las transacciones durables diferidas se confirman antes de que la entrada del registro de transacciones se grabe en el disco. Este tipo de transacciones se puede perder si se produce un error del sistema antes de que la entrada del registro se grabe en el disco. Para más información sobre la durabilidad de las transacciones diferidas, vea el tema [Durabilidad de transacciones](../relational-databases/logs/control-transaction-durability.md).  
  
-   Características de administración de transacciones que exigen la atomicidad y coherencia de la transacción. Una vez iniciada una transacción, debe concluirse correctamente (confirmarse); en caso contrario, la instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] deshará todas las modificaciones de datos realizadas desde que se inició la transacción. Nos referimos a esta operación como revertir una transacción porque devuelve los datos al estado en el que estaban antes de esos cambios.  
  
### <a name="controlling-transactions"></a>Control de transacciones  
 Las aplicaciones controlan las transacciones principalmente al especificar cuándo se inicia y finaliza una transacción. Se pueden especificar mediante instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] o funciones de la interfaz de programación de aplicaciones (API) de bases de datos. El sistema también debe ser capaz de controlar correctamente los errores que terminan una transacción antes de que se concluya. Para obtener más información, vea [Transacciones](../t-sql/language-elements/transactions-transact-sql.md), [Transacciones en ODBC](../relational-databases/native-client/odbc/performing-transactions-in-odbc.md) y [Transacciones en SQL Server Native Client (OLEDB)](../relational-databases/native-client-ole-db-transactions/transactions.md).  
  
 De manera predeterminada, las transacciones se administran en las conexiones. Cuando se inicia una transacción en una conexión, todas las instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] ejecutadas en esa conexión forman parte de la transacción hasta que ésta finaliza. No obstante, en una sesión de conjunto de resultados activos múltiples (MARS), una transacción de [!INCLUDE[tsql](../includes/tsql-md.md)] explícita o implícita se convierte en una transacción de lote que se administra en los lotes. Cuando se termina el lote, si la transacción de lote no se confirma ni se revierte, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] la revierte automáticamente. Para obtener más información, vea [Utilizar conjuntos de resultados activos múltiples (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
#### <a name="starting-transactions"></a>Iniciar transacciones  
 Mediante funciones de la API e instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)], puede iniciar transacciones en una instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] como transacciones explícitas, de confirmación automática o implícitas.  
  
 **Transacciones explícitas**  
 En una transacción explícita se define explícitamente tanto el inicio como el final de la transacción a través de una función API o emitiendo las instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)][!INCLUDE[tsql](../includes/tsql-md.md)] BEGIN TRANSACTION, COMMIT TRANSACTION, COMMIT WORK, ROLLBACK TRANSACTION o ROLLBACK WORK. Cuando la transacción termina, la conexión vuelve al modo de transacción en que estaba antes de iniciar la transacción explícita, es decir, el modo implícito o el modo de confirmación automática.  
  
 En una transacción explícita se pueden utilizar todas las instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)], excepto las siguientes:  
  
:::row:::
    :::column:::
        ALTER DATABASE
    :::column-end:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT INDEX
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        CREATE FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        RECONFIGURE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT INDEX
    :::column-end:::
    :::column:::
        CREATE FULLTEXT INDEX
    :::column-end:::
    :::column:::
        RESTORE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        BACKUP
    :::column-end:::
    :::column:::
        DROP DATABASE
    :::column-end:::
    :::column:::
        Procedimientos almacenados de la búsqueda de texto completo
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        sp_dboption para establecer opciones de base de datos ni utilizar ningún procedimiento del sistema que modifique la base de datos maestra en transacciones explícitas o implícitas.
    :::column-end:::
:::row-end:::

> [!NOTE]  
> UPDATE STATISTICS se puede utilizar dentro de una transacción explícita. Sin embargo, UPDATE STATISTICS se confirma independientemente de la transacción que la incluye y no se puede revertir.  
  
 **Transacciones de confirmación automática**  
 El modo de confirmación automática es el modo de administración de transacciones predeterminado de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Cada instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] se confirma o se revierte cuando finaliza. Si una instrucción termina correctamente, se confirma; si encuentra un error, se revierte. Una conexión a una instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] funciona en modo de confirmación automática siempre que no se suplante el modo predeterminado mediante transacciones explícitas o implícitas. El modo de confirmación automática es también el modo predeterminado para ADO, OLE DB, ODBC y DB-Library.  
  
 **Transacciones implícitas**  
 Cuando una conexión funciona en modo de transacciones implícitas, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] inicia automáticamente una nueva transacción después de confirmar o revertir la transacción actual. No tiene que realizar ninguna acción para delinear el inicio de una transacción, solo tiene que confirmar o revertir cada transacción. El modo de transacciones implícitas genera una cadena continua de transacciones. Establezca el modo de transacción implícita a través de una función de la API o la instrucción SET IMPLICIT_TRANSACTIONS ON de [!INCLUDE[tsql](../includes/tsql-md.md)].  Este modo también se denomina Autocommit OFF. Vea [Método setAutoCommit en JDBC](../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md). 
  
 Tras establecer el modo de transacciones implícitas en una conexión, la instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] inicia automáticamente una transacción la primera vez que ejecuta una de estas instrucciones:  
  
:::row:::
    :::column:::
        ALTER TABLE
    :::column-end:::
    :::column:::
        FETCH
    :::column-end:::
    :::column:::
        REVOKE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE
    :::column-end:::
    :::column:::
        GRANT
    :::column-end:::
    :::column:::
        SELECT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        Delete
    :::column-end:::
    :::column:::
        INSERT
    :::column-end:::
    :::column:::
        TRUNCATE TABLE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DROP
    :::column-end:::
    :::column:::
        OPEN
    :::column-end:::
    :::column:::
        UPDATE
    :::column-end:::
:::row-end:::

-  **Transacciones de ámbito de lote**  
   Una transacción implícita o explícita de [!INCLUDE[tsql](../includes/tsql-md.md)] que se inicia en una sesión de MARS (conjuntos de resultados activos múltiples), que solo es aplicable a MARS, se convierte en una transacción de ámbito de lote. Si no se confirma o revierte una transacción de ámbito de lote cuando se completa el lote, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] la revierte automáticamente.  
  
-  **Transacciones distribuidas**  
   Las transacciones distribuidas abarcan dos o más servidores conocidos como administradores de recursos. La administración de la transacción debe ser coordinada entre los administradores de recursos mediante un componente de servidor llamado administrador de transacciones. Cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] puede funcionar como administrador de recursos en las transacciones distribuidas que coordinan los administradores de transacciones, como el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../includes/msconame-md.md)] (MS DTC) u otros administradores que admitan la especificación Open Group XA del procesamiento de transacciones distribuidas. Para obtener más información, consulte la documentación de MS DTC.  
  
   Una transacción de una sola instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] que abarque dos o más bases de datos es, de hecho, una transacción distribuida. La instancia administra la transacción distribuida internamente; para el usuario funciona como una transacción local.  
  
   En la aplicación, una transacción distribuida se administra de forma muy parecida a una transacción local. Al final de la transacción, la aplicación solicita que se confirme o se revierta la transacción. El administrador de transacciones debe administrar una confirmación distribuida de forma diferente para reducir al mínimo el riesgo de que, si se produce un error en la red, algunos administradores de recursos realicen confirmaciones mientras los demás revierten la transacción. Esto se consigue mediante la administración del proceso de confirmación en dos fases (la fase de preparación y la fase de confirmación), que se conoce como confirmación en dos fases (2PC).  
  
   -  **Fase de preparación**  
      Cuando el administrador de transacciones recibe una solicitud de confirmación, envía un comando de preparación a todos los administradores de recursos implicados en la transacción. Cada administrador de recursos hace lo necesario para que la transacción sea duradera y todos los búferes que contienen imágenes del registro de la transacción se pasan a disco. A medida que cada administrador de recursos completa la fase de preparación, notifica si la preparación ha tenido éxito o no al administrador de transacciones. [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] introdujo la durabilidad diferida de transacciones. Las transacciones durables diferidas se confirman antes de que las imágenes del registro de la transacción se vacíen en el disco. Para más información sobre la durabilidad de las transacciones diferidas, vea el tema [Durabilidad de transacciones](../relational-databases/logs/control-transaction-durability.md).  
  
   -  **Fase de confirmación**  
      Si el administrador de transacciones recibe la notificación de que todas las preparaciones son correctas por parte de todos los administradores de recursos, envía comandos de confirmación a cada administrador de recursos. A continuación, los administradores de recursos pueden completar la confirmación. Si todos los administradores de recursos indican que la confirmación ha sido correcta, el administrador de transacciones envía una notificación de éxito a la aplicación. Si algún administrador de recursos informó de un error al realizar la preparación, el administrador de transacciones envía un comando para revertir la transacción a cada administrador de recursos e indica a la aplicación que se ha producido un error de confirmación.  
  
      Las aplicaciones de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pueden administrar transacciones distribuidas a través de [!INCLUDE[tsql](../includes/tsql-md.md)] o de la API de base de datos. Para obtener más información, vea [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
#### <a name="ending-transactions"></a>Finalizar transacciones  
 Puede finalizar las transacciones con una instrucción COMMIT o ROLLBACK, o mediante una función de la API correspondiente.  
  
-  **COMMIT**  
   Si una transacción es correcta, confírmela. La instrucción COMMIT garantiza que todas las modificaciones de la transacción se conviertan en una parte permanente de la base de datos. La instrucción COMMIT también libera recursos que utiliza la transacción como, por ejemplo, los bloqueos.  
  
-  **ROLLBACK**  
   Si se produce un error en una transacción o el usuario decide cancelar la transacción, revierta la transacción. La instrucción ROLLBACK revierte todas las modificaciones realizadas en la transacción al devolver los datos al estado en que estaban al inicio de la transacción. La instrucción ROLLBACK también libera los recursos que mantiene la transacción.  
  
> [!NOTE]  
> En conexiones habilitadas para admitir varios conjuntos de resultados activos (MARS), una transacción explícita que se haya iniciado mediante una función de la API no se puede confirmar mientras haya solicitudes de ejecución pendientes. Cualquier intento de confirmación de una transacción de este tipo mientras se ejecutan operaciones pendientes tendrá como resultado un error.  
  
#### <a name="errors-during-transaction-processing"></a>Errores al procesar la transacción  
 Si un error impide la terminación correcta de una transacción, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] revierte automáticamente la transacción y libera todos los recursos que mantiene la transacción. Si se interrumpe la conexión de red del cliente con una instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], las transacciones pendientes de la conexión revierten al estado anterior cuando la red notifica la interrupción a la instancia. Si la aplicación cliente falla o si el equipo cliente se bloquea o se reinicia, también se interrumpe la conexión y la instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] revierte las conexiones pendientes cuando la red le notifica la interrupción. Si el cliente cierra la aplicación, las transacciones pendientes se revierten.  
  
 Si se produce el error de una instrucción en tiempo de ejecución (como una infracción de restricciones) en un archivo por lotes, el comportamiento predeterminado de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] consiste en revertir solamente la instrucción que generó el error. Puede modificar este comportamiento con la instrucción `SET XACT_ABORT`. Una vez ejecutada la instrucción `SET XACT_ABORT`, los errores de instrucciones en tiempo de ejecución hacen que se revierta automáticamente la transacción actual. Los errores de compilación, como los de sintaxis, no se ven afectados por `SET XACT_ABORT`. Para obtener más información, vea [SET XACT_ABORT &#40;Transact-SQL&#41;](../t-sql/statements/set-xact-abort-transact-sql.md).  
  
 Cuando se producen errores, la acción correctora (`COMMIT` o `ROLLBACK`) debería incluirse en el código de aplicación. Una herramienta eficaz para controlar errores, incluidos los de transacciones, es la construcción TRY…CATCH de [!INCLUDE[tsql](../includes/tsql-md.md)] `TRY...CATCH`. Para obtener más información y ejemplos que incluyan transacciones, vea [TRY...CATCH &#40;Transact-SQL&#41;](../t-sql/language-elements/try-catch-transact-sql.md). A partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], puede usar la instrucción `THROW` para generar una excepción y transferir la ejecución a un bloque `CATCH` o a una construcción `TRY...CATCH`. Para obtener más información, vea [THROW &#40;Transact-SQL&#41;](../t-sql/language-elements/throw-transact-sql.md).  
  
##### <a name="compile-and-run-time-errors-in-autocommit-mode"></a>Errores de compilación y tiempo de ejecución del modo de confirmación automática  
 En el modo de confirmación automática, a veces parece que [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ha revertido un proceso por lotes completo en vez de revertir solamente una instrucción SQL. Esto sucede si se trata de un error de compilación, no en el caso de un error en tiempo de ejecución. Los errores de compilación impiden que [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] genere un plan de ejecución, por lo que no se ejecuta ninguna instrucción del proceso por lotes. Aunque parezca que se han revertido todas las instrucciones anteriores a la que generó el error, el error impidió que se ejecutara ninguna instrucción del proceso por lotes. En el ejemplo siguiente, no se ejecutó ninguna de las instrucciones `INSERT` del tercer proceso por lotes debido a un error de compilación. Parece que se han revertido las dos primeras instrucciones `INSERT` cuando, en realidad, nunca se ejecutaron.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUSE (3, 'ccc');  -- Syntax error.  
GO  
SELECT * FROM TestBatch;  -- Returns no rows.  
GO  
```  
  
 En el ejemplo siguiente, la tercera instrucción `INSERT` genera un error de clave principal duplicada en tiempo de ejecución. Las dos primeras instrucciones `INSERT` eran correctas y se han confirmado, por lo que permanecen después de producirse el error en tiempo de ejecución.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUES (1, 'ccc');  -- Duplicate key error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza la resolución demorada de nombres, en la que no se resuelven los nombres de los objetos hasta la ejecución. En el ejemplo siguiente, se ejecutaron y confirmaron las dos primeras instrucciones `INSERT` y las dos filas permanecen en la tabla `TestBatch` después de que la tercera instrucción `INSERT` generara un error en tiempo de ejecución al hacer referencia a una tabla que no existe.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBch VALUES (3, 'ccc');  -- Table name error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```   
  
##  <a name="locking-and-row-versioning-basics"></a><a name="Lock_Basics"></a> Conceptos básicos sobre las versiones de fila y bloqueo  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza los siguientes mecanismos para garantizar la integridad de las transacciones y mantener la coherencia de las bases de datos cuando varios usuarios obtienen acceso a los datos al mismo tiempo:  
  
-  **Bloqueo**    

   Cada transacción solicita diferentes tipos de bloqueo en los recursos como, por ejemplo, filas, páginas o tablas de los que depende la transacción. Estos bloqueos impiden que otras transacciones puedan modificar los recursos de forma que esto provoque problemas para la transacción que solicita el bloqueo. Cada transacción libera sus bloqueos cuando ya no depende de los recursos bloqueados.  
  
-  **Versiones de fila**    

   Cuando un nivel de aislamiento basado en versiones de fila está habilitado, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene versiones de cada fila que se ha modificado. Las aplicaciones pueden especificar que una transacción utilice las versiones de fila para ver los datos tal como eran al empezar la transacción o la consulta en lugar de proteger todas las lecturas con bloqueos. Mediante las versiones de fila, las probabilidades de que una operación de lectura bloquee otras transacciones se reduce notablemente.  
  
 El bloqueo y las versiones de fila impiden a los usuarios leer los datos no confirmados así como cualquier intento de cambiar los mismos datos a la vez. Sin el bloqueo o las versiones de fila, las consultas ejecutadas para esos datos podrían generar resultados inesperados al devolver datos que todavía no se han confirmado en la base de datos.  
  
 Las aplicaciones pueden elegir los niveles de aislamiento de las transacciones, que definen el nivel de protección de la transacción frente a las modificaciones efectuadas por otras transacciones. Las sugerencias de nivel de tabla pueden especificarse para instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] concretas para personalizar el comportamiento con el fin de que se ajuste a los requisitos de la aplicación.  
  
### <a name="managing-concurrent-data-access"></a>Administrar el acceso simultáneo a datos  
 En ocasiones, los usuarios tienen acceso a un recurso al mismo tiempo, es decir, simultáneamente. El acceso simultáneo a los datos requiere la utilización de mecanismos para impedir efectos negativos cuando varios usuarios intentan modificar recursos que otros usuarios están utilizando.  
  
#### <a name="concurrency-effects"></a>Efectos de la simultaneidad  
 Los usuarios que modifican datos pueden afectar a otros usuarios que leen o modifican los mismos datos a la vez. Se dice que estos usuarios tienen acceso a los datos de forma simultánea. Si un sistema de almacenamiento de datos no dispone de control de simultaneidad, los usuarios se pueden encontrar con los siguientes efectos secundarios:  
  
-   **Actualizaciones perdidas** 
  
     Este problema surge cuando dos o más transacciones seleccionan la misma fila y, a continuación, la actualizan de acuerdo con el valor seleccionado originalmente. Ninguna transacción es consciente de las otras transacciones. La última actualización sobrescribe las actualizaciones realizadas por las otras transacciones y, en consecuencia, se pierden datos.  
  
     Por ejemplo, dos editores realizan una copia electrónica del mismo documento. Cada editor modifica la copia de forma independiente y después la guarda, sobrescribiendo el documento original. El editor que guarda la copia modificada en último lugar sobrescribe las modificaciones que realizó el otro editor. Este problema se puede evitar si un editor no tiene acceso al archivo hasta que el otro finaliza y confirma la transacción.  
  
-   **Dependencia no confirmada (lectura no actualizada)**  
  
     Este problema se produce cuando una transacción selecciona una fila que está siendo actualizada por otra transacción. La segunda transacción lee datos que no han sido confirmados aún y pueden ser modificados por la transacción que está actualizando la fila.  
  
     Por ejemplo, un editor realiza cambios en un documento electrónico. Durante las modificaciones, un segundo editor toma una copia del documento que contiene todas las modificaciones realizadas hasta el momento y la distribuye a los destinatarios. El primer editor decide que los cambios realizados son erróneos, así que los elimina y guarda el documento. El documento distribuido contiene modificaciones que ya no existen y deben tratarse como si nunca hubieran existido. Este problema se puede evitar si nadie lee el documento modificado hasta que el primer editor realiza el almacenamiento final de las modificaciones y confirma la transacción.  
  
-   **Análisis contradictorios (lectura irrepetible)**  
  
     Este problema se produce cuando una transacción obtiene acceso a la misma fila varias veces y en cada ocasión lee datos diferentes. El análisis incoherente es similar a la dependencia no confirmada en tanto que una transacción está modificando los datos que está leyendo una segunda transacción. Sin embargo, en el caso del análisis incoherente, los datos que lee la segunda transacción están confirmados por la transacción que realizó el cambio. Además, el análisis incoherente comprende varias lecturas (dos o más) de la misma fila y las transacciones modifican la información cada vez; de ahí el término de lectura irrepetible.  
  
     Por ejemplo, un editor lee el mismo documento dos veces pero, entre cada lectura, el escritor vuelve a escribir el documento. Cuando el editor lee el documento por segunda vez, éste ha cambiado. La lectura original no era repetible. Este problema se puede evitar si el escritor no cambia el documento hasta que el editor finaliza la lectura por última vez.  
  
-   **Lecturas fantasma**  
  
     Una lectura fantasma es una situación que se produce cuando se ejecutan dos consultas idénticas y la recopilación de filas devuelta por la segunda consulta es diferente. En el siguiente ejemplo se muestra cómo se puede producir esto. Suponga que las dos transacciones siguientes se están ejecutando al mismo tiempo. Las dos instrucciones `SELECT` de la primera transacción pueden devolver resultados diferentes porque la instrucción `INSERT` de la segunda transacción cambia los datos utilizados por ambas.  
  
    ```sql  
    --Transaction 1  
    BEGIN TRAN;  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    --The INSERT statement from the second transaction occurs here.  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    COMMIT;  
    ```  
  
    ```sql  
    --Transaction 2  
    BEGIN TRAN;  
    INSERT INTO dbo.employee  
      (Id, Name) VALUES(6 ,'New');  
    COMMIT;   
    ```  
  
-   **Dobles lecturas o lecturas que faltan por causa de las actualizaciones de las filas**  
  
    -   No se encuentra una fila actualizada o una fila actualizada aparece varias veces  
  
         Las transacciones que se ejecutan en el nivel `READ UNCOMMITTED` no emiten bloqueos compartidos para impedir que otras transacciones modifiquen los datos leídos por la transacción actual. Las transacciones que se están ejecutando en el nivel de READ COMMITTED emiten bloqueos compartidos, pero los bloqueos de fila o página se liberan una vez leída la fila. En cualquier caso, al recorrer un índice, si otro usuario cambia la columna de clave de índice de la fila mientras usted lo está leyendo, la fila podría aparecer de nuevo si el cambio en la clave movió la fila a una posición situada por delante de su punto de recorrido. De igual forma, la fila podría no aparecer si el cambio en la clave movió la fila a una posición en el índice que ya había sido leída. Para evitarlo, utilice la sugerencia `SERIALIZABLE` o `HOLDLOCK`, o bien las versiones de fila. Para obtener más información, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-table.md).  
  
    -   Faltan una o más filas que no eran objeto de la actualización  
  
         Cuando utilice `READ UNCOMMITTED`, si su consulta lee filas mediante recorrido del orden de asignación (uso de páginas IAM), podría perder filas si otra transacción está produciendo una división de página. Esto no puede suceder si utiliza la lectura confirmada porque la tabla se mantiene bloqueada durante la división de la página y no pasa si la tabla no tiene un índice clúster, porque las actualizaciones no producen divisiones de página.  
  
#### <a name="types-of-concurrency"></a>Tipos de simultaneidad  
 Cuando varias personas intentan modificar los datos de una base de datos al mismo tiempo, debe implementarse un sistema de controles de forma que las modificaciones realizadas por una persona no afecten negativamente a las de otra. Esto se denomina control de simultaneidad.  
  
 La teoría del control de simultaneidad tiene dos clasificaciones para los métodos que establecen dicho control:  
  
-   Control de simultaneidad **pesimista**  
  
     Un sistema de bloqueos impide que los usuarios modifiquen los datos de forma que afecte a otros usuarios. Cuando un usuario lleve a cabo una acción que da lugar a que se aplique un bloqueo, los demás usuarios no podrán realizar acciones que crearían conflictos con el bloqueo hasta que el propietario lo libere. Esto se denomina control pesimista porque se utiliza principalmente en entornos donde hay muchos conflictos por la obtención de datos, y en los que el coste de la protección de datos con bloqueos es menor que el de revertir las transacciones si se producen conflictos de simultaneidad.  
  
-   Control de simultaneidad **optimista**  
  
     En el control de simultaneidad optimista, los usuarios no bloquean los datos cuando los leen. Cuando un usuario realiza una actualización de datos, el sistema comprueba si otro usuario ha cambiado los datos después de la lectura. Si otro usuario actualizó los datos, se produce un error. Normalmente, el usuario que recibe el error revierte la transacción y comienza de nuevo. Este tipo de control se denomina optimista porque se utiliza principalmente en entornos donde hay pocos problemas de contención por la obtención de datos y en los que el coste de revertir ocasionalmente una transacción es menor que el de bloquear los datos cuando se leen.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permite el uso de una serie de controles de simultaneidad. Los usuarios especifican el tipo de control de simultaneidad seleccionando niveles de aislamiento de transacción para las conexiones u opciones de simultaneidad en cursores. Estos atributos se pueden definir mediante instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] o bien mediante las propiedades y los atributos de interfaces de programación de aplicaciones (API) de bases de datos como ADO, ADO.NET, OLE DB y ODBC.  
  
#### <a name="isolation-levels-in-the-ssdenoversion"></a>Niveles de aislamiento en [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 Las transacciones especifican un nivel de aislamiento que define el grado en que se debe aislar una transacción de las modificaciones de recursos o datos realizadas por otras transacciones. Los niveles de aislamiento se describen en cuanto a los efectos secundarios de la simultaneidad que se permiten, como las lecturas desfasadas o fantasma.  
  
 Control de los niveles de aislamiento de transacción:  
  
-   Controla si se realizan bloqueos cuando se leen los datos y qué tipos de bloqueos se solicitan.  
-   Duración de los bloqueos de lectura.  
-   Si una operación de lectura que hace referencia a filas modificadas por otra transacción:  
    -   Se bloquea hasta que se libera el bloqueo exclusivo de la fila.  
    -   Recupera la versión confirmada de la fila que existía en el momento en el que empezó la instrucción o la transacción.  
    -   Lee la modificación de los datos no confirmados.  
  
> [!IMPORTANT]  
> La elección de un nivel de aislamiento de transacción no afecta a los bloqueos adquiridos para proteger la modificación de datos. Siempre se obtiene un bloqueo exclusivo en los datos modificados de una transacción, bloqueo que se mantiene hasta que se completa la transacción, independientemente del nivel de aislamiento seleccionado para la misma. En el caso de las operaciones de lectura, los niveles de aislamiento de transacción definen básicamente el nivel de protección contra los efectos de las modificaciones que realizan otras transacciones.  
  
 Un nivel de aislamiento menor significa que los usuarios tienen un mayor acceso a los datos simultáneamente, con lo que aumentan los efectos de simultaneidad que pueden experimentar, como las lecturas desfasadas o la pérdida de actualizaciones. Por el contrario, un nivel de aislamiento mayor reduce los tipos de efectos de simultaneidad, pero requiere más recursos del sistema y aumenta las posibilidades de que una transacción bloquee otra. El nivel de aislamiento apropiado depende del equilibrio entre los requisitos de integridad de los datos de la aplicación y la sobrecarga de cada nivel de aislamiento. El nivel de aislamiento superior, que es serializable, garantiza que una transacción recuperará exactamente los mismos datos cada vez que repita una operación de lectura, aunque para ello aplicará un nivel de bloqueo que puede afectar a los demás usuarios en los sistemas multiusuario. El nivel de aislamiento inferior, de lectura sin confirmar, puede recuperar datos modificados pero no confirmados por otras transacciones. En este nivel se pueden producir todos los efectos secundarios de simultaneidad, pero no hay bloqueos ni versiones de lectura, por lo que se minimiza la sobrecarga.  
  
##### <a name="database-engine-isolation-levels"></a>Niveles de aislamiento del motor de base de datos  
 El estándar ISO define los niveles de aislamiento siguientes, todos ellos compatibles con el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]:  
  
|Nivel de aislamiento|Definición|  
|---------------------|----------------|  
|**Lectura pendiente de confirmación**|El nivel más bajo de aislamiento donde se aíslan las transacciones lo suficiente como para garantizar que no se leen datos físicamente dañados. En este nivel, se permiten las lecturas no actualizadas por lo que es posible que una transacción vea cambios que no se han confirmado aún efectuados por otras transacciones.|  
|**Lectura confirmada**|Permite que una transacción lea los datos previamente leídos (no modificados) por otra transacción, sin tener que esperar a que la primera transacción finalice. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene los bloqueos de lectura (adquiridos en datos seleccionados) hasta el final de la transacción, pero los bloqueos de lectura se liberan tan pronto se efectúa la operación SELECT. Este es el nivel predeterminado del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].|  
|**Lectura repetible**|El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene los bloqueos de lectura y escritura adquiridos en datos seleccionados hasta el final de la transacción. Sin embargo, puesto que los bloqueos de rangos no están administrados, pueden darse lecturas fantasma.|  
|**Serializable**|El nivel más alto, en el que se aíslan completamente las transacciones entre sí. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mantiene los bloqueos de lectura y escritura adquiridos en datos seleccionados y que se liberarán al final de la transacción. Los bloqueos de rangos se adquieren cuando una operación SELECT utiliza una cláusula WHERE con rango, especialmente para evitar lecturas fantasma.<br /><br /> **Nota:** Al solicitar el nivel de aislamiento serializable se puede producir un error en las operaciones DDL y las transacciones de tablas replicadas. La causa es que en las consultas de replicación se utilizan sugerencias que pueden ser incompatibles con el nivel de aislamiento serializable.|  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] también admite dos niveles de aislamiento de transacción adicionales que utilizan versiones de fila. Uno es una implementación de aislamiento de lectura confirmada y el otro un nivel de aislamiento de transacción, la instantánea.  
  
|Nivel de aislamiento de versiones de fila|Definición|  
|------------------------------------|----------------|  
|**Instantánea de lectura confirmada (RCSI)**|Cuando el valor de la opción de base de datos READ_COMMITTED_SNAPSHOT es ON, el aislamiento de lectura confirmada utiliza las versiones de fila para proporcionar una coherencia de lectura en las instrucciones. Las operaciones de lectura solo requieren bloqueos de tablas SCH-S, pero no bloqueos de páginas ni filas. Es decir, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza versiones de fila para presentar a cada instrucción una instantánea coherente, desde el punto de vista transaccional, de los datos tal como se encontraban al comenzar la instrucción. No se emplean bloqueos para impedir que otras transacciones actualicen los datos. Una función definida por el usuario puede devolver datos confirmados después del inicio de la instrucción que contiene que la UDF.<br /><br /> Cuando la opción de base de datos `READ_COMMITTED_SNAPSHOT` está establecida en OFF, que es el valor de configuración predeterminado, el aislamiento confirmado de lectura utiliza bloqueos compartidos para evitar que otras transacciones modifiquen filas mientras la transacción actual está ejecutando una operación de lectura. Los bloqueos compartidos impiden también que la instrucción lea las filas modificadas por otras transacciones hasta que la otra transacción haya finalizado. Ambas implementaciones cumplen la definición ISO del aislamiento de lectura confirmada.|  
|**Instantánea**|El nivel de aislamiento de instantánea utiliza las versiones de fila para proporcionar una coherencia de lectura en las transacciones. No se adquiere ningún bloqueo de páginas ni filas en las operaciones de lectura, solo los bloqueos de tabla SCH-S. Cuando se leen filas modificadas por otras transacciones, se recupera la versión de la fila que existía cuando empezó la transacción. El aislamiento de instantánea solo se puede utilizar en una base de datos cuando la opción de base de datos `ALLOW_SNAPSHOT_ISOLATION` está establecida en ON. De forma predeterminada, el valor de esta opción es OFF para las bases de datos de usuarios.<br /><br /> **Nota:** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no permite controlar las versiones de los metadatos. Por ello, hay restricciones en qué operaciones de DDL se puede realizar en una transacción explícita que se está ejecutando bajo el aislamiento de instantánea. No se permiten las siguientes instrucciones DDL en el aislamiento de instantánea después de una instrucción BEGIN TRANSACTION: ALTER TABLE, CREATE INDEX, CREATE XML INDEX, ALTER INDEX, DROP INDEX, DBCC REINDEX, ALTER PARTITION FUNCTION, ALTER PARTITION SCHEME o cualquier instrucción de DDL de Common Language Runtime (CLR). Estas instrucciones se permiten cuando usa el aislamiento de la instantánea en transacciones implícitas. Una transacción implícita, por definición, es una instrucción única que permite aplicar la semántica del aislamiento de instantánea, incluso con instrucciones de DDL. Las infracciones de este principio pueden producir el error 3961: `Snapshot isolation transaction failed in database '%.*ls' because the object accessed by the statement has been modified by a DDL statement in another concurrent transaction since the start of this transaction. It is not allowed because the metadata is not versioned. A concurrent update to metadata could lead to inconsistency if mixed with snapshot isolation.`|  
  
 En la tabla siguiente se muestran los efectos secundarios de la simultaneidad habilitados por los distintos niveles de aislamiento.  
  
|Nivel de aislamiento|Lectura desfasada|Lectura no repetible|Fantasma|  
|---------------------|----------------|------------------------|-------------|  
|**Lectura pendiente de confirmación**|Sí|Sí|Sí|  
|**Lectura confirmada**|No|Sí|Sí|  
|**Lectura repetible**|No|No|Sí|  
|**Instantánea**|No|No|No|  
|**Serializable**|No|No|No|  
  
 Para obtener más información sobre los tipos de bloqueo específicos o las versiones de fila que controlan cada nivel de aislamiento de transacción, vea [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
 Se pueden establecer los niveles de aislamiento de transacción con [!INCLUDE[tsql](../includes/tsql-md.md)] o mediante una API de bases de datos.  
  
 **[!INCLUDE[tsql](../includes/tsql-md.md)]**                                      
 Los scripts [!INCLUDE[tsql](../includes/tsql-md.md)] usan la instrucción `SET TRANSACTION ISOLATION LEVEL`.  
  
 **ADO**  
 Las aplicaciones ADO establecen la propiedad `IsolationLevel` del objeto **Connection** como adXactReadUncommitted, adXactReadCommitted, adXactRepeatableRead o adXactReadSerializable.  
  
 **ADO.NET**  
 Las aplicaciones ADO.NET que usan el espacio de nombres administrado por `System.Data.SqlClient` pueden llamar al método `SqlConnection.BeginTransaction` y establecer la opción *IsolationLevel* en Unspecified, Chaos, ReadUncommitted, ReadCommitted, RepeatableRead, Serializable y Snapshot.  
  
 **OLE DB**  
 Cuando se inicia una transacción, las aplicaciones que utilizan OLE DB llaman a `ITransactionLocal::StartTransaction` con *isoLevel* establecido en ISOLATIONLEVEL_READUNCOMMITTED, ISOLATIONLEVEL_READCOMMITTED, ISOLATIONLEVEL_REPEATABLEREAD, ISOLATIONLEVEL_SNAPSHOT o ISOLATIONLEVEL_SERIALIZABLE.  
  
 Cuando se especifica el nivel de aislamiento de transacción en el modo de confirmación automática, las aplicaciones OLE DB pueden establecer la propiedad DBPROP_SESS_AUTOCOMMITISOLEVELS de DBPROPSET_SESSION en DBPROPVAL_TI_CHAOS, DBPROPVAL_TI_READUNCOMMITTED, DBPROPVAL_TI_BROWSE, DBPROPVAL_TI_CURSORSTABILITY, DBPROPVAL_TI_READCOMMITTED, DBPROPVAL_TI_REPEATABLEREAD, DBPROPVAL_TI_SERIALIZABLE, DBPROPVAL_TI_ISOLATED o DBPROPVAL_TI_SNAPSHOT.  
  
 **ODBC**  
 Las aplicaciones de ODBC llaman a `SQLSetConnectAttr` con *Attribute* establecido en SQL_ATTR_TXN_ISOLATION y *ValuePtr* establecido en SQL_TXN_READ_UNCOMMITTED, SQL_TXN_READ_COMMITTED, SQL_TXN_REPEATABLE_READ o SQL_TXN_SERIALIZABLE.  
  
 Para las transacciones de instantáneas, las aplicaciones llaman a `SQLSetConnectAttr` con Attribute establecido en SQL_COPT_SS_TXN_ISOLATION y ValuePtr establecido en SQL_TXN_SS_SNAPSHOT. Una transacción de instantánea se puede recuperar mediante SQL_COPT_SS_TXN_ISOLATION o SQL_ATTR_TXN_ISOLATION.  
  
##  <a name="locking-in-the-database-engine"></a><a name="Lock_Engine"></a> Bloqueo del motor de base de datos  
 El bloqueo es el mecanismo que utiliza el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] para sincronizar el acceso por parte de varios usuarios al mismo elemento de datos simultáneamente.  
  
 Antes de que una transacción obtenga una dependencia del estado actual de un elemento de datos, como la lectura o modificación de los datos, debe protegerse de los efectos de otra transacción que modifica los mismos datos. Para ello, la transacción solicita un bloqueo en el elemento de datos. Los bloqueos disponen de diferentes modos, como compartido o exclusivo. El modo del bloqueo indica el nivel de dependencia que la transacción tiene sobre los datos. No se puede conceder a una transacción un bloqueo que genere un conflicto con el modo de un bloqueo ya concedido para unos datos a otra transacción. Si una transacción solicita un modo de bloqueo que cree un conflicto con otro bloqueo ya concedido sobre los mismos datos, la instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pausará la transacción que realiza la solicitud hasta que se libere el primer bloqueo.  
  
 Si una transacción modifica un elemento de datos, conserva el bloqueo que protege la modificación hasta el final de la transacción. El tiempo que una transacción conserva los bloqueos obtenidos para proteger operaciones de lectura depende de la configuración del nivel de aislamiento de la transacción. Todos los bloqueos de una transacción se liberan cuando ésta finaliza (se confirma o se revierte).  
  
 Por regla general, las aplicaciones no solicitan los bloqueos directamente. Una parte de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], denominada administrador de bloqueos, es la que se encarga de administrar los bloqueos de forma interna. Cuando una instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] procesa una instrucción [!INCLUDE[tsql](../includes/tsql-md.md)], el procesador de consultas de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] determina los recursos a los que se va a tener acceso. El procesador de consultas determina también qué tipos de bloqueos se necesitan para proteger cada recurso, basándose en el tipo de acceso y en la configuración del nivel de aislamiento de la transacción. A continuación, el procesador de consultas solicita los bloqueos adecuados al administrador de bloqueos. Éste concede los bloqueos si no existen bloqueos en conflicto de otras transacciones.  
  
## <a name="lock-granularity-and-hierarchies"></a>Granularidad y jerarquías de bloqueo  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] admite bloqueo multigranular. Esta función permite que una transacción bloquee diferentes tipos de recursos. Para minimizar el costo del bloqueo, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] bloquea automáticamente los recursos en el nivel apropiado para la tarea. Los bloqueos de menor granularidad, como es el caso de las filas, aumentan la simultaneidad. Sin embargo, se produce una sobrecarga mayor porque cuantas más filas se bloquean, más bloqueos se deben mantener. Los bloqueos realizados en una granularidad alta, por ejemplo en tablas, reducen la simultaneidad porque el bloqueo de toda una tabla restringe el acceso de otras transacciones a cualquier parte de la tabla. Sin embargo, produce una sobrecarga menor debido a que se mantienen menos bloqueos.  
  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] a menudo se ve en la obligación de adquirir bloqueos en distintos niveles de granularidad para brindar una protección completa a un recurso. Este grupo de bloqueos en distintos niveles de granularidad se denomina jerarquía de bloqueos. Por ejemplo, para brindar protección completa a la lectura de un índice, probablemente sea necesario que una instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] adquiera bloqueos compartidos en filas y bloqueos con intención compartida en las páginas y la tabla.  
  
 En la siguiente tabla se muestran los recursos que el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] puede bloquear.  
  
|Recurso|Descripción|  
|--------------|-----------------|  
|RID|Identificador de fila que se utiliza para bloquear una sola fila de un montón.|  
|KEY|Bloqueo de fila dentro de un índice que se utiliza para proteger intervalos de claves en transacciones serializables.|  
|PAGE|Página de 8 kilobytes (KB) de una base de datos, como páginas de datos o de índices.|  
|EXTENT|Grupo contiguo de ocho páginas, como páginas de datos o de índices.|  
|HoBT|Montón o árbol b. Bloqueo que protege un árbol B (índice) o las páginas de datos del montón en una tabla que no posee un índice clúster.|  
|TABLE|Tabla completa, con todos los datos e índices.|  
|FILE|Archivos de la base de datos.|  
|APPLICATION|Recurso especificado por la aplicación.|  
|METADATOS|Bloqueos de metadatos.|  
|ALLOCATION_UNIT|Unidad de asignación.|  
|DATABASE|Base de datos completa.|  
  
> [!NOTE]  
> La opción LOCK_ESCALATION de [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) puede afectar a los bloqueos HoBT y TABLE.  
  
## <a name="lock-modes"></a><a name="lock_modes"></a> Modos de bloqueo  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] bloquea los recursos con diferentes modos de bloqueo que determinan el modo en que las transacciones simultáneas pueden tener acceso a los recursos.  
  
 En la siguiente tabla se indican los modos de bloqueo de recursos que emplea [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
|Modo de bloqueo|Descripción|  
|---------------|-----------------|  
|**Compartido (S)**|Se utiliza para operaciones de lectura que no cambian ni actualizan datos, como la instrucción SELECT.|  
|**Actualizado (U)**|Se utiliza en recursos que se pueden actualizar. Evita una forma común de interbloqueo que se produce cuando varias sesiones leen, bloquean y actualizan recursos.|  
|**Exclusivo (X)**|Se utiliza para operaciones de modificación de datos, como INSERT, UPDATE o DELETE. Garantiza que no puedan realizarse varias actualizaciones simultáneamente en el mismo recurso.|  
|**Intención**|Se utiliza para establecer una jerarquía de bloqueos. Los tipos de bloqueo de intención son: intención compartido (IS), intención exclusivo (IX) y compartido con intención exclusivo (SIX).|  
|**Esquema**|Se utiliza cuando se ejecuta una operación que depende del esquema de una tabla. Hay dos tipos de bloqueo de esquema: modificación del esquema (Sch-M) y modificación de estabilidad (Sch-S).|  
|**Actualización masiva (BU)**|Se usa cuando se copian datos de forma masiva en una tabla y se especifica la sugerencia **TABLOCK**.|  
|**Intervalo de claves**|Protege el intervalo de filas que lee una consulta cuando se utiliza el nivel de aislamiento de transacciones serializables. Garantiza que otras transacciones no puedan insertar filas que podrían incluirse como respuesta de las consultas de la transacción serializable si las consultas se volvieran a ejecutar.|  
  
### <a name="shared-locks"></a><a name="shared"></a> Bloqueos compartidos  
 Los bloqueos compartidos (S) permiten que varias transacciones simultáneas lean (SELECT) un recurso en situaciones de control de simultaneidad pesimista. Ninguna otra transacción podrá modificar los datos mientras el bloqueo compartido (S) exista en el recurso. Los bloqueos compartidos (S) en un recurso se liberan tan pronto como finaliza la operación de lectura, a menos que se haya establecido el nivel de aislamiento de la transacción como REPEATABLE READ o más alto, o bien se utilice una sugerencia de bloqueo para mantener los bloqueos compartidos (S) durante la transacción.  
  
### <a name="update-locks"></a><a name="update"></a> Bloqueos de actualización  
 Los bloqueos de actualización (U) evitan una forma común de interbloqueo. En una transacción de lectura repetible o serializable, la transacción lee los datos, adquiere un bloqueo compartido (S) en el recurso (página o fila) y, a continuación, modifica los datos, lo que requiere una conversión del bloqueo en un bloqueo exclusivo (X). Si dos transacciones adquieren bloqueos compartidos en un recurso y, a continuación, intentan actualizar los datos simultáneamente, una de ellas intenta convertir el bloqueo en un bloqueo exclusivo (X). La conversión de bloqueo compartido en exclusivo debe esperar, ya que el bloqueo exclusivo de una transacción no es compatible con el bloqueo compartido de la otra. Por tanto, se produce una espera de bloqueos. La segunda transacción intenta adquirir un bloqueo exclusivo (X) para realizar su actualización. Debido a que ambas transacciones intentan convertir los bloqueos en exclusivos (X) y cada una espera a que la otra libere su bloqueo de modo compartido, se produce un interbloqueo.  
  
 Para evitar este posible problema de interbloqueo, se utilizan los bloqueos de actualización (U). Dos transacciones no pueden obtener simultáneamente un bloqueo de actualización (U) para un recurso. Si una transacción modifica un recurso, el bloqueo de actualización (U) se convierte en un bloqueo exclusivo (X).  
  
### <a name="exclusive-locks"></a><a name="exclusive"></a> Bloqueos exclusivos  
 Los bloqueos exclusivos (X) evitan que transacciones simultáneas tengan acceso a un recurso. Al utilizar un bloqueo exclusivo (X), el resto de las transacciones no pueden modificar los datos; las operaciones de lectura solo se pueden realizar si se utiliza la sugerencia NOLOCK o el nivel de aislamiento de lectura no confirmada.  
  
 Las instrucciones para modificar datos, como INSERT, UPDATE y DELETE combinan las operaciones de modificación con las de lectura. En primer lugar, la instrucción lleva a cabo operaciones de lectura para adquirir los datos antes de proceder a ejecutar las operaciones de modificación necesarias. Por tanto, las instrucciones de modificación de datos suelen solicitar bloqueos compartidos y exclusivos. Por ejemplo, una instrucción UPDATE puede modificar las filas de una tabla a partir de una combinación con otra tabla. En este caso, la instrucción UPDATE solicita bloqueos compartidos para la filas leídas en la tabla de combinación, además de bloqueos exclusivos para las filas actualizadas.  
  
### <a name="intent-locks"></a><a name="intent"></a> Bloqueos con intención  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza bloqueos con intención para proteger la aplicación de un bloqueo compartido (S) o exclusivo (X) en un recurso inferior en la jerarquía de bloqueos. Los bloqueos con intención se denominan así porque se adquieren antes que los bloqueos de los niveles inferiores y, por lo tanto, señalan la intención de aplicar bloqueos en un nivel inferior.  
  
 Los bloqueos con intención se utilizan con dos fines:  
  
-   Para evitar que otras transacciones modifiquen el recurso de nivel superior de forma que invaliden el bloqueo del nivel inferior. 
-   Para mejorar la eficacia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] para detectar conflictos de bloqueo en el nivel superior de granularidad.  
  
 Por ejemplo, un bloqueo con intención compartida para el nivel de tabla se solicita antes que los bloqueos compartidos (S) para las páginas o filas de la tabla. Establecer un bloqueo con intención en una tabla evita que otra transacción adquiera un bloqueo exclusivo (X) para la tabla que contiene esa página. Los bloqueos con intención mejoran el rendimiento, porque [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] examina los bloqueos con intención solo en el nivel de tabla para determinar si una transacción puede adquirir un bloqueo de dicha tabla de forma segura. Esto elimina la necesidad de examinar cada bloqueo de fila o de página de la tabla para determinar si una transacción puede bloquear toda la tabla.  
  
<a name="lock_intent_table"></a> Los bloqueos con intención incluyen: Intención compartida (IS), Intención exclusiva (IX) e Intención compartida exclusiva (SIX).  
  
|Modo de bloqueo|Descripción|  
|---------------|-----------------|  
|**Intención compartida (IS)**|Protege los bloqueos compartidos solicitados o adquiridos de algunos recursos (aunque no todos) situados en un nivel inferior de la jerarquía.|  
|**Intención exclusiva (IX)**|Protege los bloqueos exclusivos solicitados o adquiridos de algunos recursos (aunque no todos) situados en un nivel inferior de la jerarquía. IX es un superconjunto de IS, y protege las solicitudes de bloqueos compartidos en recursos de niveles inferiores.|  
|**Compartido con intención exclusiva (SIX)**|Protege los bloqueos compartidos solicitados o adquiridos de todos los recursos situados en un nivel inferior de la jerarquía y los bloqueos con intención exclusiva de algunos (aunque no todos) los recursos de niveles inferiores. Se permiten los bloqueos IS simultáneos en el recurso de nivel superior. Por ejemplo, al adquirir un bloqueo SIX para una tabla, también se adquieren bloqueos con intención exclusiva de las páginas que se modifican y bloqueos exclusivos de las filas modificadas. Solo puede haber un bloqueo SIX simultáneo por recurso, para impedir que otras transacciones lo actualicen, aunque otras transacciones pueden leer los recursos inferiores de la jerarquía obteniendo bloqueos IS en el nivel de tabla.|  
|**Actualizar intención (IU)**|Protege los bloqueos de actualización solicitados o adquiridos de todos los recursos de niveles inferiores de la jerarquía. Los bloqueos IU solo se utilizan para los recursos de página. Los bloqueos IU se convierten en bloqueos IX cuando se ejecutan operaciones de actualización.|  
|**Actualizar intención compartida (SIU)**|Combinación de bloqueos S e IU que resulta de adquirir estos bloqueos por separado y de mantenerlos simultáneamente. Por ejemplo, sería el caso de una transacción que ejecuta una consulta con la sugerencia PAGLOCK y luego ejecuta una operación de actualización. La consulta con la sugerencia PAGLOCK adquiere el bloqueo S y la operación de actualización, el bloqueo IU.|  
|**Actualizar intención exclusiva (UIX)**|Combinación de bloqueos U e IX que resulta de adquirir estos bloqueos por separado y de mantenerlos simultáneamente.|  
  
### <a name="schema-locks"></a><a name="schema"></a> Bloqueos de esquema  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza bloqueos de modificación del esquema (Sch-M) cuando se realiza una operación de lenguaje de definición de datos (DDL) en tablas como, por ejemplo, agregar una columna o quitar una tabla. Mientras se conserva, el bloqueo Sch-M evita el acceso simultáneo a la tabla. Esto significa que el bloqueo Sch-M bloquea todas las operaciones externas hasta que el bloqueo se libera.  
  
 Algunas operaciones del lenguaje de manipulación de datos (DML), como el truncamiento de tablas, utilizan los bloqueos Sch-M para impedir el acceso a las tablas afectadas por operaciones simultáneas.  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] usa bloqueos de estabilidad del esquema (Sch-S) al compilar y ejecutar consultas. Los bloqueos Sch-S no impiden los bloqueos de transacciones, incluidos los bloqueos exclusivos (X). Por tanto, otras transacciones, incluidas las que tienen bloqueos X de una tabla, pueden seguir ejecutándose mientras se compila una consulta. No obstante, en la tabla no se pueden realizar operaciones DDL simultáneas ni operaciones DML simultáneas que adquieren bloqueos Sch-M.  
  
### <a name="bulk-update-locks"></a><a name="bulk_update"></a> Bloqueos de actualización masiva  
 Los bloqueos de actualización masiva (BU) permiten que varios subprocesos copien datos de forma masiva y simultánea en la misma tabla, pero impiden que otros procesos que no están copiando datos de forma masiva tengan acceso a la tabla. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza bloqueos de actualización masiva cuando las dos condiciones siguientes son verdaderas.  
  
-   Está usando la instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] BULK INSERT o la función OPENROWSET(BULK), o está usando uno de los comandos de API Bulk Insert como .NET SqlBulkCopy, las API de carga rápida de OLEDB o las API de copia masiva de ODBC para copiar de forma masiva datos en una tabla.  
-   Se especifica la sugerencia **TABLOCK** o se establece la opción de tabla **table lock on bulk load** con **sp_tableoption**.  
  
> [!TIP]  
> A diferencia de la instrucción BULK INSERT, que contiene un bloqueo Bulk Update menos restrictivo, INSERT INTO…SELECT con la sugerencia TABLOCK retiene un bloqueo exclusivo (X) en la tabla. Esto significa que no se pueden insertar filas mediante operaciones de inserción en paralelo.  
  
### <a name="key-range-locks"></a><a name="key_range"></a> Bloqueos de intervalo de claves  
 Los bloqueos de rangos con clave protegen un intervalo de filas incluido implícitamente en un conjunto de registros que se lee con una instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] mientras se utiliza el nivel de aislamiento de transacción serializable. El bloqueo de intervalos con clave impide las lecturas fantasma. Al proteger los intervalos de claves entre filas, también se evitan inserciones o eliminaciones fantasma en los conjuntos de registros a los que obtienen acceso las transacciones.  
  
## <a name="lock-compatibility"></a><a name="lock_compatibility"></a> Compatibilidad de bloqueos  
 La compatibilidad de bloqueos controla si varias transacciones pueden adquirir bloqueos sobre el mismo recurso a la vez. Si un recurso ya está bloqueado por otra transacción, solo se puede conceder una nueva solicitud de bloqueo si el bloqueo solicitado es compatible con el modo del bloqueo existente. Si el modo del bloqueo solicitado no es compatible con el bloqueo existente, la transacción que solicita el nuevo bloqueo espera a que se libere el bloqueo existente o a que expire el intervalo de tiempo de espera del bloqueo. Por ejemplo, ningún modo de bloqueo es compatible con bloqueos exclusivos. Mientras se mantiene un bloqueo exclusivo (X), ninguna otra transacción puede adquirir un bloqueo de ninguna clase (compartido, de actualización o exclusivo) en dicho recurso hasta que se libere el bloqueo exclusivo. Como alternativa, si se ha aplicado un bloqueo compartido (S) a un recurso, otras transacciones también pueden adquirir un bloqueo compartido o de actualización (U) en el elemento, aunque la primera transacción no haya terminado. Sin embargo, otras transacciones no pueden adquirir un bloqueo exclusivo si no se anula el bloqueo compartido.  
  
<a name="lock_compat_table"></a> En la tabla siguiente se muestra la compatibilidad de los modos de bloqueo más frecuentes.  
  
||Modo concedido existente||||||  
|------|---------------------------|------|------|------|------|------|  
|**Modo solicitado**|**IS**|**S**|**U**|**IX**|**SIX**|**X**|  
|**Intención compartida (IS)**|Sí|Sí|Sí|Sí|Sí|No|  
|**Compartido (S)**|Sí|Sí|Sí|No|No|No|  
|**Actualizado (U)**|Sí|Sí|No|No|No|No|  
|**Intención exclusiva (IX)**|Sí|No|No|Sí|No|No|  
|**Compartido con intención exclusiva (SIX)**|Sí|No|No|No|No|No|  
|**Exclusivo (X)**|No|No|No|No|No|No|  
  
> [!NOTE]  
> Un bloqueo con intención exclusivo (IX) es compatible con un modo de bloqueo IX, porque IX indica la intención de actualizar solamente algunas de las filas, no todas. También se permite que otras transacciones intenten leer o actualizar algunas filas, siempre y cuando no se trate de las mismas filas que están actualizando las demás transacciones. Además, si dos transacciones intentan actualizar la misma fila, se permitirá a ambas transacciones un bloqueo IX en el nivel de tabla y de página. Sin embargo, un bloqueo X en el nivel de fila solo se permitirá a una transacción. La otra transacción deberá esperar a que se quite el bloqueo en el nivel de fila.  
  
<a name="lock_matrix"></a> Utilice la siguiente tabla para determinar la compatibilidad de todos los modos de bloqueo disponibles en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 ![lock_conflicts](../relational-databases/media/LockConflictTable.png)  
  
## <a name="key-range-locking"></a>Bloquear intervalos con clave  
 Los bloqueos de rangos con clave protegen un intervalo de filas incluido implícitamente en un conjunto de registros que se lee con una instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] mientras se utiliza el nivel de aislamiento de transacción serializable. El nivel de aislamiento serializable requiere que las consultas ejecutadas durante una transacción deben obtener el mismo conjunto de filas cada vez que se ejecutan en la transacción. El bloqueo de intervalos con clave protege este requisito, ya que impide que otras transacciones inserten nuevas filas cuyas claves se incluirían en el intervalo de claves leído por la transacción serializable.  
  
 El bloqueo de intervalos con clave impide las lecturas fantasma. La protección de los intervalos de claves entre filas también impide las inserciones fantasma en un conjunto de registros a los que tiene acceso una transacción.  
  
 El bloqueo de intervalos con clave se incluye en un índice, especificando los valores de clave inicial y final. Este bloqueo impide la inserción, actualización o eliminación de filas con un valor de clave incluido en el intervalo, ya que estas operaciones deben obtener en primer lugar un bloqueo en el índice. Por ejemplo, una transacción serializable podría emitir una instrucción `SELECT` que lee todas las filas cuyos valores clave coincidan con la condición `BETWEEN 'AAA' AND 'CZZ'`. El bloqueo de intervalos con clave en los valores de clave del intervalo **'** AAA **'** a **'** CZZ **'** impide que otras transacciones inserten filas con valores de clave situados en dicho intervalo, como **'** ADG **'** , **'** BBD **'** o **'** CAL **'** .  
  
### <a name="key-range-lock-modes"></a><a name="key_range_modes"></a> Modos de bloqueo de intervalos con clave  
 Los bloqueos de intervalos con clave incluyen dos componentes, una fila y un intervalo, especificados con el formato intervalo-fila:  
  
-   El intervalo representa el modo de bloqueo que protege el intervalo entre dos entradas de índice consecutivas.  
-   La fila representa el modo de bloqueo que protege la entrada de índice.  
-   El modo representa el modo de bloqueo combinado que se utiliza. Los modos de bloqueo del intervalo de claves constan de dos partes. La primera representa el tipo de bloqueo que se utiliza para bloquear el intervalo del índice (Range *T*) y la segunda representa el tipo de bloqueo que se utiliza para bloquear una clave específica (*K*). Ambas partes se conectan con un guion (-), como Range *T*-*K*.  
  
    |Intervalo|Row|Mode|Descripción|  
    |-----------|---------|----------|-----------------|  
    |RangeS|S|RangeS-S|Intervalo compartido, bloqueo de recurso compartido; recorrido de intervalo serializable.|  
    |RangeS|U|RangeS-U|Intervalo compartido, bloqueo de recurso de actualización; recorrido de actualización serializable.|  
    |RangeI|Null|RangeI-N|Intervalo de inserción, bloqueo de recurso nulo; se utiliza para comprobar los intervalos antes de insertar una nueva clave en un índice.|  
    |RangeX|X|RangeX-X|Intervalo exclusivo, bloqueo de recurso exclusivo; se utiliza al actualizar una clave de un intervalo.|  
  
> [!NOTE]  
> El modo de bloqueo Null interno es compatible con los demás modos de bloqueo.  
  
 Los modos de bloqueo de intervalos con clave tienen una matriz de compatibilidad que muestra los bloqueos que son compatibles con otros bloqueos obtenidos en claves e intervalos superpuestos.  
  
||Modo concedido existente|||||||  
|------|---------------------------|------|------|------|------|------|------|  
|**Modo solicitado**|**S**|**U**|**X**|**RangeS-S**|**RangeS-U**|**RangeI-N**|**RangeX-X**|  
|**Compartido (S)**|Sí|Sí|No|Sí|Sí|Sí|No|  
|**Actualizado (U)**|Sí|No|No|Sí|No|Sí|No|  
|**Exclusivo (X)**|No|No|No|No|No|Sí|No|  
|**RangeS-S**|Sí|Sí|No|Sí|Sí|No|No|  
|**RangeS-U**|Sí|No|No|Sí|No|No|No|  
|**RangeI-N**|Sí|Sí|Sí|No|No|Sí|No|  
|**RangeX-X**|No|No|No|No|No|No|No|  
  
### <a name="conversion-locks"></a><a name="lock_conversion"></a> Bloqueos de conversión  
 Los bloqueos de conversión se crean cuando un bloqueo de intervalos con clave se superpone a otro bloqueo.  
  
|Bloqueo 1|Bloqueo 2|Bloqueo de conversión|  
|------------|------------|---------------------|  
|S|RangeI-N|RangeI-S|  
|U|RangeI-N|RangeI-U|  
|X|RangeI-N|RangeI-X|  
|RangeI-N|RangeS-S|RangeX-S|  
|RangeI-N|RangeS-U|RangeX-U|  
  
 Los bloqueos de conversión se producen durante breves períodos de tiempo en circunstancias diversas y complejas, y en ocasiones mientras se ejecutan procesos simultáneos.  
  
### <a name="serializable-range-scan-singleton-fetch-delete-and-insert"></a>Recorrido de intervalo serializable, captura de singleton, eliminación e inserción  
 El bloqueo de intervalos con clave garantiza que las siguientes operaciones son serializables:  
  
-   Consulta de recorrido de intervalos  
-   Captura de singleton de fila inexistente  
-   Operación de eliminación  
-   Operación de inserción  
  
 Para que el bloqueo de intervalos con clave se produzca, es necesario que se cumplan las condiciones siguientes:  
  
-   El nivel de aislamiento de las transacciones se debe establecer en SERIALIZABLE.  
-   El procesador de consultas debe utilizar un índice para implementar el predicado del filtro de intervalo. Por ejemplo, la cláusula WHERE de una instrucción SELECT puede establecer una condición de intervalo con este predicado: ColumnX BETWEEN N **"** AAA **"** AND N **"** CZZ **"** . El bloqueo de intervalos con clave solo se puede adquirir si una clave de índice abarca **ColumnX**.  
  
### <a name="examples"></a>Ejemplos  
 La tabla y el índice siguientes se utilizan como base para los ejemplos de bloqueo de intervalos con clave que se muestran a continuación.  
  
 ![btree](../relational-databases/media/btree4.png)  
  
#### <a name="range-scan-query"></a>Consulta de recorrido de intervalos  
 Para poder asegurar que una consulta de recorrido de intervalos es serializable, la misma consulta debe devolver los mismos resultados cada vez que se ejecuta en la misma transacción. Otras transacciones no deben insertar nuevas filas en la consulta de recorrido de intervalos; de lo contrario, se convierten en inserciones fantasma. Por ejemplo, la siguiente consulta utiliza la tabla y el índice de la ilustración anterior:  
  
```sql  
SELECT name  
FROM mytable  
WHERE name BETWEEN 'A' AND 'C';  
```  
  
 Los bloqueos de intervalos con clave se colocan en las entradas de índice que se corresponden al intervalo de filas de datos cuyo nombre se encuentra entre los valores `Adam` y `Dale`, lo que impide que se agreguen o eliminen nuevas filas obtenidas en la consulta anterior. Aunque el primer nombre del intervalo es `Adam`, el bloqueo de intervalos con clave RangeS-S en esta entrada de índice garantiza que no se pueden agregar nombres nuevos que empiecen por la letra A delante de `Adam`, como `Abigail`. De forma similar, el bloqueo de intervalos con clave RangeS-S en la entrada de índice de `Dale` garantiza que no se van a agregar nombres nuevos que empiecen por la letra C detrás de `Carlos`, como `Clive`.  
  
> [!NOTE]  
> El número de bloqueos RangeS-S que se mantiene es *n*+1, siendo *n* el número de filas que satisfacen la consulta.  
  
#### <a name="singleton-fetch-of-nonexistent-data"></a>Captura de singleton de datos inexistentes  
 Si una consulta de una transacción intenta seleccionar una fila que no existe, la ejecución de la consulta en un punto posterior de la misma transacción tiene que devolver el mismo resultado. No se puede permitir a otra transacción insertar la fila inexistente. Por ejemplo, con esta consulta:  
  
```sql  
SELECT name  
FROM mytable  
WHERE name = 'Bill';  
```  
  
 Se aplica un bloqueo de intervalos con clave a la entrada de índice correspondiente al intervalo de nombres comprendido entre `Ben` y `Bing`, ya que se podría insertar el nombre `Bill` entre estas dos entradas de índice adyacentes. El bloqueo de intervalos con clave del modo RangeS-S se coloca en la entrada de índice `Bing`. Esto impide que otra transacción inserte valores, como `Bill`, entre las entradas de índice `Ben` y `Bing`.  
  
#### <a name="delete-operation"></a>Operación de eliminación  
 Cuando se elimina un valor en una transacción, el intervalo en el que entra el valor no debe estar bloqueado mientras se ejecuta la transacción que realiza la operación de eliminación. Para mantener la seriabilidad basta con bloquear el valor de la clave eliminada hasta el final de la transacción. Por ejemplo, con esta instrucción DELETE:  
  
```sql  
DELETE mytable  
WHERE name = 'Bob';  
```  
  
 Se ha colocado un bloqueo exclusivo (X) en la entrada de índice correspondiente al nombre `Bob`. Otras transacciones pueden insertar o eliminar valores antes o después del valor eliminado `Bob`. Sin embargo, cualquier transacción que intente leer, insertar o eliminar el valor `Bob` se bloqueará hasta que la transacción de eliminación se confirme o se revierta.  
  
 La eliminación del intervalo se puede ejecutar con tres modos de bloqueo básicos: bloqueo de fila, de página o de tabla. El optimizador de consultas decide la estrategia de bloqueo de página, tabla o fila, o bien la especifica el usuario mediante sugerencias del optimizador como ROWLOCK, PAGLOCK o TABLOCK. Cuando se utiliza PAGLOCK o TABLOCK, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] anula de forma inmediata la asignación de una página de índice si se eliminan todas sus filas. Por el contrario, cuando se utiliza ROWLOCK, todas las filas eliminadas se marcan solo como eliminadas, y se quitan de la página de índice posteriormente mediante una tarea en segundo plano.  
  
#### <a name="insert-operation"></a>Operación de inserción  
 Cuando se inserta un valor en una transacción, el intervalo en el que entra el valor no debe estar bloqueado mientras se ejecuta la transacción que realiza la operación de inserción. Basta con bloquear el valor de clave insertado hasta el final de la transacción para mantener la seriabilidad. Por ejemplo, con esta instrucción INSERT:  
  
```sql  
INSERT mytable VALUES ('Dan');  
```  
  
 El bloqueo de intervalos con clave de modo RangeI-N se coloca en la entrada de índice correspondiente al nombre David para probar el intervalo. Si se concede el bloqueo, se inserta `Dan` y se coloca un bloqueo exclusivo (X) en el valor `Dan`. El bloqueo de intervalos con clave de modo RangeI-N solo es necesario para probar el intervalo y no se mantiene mientras se ejecuta la transacción que realiza la operación de inserción. Otras transacciones pueden insertar o eliminar valores antes o después del valor insertado `Dan`. Sin embargo, cualquier transacción que intente leer, insertar o eliminar el valor `Dan` se bloqueará hasta que se confirme o se revierta la transacción de inserción.  
  
## <a name="lock-escalation"></a>Extensión de bloqueo
La extensión de bloqueo es el proceso de convertir muchos bloqueos concretos en menos bloqueos más generales, lo que reduce la sobrecarga del sistema al tiempo que aumenta la probabilidad de contención de simultaneidad.

A medida que [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] adquiere bloqueos de bajo nivel, también coloca bloqueos preventivos en los objetos que contienen los objetos de nivel más bajo:

-   Al bloquear filas o intervalos de clave de índice, [!INCLUDE[ssde_md](../includes/ssde_md.md)] coloca un bloqueo preventivo en las páginas que contienen las filas o claves.
-   Al bloquear páginas, [!INCLUDE[ssde_md](../includes/ssde_md.md)] coloca un bloqueo preventivo en los objetos de más alto nivel que contienen las páginas. Además del bloqueo de intención en el objeto, los bloqueos de página de intento se solicitan en los objetos siguientes:
    -  Páginas de nivel de hoja de índices no clúster
    -  Páginas de datos de los índices clúster
    -  Páginas de datos de montón

Es posible que [!INCLUDE[ssde_md](../includes/ssde_md.md)] realice bloqueos de fila y página para una misma instrucción a fin de minimizar el número de bloqueos y reducir la probabilidad de que sea necesario realizar la extensión de bloqueo. Por ejemplo, el Motor de base de datos podría colocar bloqueos de página en un índice no agrupado (si se seleccionan suficientes claves contiguas del nodo del índice como para satisfacer la consulta) y bloqueos de fila en los datos.

Para extender los bloqueos, [!INCLUDE[ssde_md](../includes/ssde_md.md)] intenta cambiar el bloqueo preventivo de la tabla por el correspondiente bloqueo completo; por ejemplo, cambiar un bloqueo preventivo exclusivo (IX) por un bloqueo exclusivo (X), o bien un bloqueo preventivo compartido (IS) por un bloqueo compartido (S). Si el intento de extensión de bloqueo se realiza correctamente y se adquiere el bloqueo de toda la tabla, se liberan todos los bloqueos de montón o árbol B, página (PAGE) o fila (RID) retenidos por la transacción en el montón o el índice. Si no se puede adquirir el bloqueo completo, no se produce ninguna extensión de bloqueo en el momento y el Motor de base de datos continúa para adquirir bloqueos de fila, clave o página.

[!INCLUDE[ssde_md](../includes/ssde_md.md)] no extiende los bloqueos de filas o de intervalo de claves a bloqueos de páginas, sino que lo hace directamente en bloqueos de tablas. De manera similar, los bloqueos de páginas se concentran siempre en bloqueos de tablas. El bloqueo de tablas con particiones se puede extender al nivel HoBT para la partición asociada en lugar de hacerlo al bloqueo de la tabla. Un bloqueo de nivel HOBT no bloquea necesariamente los HoBTs alineados para la partición.

> [!NOTE]
> Los bloqueos de nivel HOBT normalmente aumentan la simultaneidad, pero suponen un riesgo potencial de interbloqueos cuando cada una de las transacciones que están bloqueando diferentes particiones quiere ampliar sus bloqueos exclusivos a las demás particiones. En algunos casos poco habituales, la granularidad de bloqueo TABLE dará mejores resultados.

Si no se produce un intento de extensión de bloqueo debido a conflictos de bloqueo retenidos por transacciones simultáneas, [!INCLUDE[ssde_md](../includes/ssde_md.md)] intentará realizar de nuevo la extensión de bloqueo por cada 1250 bloqueos adicionales adquiridos por la transacción.

Cada evento de extensión funciona principalmente en una sola instrucción [!INCLUDE[tsql](../includes/tsql-md.md)]. Cuando el evento se inicia, [!INCLUDE[ssde_md](../includes/ssde_md.md)] intenta extender todos los bloqueos propiedad de la transacción actual que se encuentren en alguna de las tablas a las que se hace referencia en la instrucción activa, siempre que cumpla los requisitos de umbral de extensión. Si el evento de concentración se inicia antes de que la instrucción tenga acceso a la tabla, no se hace ningún intento de concentrar los bloqueos de la tabla. Si la extensión de bloqueo se realiza correctamente, los bloqueos adquiridos por la transacción en una instrucción anterior que sigan retenidos en el momento en que se inicia el evento se concentrarán, si se hace referencia a la tabla en la instrucción actual y se incluye la tabla en el evento de concentración.

Por ejemplo, suponga que la sesión realiza estas operaciones:

-  Inicia una transacción.
-  Actualiza `TableA`. Esto genera bloqueos exclusivos de fila en TableA que se retienen hasta que se completa la transacción.
-  Actualiza `TableB`. Esto genera bloqueos exclusivos de fila en TableB que se retienen hasta que se completa la transacción.
-  Realiza una operación SELECT que combina `TableA` con `TableC`. El plan de ejecución de la consulta llama a las filas que se van a recuperar de `TableA` antes de que se recuperen las de `TableC`.
-  La instrucción SELECT desencadena la extensión de bloqueo mientras recupera filas de `TableA` y antes de acceder a `TableC`.

Si la extensión de bloqueo se realiza correctamente, solo se extienden los bloqueos retenidos por la sesión en `TableA`. Esto incluye los bloqueos compartidos de la instrucción SELECT y los bloqueos exclusivos de la instrucción UPDATE anterior. Aunque para determinar si se debe realizar la extensión de bloqueo solo se cuentan los bloqueos que la sesión ha adquirido en `TableA` para la instrucción SELECT, cuando la extensión se realiza correctamente, todos los bloqueos retenidos por la sesión en `TableA` se extienden a un bloqueo exclusivo en la tabla y se liberan los demás bloqueos de granularidad inferior, incluidos los bloqueos preventivos, de `TableA`.

No se intenta extender los bloqueos de `TableB` porque no había una referencia activa a `TableB` en la instrucción SELECT. De manera similar, no se intenta extender los bloqueos de `TableC`, ya que no están extendidos porque todavía no se había accedido a esta tabla cuando se produjo la extensión.

### <a name="lock-escalation-thresholds"></a>Umbrales de extensión de bloqueo

La extensión de bloqueo se activa cuando esa operación no se ha deshabilitado en la tabla mediante la opción `ALTER TABLE SET LOCK_ESCALATION` y cuando se da cualquiera de las condiciones siguientes:

-  Una sola instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] adquiere al menos 5 000 bloqueos en un solo índice o tabla sin particiones.
-  Una sola instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] adquiere al menos 5 000 bloqueos en una sola partición de una tabla o índice con particiones, y la opción `ALTER TABLE SET LOCK_ESCALATION` está establecida en AUTO.
-  El número de bloqueos en una instancia de [!INCLUDE[ssde_md](../includes/ssde_md.md)] supera los umbrales de memoria o de configuración.

Si los bloqueos no se pueden extender debido a conflictos de bloqueo, [!INCLUDE[ssde_md](../includes/ssde_md.md)] desencadena periódicamente la extensión de bloqueo cada 1250 nuevos bloqueos adquiridos.

### <a name="escalation-threshold-for-a-transact-sql-statement"></a>Umbral de concentración para una instrucción de Transact-SQL
Cuando [!INCLUDE[ssde_md](../includes/ssde_md.md)] comprueba si hay posibles extensiones cada 1250 bloqueos recién adquiridos, se producirá una extensión de bloqueo solo si una instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] ha adquirido al menos 5 000 bloqueos en una única referencia de una tabla. La extensión de bloqueo se desencadena cuando una instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] adquiere al menos 5 000 bloqueos en una única referencia de una tabla. Por ejemplo, la extensión de bloqueo no se desencadena si una instrucción adquiere 3.000 bloqueos en un índice y 3.000 bloqueos en otro índice de la misma tabla. De forma similar, la extensión de bloqueo no se desencadena si una instrucción tiene una autocombinación en una tabla y cada referencia a la tabla solo adquiere 3.000 bloqueos de la tabla.

La extensión de bloqueo solo se produce para tablas a las que se ha obtenido acceso en el momento en que se desencadena la concentración. Imagine que una única instrucción SELECT es una combinación que accede a tres tablas por este orden: `TableA`, `TableB` y `TableC`. La instrucción adquiere 3 000 bloqueos de fila del índice agrupado de `TableA` y, al menos, 5 000 bloqueos de fila del índice agrupado de `TableB`, pero todavía no ha accedido a `TableC`. Cuando [!INCLUDE[ssde_md](../includes/ssde_md.md)] detecta que la instrucción ha adquirido al menos 5 000 bloqueos de fila de `TableB`, intenta extender todos los bloqueos retenidos por la transacción actual en `TableB`. También intenta extender todos los bloqueos retenidos por la transacción actual en `TableA`, pero como el número de bloqueos de `TableA` es inferior a 5 000, no se realiza la extensión. No se intenta ninguna extensión de bloqueo en `TableC` porque todavía no se había accedido a ella cuando se produjo la extensión.

### <a name="escalation-threshold-for-an-instance-of-the-database-engine"></a>Umbral de concentración para una instancia del motor de base de datos
Siempre que el número de bloqueos sea mayor que el umbral de memoria para la extensión de bloqueo, [!INCLUDE[ssde_md](../includes/ssde_md.md)] desencadena la extensión de bloqueo. El umbral de memoria depende del valor de la [opción de configuración locks](../database-engine/configure-windows/configure-the-locks-server-configuration-option.md):

-   Si la opción **locks** se establece en el valor predeterminado de 0, el umbral de extensión de bloqueo se alcanza cuando la memoria que usan los objetos del bloqueo es un 24 % de la que usa el Motor de base de datos, sin contar la memoria AWE. La estructura de datos que se utiliza para representar un bloqueo tiene aproximadamente una longitud de 100 bytes. Este umbral es dinámico debido a que el Motor de base de datos adquiere y libera memoria de forma dinámica para ajustarse a las variaciones de las cargas de trabajo.

-   Si la opción **locks** es un valor distinto de 0, el umbral de extensión de bloqueo es 40 % (o menos si existe presión de memoria) del valor de la opción.

El [!INCLUDE[ssde_md](../includes/ssde_md.md)] puede elegir cualquier instrucción activa de cualquier sesión para la extensión; además, por cada 1250 nuevos bloqueos, elegirá instrucciones para la extensión siempre y cuando la memoria de bloqueo utilizada en la instancia se mantenga por encima del umbral.

### <a name="escalating-mixed-lock-types"></a>Concentrar tipos de bloqueo mixtos
Cuando se produce una extensión de bloqueo, el bloqueo seleccionado para el montón o índice es lo suficientemente fuerte como para cumplir con los requisitos del bloqueo de bajo nivel más restrictivo.

Por ejemplo, suponga que una sesión:

-  Inicia una transacción.
-  Actualiza una tabla que contiene un índice clúster.
-  Emite una instrucción SELECT que hace referencia a la misma tabla.

La instrucción UPDATE adquiere estos bloqueos:

-  Bloqueos exclusivos (X) en las filas de datos actualizadas.
-  Bloqueos con intención exclusivos (IX) en las páginas del índice clúster que contienen estas filas.
-  Un bloqueo IX en el índice clúster y otro en la tabla.

La instrucción SELECT adquiere estos bloqueos:

-  Bloqueos compartidos (S) en todas las filas de datos que lee, a no ser que la fila esté ya protegida por un bloqueo X de la instrucción UPDATE.
-  Bloqueos con intención compartidos en todas las páginas de índice clúster que contienen dichas filas, a no ser que la página esté ya protegida por un bloqueo IX.
-  Ningún bloqueo en el índice clúster o la tabla porque ya están protegidos por bloqueos IX.

Si la instrucción SELECT adquiere suficientes bloqueos como para desencadenar la extensión de bloqueo y ésta se realiza correctamente, el bloqueo IX de la tabla se convierte en un bloqueo X y se liberan todos los bloqueos de fila, página e índice. Las actualizaciones y las lecturas están protegidas por el bloqueo X de la tabla.

### <a name="reducing-locking-and-escalation"></a>Reducir bloqueos y extensiones
En la mayoría de los casos, [!INCLUDE[ssde_md](../includes/ssde_md.md)] presta el mejor rendimiento cuando funciona con la configuración predeterminada de bloqueo y extensión de bloqueo. Si una instancia de [!INCLUDE[ssde_md](../includes/ssde_md.md)] genera gran cantidad de bloqueos y se producen extensiones de bloqueo frecuentes, considere la posibilidad de reducir la cantidad de bloqueos mediante:

-   El uso de un nivel de aislamiento que no genere bloqueos compartidos para las operaciones de lectura:
    -  Un nivel de aislamiento READ COMMITTED si la opción de base de datos READ_COMMITTED_SNAPSHOT está en ON.
    -  Un nivel de aislamiento SNAPSHOT.
    -  Un nivel de aislamiento READ UNCOMMITTED. Este nivel solo se puede utilizar en sistemas que puedan funcionar con lecturas no actualizadas.    
  
    > [!NOTE]
    > Cambiar el nivel de aislamiento afecta a todas las tablas en la instancia de [!INCLUDE[ssde_md](../includes/ssde_md.md)].

-   El uso de las sugerencias de tabla PAGLOCK o TABLOCK para que el Motor de base de datos utilice bloqueos de página, montón o índice en lugar de bloqueos de filas. Esta opción, sin embargo, aumenta los problemas derivados de usuarios que bloquean a otros usuarios al intentar obtener acceso a los mismos datos y solo se debe utilizar en sistemas con pocos usuarios simultáneos.

-   Para las tablas con particiones, use la opción LOCK_ESCALATION de [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) para extender los bloqueos al nivel HoBT en lugar de hacerlo al nivel de la tabla, o bien para deshabilitar la extensión de bloqueo.

-   Divida las operaciones en lote grandes en varias operaciones más pequeñas. Por ejemplo, imagine que ha ejecutado la consulta siguiente para quitar varios cientos de miles de registros antiguos de una tabla de auditoría y, después, ha comprobado que se ha producido una extensión de bloqueo que ha bloqueado a otros usuarios:
   
    ```sql
    DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    ```

    Si quita varios cientos de estos registros cada vez, puede reducir drásticamente el número de bloqueos que se acumulan por transacción y evitar la extensión de bloqueo. Por ejemplo:

    ```sql
    SET ROWCOUNT 500
    delete_more:
      DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    IF @@ROWCOUNT > 0 GOTO delete_more
    SET ROWCOUNT 0
    ```

-   Reduzca la superficie de bloqueo de una consulta; para ello, intente que la consulta sea lo más eficaz posible. Los recorridos grandes o un gran número de búsquedas de marcadores pueden aumentar la posibilidad de que se necesite la extensión de bloqueo; además, aumenta la posibilidad de los interbloqueos y, por lo general, afecta de manera negativa a la simultaneidad y el rendimiento. Después de encontrar la consulta que provoca la extensión de bloqueo, busque oportunidades para crear índices o agregar columnas a un índice existente a fin de quitar los recorridos de índice o de tabla, y maximizar la eficacia de las búsquedas de índice. Considere la posibilidad de usar el [Asistente para la optimización de motor de base de datos](../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md) para realizar un análisis automático de índices en la consulta. Para más información, consulte el [Tutorial: Asistente para la optimización de motor de base de datos](../tools/dta/tutorial-database-engine-tuning-advisor.md).
    Un objetivo de esta optimización es hacer que las búsquedas de índice devuelvan el menor número posible de filas para minimizar el costo de las búsquedas de marcadores (se maximiza la selectividad del índice para esa consulta concreta). Si [!INCLUDE[ssde_md](../includes/ssde_md.md)] estima que un operador lógico de búsqueda de marcadores puede devolver muchas filas, puede usar una instrucción PREFETCH para realizar la búsqueda de marcadores. Si [!INCLUDE[ssde_md](../includes/ssde_md.md)] usa PREFETCH para una búsqueda de marcadores, debe aumentar el nivel de aislamiento de transacción de una parte de la consulta para leer de forma repetida una parte de la consulta. Esto significa que lo que puede parecer similar a una instrucción SELECT en un nivel de aislamiento de lectura confirmada puede adquirir muchos miles de bloqueos de clave (tanto en el índice agrupado como en el no agrupado), lo que puede hacer que esa consulta supere los umbrales de extensión de bloqueo. Esto es especialmente importante si observa que el bloqueo extendido es un bloqueo de tabla compartido que no se suele ver en el nivel de aislamiento predeterminado de lectura confirmada. Si una búsqueda de marcadores con una cláusula PREFETCH es el origen de la extensión, considere la posibilidad de agregar columnas adicionales al índice no agrupado que aparece en el operador lógico de Index Seek o Index Scan debajo del operador lógico de Bookmark Lookup en el plan de consulta. Puede crear un índice de cobertura (que incluya todas las columnas de una tabla que se han usado en la consulta), o al menos un índice que abarque las columnas que se han usado para los criterios de combinación o en la cláusula WHERE si no es práctico incluir todo en la lista de columnas de selección.
    Una combinación de bucle anidado también puede usar PREFETCH, lo que provoca el mismo comportamiento de bloqueo.
   
-   No se puede realizar la extensión de bloqueo si otro SPID contiene un bloqueo de tabla no compatible. La extensión de bloqueo siempre se extiende a un bloqueo de tabla y nunca a bloqueos de página. Además, si se produce un error en un intento de extensión de bloqueo porque otro SPID contiene un bloqueo TAB no compatible, la consulta que ha intentado la extensión no se bloquea mientras espera un bloqueo TAB. En su lugar, continúa con la adquisición de bloqueos en su nivel original y más granular (fila, clave o página), realizando periódicamente más intentos de extensión. Por tanto, un método para evitar la extensión de bloqueo en una tabla concreta consiste en adquirir y mantener un bloqueo en otra conexión que no sea compatible con el tipo de bloqueo extendido. Un bloqueo IX (preventivo exclusivo) en el nivel de tabla no bloquea ninguna fila o página, pero todavía no es compatible con un bloqueo TAB S (compartido) o X (exclusivo). Por ejemplo, imagine que tiene que ejecutar un trabajo por lotes que modifique un gran número de filas en la tabla mytable y que haya provocado el bloqueo debido a la extensión de bloqueo. Si este trabajo se completa siempre en menos de una hora, puede crear un trabajo de [!INCLUDE[tsql](../includes/tsql-md.md)] que contenga el código siguiente y programar el nuevo trabajo para que se inicie varios minutos antes del trabajo por lotes:
  
    ```sql
    BEGIN TRAN
    SELECT * FROM mytable (UPDLOCK, HOLDLOCK) WHERE 1=0
    WAITFOR DELAY '1:00:00'
    COMMIT TRAN
    ```
   
    Esta consulta adquiere y mantiene un bloqueo IX en mytable durante una hora, lo que evita la extensión de bloqueo en la tabla durante ese tiempo. Este lote no modifica ningún dato ni bloquea otras consultas (a menos que la otra consulta fuerce un bloqueo de tabla con la sugerencia TABLOCK, o bien si un administrador ha deshabilitado los bloqueos de página o fila mediante un procedimiento almacenado sp_indexoption).

También se pueden utilizar las marcas de seguimiento 1211 y 1224 para deshabilitar todas las extensiones de bloqueo o solo algunas. Pero estas [marcas de seguimiento](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) deshabilitan toda la extensión de bloqueo globalmente para la totalidad de [!INCLUDE[ssde_md](../includes/ssde_md.md)]. La extensión de bloqueo tiene una finalidad muy útil en [!INCLUDE[ssde_md](../includes/ssde_md.md)], ya que maximiza la eficacia de las consultas que de otro modo se ralentizan por la sobrecarga de la adquisición y liberación de varios miles de bloqueos. La extensión de bloqueo también ayuda a minimizar la memoria necesaria para realizar el seguimiento de los bloqueos. La memoria que [!INCLUDE[ssde_md](../includes/ssde_md.md)] puede asignar de forma dinámica para las estructuras de bloqueo es finita, por lo que si deshabilita la extensión de bloqueo y la memoria de bloqueo crece lo suficiente, se pueden producir errores en los intentos de asignar bloqueos adicionales a cualquier consulta y se produce el siguiente error:

```Error: 1204, Severity: 19, State: 1
The SQL Server cannot obtain a LOCK resource at this time. Rerun your statement when there are fewer active users or ask the system administrator to check the SQL Server lock and memory configuration.
```

> [!NOTE]
> Cuando se produce el [error 1204](../relational-databases/errors-events/mssqlserver-1204-database-engine-error.md), se detiene el procesamiento de la instrucción actual y se provoca la reversión de la transacción activa. La propia reversión puede bloquear a los usuarios o generar un tiempo de recuperación largo de la base de datos si se reinicia el servicio de base de datos.

> [!NOTE]
> El uso de una sugerencia de bloqueo como ROWLOCK solo altera el plan de bloqueo inicial. Las sugerencias de bloqueo no impiden la extensión de bloqueo. 

Además, supervise la extensión de bloqueo mediante el evento extendido `lock_escalation` (xEvent), como en el ejemplo siguiente:

```sql
-- Session creates a histogram of the number of lock escalations per database 
CREATE EVENT SESSION [Track_lock_escalation] ON SERVER 
ADD EVENT sqlserver.lock_escalation(SET collect_database_name=(1),collect_statement=(1)
    ACTION(sqlserver.database_id,sqlserver.database_name,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,sqlserver.sql_text,sqlserver.username))
ADD TARGET package0.histogram(SET source=N'sqlserver.database_id')
GO
```

> [!IMPORTANT]
> El evento extendido `lock_escalation` (xEvent) se debe usar en lugar de la clase de eventos Lock:Escalation en Seguimiento SQL o SQL Profiler

## <a name="dynamic-locking"></a><a name="dynamic_locks"></a> Bloqueo dinámico
 La utilización de bloqueos de bajo nivel, como los de fila, aumenta la simultaneidad reduciendo la probabilidad de que dos transacciones soliciten bloqueos de los mismos datos al mismo tiempo. También aumenta el número de bloqueos y los recursos necesarios para administrarlos. Los bloqueos de alto nivel de tabla o página producen una sobrecarga menor, pero a costa de reducir la simultaneidad.  
  
 ![Costo de bloqueo frente a costo de simultaneidad](../relational-databases/media/lockcht.png) 
  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza una estrategia de bloqueo dinámico para determinar los bloqueos que son más eficaces. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] determina automáticamente los bloqueos más apropiados cuando se ejecuta la consulta, basándose en las características del esquema y de la consulta. Por ejemplo, para reducir la sobrecarga de bloqueos, el optimizador puede decidir la realización de bloqueos de página en un índice al realizar un recorrido del índice.  
  
 El bloqueo dinámico presenta las ventajas siguientes:  
  
-   Administración simplificada de la base de datos. Los administradores de bases de datos no tienen que preocuparse de ajustar los umbrales de extensión de bloqueo.  
-   Mayor rendimiento. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] minimiza la sobrecarga del sistema al utilizar los bloqueos apropiados para la tarea.  
-   Los programadores de aplicaciones se pueden concentrar en la programación. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ajusta los bloqueos automáticamente.  
  
 A partir de [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)], el comportamiento de la extensión de bloqueo ha cambiado con la introducción de la opción `LOCK_ESCALATION`. Para obtener más información, vea la opción `LOCK_ESCALATION` de [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md). 
   
## <a name="deadlocks"></a><a name="deadlocks"></a> Interbloqueos  
 Un interbloqueo se produce cuando dos o más tareas se bloquean entre sí permanentemente teniendo cada tarea un bloqueo en un recurso que las otras tareas intentan bloquear. Por ejemplo:  
  
-   La transacción A adquiere un bloqueo compartido de la fila 1.  
-   La transacción B adquiere un bloqueo compartido de la fila 2.  
-   Ahora la transacción A solicita un bloqueo exclusivo de la fila 2 y se bloquea hasta que la transacción B finalice y libere el bloqueo compartido que tiene de la fila 2.  
-   Ahora la transacción B solicita un bloqueo exclusivo de la fila 1 y se bloquea hasta que la transacción A finalice y libere el bloqueo compartido que tiene de la fila 1.  
  
 La transacción A no se puede completar hasta que se complete la transacción B, pero la transacción B está bloqueada por la transacción A. Esta condición también se denomina una dependencia cíclica: La transacción A tiene una dependencia de la transacción B y la transacción B cierra el círculo con una dependencia en la transacción A.  
  
 Ambas transacciones con un interbloqueo esperarán para siempre, a no ser que un proceso externo rompa el interbloqueo. La supervisión de interbloqueos del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] comprueba periódicamente si hay tareas con un interbloqueo. Si el monitor detecta una dependencia cíclica, elige una de las tareas como el sujeto y finaliza su transacción con un error. Esto permite a la otra tarea completar su transacción. La aplicación con la transacción que terminó con un error puede reintentar la transacción, que suele completarse después de que la otra transacción interbloqueada haya finalizado.  
  
 A menudo se confunden los interbloqueos con los bloqueos normales. Cuando una transacción solicita un bloqueo en un recurso bloqueado por otra transacción, la transacción solicitante espera hasta que se libere el bloqueo. De forma predeterminada, las transacciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no tienen tiempo de espera, a menos que se establezca LOCK_TIMEOUT. La transacción solicitante está bloqueada, no interbloqueada, porque la transacción solicitante no ha hecho nada para bloquear la transacción a la que pertenece el bloqueo. Finalmente, la transacción a la que pertenece el bloqueo se completará y liberará el bloqueo, y a la transacción solicitante se le concederá el bloqueo y continuará.  
  
> [!NOTE]
> A veces, los interbloqueos se denominan "abrazo mortal".  
  
 Un interbloqueo es una condición que se puede dar en cualquier sistema con varios subprocesos, no solo en un sistema de administración de bases de datos relacionales, y puede producirse para recursos distintos a los bloqueos en objetos de base de datos. Por ejemplo, un subproceso en un sistema operativo con varios subprocesos puede adquirir uno o más recursos, como bloqueos de memoria. Si el recurso que se desea adquirir pertenece actualmente a otro subproceso, puede que el primer subproceso deba esperar a que el otro libere el recurso de destino. En consecuencia, se dice que el subproceso que está en espera depende del subproceso que posee ese recurso concreto. En una instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], las sesiones pueden interbloquearse cuando adquieren recursos ajenos a la base de datos, como memoria o subprocesos.  
  
 ![Diagrama en el que se muestra un interbloqueo de transacciones](../relational-databases/media/deadlock.png)  
  
 En la ilustración, la transacción T1 tiene una dependencia de la transacción T2 para el recurso de bloqueo de la tabla `Part`. Del mismo modo, la transacción T2 tiene una dependencia de la transacción T1 para el recurso de bloqueo de la tabla `Supplier`. Puesto que estas dependencias forman un ciclo, hay un interbloqueo entre las transacciones T1 y T2.  
  
 Los interbloqueos también se pueden producir cuando se crean particiones en una tabla y el valor de `LOCK_ESCALATION` de `ALTER TABLE` se fija en AUTO. Cuando `LOCK_ESCALATION` es AUTO, la simultaneidad aumenta permitiendo a [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] bloquear las particiones de la tabla en el nivel HoBT en lugar de en el nivel de tabla. Sin embargo, cuando transacciones independientes mantienen bloqueos de partición en una tabla y desean un bloqueo en algún punto de la partición de otras transacciones, se produce un interbloqueo. Este tipo de interbloqueo se puede evitar asignando `LOCK_ESCALATION` a `TABLE`; aunque este valor reducirá la simultaneidad obligando a las grandes actualizaciones de las particiones a esperar a que se produzca un bloqueo de la tabla.  
  
### <a name="detecting-and-ending-deadlocks"></a>Detectar y finalizar interbloqueos  
 Un interbloqueo se produce cuando dos o más tareas se bloquean entre sí permanentemente teniendo cada tarea un bloqueo en un recurso que las otras tareas intentan bloquear. En el siguiente gráfico se presenta una vista de alto nivel de un estado de interbloqueo donde:  
  
-   La tarea T1 tiene un bloqueo en el recurso R1 (indicado por la flecha de R1 a T1) y ha solicitado un bloqueo en el recurso R2 (indicado por la flecha de T1 a R2).  
-   La tarea T2 tiene un bloqueo en el recurso R2 (indicado por la flecha de R2 a T2) y ha solicitado un bloqueo en el recurso R1 (indicado por la flecha de T2 a R1).  
-   Dado que ninguna tarea puede continuar hasta que un recurso esté disponible y ningún recurso puede liberarse hasta que continúe una tarea, existe un estado de interbloqueo.  
  
 ![Diagrama en el que se muestran tareas en un estado de interbloqueo](../relational-databases/media/Task_Deadlock_State.png)  
  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] detecta automáticamente los ciclos de interbloqueo en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] elige una de las sesiones como sujeto del interbloqueo y la transacción actual finaliza con un error para romper el interbloqueo.  
  
#### <a name="resources-that-can-deadlock"></a><a name="deadlock_resources"></a> Recursos que pueden causar interbloqueos  
 Cada sesión de usuario puede tener una o más tareas en ejecución y cada tarea puede adquirir o esperar para adquirir una serie de recursos. Los siguientes tipos de recursos pueden causar bloqueos que podrían dar como resultado un interbloqueo.  
  
-   **Bloqueos**. Esperar para adquirir bloqueos en recursos, como objetos, páginas, filas, metadatos y aplicaciones, puede causar un interbloqueo. Por ejemplo, la transacción T1 tiene un bloqueo compartido (S) en la fila f1 y está esperando para obtener un bloqueo exclusivo (X) en f2. La transacción T2 tiene un bloqueo compartido (S) en f2 y está esperando para obtener un bloqueo exclusivo (X) en la fila f1. Esta situación tiene como resultado un ciclo de bloqueo en el que T1 y T2 esperan que la otra transacción libere los recursos bloqueados.  
  
-   **Subprocesos de trabajo**. Una tarea en cola que espera un subproceso de trabajo disponible puede causar un interbloqueo. Si la tarea en cola es propietaria de recursos que están bloqueando todos los subprocesos de trabajo, se generará un interbloqueo. Por ejemplo, la sesión S1 inicia una transacción y adquiere un bloqueo compartido (S) en la fila f1 y, a continuación, se suspende. Las sesiones activas que se ejecutan en todos los subprocesos de trabajo disponibles intentan adquirir bloqueos exclusivos (X) en la fila f1. Dado que la sesión S1 no puede adquirir un subproceso de trabajo, no puede confirmar la transacción y liberar el bloqueo de la fila f1. Esta situación tiene como resultado un interbloqueo.  
  
-   **Memoria**. Cuando hay solicitudes simultáneas esperando concesiones de memoria que no se pueden satisfacer con la memoria disponible, puede producirse un interbloqueo. Por ejemplo, dos consultas simultáneas, C1 y C2, se ejecutan como funciones definidas por el usuario que adquieren 10 MB y 20 MB de memoria respectivamente. Si cada consulta necesita 30 MB y el total de memoria disponible es 20 MB, C1 y C2 tienen que esperar a que la otra consulta libere memoria, y esta situación tiene como resultado un interbloqueo.  
  
-   **Recursos relacionados con la ejecución de consultas en paralelo**. Los subprocesos de coordinador, productor o consumidor asociados a un puerto de intercambio se pueden bloquear entre sí y provocar un interbloqueo si incluyen al menos otro proceso que no forma parte de la consulta en paralelo. Además, cuando se inicia la ejecución de una consulta en paralelo, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] determina el grado de paralelismo, o el número de subprocesos de trabajo, en función de la carga de trabajo actual. Si la carga de trabajo del sistema cambia de forma inesperada, por ejemplo, si se empiezan a ejecutar nuevas consultas en el servidor o el sistema se queda sin subprocesos de trabajo, se puede producir un interbloqueo.  
  
-   **Conjuntos de resultados activos múltiples (MARS)** . Estos recursos se utilizan para controlar la intercalación de varias solicitudes activas en MARS. Para obtener más información, vea [Utilizar conjuntos de resultados activos múltiples (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
    -   **Recurso de usuario**. Cuando un subproceso espera un recurso que potencialmente está controlado por una aplicación de usuario, se considera que el recurso es externo o de usuario y se trata como un bloqueo.  
  
    -   **Exclusión mutua de sesión**. Las tareas que se ejecutan en una sesión se intercalan, lo que significa que solo puede ejecutarse una tarea en la sesión en un momento dado. Antes de que se pueda ejecutar la tarea, debe tener acceso exclusivo a la exclusión mutua de sesión.  
  
    -   **Exclusión mutua de transacción**. Todas las tareas que se ejecutan en una transacción se intercalan, lo que significa que solo puede ejecutarse una tarea en la transacción en un momento dado. Antes de que se pueda ejecutar la tarea, debe tener acceso exclusivo a la exclusión mutua de transacción.  
  
     Para que una tarea se ejecute en MARS, debe adquirir la exclusión mutua de sesión. Si la tarea se ejecuta en una transacción, debe adquirir la exclusión mutua de transacción. Esto garantiza que solo una tarea esté activa en un momento dado en una sesión determinada y en una transacción concreta. Una vez adquiridas las exclusiones mutuas necesarias, se puede ejecutar la tarea. Cuando finaliza la tarea, o se produce en medio de la solicitud, primero liberará la exclusión mutua de transacción seguida de la exclusión mutua de sesión en el orden inverso a la adquisición. Sin embargo, pueden producirse interbloqueos con estos recursos. En el ejemplo de código siguiente hay dos tareas, la solicitud de usuario U1 y la solicitud de usuario U2, que se ejecutan en la misma sesión.  
  
    ```  
    U1:    Rs1=Command1.Execute("insert sometable EXEC usp_someproc");  
    U2:    Rs2=Command2.Execute("select colA from sometable");  
    ```  
  
     El procedimiento almacenado que se ejecuta a partir de la solicitud de usuario U1 ha adquirido la exclusión mutua de sesión. Si el procedimiento almacenado tarda mucho tiempo en ejecutarse, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] considerará que el procedimiento almacenado está esperando la intervención del usuario. La solicitud de usuario U2 está esperando la exclusión mutua de sesión mientras que el usuario está esperando el conjunto de resultados de U2, y U1 está esperando un recurso de usuario. Éste es un estado de interbloqueo que se ilustra de forma lógica como:  
  
 ![LogicFlowExamplec](../relational-databases/media/udb9_LogicFlowExamplec.png)  
  
### <a name="deadlock-detection"></a><a name="deadlock_detection"></a> Detección de interbloqueos  
 Todos los recursos enumerados en la sección anterior participan en el esquema de detección de interbloqueos del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. La detección de interbloqueos la realiza un subproceso de supervisión de bloqueos que periódicamente inicia una búsqueda por todas las tareas de una instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. En los siguientes puntos se describe el proceso de búsqueda:  
  
-   El intervalo predeterminado es de 5 segundos.  
-   Si el subproceso de supervisión de bloqueos encuentra interbloqueos, el intervalo de detección de interbloqueos pasará de 5 segundos a hasta solo 100 milisegundos, en función de la frecuencia de los interbloqueos.  
-   Si el subproceso de supervisión de bloqueos deja de encontrar interbloqueos, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] aumentará los intervalos entre las búsquedas a 5 segundos.  
-   Si se acaba de detectar un interbloqueo, se considera que los siguientes subprocesos que deben esperar un bloqueo entran en el ciclo de interbloqueo. La primera pareja de esperas de bloqueo después de que se haya detectado un interbloqueo desencadenará inmediatamente una búsqueda de interbloqueos, en vez de esperar al siguiente intervalo de detección de interbloqueos. Por ejemplo, si el intervalo actual es de 5 segundos y se acaba de detectar un interbloqueo, la siguiente espera de bloqueo activará inmediatamente el detector de interbloqueos. Si esta espera de bloqueo forma parte de un interbloqueo, se detectará en seguida en lugar de durante la siguiente búsqueda de interbloqueos.  
  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] solo suele realizar detecciones de interbloqueos periódicas. Dado que el número de interbloqueos que se encuentran en el sistema suele ser pequeño, si se detectan periódicamente, el sistema no se ve sobrecargado por este tipo de detecciones.  
  
 Cuando el monitor de bloqueos inicia una búsqueda de interbloqueos para un subproceso determinado, identifica el recurso que está esperando. Después, el monitor de bloqueos encuentra al propietario o a los propietarios de ese recurso y continúa recursivamente la búsqueda de interbloqueos para esos subprocesos hasta que encuentra un ciclo. Un ciclo que se identifica de esta manera forma un interbloqueo.  
  
 Una vez detectado un interbloqueo, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] finaliza un interbloqueo eligiendo uno de los subprocesos como sujeto del interbloqueo. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] finaliza el lote actual que se está ejecutando para el subproceso, revierte la transacción del sujeto del interbloqueo y devuelve un error 1205 a la aplicación. Revertir la transacción para el sujeto del interbloqueo libera todos los bloqueos que tiene la transacción. Esto permite que las transacciones de otros subprocesos se desbloqueen y continúen. El error 1205 del sujeto del interbloqueo registra información sobre los subprocesos y recursos implicados en un interbloqueo en el registro de errores.  
  
 De forma predeterminada, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] elige como sujeto del interbloqueo la sesión que ejecuta la transacción cuya reversión resulta menos costosa. Como alternativa, un usuario puede especificar la prioridad de las sesiones en una situación de interbloqueo mediante la instrucción `SET DEADLOCK_PRIORITY`. DEADLOCK_PRIORITY puede establecerse como LOW, NORMAL o HIGH; también puede establecerse como un valor entero en el intervalo de -10 a 10. El valor predeterminado de la prioridad de interbloqueo es NORMAL. Si dos sesiones tienen distintas prioridades de interbloqueo, la sesión con la prioridad menor se elige como el sujeto del interbloqueo. Si ambas sesiones tienen la misma prioridad de interbloqueo, se elige la sesión con la transacción cuya reversión resulta menos costosa. Si las sesiones implicadas en el ciclo de interbloqueo tienen la misma prioridad de interbloqueo y el mismo costo, se elige un sujeto de forma aleatoria.  
  
 Cuando se trabaja con CLR, el monitor de interbloqueos detecta automáticamente el interbloqueo de los recursos de sincronización (monitores, bloqueo de lectura y escritura, y combinación de subprocesos) a los que se ha tenido acceso dentro de los procedimientos administrados. Si embargo, el interbloqueo se resuelve iniciando una excepción en el procedimiento que se seleccionó como sujeto del interbloqueo. Es importante comprender que la excepción no libera automáticamente los recursos que posee actualmente el sujeto; los recursos se tienen que liberar de forma explícita. De forma coherente con el comportamiento de la excepción, la excepción utilizada para identificar un sujeto del interbloqueo se puede interceptar y descartar.  
  
### <a name="deadlock-information-tools"></a><a name="deadlock_tools"></a> Herramientas de información de interbloqueos  
 Para ver la información de interbloqueos, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] proporciona herramientas de supervisión en la forma de la sesión system\_health de xEvent, dos marcas de seguimiento y el evento de grafo de interbloqueo en SQL Profiler.  

#### <a name="deadlock-extended-event"></a><a name="deadlock_xevent"></a> Evento extendido de interbloqueo
A partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], se debe usar el evento extendido de `xml_deadlock_report` (xEvent) en lugar de la clase de eventos Deadlock Graph en Seguimiento de SQL o SQL Profiler.

También a partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], cuando se producen interbloqueos, la sesión ***system\_health** _ captura todos los xEvents `xml_deadlock_report` que contienen el grafo de interbloqueo. Como la sesión _system\_health* está habilitada de forma predeterminada, no es necesario configurar una sesión de xEvent independiente para capturar la información de interbloqueo. 

El grafo de interbloqueo que se capturaba normalmente consta de tres nodos distintos:
-   **victim-list**. Identificador de proceso del elemento afectado por el interbloqueo.
-   **process-list**. Información sobre todos los procesos implicados en el interbloqueo.
-   **resource-list**. Información sobre todos los recursos implicados en el interbloqueo.

Al abrir el archivo de la sesión system\_health o el búfer en anillo, si se registra el xEvent `xml_deadlock_report`, [!INCLUDE[ssManStudio](../includes/ssManStudio-md.md)] presenta una representación gráfica de las tareas y recursos que participan en un interbloqueo, tal como se muestra en el ejemplo siguiente: 

![Grafo de interbloqueo de xEvent](../relational-databases/media/udb9_xEventDeadlockGraphc.png)

La consulta siguiente puede ver todos los eventos de interbloqueo capturados por el búfer en anillo de la sesión *system\_health*:

```sql
SELECT xdr.value('@timestamp', 'datetime') AS [Date],
    xdr.query('.') AS [Event_Data]
FROM (SELECT CAST([target_data] AS XML) AS Target_Data
            FROM sys.dm_xe_session_targets AS xt
            INNER JOIN sys.dm_xe_sessions AS xs ON xs.address = xt.event_session_address
            WHERE xs.name = N'system_health'
              AND xt.target_name = N'ring_buffer'
    ) AS XML_Data
CROSS APPLY Target_Data.nodes('RingBufferTarget/event[@name="xml_deadlock_report"]') AS XEventData(xdr)
ORDER BY [Date] DESC
```

[!INCLUDE[ssResult](../includes/ssresult-md.md)]

![system_health_xevent_query_result](../relational-databases/media/system_health_qry.png)

En el ejemplo siguiente se muestra el resultado, después de hacer clic en el primer vínculo del resultado anterior:

```xml
<event name="xml_deadlock_report" package="sqlserver" timestamp="2018-02-18T08:26:24.698Z">
  <data name="xml_report">
    <type name="xml" package="package0" />
    <value>
      <deadlock>
        <victim-list>
          <victimProcess id="process27b9b0b9848" />
        </victim-list>
        <process-list>
          <process id="process27b9b0b9848" taskpriority="0" logused="0" waitresource="KEY: 5:72057594214350848 (1a39e6095155)" waittime="1631" ownerId="11088595" transactionname="SELECT" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27b9f79fac0" lockMode="S" schedulerid="9" kpid="15336" status="suspended" spid="62" sbid="0" ecid="0" priority="0" trancount="0" lastbatchstarted="2018-02-18T00:26:22.893" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="7908" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088595" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p1" line="3" stmtstart="78" stmtend="180" sqlhandle="0x0300050020766505ca3e07008ba8000001000000000000000000000000000000000000000000000000000000">
SELECT c2, c3 FROM t1 WHERE c2 BETWEEN @p1 AND @p1+    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x020000006263ec01ebb919c335024a072a2699958d3fcce60000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p1 4
END
   </inputbuf>
          </process>
          <process id="process27b9ee33c28" taskpriority="0" logused="252" waitresource="KEY: 5:72057594214416384 (e5b3d7e750dd)" waittime="1631" ownerId="11088593" transactionname="UPDATE" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27ba15a4490" lockMode="X" schedulerid="6" kpid="5584" status="suspended" spid="58" sbid="0" ecid="0" priority="0" trancount="2" lastbatchstarted="2018-02-18T00:26:22.890" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="15316" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088593" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p2" line="3" stmtstart="76" stmtend="150" sqlhandle="0x03000500599a5906ce3e07008ba8000001000000000000000000000000000000000000000000000000000000">
UPDATE t1 SET c2 = c2+1 WHERE c1 = @p    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x02000000008fe521e5fb1099410048c5743ff7da04b2047b0000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p2 4
END
   </inputbuf>
          </process>
        </process-list>
        <resource-list>
          <keylock hobtid="72057594214350848" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="cidx" id="lock27b9dd26a00" mode="X" associatedObjectId="72057594214350848">
            <owner-list>
              <owner id="process27b9ee33c28" mode="X" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9b0b9848" mode="S" requestType="wait" />
            </waiter-list>
          </keylock>
          <keylock hobtid="72057594214416384" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="idx1" id="lock27afa392600" mode="S" associatedObjectId="72057594214416384">
            <owner-list>
              <owner id="process27b9b0b9848" mode="S" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9ee33c28" mode="X" requestType="wait" />
            </waiter-list>
          </keylock>
        </resource-list>
      </deadlock>
    </value>
  </data>
</event>
```

Para obtener más información, vea [Usar la sesión system_health](../relational-databases/extended-events/use-the-system-health-session.md).

#### <a name="trace-flag-1204-and-trace-flag-1222"></a><a name="deadlock_traceflags"></a> Marcas de seguimiento 1204 y 1222  
 Cuando se produce el interbloqueo, los marcadores de seguimiento 1204 y 1222 devuelven la información que se ha capturado en el registro de errores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. El marcador de seguimiento 1204 informa sobre el interbloqueo con un formato que especifica cada nodo implicado en el mismo. El marcador de seguimiento 1222 aplica formato a la información de interbloqueo, primero por procesos y luego por recursos. Es posible habilitar ambas marcas de seguimiento para obtener dos representaciones del mismo evento de interbloqueo.  

> [!IMPORTANT]
> Evite utilizar la marca de seguimiento 1204 y 1222 en sistemas con un uso intensivo de cargas de trabajo que causan interbloqueos. El uso de estas marcas de seguimiento puede presentar problemas de rendimiento. En su lugar, use el evento de interbloqueo extendido (#deadlock_xevent).
  
 Además de definir las propiedades de las marcas de seguimiento 1204 y 1222, en la siguiente tabla se muestran las similitudes y las diferencias.  
  
|Propiedad|Marcas de seguimiento 1204 y 1222|Solo marca de seguimiento 1204|Solo marca de seguimiento 1222|  
|--------------|-----------------------------------------|--------------------------|--------------------------|  
|Formato de salida|Los resultados se capturan en el registro de errores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|Se centra en los nodos implicados en el interbloqueo. Cada nodo tiene una sección dedicada y la última sección describe al sujeto del interbloqueo.|Devuelve información en un formato XML que no se ajusta a un esquema de definición de esquemas XML (XSD). El formato tiene tres secciones principales. La primera sección declara el sujeto del interbloqueo. La segunda sección describe los procesos implicados en el interbloqueo. La tercera sección describe los recursos que son sinónimos de nodos en la marca de seguimiento 1204.|  
|Atributos de identificación|**SPID:<x\> ECID:<x\>.** Identifica el subproceso del identificador de proceso del sistema en los casos de procesos paralelos. La entrada `SPID:<x> ECID:0`, en que <x\> se sustituye por el valor del SPID, representa el subproceso principal. La entrada `SPID:<x> ECID:<y>`, en que <x\> se sustituye por el valor del SPID e <y\> es mayor que 0, representa los subprocesos secundarios del mismo SPID.<br /><br /> **BatchID** (**sbid** para la marca de seguimiento 1222). Identifica el lote desde el que la ejecución del código está solicitando o manteniendo un bloqueo. Cuando se deshabilita Multiple Active Result Sets (MARTE), el valor de BatchID es 0. Cuando se habilita MART, el valor para los lotes activos es 1 para *n*. Si en la sesión no hay lotes activos, BatchID es 0.<br /><br /> **Mode**. Especifica el tipo de bloqueo de un recurso en concreto que un subproceso solicita, concede o espera. Mode puede ser IS (Intención compartida), S (Compartido), U (Actualizar), IX (Intención exclusiva), SIX (Intención compartida exclusiva) y X (Exclusiva).<br /><br /> **Line #** (**line** para la marca de seguimiento 1222). Indica el número de línea en el lote actual de instrucciones que se estaba ejecutando cuando se produjo el interbloqueo.<br /><br /> **Input Buf** (**inputbuf** para la marca de seguimiento 1222). Indica todas las instrucciones del lote actual.|**Node**. Representa el numero de entrada en la cadena de interbloqueo.<br /><br /> **Lists**. El propietario del bloqueo puede formar parte de estas listas:<br /><br /> **Grant List**. Enumera los propietarios actuales del recurso.<br /><br /> **Convert List**. Enumera los propietarios actuales que están intentando convertir sus bloqueos a un nivel más alto.<br /><br /> **Wait List**. Enumera las solicitudes actuales del nuevo bloqueo para el recurso.<br /><br /> **Statement Type**. Describe el tipo de instrucción DML (SELECT, INSERT, UPDATE o DELETE) en que los subprocesos tienen permisos.<br /><br /> **Victim Resource Owner**. Especifica el subproceso participante que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] elige como sujeto para interrumpir el ciclo de interbloqueo. El subproceso elegido y todos los subprocesos secundarios finalizan.<br /><br /> **Next Branch**. Representa los dos o más subprocesos secundarios del mismo SPID que están implicados en el ciclo de interbloqueo.|**deadlock victim**. Representa la dirección de la memoria física de la tarea (vea [sys.dm_os_tasks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)) que se seleccionó como sujeto del interbloqueo. Puede ser 0 (cero) en caso de un interbloqueo no resuelto. Una tarea que se está revirtiendo no se puede seleccionar como sujeto del interbloqueo.<br /><br /> **executionstack**. Representa el código [!INCLUDE[tsql](../includes/tsql-md.md)] que se está ejecutando en el momento en que se produce el interbloqueo.<br /><br /> **priority**. Representa la prioridad de interbloqueo. En ciertos casos, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] puede optar por modificar la prioridad de interbloqueo durante una breve duración para conseguir una mejor simultaneidad.<br /><br /> **logused**. Espacio de registro utilizado por la tarea.<br /><br /> **owner id**. El Id. de la transacción que tiene el control de la solicitud.<br /><br /> **status**. Estado de la tarea. Es uno de los siguientes valores:<br /><br /> >> **pending**. Esperando un subproceso de trabajo.<br /><br /> >> **runnable**. Preparado para ejecutarse pero esperando un cuanto.<br /><br /> >> **running**. Ejecutándose actualmente en el programador.<br /><br /> >> **suspended**. La ejecución se ha suspendido.<br /><br /> >> **done**. La tarea se ha completado.<br /><br /> >> **spinloop**. Esperando que un bloqueo por bucle esté disponible.<br /><br /> **waitresource**. El recurso que la tarea necesita.<br /><br /> **waittime**. Tiempo en milisegundos de espera del recurso.<br /><br /> **schedulerid**. Programador asociado a esta tarea. Consulte [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).<br /><br /> **hostname**. El nombre de la estación de trabajo.<br /><br /> **isolationlevel**. El nivel de aislamiento de transacción actual.<br /><br /> **Xactid**. El Id. de la transacción que tiene el control de la solicitud.<br /><br /> **currentdb**. El Id. de la base de datos.<br /><br /> **lastbatchstarted**. La última vez que un proceso de cliente inició la ejecución de lotes.<br /><br /> **lastbatchcompleted**. La última vez que un proceso de cliente completó la ejecución de lotes.<br /><br /> **clientoption1 y clientoption2**. Opciones establecidas en esta conexión de cliente. Se trata de una máscara de bits que incluye información acerca de las opciones controladas normalmente por instrucciones SET, como SET NOCOUNT y SET XACTABORT.<br /><br /> **associatedObjectId**. Representa el Id. del árbol b o montón (HoBT).|  
|Atributos del recurso|**RID**. Identifica la única fila de una tabla en la que se mantiene o se solicita un bloqueo. RID se representa como RID: *db_id:file_id:page_no:row_no*. Por ejemplo, `RID: 6:1:20789:0`.<br /><br /> **OBJECT**. Identifica la tabla en la que se mantiene o se solicita un bloqueo. OBJECT se representa como OBJECT: *db_id:object_id*. Por ejemplo, `TAB: 6:2009058193`.<br /><br /> **KEY**. Identifica el intervalo de clave de un índice en el que se mantiene o se solicita un bloqueo. KEY se representa como KEY: *db_id:hobt_id* (*valor de hash de clave de índice*). Por ejemplo, `KEY: 6:72057594057457664 (350007a4d329)`.<br /><br /> **PAG**. Identifica el recurso de página en el que se mantiene o se solicita un bloqueo. PAG se representa como PAG: *db_id:file_id:page_no*. Por ejemplo, `PAG: 6:1:20789`.<br /><br /> **EXT**. Identifica la estructura de extensión. EXT se representa como EXT: *db_id:file_id:extent_no*. Por ejemplo, `EXT: 6:1:9`.<br /><br /> **DB**. Identifica el bloqueo de la base de datos. **DB se representa de una de las siguientes maneras:**<br /><br /> DB: *db_id*<br /><br /> DB: *db_id*[BULK-OP-DB], que identifica el bloqueo de base de datos realizado por la copia de seguridad de la base de datos.<br /><br /> DB: *db_id*[BULK-OP-LOG], que identifica el bloqueo realizado por el registro de copia de seguridad de esa base de datos en concreto.<br /><br /> **APP**. Identifica el bloqueo realizado por un recurso de la aplicación. Se representa como APP: *lock_resource*. Por ejemplo, `APP: Formf370f478`.<br /><br /> **METADATA**. Representa los recursos de metadatos implicados en un interbloqueo. Debido a que METADATA tiene muchos recursos secundarios, el valor devuelto depende del recurso secundario que se haya interbloqueado. Por ejemplo, METADATA.USER_TYPE devuelve `user_type_id =` <*integer_value*>. Para obtener más información acerca de los recursos y recursos secundarios de METADATA, vea [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).<br /><br /> **HOBT**. Representa al montón o árbol b implicados en un interbloqueo.|Nada exclusivo de esta marca de seguimiento.|Nada exclusivo de esta marca de seguimiento.|  
  
##### <a name="trace-flag-1204-example"></a>Ejemplo de marca de seguimiento 1204  
 En el siguiente ejemplo se muestra el resultado que se obtiene cuando se activa una marca de seguimiento 1204. En este caso, la tabla de Node 1 es un montón sin índices, y la tabla de Node 2 es un montón con un índice no clúster. La clave de índice de Node 2 se está actualizando cuando se produce el interbloqueo.  
  
```  
Deadlock encountered .... Printing deadlock information  
Wait-for graph  
  
Node:1  
  
RID: 6:1:20789:0               CleanCnt:3 Mode:X Flags: 0x2  
 Grant List 0:  
   Owner:0x0315D6A0 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:55 ECID:0 XactLockInfo: 0x04D9E27C  
   SPID: 55 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
BEGIN TRANSACTION  
   EXEC usp_p2  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x03A3DAD0   
     Mode: U SPID:54 BatchID:0 ECID:0 TaskProxy:(0x04976374) Value:0x315d200 Cost:(0/868)  
  
Node:2  
  
KEY: 6:72057594057457664 (350007a4d329) CleanCnt:2 Mode:X Flags: 0x0  
 Grant List 0:  
   Owner:0x0315D140 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:54 ECID:0 XactLockInfo: 0x03A3DAF4  
   SPID: 54 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
     BEGIN TRANSACTION  
       EXEC usp_p1  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
  
Victim Resource Owner:  
 ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
```  
  
##### <a name="trace-flag-1222-example"></a>Ejemplo de marca de seguimiento 1222  
 En el siguiente ejemplo se muestra el resultado que se obtiene cuando se activa una marca de seguimiento 1222. En este caso, una tabla es un montón sin índices y la otra tabla es un montón con un índice no clúster. En la segunda tabla, la clave de índice se está actualizando cuando se produce el interbloqueo.  
  
```  
deadlock-list  
 deadlock victim=process689978  
  process-list  
   process id=process6891f8 taskpriority=0 logused=868   
   waitresource=RID: 6:1:20789:0 waittime=1359 ownerId=310444   
   transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:42.733 XDES=0x3a3dad0   
   lockMode=U schedulerid=1 kpid=1952 status=suspended spid=54   
   sbid=0 ecid=0 priority=0 transcount=2   
   lastbatchstarted=2005-09-05T11:22:42.733   
   lastbatchcompleted=2005-09-05T11:22:42.733   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310444 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p1 line=6 stmtstart=202   
     sqlhandle=0x0300060013e6446b027cbb00c69600000100000000000000  
     UPDATE T2 SET COL1 = 3 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600856aa70f503b8104000000000000000000000000  
     EXEC usp_p1       
    inputbuf  
      BEGIN TRANSACTION  
       EXEC usp_p1  
   process id=process689978 taskpriority=0 logused=380   
   waitresource=KEY: 6:72057594057457664 (350007a4d329)     
   waittime=5015 ownerId=310462 transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:44.077 XDES=0x4d9e258 lockMode=U   
   schedulerid=1 kpid=3024 status=suspended spid=55 sbid=0 ecid=0   
   priority=0 transcount=2 lastbatchstarted=2005-09-05T11:22:44.077   
   lastbatchcompleted=2005-09-05T11:22:44.077   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310462 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p2 line=6 stmtstart=200   
     sqlhandle=0x030006004c0a396c027cbb00c69600000100000000000000  
     UPDATE T1 SET COL1 = 4 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600d688e709b85f8904000000000000000000000000  
     EXEC usp_p2       
    inputbuf  
      BEGIN TRANSACTION  
        EXEC usp_p2      
  resource-list  
   ridlock fileid=1 pageid=20789 dbid=6 objectname=AdventureWorks2016.dbo.T2   
   id=lock3136940 mode=X associatedObjectId=72057594057392128  
    owner-list  
     owner id=process689978 mode=X  
    waiter-list  
     waiter id=process6891f8 mode=U requestType=wait  
   keylock hobtid=72057594057457664 dbid=6 objectname=AdventureWorks2016.dbo.T1   
   indexname=nci_T1_COL1 id=lock3136fc0 mode=X   
   associatedObjectId=72057594057457664  
    owner-list  
     owner id=process6891f8 mode=X  
    waiter-list  
     waiter id=process689978 mode=U requestType=wait  
```  
  
#### <a name="profiler-deadlock-graph-event"></a>Evento Deadlock Graph del Analizador  
Este es un evento de SQL Profiler que presenta una descripción gráfica de las tareas y los recursos implicados en un interbloqueo. En el siguiente ejemplo se muestra el resultado de SQL Profiler cuando se ha activado el evento del grafo de interbloqueo.  
  
 ![ProfilerDeadlockGraphc](../relational-databases/media/udb9_ProfilerDeadlockGraphc.png)  
  
Para obtener más información sobre el evento de interbloqueo, consulte [Lock:Deadlock Event Class](../relational-databases/event-classes/lock-deadlock-event-class.md) (Clase de evento Lock:Deadlock).

Para obtener más información sobre cómo ejecutar el grafo de interbloqueo de SQL Profiler, vea [Guardar grafos de interbloqueo &#40;SQL Server Profiler&#41;](../relational-databases/performance/save-deadlock-graphs-sql-server-profiler.md).  
  
### <a name="handling-deadlocks"></a>Controlar interbloqueos  
 Cuando una instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] elige una transacción como elemento afectado por un interbloqueo, finaliza el lote actual, revierte la transacción y devuelve el mensaje de error 1205 a la aplicación.  
  
 `Your transaction (process ID #52) was deadlocked on {lock | communication buffer | thread} resources with another process and has been chosen as the deadlock victim. Rerun your transaction.`  
  
 Dado que cualquier aplicación que envía consultas [!INCLUDE[tsql](../includes/tsql-md.md)] puede elegirse como sujeto de un interbloqueo, las aplicaciones deben tener un controlador de errores que pueda interceptar el mensaje de error 1205. Si una aplicación no intercepta el error, puede continuar sin ser consciente de que se ha revertido la transacción y de que se pueden producir errores.  
  
 La implementación de un controlador de errores que intercepte el mensaje 1205 permite a una aplicación controlar la situación de interbloqueo y realizar una acción apropiada para solucionarla, por ejemplo, volver a enviar automáticamente la consulta implicada en el interbloqueo. Si se vuelve a enviar la consulta de forma automática, no es necesario que el usuario sepa que se ha producido un interbloqueo.  
  
 La aplicación debería realizar una pausa breve antes de volver a enviar su consulta. Esto ofrece a la otra transacción implicada en el interbloqueo una oportunidad de completarse y liberar sus bloqueos que formaban parte del ciclo de interbloqueo. Así se minimiza la probabilidad de que el interbloqueo vuelva a ocurrir cuando la consulta que se ha vuelto a enviar solicite sus bloqueos.  
  
### <a name="minimizing-deadlocks"></a><a name="deadlock_minimizing"></a> Minimizar los interbloqueos  
 A pesar de que los interbloqueos no se pueden evitar totalmente, si se siguen ciertas convenciones de codificación se puede reducir su generación. La minimización de los interbloqueos puede aumentar el rendimiento de las transacciones y reducir la sobrecarga del sistema, debido a que:  
  
-   Se revierten menos transacciones, al deshacer todo el trabajo que realiza la transacción.  
-   Las aplicaciones vuelven a enviar menos transacciones debido a que se revirtieron cuando se produjo el interbloqueo.  
  
 Para ayudar a reducir los interbloqueos:  
  
-   Obtenga acceso a los objetos en el mismo orden.  
-   Evite la interacción con los usuarios en las transacciones.  
-   Mantenga transacciones cortas y en un proceso por lotes.  
-   Utilice un nivel de aislamiento inferior.  
-   Utilice un nivel de aislamiento basado en versiones de fila.  
    -   Establezca la opción de base de datos `READ_COMMITTED_SNAPSHOT` en ON para habilitar las transacciones de lectura confirmada y que usen las versiones de fila.  
    -   Utilice el aislamiento de instantánea.  
-   Utilice conexiones enlazadas.  
  
#### <a name="access-objects-in-the-same-order"></a>Acceder a los objetos en el mismo orden  
 Si todas las transacciones simultáneas tienen acceso a los objetos en el mismo orden, es menos probable que se produzcan interbloqueos. Por ejemplo, si dos transacciones simultáneas obtienen un bloqueo en la tabla **Supplier** y después en la tabla **Part**, una transacción se bloquea en la tabla **Supplier** hasta que finalice la otra transacción. Una vez confirmada o revertida la primera transacción, continúa la segunda, por lo que no se produce un interbloqueo. La utilización de procedimientos almacenados para todas las modificaciones de datos puede normalizar el orden de acceso a los objetos.  
  
 ![deadlock2](../relational-databases/media/dedlck2.png)  
  
#### <a name="avoid-user-interaction-in-transactions"></a>Evitar la interacción con los usuarios en las transacciones  
 Evite escribir transacciones que incluyan la intervención del usuario, ya que la velocidad de ejecución de los lotes que no requieren esta intervención es mucho mayor que la velocidad con la que el usuario debe responder manualmente a las consultas como, por ejemplo, contestar a la solicitud de un parámetro por parte de una aplicación. Por ejemplo, si una transacción espera una entrada del usuario y éste sale a comer o no vuelve hasta pasado el fin de semana, dicho usuario retrasa la finalización de la transacción. De esta forma, se degrada el rendimiento del sistema, ya que los bloqueos que mantiene la transacción solo se liberan cuando se confirma o se revierte la transacción. Aunque no surja una situación de interbloqueo, las demás transacciones que obtienen acceso a los mismos recursos se bloquean mientras esperan a que la transacción finalice.  
  
#### <a name="keep-transactions-short-and-in-one-batch"></a>Mantener transacciones cortas y en un proceso por lotes  
 Normalmente, los interbloqueos se producen cuando varias transacciones de larga duración se ejecutan simultáneamente en la misma base de datos. Cuanto más dure la transacción, más tiempo se mantendrán los bloqueos exclusivos o de actualización, con lo cual se bloquean otras actividades y se originan posibles situaciones de interbloqueo.  
  
 Al mantener las transacciones en un proceso por lotes, se minimizan los viajes de ida y vuelta en la red durante una transacción y se reducen los posibles retrasos al completar la transacción y liberar los bloqueos.  
  
#### <a name="use-a-lower-isolation-level"></a>Utilizar un nivel de aislamiento inferior  
 Determine si una transacción se puede ejecutar con un nivel de aislamiento inferior. Al implementar la lectura confirmada, se permite a una transacción leer los datos previamente leídos (no modificados) por otra transacción, sin tener que esperar a que la primera transacción finalice. Utilizar un nivel inferior de aislamiento, como la lectura confirmada, mantiene los bloqueos compartidos durante menos tiempo que un nivel superior de aislamiento, como el nivel serializable. De esta forma, se reduce la contención de bloqueos.  
  
#### <a name="use-a-row-versioning-based-isolation-level"></a>Utilizar un nivel de aislamiento basado en versiones de fila  
 Si la opción de base de datos `READ_COMMITTED_SNAPSHOT` se ha establecido en ON, la transacción que se ejecuta con el nivel de aislamiento de lectura confirmada utiliza las versiones de fila en lugar de bloqueos compartidos durante las operaciones de lectura.  
  
> [!NOTE]  
> Algunas aplicaciones dependen del comportamiento de los bloqueos en el aislamiento de lectura confirmada. En estas aplicaciones, es preciso efectuar algunos cambios antes de habilitar esta opción.  
  
 El aislamiento de instantánea también utiliza las versiones de fila, que no emplean bloqueos compartidos en las operaciones de lectura. Antes de ejecutar una transacción con aislamiento de instantánea, debe establecerse en ON la opción de base de datos `ALLOW_SNAPSHOT_ISOLATION`.  
  
 Implemente estos niveles de aislamiento para reducir los interbloqueos que se pueden producir entre operaciones de lectura y escritura.  
  
#### <a name="use-bound-connections"></a>Utilizar conexiones enlazadas  
 Al utilizar conexiones enlazadas, dos o más conexiones abiertas por la misma aplicación pueden cooperar entre sí. Los bloqueos adquiridos por las conexiones secundarias se mantienen como si los adquiriera la conexión principal y viceversa. Por lo tanto, no se bloquean entre sí.  
  
## <a name="lock-partitioning"></a><a name="lock_partitioning"></a> Partición de bloqueos  
 Para los grandes sistemas, los bloqueos en los objetos a los que se hace referencia asiduamente pueden convertirse en un cuello de botella para el rendimiento, puesto que la adquisición y liberación de los bloqueos genera contención en los recursos de bloqueo internos. La partición de bloqueos mejora el rendimiento porque divide un solo recurso de bloqueo entre varios recursos de bloqueo más. Esta característica solo está disponible para los sistemas con 16 o más CPU, se habilita automáticamente y no se puede deshabilitar. Solo los bloqueos de objetos pueden tener particiones. Los bloqueos de objetos que tengan un subtipo no pueden tener particiones. Para obtener más información, vea [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="understanding-lock-partitioning"></a>Descripción de partición de bloqueos  
 Las tareas de bloqueo obtienen acceso a varios recursos compartidos, dos de los cuales se optimizan mediante la partición de bloqueos:  
  
-   **Spinlock**. Controla el acceso a un recuso de bloqueo, como una fila o una tabla.  
  
     Sin la partición de bloqueos, un spinlock administra todas las solicitudes de bloqueo para un solo recurso de bloqueo. En los sistemas con un gran volumen de actividad, puede producirse contención a medida que las solicitudes de bloqueo esperan a que un spinlock esté disponible. En esta situación, la adquisición de bloqueos puede generar un cuello de botella que puede afectar negativamente al rendimiento.  
  
     Para reducir la contención en un solo recurso de bloqueo, la partición de bloqueos divide un recurso de bloqueo en varios recursos de bloqueo para repartir la carga entre varios spinlock.  
  
-   **Memoria**. Se utiliza para almacenar las estructuras de los recursos de bloqueo.  
  
     Cuando ya se ha adquirido el spinlock, las estructuras de bloqueo se almacenan en memoria para que, a continuación, estén disponibles para el acceso y realizar modificaciones. La distribución del acceso a los bloqueos entre varios recursos ayuda a eliminar la necesidad de transferir bloqueos de memoria entre CPU, lo que ayuda a mejorar el rendimiento.  
  
### <a name="implementing-and-monitoring-lock-partitioning"></a>Implementar y supervisar las particiones de bloqueos  
 La partición de bloqueos está activada de forma predeterminada para los sistemas con 16 CPU o más. Cuando la partición de bloqueos está habilitada, se registra un mensaje informativo en el registro de errores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Al adquirir bloqueos en un recurso con particiones:  
  
-   Solo los modos de bloqueo NL, SCH-S, IS, IU e IX se adquieren en una sola partición.  
  
-   Los bloqueos compartidos (S), exclusivos (X) y otros bloqueos en modos que no sean NL, SCH-S, IS, IU e IX deben adquirirse en todas las particiones empezando por el Id. de partición 0 seguido del resto de Id. en orden. Estos bloqueos en un recurso con particiones utilizarán más memoria que los bloqueos del mismo modo en un recurso sin particiones puesto que cada partición es de hecho un bloqueo independiente. El número de particiones determina los incrementos de memoria. Los contadores de bloqueo de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en el Monitor de rendimiento de Windows mostrarán información acerca de la memoria utilizada por los bloqueos con y sin particiones.  
  
 Una transacción se asigna a una partición cuando se inicia la transacción. Para la transacción, todas las solicitudes de bloqueo que pueden dividirse utilizan la partición asignada a esa transacción. Con este método, el acceso por parte de diferentes transacciones a los recursos de bloqueo del mismo objeto se distribuye a través de diferentes particiones.  
  
 La columna `resource_lock_partition` de la vista de administración dinámica `sys.dm_tran_locks` proporciona el identificador de la partición de bloqueo para un recurso con particiones de bloqueo. Para obtener más información, vea [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="working-with-lock-partitioning"></a>Trabajar con la partición de bloqueos  
 En los siguientes ejemplos de código se muestra la partición de bloqueos. En estos ejemplos se ejecutan dos transacciones en dos sesiones diferentes para mostrar el comportamiento de la partición de bloqueos en sistemas grandes con 16 CPU.  
  
 Estas instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] crean objetos de prueba que se utilizan en los siguientes ejemplos.  
  
```sql  
-- Create a test table.  
CREATE TABLE TestTable  (col1 int);  
GO  
  
-- Create a clustered index on the table.  
CREATE CLUSTERED INDEX ci_TestTable   
    ON TestTable (col1);  
GO  
  
-- Populate the table.  
INSERT INTO TestTable VALUES (1);  
GO  
```  
  
#### <a name="example-a"></a>Ejemplo A  
 Sesión 1:  
  
 Una instrucción `SELECT` se ejecuta en una transacción. Debido a la sugerencia de bloqueo `HOLDLOCK`, esta instrucción adquirirá y retendrá un bloqueo Intención compartida (IS) en una tabla (en esta ilustración, los bloqueos de fila y página se pasan por alto). El bloqueo IS solo se adquirirá en la partición asignada a la transacción. Para este ejemplo, se supone que el bloqueo IS se adquiere en el id. 7 de la partición.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Sesión 2:  
  
 Se inicia una transacción y la instrucción `SELECT` que se ejecuta bajo esta transacción adquirirá y retendrá un bloqueo compartido (S) en la tabla. El bloqueo S se adquirirá en todas las particiones que tengan como resultado varios bloqueos de tabla, uno para cada partición. Por ejemplo, en un sistema de 16 cpu, 16 bloqueos S se emitirán por el bloqueo en los id. 0-15 de la partición. Dado que el bloqueo S es compatible con el bloqueo IS que se retiene en el id. 7 de la partición por la transacción de la sesión 1, no hay ningún bloqueo entre las transacciones.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCK, HOLDLOCK);  
```  
  
 Sesión 1:  
  
 La siguiente instrucción `SELECT` se ejecuta bajo la transacción que todavía está activa bajo la sesión 1. Debido a la sugerencia de bloqueo de tabla (X) exclusiva, la transacción intentará adquirir un bloqueo X en la tabla. Sin embargo, el bloqueo S que retiene la transacción en la sesión 2 bloqueará el bloqueo X en el id. 0 de la partición.  
  
```sql  
SELECT col1  
FROM TestTable  
WITH (TABLOCKX);  
```  
  
#### <a name="example-b"></a>Ejemplo B  
 Sesión 1:  
  
 Una instrucción `SELECT` se ejecuta en una transacción. Debido a la sugerencia de bloqueo `HOLDLOCK`, esta instrucción adquirirá y retendrá un bloqueo Intención compartida (IS) en una tabla (en esta ilustración, los bloqueos de fila y página se pasan por alto). El bloqueo IS solo se adquirirá en la partición asignada a la transacción. Para este ejemplo, se supone que el bloqueo IS se adquiere en el id. 6 de la partición.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Sesión 2:  
  
 Una instrucción `SELECT` se ejecuta en una transacción. Debido a la sugerencia de bloqueo de `TABLOCKX`, la transacción intenta adquirir un bloqueo exclusivo (X) en la tabla. Recuerde que el bloqueo X se debe adquirir en todas las particiones comenzando en el id. 0 de la partición. El bloqueo X se adquirirá en los id. 0-5 de todas las particiones pero se bloqueará por el bloqueo IS adquirido por el id. 6 de la partición.  
  
 En los Id. 7 a 15 de la partición que el bloqueo X aún no ha alcanzado, otras transacciones puede seguir adquiriendo bloqueos.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCKX, HOLDLOCK);  
```   
  
##  <a name="row-versioning-based-isolation-levels-in-the-ssdenoversion"></a><a name="Row_versioning"></a> Niveles de aislamiento basados en versiones de fila en el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 A partir de [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ofrece una implementación de un nivel de aislamiento de transacciones existente, con confirmación de lectura, que proporciona una instantánea de nivel de instrucción con versiones de fila. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] también ofrece un nivel de aislamiento de transacciones, instantánea, que proporciona una instantánea de nivel de transacción que también usa versiones de fila.  
  
 Las versiones de fila es un marco general en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que invoca un mecanismo de copia por escritura cuando se modifica o elimina una fila. Esto requiere que, mientras se ejecuta la transacción, la versión anterior de la fila debe estar disponible para las transacciones que requieran un estado anterior transaccionalmente coherente. Las versiones de fila hacen lo siguiente:  
  
-   Crear las tablas **inserted** y **deleted** en desencadenadores. Se crean versiones de las filas modificadas por el desencadenador. Esto incluye las filas modificadas por la instrucción que activó el desencadenador, así como las modificaciones de datos realizadas por el desencadenador.  
-   Compatibilidad con los conjuntos de resultados activos múltiples (MARS). Si una sesión MARS emite una instrucción de modificación de datos (como `INSERT`, `UPDATE` o `DELETE`) en un momento en el que hay un conjunto de resultados activos, se crean versiones de las filas afectadas por la instrucción de modificación.  
-   Compatibilidad con las operaciones de índice que especifican la opción ONLINE.  
-   Compatibilidad con los niveles de aislamiento de transacción basados en versiones de fila:  
    -   Nueva implementación del nivel de aislamiento de lectura confirmada que utiliza las versiones de fila para proporcionar una coherencia de lectura en las instrucciones.  
    -   Nuevo nivel de aislamiento de instantánea que proporciona una coherencia de lectura en las transacciones.  
  
 La base de datos `tempdb` debe tener espacio suficiente para el almacén de versiones. Cuando `tempdb` esté llena, las operaciones de actualización dejarán de generar versiones y se continuarán funcionando correctamente, pero es posible que las operaciones de lectura provoquen errores porque es necesaria una determinada versión de fila que ya no existe. Esto afecta a las operaciones como los desencadenadores, MARS y los índices en línea.  
  
 La utilización de versiones de fila para las transacciones de lectura confirmada e instantáneas es un proceso de dos pasos:  
  
1.  Establezca una o dos de las opciones de base de datos (`READ_COMMITTED_SNAPSHOT` y `ALLOW_SNAPSHOT_ISOLATION`) en ON.  
2.  Seleccione el nivel de aislamiento de transacción apropiado en una aplicación:  

    -   Cuando el valor de la opción de base de datos `READ_COMMITTED_SNAPSHOT` sea ON, las transacciones que establezcan el nivel de aislamiento de lectura confirmada utilizarán las versiones de fila.  
    -   Cuando el valor de la opción de base de datos `ALLOW_SNAPSHOT_ISOLATION` sea ON, las transacciones podrán establecer el nivel de aislamiento de instantánea.  
  
 Cuando el valor de la opción de base de datos `READ_COMMITTED_SNAPSHOT` o `ALLOW_SNAPSHOT_ISOLATION` está establecido en ON, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] asignará un número de secuencia de la transacción (XSN) a cada transacción que manipule datos que utilicen las versiones de fila. Las transacciones empiezan en el momento en que se ejecuta una instrucción `BEGIN TRANSACTION`. No obstante, el número de secuencia de la transacción empieza con la primera operación de lectura/escritura después de la instrucción BEGIN TRANSACTION. El número de secuencia de la transacción aumenta en incrementos de uno cada vez que se asigna.  
  
 Cuando el valor de las opciones de base de datos `READ_COMMITTED_SNAPSHOT` o `ALLOW_SNAPSHOT_ISOLATION`, se mantienen las copias lógicas (versiones) para todas las modificaciones de datos realizadas en la base de datos. Cada vez que se modifica una fila mediante una transacción determinada, la instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] almacena una versión de la imagen previamente confirmada de la fila en `tempdb`. Cada versión se marca con el número de secuencia de la transacción que realizó el cambio. Las versiones de filas modificadas se encadenan mediante una lista de vínculos. El valor de fila más reciente se almacena siempre en la base de datos actual y se encadena a las filas de versiones almacenadas en `tempdb`.  
  
> [!NOTE]  
> En los casos de modificación de objetos grandes (LOB), solo se copia el fragmento cambiado al almacén de versiones de `tempdb`.  
  
 Las versiones de fila se conservan durante un tiempo suficiente para cumplir los requisitos de las transacciones ejecutadas con niveles de aislamiento basados en versiones de fila. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] realiza un seguimiento del número de secuencia de la transacción útil más antiguo y elimina periódicamente todas las versiones de filas marcadas con números de secuencia de la transacción anteriores al número de secuencia útil más antiguo.  
  
 Cuando el valor de ambas opciones de base de datos es OFF, solo se crean versiones de las filas modificadas por desencadenadores o sesiones MARS, o bien leídas por operaciones de índice ONLINE. Estas versiones de filas se liberan cuando dejan de ser necesarias. Un subproceso en segundo plano se ejecuta periódicamente para eliminar las versiones de filas obsoletas.  
  
> [!NOTE]  
> En el caso de las transacciones de ejecución breve, puede que se almacene en caché una versión de una fila modificada en el grupo de búferes sin que se escriba en los archivos de disco de la base de datos `tempdb`. Si la fila versionada no va a ser necesaria durante mucho tiempo, simplemente se eliminará del grupo de búferes y puede que no provoque una sobrecarga de E/S.  
  
### <a name="behavior-when-reading-data"></a>Comportamiento durante la lectura de datos  
 Cuando las transacciones que se ejecutan con niveles de aislamiento basados en versiones de fila leen datos, las operaciones de lectura no adquieren bloqueos compartidos (S) para los datos que se leen, por lo que no bloquean las transacciones que están modificando datos. Asimismo, se minimiza la sobrecarga de los recursos de bloqueo, ya que se reduce el número de bloqueos adquiridos. El aislamiento de lectura confirmada mediante las versiones de fila y el aislamiento de instantánea están diseñados para proporcionar una coherencia de lectura de datos con versiones en las instrucciones o las transacciones.  
  
 Todas las consultas, incluidas las transacciones que se ejecutan en niveles de aislamiento basados en versiones de fila, adquieren bloqueos de estabilidad del esquema (Sch-S) durante la compilación y la ejecución. Debido a ello, las consultas se bloquean cuando una transacción simultánea aloja un bloqueo de modificación del esquema (Sch-M) en la tabla. Por ejemplo, una operación de lenguaje de definición de datos (DDL) adquiere un bloqueo Sch-M antes de modificar la información del esquema de la tabla. Las transacciones de consulta, incluidas las que se ejecutan en un nivel de aislamiento basado en versiones de fila, se bloquean cuando se intenta adquirir un bloqueo Sch-S. A la inversa, una consulta que mantiene un bloqueo Sch-S bloquea una transacción simultánea que intenta adquirir un bloqueo Sch-M.  
  
 Cuando se inicia una transacción con el nivel de aislamiento de instantánea, la instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] registra todas las transacciones actualmente activas. Cuando la transacción de instantánea lee una fila que tiene una cadena de versiones, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sigue la cadena y recupera la fila en la que el número de secuencia de la transacción cumple las condiciones siguientes:  
  
-   Es el más cercano al número de secuencia de la transacción de instantánea que lee la fila, pero inferior al mismo.  
  
-   No se encuentra en la lista de transacciones activas cuando se inició la transacción de instantánea.  
  
 Las operaciones de lectura realizadas por una transacción de instantánea recuperan la versión más reciente de cada fila confirmada en el momento en el que empezó la transacción de instantánea. De este modo se consigue una instantánea coherente con las transacciones de los datos tal como existían en el momento de inicio de la transacción.  
  
 Las transacciones de lectura confirmada que utilizan versiones de fila funcionan de forma muy parecida. La diferencia es que las transacciones de lectura confirmada no utilizan su propio número de secuencia de la transacción cuando eligen versiones de filas. Cada vez que se inicia una instrucción, la transacción de lectura confirmada lee el número de secuencia de la transacción más reciente emitido para esa instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Éste es el número de secuencia de la transacción utilizado para seleccionar las versiones de filas correctas para esa instrucción. Esto permite a las transacciones de lectura confirmada ver una instantánea de los datos tal como existían en el momento de inicio de cada instrucción.  
  
> [!NOTE]  
> Aunque las transacciones de lectura confirmada que utilizan las versiones de fila proporcionan una vista de los datos en el nivel de instrucciones coherente desde el punto de vista de las transacciones, las versiones de fila que se generan o a las que se tiene acceso con este tipo de transacción se mantienen hasta que la transacción finaliza.  
  
### <a name="behavior-when-modifying-data"></a>Comportamiento durante la modificación de datos  
 En las transacciones de lectura confirmada que utilizan las versiones de fila, la selección de las filas que se deben actualizar se realiza mediante un recorrido de bloqueo en el que se obtiene un bloqueo de actualización (U) en la fila de datos cuando se leen los valores de datos. Es lo mismo que una transacción de lectura confirmada que no utiliza las versiones de fila. Si la fila de datos no cumple los criterios de actualización, se liberará el bloqueo de actualización en esa fila y se bloqueará y recorrerá la siguiente.  
  
 Las transacciones que se ejecutan con aislamiento de instantánea obtienen un enfoque optimista de la modificación de datos mediante la adquisición de bloqueos de datos antes de realizar la modificación solo para forzar restricciones. De lo contrario, los bloqueos no se adquieren en los datos hasta que se van a modificar los datos. Cuando una fila de datos cumple los criterios de actualización, la transacción de instantánea comprueba que la fila de datos no haya sido modificada por una transacción simultánea confirmada antes de que empezara la transacción de instantánea. Si se ha modificado la fila de datos fuera de la transacción de instantánea, se producirá un conflicto de actualizaciones y se finalizará la transacción de instantánea. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] controla el conflicto de actualizaciones; no se puede deshabilitar su detección.  
  
> [!NOTE]  
> Las operaciones de actualización que se ejecutan con aislamiento de instantánea se ejecutan internamente con aislamiento de lectura confirmada cuando la transacción de instantánea tiene acceso a cualquiera de los elementos siguientes:  
>  
> Una tabla con una restricción FOREIGN KEY.  
>  
> Una tabla a la que se hace referencia en la restricción FOREIGN KEY de otra tabla.  
>  
> Una vista indizada que hace referencia a más de una tabla.  
>  
> No obstante, incluso en estas condiciones, la operación de actualización seguirá comprobando que los datos no hayan sido modificados por otra transacción. Si se han modificado, la transacción de instantánea detectará un conflicto de actualización y terminará.  
  
### <a name="behavior-in-summary"></a>Resumen del comportamiento  
 En la tabla siguiente se resumen las diferencias entre el aislamiento de instantánea y el aislamiento de lectura confirmada mediante las versiones de fila.  
  
|Propiedad|Nivel de aislamiento de lectura confirmada mediante las versiones de fila|Nivel de aislamiento de instantánea|  
|--------------|----------------------------------------------------------|------------------------------|  
|La opción de base de datos cuyo valor debe ser ON para habilitar la compatibilidad necesaria.|READ_COMMITTED_SNAPSHOT|ALLOW_SNAPSHOT_ISOLATION|  
|Forma en la que una sesión solicita el tipo específico de versiones de fila.|Utilice el nivel de aislamiento de lectura confirmada predeterminado o ejecute la instrucción SET TRANSACTION ISOLATION LEVEL para especificar el nivel de aislamiento READ COMMITTED. Se puede hacer una vez iniciada la transacción.|Requiere la ejecución de SET TRANSACTION ISOLATION LEVEL para especificar el nivel de aislamiento SNAPSHOT antes de que se inicie otra transacción.|  
|La versión de los datos leídos por las instrucciones.|Todos los datos confirmados antes del inicio de cada instrucción.|Todos los datos confirmados antes del inicio de cada transacción.|  
|Modo de control de las actualizaciones.|Vuelve desde las versiones de filas a los datos reales para seleccionar las filas que se actualizarán y utiliza bloqueos de actualización en las filas de datos seleccionadas. Adquiere bloqueos exclusivos en las filas de datos reales que se modificarán. Sin detección de conflictos de actualizaciones.|Utiliza las versiones de filas para seleccionar las filas que se actualizarán. Intenta adquirir un bloqueo exclusivo en la fila de datos real que se modificará y, si otra transacción ha modificado los datos, se producirá un conflicto de actualizaciones y se finalizará la transacción de instantánea.|  
|Detección de conflictos de actualizaciones.|Ninguno.|Compatibilidad integrada. No se puede deshabilitar.|  
  
### <a name="row-versioning-resource-usage"></a>Uso de recursos de versiones de fila  
 El marco de las versiones de fila admite las siguientes características disponibles en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]:  
  
-   Desencadenadores  
-   Conjuntos de resultados activos múltiples (MARS)  
-   Índices en línea  
  
 El marco de las versiones de fila también admite los siguientes niveles de aislamiento de transacción basado en las versiones de fila que, de forma predeterminada, no se habilitan:  
  
-   Cuando la opción de base de datos `READ_COMMITTED_SNAPSHOT` es ON, las transacciones `READ_COMMITTED` proporcionan coherencia de lectura de nivel de instrucciones con versiones de fila.  
-   Cuando la opción de base de datos `ALLOW_SNAPSHOT_ISOLATION` es ON, las transacciones `SNAPSHOT` proporcionan coherencia de lectura de nivel de transacciones con versiones de fila.  
  
 Los niveles de aislamiento basado en las versiones de fila reducen el número de bloqueos adquiridos por transacción mediante la eliminación del uso de bloqueos compartidos en operaciones de lectura. Esto aumenta el rendimiento del sistema al reducir los recursos utilizados para administrar bloqueos. El rendimiento también aumenta al reducir el número de veces que una transacción se bloquea mediante bloqueos adquiridos por otras transacciones.  
  
 Los niveles de aislamiento basados en las versiones de fila aumentan los recursos necesarios para la modificación de datos. Al habilitar estas opciones se crean versiones de filas de todas las modificaciones de datos para la base de datos. Se guarda una copia de los datos sin modificar en tempdb aunque no haya ninguna transacción activa que utilice el aislamiento basado en las versiones de fila. Los datos modificados incluyen un puntero a los datos con versiones almacenados en tempdb. En el caso de objetos grandes, solo se copia en tempdb la parte del objeto modificada.  
  
#### <a name="space-used-in-tempdb"></a>Espacio utilizado en TempDB  
 En cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], tempdb debe disponer de espacio suficiente para contener las versiones de fila generadas por todas las bases de datos de la instancia. El administrador de la base de datos debe asegurarse de que TempDB cuenta con espacio más que suficiente para dar cabida al almacén de versiones. Existen dos almacenes de versiones en TempDB:  
  
-   El almacén de versiones de generación de índices en línea se utiliza para generar índices en línea en todas las bases de datos.  
-   El almacén de versiones común se utiliza en las demás operaciones de modificación de datos de todas las bases de datos.  
  
 Las versiones de filas deben estar almacenadas mientras una transacción activa necesite tener acceso a ella. Cada minuto, un subproceso en segundo plano elimina las versiones de filas que ya no se necesitan y libera el espacio de versiones en TempDB. Una transacción de larga duración impide que se libere el espacio del almacén de versiones si se cumple alguna de las siguientes condiciones:  
  
-   Se utiliza el aislamiento basado en las versiones de fila.  
-   Se utilizan desencadenadores, MARS u operaciones de generación de índices en línea.  
-   Se generan versiones de filas.  
  
> [!NOTE]  
> Cuando se invoca un desencadenador dentro de una transacción, las versiones de filas creadas por el desencadenador se mantienen hasta el final de la transacción, incluso cuando las versiones de filas dejan de necesitarse una vez completado el desencadenador. Esto también se aplica a las transacciones de lectura confirmada que usan las versiones de fila. Con este tipo de transacción, solo se necesita una vista de la base de datos transaccionalmente coherente para cada instrucción de la transacción. De este modo, las versiones de filas creadas para una instrucción de la transacción dejan de necesitarse una vez completada la instrucción. No obstante, las versiones de filas creadas por cada instrucción de la transacción se mantienen hasta que se complete la transacción.  
  
 Cuando TempDB se queda sin espacio, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] fuerza la reducción de los almacenes de versiones. Durante el proceso de reducción, las transacciones de mayor duración que todavía no han generado versiones de filas se marcan como sujetos. Se genera el mensaje 3967 en el registro de errores para cada transacción marcada como sujeto. Si una traducción se marca como sujeto, no podrá leer las versiones de filas del almacén de versiones. Cuando intenta leer versiones de filas, se genera el mensaje 3966 y la transacción se revierte. Si el proceso de reducción se realiza correctamente, pasa a quedar espacio disponible en tempdb. Si no, tempdb se queda sin espacio y se produce lo siguiente:  
  
-   Las operaciones de escritura continúan, pero no generan versiones. Aparece un mensaje informativo (3959) en el registro de errores, pero la transacción que escribe datos no se ve afectada.  
  
-   Las transacciones que intentan obtener acceso a versiones de filas que no se generaron debido a una reversión completa de tempdb terminan en un error 3958.  
  
#### <a name="space-used-in-data-rows"></a>Espacio utilizado en filas de datos  
 Cada fila de base de datos puede utilizar hasta 14 bytes al final de la fila para información de las versiones de fila. La información de versiones de fila contiene el número de secuencia de la transacción que confirmó la versión y el puntero a la fila cuya versión se ha creado. Estos 14 bytes se agregan la primera vez que se modifica una fila o se inserta una nueva fila, si se cumple alguna de las siguientes condiciones:  
  
-   Las opciones `READ_COMMITTED_SNAPSHOT` o `ALLOW_SNAPSHOT_ISOLATION` están en ON.  
-   La tabla tiene un desencadenador.  
-   Se utilizan conjuntos de resultados activos múltiples (MARS)  
-   En la actualidad, se ejecutan en la tabla operaciones de compilación de índices en línea.  
  
 Estos 14 bytes se eliminan de la fila de base de datos la primera vez que se modifica la fila si se cumplen todas estas condiciones:  
  
-   Las opciones `READ_COMMITTED_SNAPSHOT` y `ALLOW_SNAPSHOT_ISOLATION` están en OFF.  
-   El desencadenador ya no existe en la tabla.  
-   No se utiliza MARS.  
-   No se ejecutan en ese momento operaciones de generación de índices en línea.  
  
 Si se utiliza alguna de las características de versiones de fila, puede que sea necesario asignar espacio de disco adicional para que la base de datos dé cabida a los 14 bytes por fila de base de datos. Al agregar información de versiones de fila puede provocarse la división de la página de índices o la asignación de una nueva página de datos si no hay suficiente especio disponible en la página actual. Por ejemplo, si la longitud media de fila es 100 bytes, los 14 bytes adicionales hacen que una tabla existente crezca hasta un 14 por ciento.  
  
 Si se reduce el [factor de relleno](../relational-databases/indexes/specify-fill-factor-for-an-index.md), se puede impedir o reducir la fragmentación de las páginas de índice. Para ver información sobre la fragmentación de los datos e índices de una tabla o vista, puede usar [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).  
  
#### <a name="space-used-in-large-objects"></a>Espacio utilizado en objetos grandes  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] admite seis tipos de datos que pueden contener cadenas grandes de hasta dos gigabytes (GB) de longitud: `nvarchar(max)`, `varchar(max)`, `varbinary(max)`, `ntext`, `text` e `image`. Las cadenas grandes almacenadas con estos tipos de datos se almacenan en una serie de fragmentos de datos que se vinculan a la fila de datos. La información de versiones de fila se almacena en cada uno de los fragmentos utilizados para almacenar estas cadenas grandes. Los fragmentos de datos son una colección de páginas dedicadas a objetos grandes en una tabla.  
  
 A medida que se agregan nuevos valores grandes a una base de datos, se asignan utilizando un máximo de 8.040 bytes de datos por fragmento. En versiones anteriores del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] se almacenaban hasta 8.080 bytes de datos `ntext`, `text` o `image` por fragmento.  
  
 Los datos de objetos grandes (LOB) `ntext`, `text` e `image` existentes no se actualizan para dejar espacio para la información de versiones de fila cuando una base de datos se actualiza a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde una versión anterior de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Sin embargo, la primera vez que se modifican los datos de LOB, se actualizan dinámicamente para habilitar el almacenamiento de información del control de versiones. Esto sucederá aunque no se generen versiones de filas. Una vez actualizados los datos de LOB, el número máximo de bytes almacenados por fragmento se reduce de 8.080 bytes a 8.040 bytes. El proceso de actualización es equivalente a eliminar el valor de LOB y volver a insertar el mismo valor. Los datos de LOB se actualizan aunque solo se haya modificado un solo byte. Esta operación se realiza una sola vez para cada columna `ntext`, `text` o `image`, pero, dependiendo del tamaño de los datos de LOB, puede que cada operación genere gran cantidad de asignaciones de página y actividad de E/S. Puede que también se genere gran cantidad de actividad de registro si la modificación se registra por completo. Las operaciones WRITETEXT y UPDATETEXT se registran mínimamente si el modo de recuperación de la base de datos no se establece en FULL.  
  
 Los tipos de datos `nvarchar(max)`, `varchar(max)` y `varbinary(max)` no están disponibles en versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Por lo tanto, no presentan problemas de actualización.  
  
 Debe asignarse suficiente espacio de disco para dar cabida a este requisito.  
  
#### <a name="monitoring-row-versioning-and-the-version-store"></a>Supervisar las versiones de fila y el almacén de versiones  
 Para los procesos de supervisión de versiones de fila, almacén de versiones y aislamiento de instantánea en cuanto al rendimiento y otros problemas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] proporciona herramientas en forma de Vistas de administración dinámica (DMV) y contadores de rendimiento del Monitor de sistema de Windows.  
  
##### <a name="dmvs"></a>DMV  
 Las siguientes DMV proporcionan información sobre el estado actual del sistema de tempdb y el almacén de versiones, así como de las transacciones que utilizan las versiones de fila.  
  
 sys.dm_db_file_space_usage. Devuelve información de uso del espacio para cada fila de la base de datos. Para obtener más información, consulte [sys.dm_db_file_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md).  
  
 sys.dm_db_session_space_usage. Devuelve la actividad de asignación y desasignación de páginas por sesión de la base de datos. Para obtener más información, consulte [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md).  
  
 sys.dm_db_task_space_usage. Devuelve la actividad de asignación y desasignación de páginas por tarea de la base de datos. Para obtener más información, consulte [sys.dm_db_task_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md).  
  
 sys.dm_tran_top_version_generators. Devuelve una tabla virtual para los objetos que producen la mayoría de las versiones del almacén de versiones. Agrupa las 256 longitudes de registro principales agregadas mediante su database_id y rowset_id. Use esta función para encontrar los principales consumidores del almacén de versiones. Para obtener más información, consulte [sys.dm_tran_top_version_generators &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-top-version-generators-transact-sql.md).  
  
 sys.dm_tran_version_store. Devuelve una tabla virtual que muestra todos los registros de versión del almacén de versiones común. Para obtener más información, consulte [sys.dm_tran_version_store &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md).  

 sys.dm_tran_version_store_space_usage. Devuelve una tabla virtual que muestra el espacio total en tempdb usado por los registros del almacén de versiones para cada base de datos. Para obtener más información, consulte [sys.dm_tran_version_store_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-space-usage.md).  

> [!NOTE]  
> La ejecución de las funciones sys.dm_tran_top_version_generators y sys.dm_tran_version_store puede ser muy costosa, puesto que ambas consultan todo el almacén de versiones, que puede ser muy grande.  
> La función sys.dm_tran_version_store_space_usage es eficaz y económica de ejecutar, ya que no navega por registros individuales del almacén de versiones y devuelve el espacio agregado del almacén de versiones que se ha utilizado en tempdb por base de datos.
  
 sys.dm_tran_active_snapshot_database_transactions. Devuelve una tabla virtual para todas las transacciones activas de todas las bases de datos en la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que utiliza versiones de fila. Las transacciones del sistema no aparecen en esta DMV. Para obtener más información, consulte [sys.dm_tran_active_snapshot_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md).  
  
 sys.dm_tran_transactions_snapshot. Devuelve una tabla virtual que muestra las instantáneas tomadas por cada transacción. La instantánea contiene el número de secuencia de las transacciones activas que utilizan versiones de fila. Para obtener más información, consulte [sys.dm_tran_transactions_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-transactions-snapshot-transact-sql.md).  
  
 sys.dm_tran_current_transaction. Devuelve una sola fila que muestra información de estado relacionada con las versiones de fila de la transacción de la sesión actual. Para obtener más información, consulte [sys.dm_tran_current_transaction &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql.md).  
  
 sys.dm_tran_current_snapshot. Devuelve una tabla virtual que muestra todas las transacciones activas en el momento en que se inicia la transacción actual de aislamiento de instantánea. Si la transacción actual está usando un aislamiento de instantáneas, esta función no devuelve filas. La función sys.dm_tran_current_snapshot es similar a sys.dm_tran_transactions_snapshot, excepto en que solo devuelve las transacciones activas de la instantánea actual. Para obtener más información, consulte [sys.dm_tran_current_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-snapshot-transact-sql.md).  
  
##### <a name="performance-counters"></a>Contadores de rendimiento  
 Los contadores de rendimiento de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] proporcionan información sobre el rendimiento del sistema afectado por los procesos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Los siguientes contadores de rendimiento supervisan tempdb y el almacén de versiones, así como las transacciones que utilizan versiones de fila. Los contadores de rendimiento se encuentran en el objeto de rendimiento SQLServer:Transactions.  
  
 **Espacio disponible en tempdb (KB)** . Supervisa la cantidad, en kilobytes (KB), de espacio libre en la base de datos tempdb. Debe haber suficiente espacio libre en tempdb para controlar el almacén de versiones que admite aislamiento de instantánea.  
  
 La fórmula siguiente ofrece una estimación aproximada del tamaño del almacén de versiones. En el caso de transacciones de larga duración, puede que sea conveniente supervisar la velocidad de generación y limpieza para estimar el tamaño máximo del almacén de versiones.  
  
 [tamaño del almacén de versiones común] = 2 * [datos del almacén de versiones generados por minuto] \* [mayor tiempo de ejecución (minutos) de la transacción]  
  
 El mayor tiempo de ejecución de transacciones no debe incluir generaciones de índices en línea. Dado que estas operaciones pueden llevar mucho tiempo en tablas muy grandes, las generaciones de índices en línea utilizan un almacén de versiones independiente. El tamaño aproximado del almacén de versiones de generaciones de índices en línea equivale a la cantidad de datos modificados en la tabla, incluidos todos los índices, mientras la generación de índices en línea esté activa.  
  
 **Tamaño de almacén de versiones (KB)** . Supervisa el tamaño en KB de todos los almacenes de versiones. Esta información ayuda a determinar el espacio necesario para el almacén de versiones en la base de datos tempdb. La supervisión de este contador durante cierto tiempo proporciona una estimación útil del espacio adicional necesario para tempdb.  
  
 `Version Generation rate (KB/s)`. Supervisa la velocidad de generación de versión en KB por segundo en todos los almacenes de versiones.  
  
 `Version Cleanup rate (KB/s)`. Supervisa la velocidad de limpieza de versión en KB por segundo en todos los almacenes de versiones.  
  
> [!NOTE]  
> La información procedente de Velocidad de generación de versión (KB/seg.) y Velocidad de limpieza de versión (KB/seg.) se puede utilizar para predecir los requisitos de espacio de tempdb.  
  
 **Recuento de unidad de almacén de versiones**. Supervisa el recuento de unidades del almacén de versiones.  
  
 **Creación de unidad de almacén de versiones**. Supervisa el número total de unidades del almacén de versiones creadas para almacenar versiones de filas desde que se inició la instancia.  
  
 **Truncamiento de unidad de almacén de versiones**. Supervisa el número total de unidades del almacén de versiones truncadas desde que se inició la instancia. Una unidad del almacén de versiones se trunca cuando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] determina que no se necesita ninguna de las filas de versiones almacenadas en la unidad del almacén de versiones para ejecutar transacciones activas.  
  
 **Frecuencia de conflictos de actualización**. Supervisa la frecuencia de las transacciones de instantánea que tienen conflictos de actualización con respecto al número total de transacciones de instantánea de actualización.  
  
 **Tiempo mayor de ejecución de transacción**. Supervisa el tiempo mayor de ejecución en segundos de cualquier transacción que utilice versiones de fila. Esto permite determinar si una transacción se ejecuta durante una cantidad de tiempo desproporcionada.  
  
 **Transacciones**. Supervisa el número total de transacciones activas. No incluye las transacciones del sistema.  
  
 `Snapshot Transactions`. Supervisa el número total de transacciones de instantáneas activas.  
  
 `Update Snapshot Transactions`. Supervisa el número total de transacciones de instantáneas activas que realizan operaciones de actualización.  
  
 `NonSnapshot Version Transactions`. Supervisa el número total de transacciones que no son instantáneas activas que generan registros de versión.  
  
> [!NOTE]  
> La suma de Transacciones de instantáneas de actualización y Transacciones de versión que no son instantáneas representa el número total de transacciones que participan en la generación de versiones. La diferencia entre Transacciones de instantáneas y Transacciones de instantáneas de actualización notifica el número de transacciones de instantáneas de solo lectura.  
  
### <a name="row-versioning-based-isolation-level-example"></a>Ejemplo de nivel de aislamiento basado en versiones de fila  
 En los siguientes ejemplos se muestran las diferencias de comportamiento entre transacciones de aislamiento de instantánea y transacciones de lectura confirmada que usan las versiones de fila.  
  
#### <a name="a-working-with-snapshot-isolation"></a>A. Trabajar con aislamiento de instantánea  
 En este ejemplo, una transacción que se ejecuta con aislamiento de instantánea lee los datos que a continuación modifica otra transacción. La transacción de instantáneas no bloquea la operación de actualización ejecutada por la otra transacción y sigue leyendo datos de la fila con versiones, omitiendo la modificación de datos. No obstante, cuando la transacción de instantáneas intenta modificar los datos que ya han sido modificados por la otra transacción, genera un error y finaliza.  
  
 En la sesión 1:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Enable snapshot isolation on the database.  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Start a snapshot transaction  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 En la sesión 2:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under snapshot isolation shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 En la sesión 1:  
  
```sql  
    -- Reissue the SELECT statement - this shows  
    -- the employee having 48 vacation hours.  The  
    -- snapshot transaction is still reading data from  
    -- the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 En la sesión 2:  
  
```sql  
-- Commit the transaction; this commits the data  
-- modification.  
COMMIT TRANSACTION;  
GO  
```  
  
 En la sesión 1:  
  
```sql  
    -- Reissue the SELECT statement - this still   
    -- shows the employee having 48 vacation hours  
    -- even after the other transaction has committed  
    -- the data modification.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- Because the data has been modified outside of the  
    -- snapshot transaction, any further data changes to   
    -- that data by the snapshot transaction will cause   
    -- the snapshot transaction to fail. This statement   
    -- will generate a 3960 error and the transaction will   
    -- terminate.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION  
GO  
```  
  
#### <a name="b-working-with-read-committed-using-row-versioning"></a>B. Trabajar con transacciones de lectura confirmada utilizando las versiones de fila  
 En este ejemplo, una transacción de lectura confirmada que utiliza las versiones de fila se ejecuta simultáneamente con otra transacción. La transacción de lectura confirmada se comporta de diferente manera que una transacción de instantáneas. Al igual que una transacción de instantáneas, la transacción de lectura confirmada lee filas con versiones incluso después de que la otra transacción haya modificado los datos. Sin embargo, a diferencia de una transacción de instantáneas, la transacción de lectura confirmada:  
  
-   Leerá los datos modificados después de que la otra transacción confirme los cambios en los datos.  
-   Podrá actualizar los datos modificados por la otra transacción cuando la transacción de instantáneas no pueda.  
  
 En la sesión 1:  
  
```sql  
USE AdventureWorks2016;  -- Or any earlier version of the AdventureWorks database.  
GO  
  
-- Enable READ_COMMITTED_SNAPSHOT on the database.  
-- For this statement to succeed, this session  
-- must be the only connection to the AdventureWorks2016  
-- database.  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
GO  
  
-- Start a read-committed transaction  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 En la sesión 2:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under read-committed using row versioning shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 En la sesión 1:  
  
```sql  
    -- Reissue the SELECT statement - this still shows  
    -- the employee having 48 vacation hours.  The  
    -- read-committed transaction is still reading data   
    -- from the versioned row and the other transaction   
    -- has not committed the data changes yet.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 En la sesión 2:  
  
```sql  
-- Commit the transaction.  
COMMIT TRANSACTION;  
GO  
```  
  
 En la sesión 1:  
  
```sql  
    -- Reissue the SELECT statement which now shows the   
    -- employee having 40 vacation hours.  Being   
    -- read-committed, this transaction is reading the   
    -- committed data. This is different from snapshot  
    -- isolation which reads from the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- This statement, which caused the snapshot transaction   
    -- to fail, will succeed with read-committed using row versioning.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION;  
GO  
```  
  
### <a name="enabling-row-versioning-based-isolation-levels"></a>Habilitar niveles de aislamiento basado en versiones de fila  
 Los administradores de bases de datos determinan la configuración de la base de datos para versiones de fila mediante las opciones de base de datos `READ_COMMITTED_SNAPSHOT` y `ALLOW_SNAPSHOT_ISOLATION` de la instrucción ALTER DATABASE.  
  
 Cuando se establece la opción de base de datos `READ_COMMITTED_SNAPSHOT` en ON, se activan inmediatamente los mecanismos utilizados para admitir la opción. Al establecer la opción READ_COMMITTED_SNAPSHOT, solo se permite en la base de datos la conexión que ejecuta el comando `ALTER DATABASE`. No debe haber ninguna otra conexión de base de datos abierta hasta que ALTER DATABASE haya finalizado. La base de datos no tiene que estar en modo de usuario único.  
  
 La instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] siguiente permite `READ_COMMITTED_SNAPSHOT`:  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
```  
  
 Si la opción de base de datos `ALLOW_SNAPSHOT_ISOLATION` se establece en ON, la instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] no genera versiones de filas para datos modificados hasta que finalicen todas las transacciones activas que han modificado los datos en la base de datos. Si hay transacciones de modificación activas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] establece el estado de la opción en `PENDING_ON`. Una vez finalizadas todas las transacciones de modificación, el estado de la opción cambia a ON. Los usuarios no pueden iniciar una transacción de instantáneas en la base de datos hasta que la opción esté completamente en ON. La base de datos pasa a un estado PENDING_OFF cuando el administrador de la base de datos establece la opción `ALLOW_SNAPSHOT_ISOLATION` en OFF.  
  
 La siguiente instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] habilita ALLOW_SNAPSHOT_ISOLATION:  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
```  
  
 En la tabla siguiente se enumeran y describen los estados de la opción ALLOW_SNAPSHOT_ISOLATION. El uso de ALTER DATABASE con la opción ALLOW_SNAPSHOT_ISOLATION no bloquea a los usuarios que actualmente tienen acceso a la base de datos.  
  
|Estado del marco de aislamiento de instantánea para la base de datos actual|Descripción|  
|----------------------------------------------------------------|-----------------|  
|Apagado|La compatibilidad de transacciones de aislamiento de instantánea no está activada. No se permiten transacciones de aislamiento de instantánea.|  
|PENDING_ON|La compatibilidad de transacciones de aislamiento de instantánea se encuentra en estado de transición (de OFF a ON). Las operaciones abiertas deben finalizar.<br /><br /> No se permiten transacciones de aislamiento de instantánea.|  
|ACTIVAR|La compatibilidad de transacciones de aislamiento de instantánea está activada.<br /><br /> Se permiten transacciones de instantáneas.|  
|PENDING_OFF|La compatibilidad de transacciones de aislamiento de instantánea se encuentra en estado de transición (de ON a OFF).<br /><br /> Las transacciones de instantáneas iniciadas con posterioridad no tienen acceso a esta base de datos. La actualización de transacciones sigue pagando el costo de crear versiones en esta base de datos. Las transacciones de instantáneas existentes siguen teniendo acceso a la base de datos sin problema. El estado PENDING_OFF no pasa a OFF hasta que finalicen las transacciones de instantáneas que estaban activas cuando el estado de aislamiento de instantánea de base de datos estaba en ON.|  
  
 Use la vista de catálogo `sys.databases` para determinar el estado de ambas opciones de base de datos para las versiones de fila.  
  
 Todas las actualizaciones de tablas de usuario y algunas tablas de sistema almacenadas en la base de datos maestra y msdb generan versiones de fila.  
  
 La opción `ALLOW_SNAPSHOT_ISOLATION` se establece automáticamente en ON en las bases de datos maestra y msdb, y no puede deshabilitarse.  
  
 Los usuarios no pueden establecer en ON la opción `READ_COMMITTED_SNAPSHOT` en la bases de datos maestra, tempdb ni msdb.  
  
### <a name="using-row-versioning-based-isolation-levels"></a>Usar niveles de aislamiento basados en versiones de fila  
 El marco de versiones de fila está siempre habilitado en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y lo utilizan varias características. Además de proporcionar niveles de aislamiento basados en versiones de fila, se utiliza para admitir las modificaciones efectuadas en desencadenadores y sesiones de conjuntos de resultados activos múltiples (MARS), así como para admitir lecturas de datos en operaciones de índice ONLINE.  
  
 Los niveles de aislamiento basados en versiones de fila se habilitan en la base de datos. Cualquier aplicación que tenga acceso a objetos de bases de datos habilitadas puede ejecutar consultas con los siguientes niveles de aislamiento:  
  
-   Lectura de confirmadas, que utiliza versiones de fila al establecer la opción de base de datos `READ_COMMITTED_SNAPSHOT` en `ON`, como se muestra en el siguiente ejemplo de código:  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET READ_COMMITTED_SNAPSHOT ON;  
    ```  
  
     Cuando la base de datos se habilita para `READ_COMMITTED_SNAPSHOT`, todas las consultas que se ejecutan en el nivel de aislamiento de lectura de confirmadas utilizan versiones de fila, lo que significa que las operaciones de lectura no bloquean las operaciones de actualización.  
  
-   Puede habilitar el aislamiento de instantáneas configurando la opción de base de datos `ALLOW_SNAPSHOT_ISOLATION` en `ON`, como se muestra en el ejemplo de código siguiente:  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET ALLOW_SNAPSHOT_ISOLATION ON;  
    ```  
  
     Una transacción que se ejecute en aislamiento de instantánea puede tener acceso a tablas de la base de datos que se hayan habilitado para instantáneas. Para obtener acceso a tablas que no se han habilitado para instantáneas, debe cambiarse el nivel de aislamiento. El siguiente ejemplo de código muestra una instrucción `SELECT` que une dos tablas mientras se ejecuta en una transacción de instantáneas. Una tabla pertenece a una base de datos en la que no se habilitó el aislamiento de instantánea. Cuando la instrucción `SELECT` se ejecuta en aislamiento de instantánea, no lo hace correctamente.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
     El siguiente ejemplo de código muestra la misma instrucción `SELECT` modificada para cambiar el nivel de aislamiento de transacción a lectura de confirmadas. Gracias a este cambio, la instrucción `SELECT` se ejecuta correctamente.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            WITH (READCOMMITTED)  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
#### <a name="limitations-of-transactions-using-row-versioning-based-isolation-levels"></a>Limitaciones de transacciones con niveles de aislamiento basados en versiones de fila  
 Tenga en cuenta las siguientes limitaciones cuando trabaje con niveles de aislamiento basados en versiones de fila:  
  
-   No se puede habilitar `READ_COMMITTED_SNAPSHOT` en tempdb, msdb ni master.  
-   Las tablas temporales globales se almacenan en tempdb. Cuando se obtiene acceso a tablas temporales globales en una transacción de instantáneas, debe realizarse alguna de las siguientes acciones:  
    -   Establezca la opción de base de datos `ALLOW_SNAPSHOT_ISOLATION` en ON en tempdb.  
    -   Usar una sugerencia de aislamiento para cambiar el nivel de aislamiento de la instrucción.  
-   Las transacciones de instantáneas provocan errores cuando:  
    -   Una base de datos pasa a ser de solo lectura una vez iniciada la transacción de instantáneas, pero antes de que ésta obtenga acceso a la base de datos.  
    -   Si al tener acceso a objetos de varias bases de datos, el estado de una base de datos se modificó de forma que la recuperación de la base de datos se produjo después del inicio de la transacción, pero antes del acceso de la transacción de instantáneas a la base de datos. Por ejemplo, la base de datos se estableció en OFFLINE y luego en ONLINE, cierre automático y apertura de base de datos, o adjuntar y separar una base de datos.  
-   Las transacciones distribuidas, incluidas las consultas de bases de datos con particiones distribuidas, no se admiten en aislamiento de instantánea.  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no mantiene varias versiones de los metadatos del sistema. Las instrucciones del lenguaje de definición de datos (DDL) de tablas y otros objetos de base de datos (índices, vistas, tipos de datos, procedimientos almacenados y funciones de CLR (Common Language Runtime)) cambian los metadatos. Si una instrucción DDL modifica un objeto, cualquier referencia simultánea al objeto en aislamiento de instantánea provoca errores en la transacción de instantáneas. Las transacciones de lectura de confirmadas no tienen esta limitación cuando la opción de base de datos READ_COMMITTED_SNAPSHOT es ON.  
  
     Por ejemplo, el administrador de la base de datos ejecuta la siguiente instrucción `ALTER INDEX`.  
  
    ```sql  
    USE AdventureWorks2016;  
    GO  
    ALTER INDEX AK_Employee_LoginID  
        ON HumanResources.Employee REBUILD;  
    GO  
    ```  
  
     Cualquier transacción de instantáneas que esté activa cuando se ejecuta la instrucción `ALTER INDEX` recibirá un error si intenta hacer referencia a la tabla `HumanResources.Employee` una vez ejecutada la instrucción `ALTER INDEX`. Las transacciones de lectura de confirmadas que utilicen versiones de fila no se verán afectadas.  
  
    > [!NOTE]  
    > Las operaciones BULK INSERT pueden provocar cambios en los metadatos de la tabla de destino (por ejemplo, al deshabilitar las comprobaciones de restricciones). Cuando esto sucede, las transacciones simultáneas de aislamiento de instantánea que obtengan acceso a tablas de inserciones masivas generarán un error.  
  
   
## <a name="customizing-locking-and-row-versioning"></a>Personalizar las versiones de fila y bloqueo  
  
### <a name="customizing-the-lock-time-out"></a>Personalizar el tiempo de espera de bloqueo  
 Cuando una instancia del [!INCLUDE[msCoName](../includes/msconame-md.md)] de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] no puede conceder un bloqueo a una transacción porque otra transacción ya posee un bloqueo en conflicto para el recurso, la primera transacción queda bloqueada a la espera de que se libere el bloqueo existente. De forma predeterminada no hay un tiempo de espera obligatorio, ni tampoco existe ningún modo de comprobar si un recurso está bloqueado antes de intentar bloquearlo, excepto intentar tener acceso a los datos (con el riesgo de quedar bloqueado indefinidamente).  
  
> [!NOTE]  
> En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], use la vista de administración dinámica **sys.dm_os_waiting_tasks** para determinar si un proceso está bloqueado y quién lo bloquea. En versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], use el procedimiento almacenado del sistema **sp_who**.  
  
 El parámetro `LOCK_TIMEOUT` permite a una aplicación establecer el tiempo máximo que una instrucción esperará en un recurso bloqueado. Cuando una instrucción ha esperado más tiempo del indicado en LOCK_TIMEOUT, la instrucción bloqueada se cancela automáticamente y se devuelve el mensaje de error 1222 (`Lock request time-out period exceeded`) a la aplicación. Sin embargo, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no cancela ni revierte ninguna transacción que contenga la instrucción. Por consiguiente, la aplicación debe tener un controlador de errores que pueda interceptar el mensaje de error 1222. Si una aplicación no intercepta el error, puede continuar sin ser consciente de que se ha cancelado una instrucción individual de la transacción y de que esto puede producir errores, ya que las instrucciones posteriores de la transacción podrían depender de la instrucción que nunca se ejecutó.  
  
 Al implementar un controlador de errores que intercepte el mensaje de error 1222 se permite a una aplicación controlar la situación de tiempo de espera y realizar una acción apropiada para solucionarla, como volver a enviar automáticamente la instrucción bloqueada o revertir la transacción completa.  
  
 Para determinar el valor `LOCK_TIMEOUT` actual, ejecute la función `@@LOCK_TIMEOUT`:  
  
```sql  
SELECT @@lock_timeout;  
GO  
```  
  
### <a name="customizing-transaction-isolation-level"></a>Personalizar el nivel de aislamiento de transacción  
 READ COMMITTED es el nivel de aislamiento predeterminado del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] de [!INCLUDE[msCoName](../includes/msconame-md.md)]. Cuando es necesario utilizar una aplicación en un nivel de aislamiento distinto, se pueden utilizar los métodos que se indican a continuación para configurar el nivel de aislamiento:  
  
-   Ejecute la instrucción [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
-   En las aplicaciones ADO.NET que utilizan el espacio de nombres administrado System.Data.SqlClient, especifique una opción *IsolationLevel* mediante el método SqlConnection.BeginTransaction.  
-   Las aplicaciones que usan ADO pueden establecer la propiedad `Autocommit Isolation Levels`.  
-   Al iniciar una transacción, las aplicaciones que utilicen OLE DB pueden llamar a ITransactionLocal::StartTransaction con la propiedad *isoLevel* establecida en el nivel de aislamiento de transacción deseado. Si se especifica el nivel de aislamiento en el modo de confirmación automática, para las aplicaciones que utilicen OLE DB se puede establecer la propiedad DBPROPSET_SESSION, DBPROP_SESS_AUTOCOMMITISOLEVELS en el nivel de aislamiento de transacción deseado.  
-   Las aplicaciones que utilizan ODBC pueden establecer el atributo SQL_COPT_SS_TXN_ISOLATION mediante SQLSetConnectAttr.  
  
Si se especifica el nivel de aislamiento, el bloqueo de todas las consultas e instrucciones del lenguaje de manipulación de datos (DML) en la sesión de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se aplica en ese nivel de aislamiento. El nivel de aislamiento permanece vigente hasta que finaliza la sesión o hasta que se cambia la configuración del nivel de aislamiento.  
  
En el siguiente ejemplo, se configura el nivel de aislamiento `SERIALIZABLE`:  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
SELECT BusinessEntityID  
    FROM HumanResources.Employee;  
GO  
```  
  
El nivel de aislamiento se puede pasar por alto, si es necesario, para consultas o instrucciones DML individuales. Para ello debe especificarse una sugerencia de tabla. El hecho de especificar una sugerencia de tabla no afecta a las demás instrucciones de la sesión. Se recomienda que las sugerencias de tabla solo se utilicen para modificar el comportamiento predeterminado en los casos estrictamente necesarios.  
  
Es posible que [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] tenga que adquirir bloqueos al leer los metadatos incluso si el nivel de aislamiento se ha establecido en un nivel en el que no se soliciten bloqueos compartidos al leer los datos. Por ejemplo, una transacción que se ejecute en el nivel de aislamiento de lectura sin confirmar no adquiere bloqueos compartidos al leer datos, pero es probable que tenga que solicitar bloqueos en alguna ocasión al leer una vista de catálogo del sistema. Esto significa que es posible que una transacción con lectura sin confirmar provoque un bloqueo cuando ejecute consultas en una tabla mientras otra transacción simultánea modifica los metadatos de la tabla.  
  
Para determinar el nivel de aislamiento de transacción que está establecido actualmente, utilice la instrucción `DBCC USEROPTIONS` como se indica en el siguiente ejemplo. El conjunto de resultados puede variar según el conjunto de resultados del sistema.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
DBCC USEROPTIONS;  
GO  
```  
  
[!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```
Set Option                   Value  
---------------------------- -------------------------------------------  
textsize                     2147483647  
language                     us_english  
dateformat                   mdy  
datefirst                    7  
...                          ...  
Isolation level              repeatable read  
 
(14 row(s) affected)   
 
DBCC execution completed. If DBCC printed error messages, contact your system administrator.
```  
  
### <a name="locking-hints"></a>Sugerencias de bloqueo  
 Se pueden especificar sugerencias de bloqueo para referencias de tablas individuales en las instrucciones SELECT, INSERT, UPDATE y DELETE. Las sugerencias especifican el tipo de bloqueo o las versiones de fila que utiliza la instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] para los datos de la tabla. Se pueden utilizar las sugerencias de bloqueo de tabla cuando se requiere un control más ajustado de los tipos de bloqueo adquiridos en un objeto. Estas sugerencias de bloqueo suplantan el nivel de aislamiento de la transacción actual durante la sesión.  
  
 Para obtener más información sobre las sugerencias de bloqueo específicas y sus comportamientos, vea [Sugerencias &#40;Transact-SQL&#41; - tabla](../t-sql/queries/hints-transact-sql-table.md).  
  
> [!NOTE]  
> [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] suele elegir el nivel de bloqueo correcto casi siempre. Se recomienda utilizar las sugerencias de bloqueo de tabla para modificar el comportamiento de bloqueo predeterminado en los casos estrictamente necesarios. Impedir un nivel de bloqueo puede ir en detrimento de la simultaneidad.  
  
 El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] podría verse forzado a adquirir bloqueos al leer los metadatos, incluso cuando procese una instrucción SELECT con una sugerencia de bloqueo que impida a las solicitudes compartir bloqueos al leer los datos. Por ejemplo, una instrucción `SELECT` que utiliza la sugerencia `NOLOCK` no adquiere bloqueos compartidos al leer los datos, pero podría solicitar bloqueos en alguna oportunidad al leer la vista de catálogo del sistema. Esto significa que es posible que una instrucción `SELECT` que utilice `NOLOCK` esté bloqueada.  
  
 Según se muestra en el siguiente ejemplo, si se establece el nivel de aislamiento de la transacción en `SERIALIZABLE` y se utiliza la sugerencia de bloqueo de nivel de tabla `NOLOCK` con la instrucción `SELECT`, no se aplican los bloqueos de intervalos de claves usados normalmente para mantener las transacciones serializables.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT JobTitle  
    FROM HumanResources.Employee WITH (NOLOCK);  
GO  
  
-- Get information about the locks held by   
-- the transaction.  
SELECT    
        resource_type,   
        resource_subtype,   
        request_mode  
    FROM sys.dm_tran_locks  
    WHERE request_session_id = @@spid;  
  
-- End the transaction.  
ROLLBACK;  
GO  
```  
  
 El único bloqueo utilizado que hace referencia a `HumanResources.Employee` es un bloqueo de estabilidad de esquema (Sch-S). En este caso, ya no se garantiza la serialidad.  
  
 En [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], la opción `LOCK_ESCALATION` de `ALTER TABLE` puede penalizar los bloqueos de tabla y habilitar los bloqueos HoBT en tablas con particiones. Esta opción no es una sugerencia de bloqueo, pero puede utilizarse para reducir la extensión de los bloqueos. Para obtener más información, vea [ALTER TABLE &#40;Transact-SQL&#41;](../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="customizing-locking-for-an-index"></a><a name="Customize"></a> Personalizar el bloqueo de un índice  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza una estrategia de bloqueo dinámico que, en la mayoría de casos, elige automáticamente la mejor granularidad de bloqueo para las consultas. Se recomienda no invalidar los niveles de bloqueo predeterminados, que tienen el bloqueo de página y fila habilitados, a menos que se tenga un conocimiento claro de los patrones de acceso a tablas o índices y de que estos sean coherentes, y de que se surja un problema de contención de recursos que haya que resolver. Si se invalida un nivel de bloqueo, se puede obstaculizar considerablemente el acceso simultáneo a una tabla o índice. Por ejemplo, si se especifican bloqueos solamente para el nivel de tabla en una tabla de gran tamaño a la que los usuarios obtienen acceso frecuentemente, se pueden producir cuellos de botella, ya que los usuarios deben esperar a que se libere el bloqueo de la tabla para tener acceso a ella.  
  
 Hay algunos casos en lo que puede ser conveniente impedir los bloqueos de página o fila, siempre y cuando se comprendan perfectamente los patrones y estos sean coherentes. Por ejemplo, una aplicación de bases de datos utiliza una tabla de búsqueda que se actualiza semanalmente en un proceso por lotes. Los lectores simultáneos tienen acceso a la tabla con un bloqueo compartido (S) y las actualizaciones por lotes semanales obtienen acceso a la tabla con un bloqueo exclusivo (X). Al desactivar el bloqueo de página y fila en la tabla se reduce la sobrecarga de bloqueo durante la semana, ya que los lectores pueden tener acceso simultáneo a la tabla a través de bloqueos de tabla compartidos. Cuando se ejecuta el trabajo por lotes, este puede completar la actualización de forma eficaz porque obtiene un bloqueo de tabla exclusivo.  
  
 La desactivación del bloqueo de página y fila podría o no ser aceptable, ya que la actualización por lotes semanal impedirá que los lectores simultáneos tengan acceso a la tabla mientras se ejecuta la actualización. Si el trabajo por lotes solamente cambia unas cuantas filas o páginas, puede modificar el nivel de bloqueo para permitir el bloqueo de nivel de fila o página y, de esta forma, permitir que otras sesiones lean la tabla sin bloqueos. Si el trabajo por lotes tiene un gran número de actualizaciones, la obtención de un bloqueo exclusivo sobre la tabla puede ser la forma más eficaz de asegurarse de que el trabajo por lotes finaliza de modo eficaz.  
  
 En ocasiones se puede producir un interbloqueo si dos operaciones simultáneas adquieren bloqueos de fila en la misma tabla y se bloquean porque necesitan bloquear la página. Al impedir los bloqueos de fila se obliga a una de las operaciones a que espere y se evita el interbloqueo.  
  
 La granularidad del bloqueo utilizado en un índice se puede establecer mediante las instrucciones `CREATE INDEX` y `ALTER INDEX`. La configuración del bloqueo se aplica a las páginas de índice y a las páginas de tabla. Además, las instrucciones `CREATE TABLE` y `ALTER TABLE` se pueden usar para establecer la granularidad del bloqueo de en las restricciones `PRIMARY KEY` y `UNIQUE`. Para mantener la compatibilidad con versiones anteriores, el procedimiento almacenado del sistema `sp_indexoption` también puede establecer la granularidad. Para mostrar la opción de bloqueo actual de un determinado índice, utilice la función `INDEXPROPERTY`. Se pueden impedir los bloqueos de página, los de fila o una combinación de bloqueos de página y fila para un determinado índice.  
  
|Bloqueos no permitidos|Tienen acceso al índice|  
|----------------------|-----------------------|  
|De página|Bloqueos de fila y tabla|  
|De fila|Bloqueos de página y tabla|  
|De página y fila|Bloqueos de tabla|  
  
##  <a name="advanced-transaction-information"></a><a name="Advanced"></a> Información avanzada sobre transacciones  
  
### <a name="nesting-transactions"></a>Anidar transacciones  
 Las transacciones explícitas se pueden anidar. El objetivo principal de esto es aceptar transacciones en procedimientos almacenados a los que se puede llamar desde un proceso que ya esté en una transacción o desde procesos que no tengan transacciones activas.  
  
 En el ejemplo siguiente se muestra el uso para el que están diseñadas las transacciones anidadas. El procedimiento *TransProc* exige su transacción, sin tener en cuenta el modo de transacción del proceso que lo ejecute. Si se llama a *TransProc* cuando una transacción está activa, la transacción anidada en *TransProc* se pasará por alto y se confirmarán o revertirán sus instrucciones `INSERT` basándose en la acción final adoptada para la transacción externa. Si un proceso que no tiene ninguna transacción pendiente ejecuta `TransProc`, la instrucción `COMMIT TRANSACTION` al final del procedimiento confirmará de manera efectiva las instrucciones `INSERT`.  
  
```sql  
SET QUOTED_IDENTIFIER OFF;  
GO  
SET NOCOUNT OFF;  
GO  
CREATE TABLE TestTrans(Cola INT PRIMARY KEY,  
               Colb CHAR(3) NOT NULL);  
GO  
CREATE PROCEDURE TransProc @PriKey INT, @CharCol CHAR(3) AS  
BEGIN TRANSACTION InProc  
INSERT INTO TestTrans VALUES (@PriKey, @CharCol)  
INSERT INTO TestTrans VALUES (@PriKey + 1, @CharCol)  
COMMIT TRANSACTION InProc;  
GO  
/* Start a transaction and execute TransProc. */  
BEGIN TRANSACTION OutOfProc;  
GO  
EXEC TransProc 1, 'aaa';  
GO  
/* Roll back the outer transaction, this will  
   roll back TransProc's nested transaction. */  
ROLLBACK TRANSACTION OutOfProc;  
GO  
EXECUTE TransProc 3,'bbb';  
GO  
/* The following SELECT statement shows only rows 3 and 4 are   
   still in the table. This indicates that the commit  
   of the inner transaction from the first EXECUTE statement of  
   TransProc was overridden by the subsequent rollback. */  
SELECT * FROM TestTrans;  
GO  
```  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] omite la confirmación de las transacciones internas. La transacción se confirma o se revierte basándose en la acción realizada al final de la transacción más externa. Si se confirma la transacción externa, también se confirmarán las transacciones anidadas internas. Si se revierte la transacción externa, también se revertirán todas las transacciones internas, independientemente de si se confirmaron individualmente o no.  
  
 Cada llamada a `COMMIT TRANSACTION` o a `COMMIT WORK` se aplica a la última instrucción `BEGIN TRANSACTION` ejecutada. Si las instrucciones `BEGIN TRANSACTION` están anidadas, la instrucción `COMMIT` solo se aplica a la última transacción anidada, que es la más interna. Aunque una instrucción `COMMIT TRANSACTION` *transaction_name* de una transacción anidada haga referencia al nombre de la transacción externa, la confirmación solo se aplicará a la transacción más interna.  
  
 No es válido que el parámetro *transaction_name* de una instrucción `ROLLBACK TRANSACTION` haga referencia a las transacciones internas de un conjunto de transacciones anidadas con nombre. *transaction_name* solo puede hacer referencia al nombre de la transacción más externa. Si se ejecuta una instrucción ROLLBACK TRANSACTION *transaction_name* con el nombre de la transacción externa en cualquier nivel de un conjunto de transacciones anidadas, se revertirán todas las transacciones anidadas. Si se ejecuta una instrucción `ROLLBACK WORK` o `ROLLBACK TRANSACTION` sin el parámetro *transaction_name* en cualquier nivel de un conjunto de transacciones anidadas, se revertirán todas las transacciones anidadas, incluida la más externa.  
  
 La función `@@TRANCOUNT` registra el nivel de anidamiento de la transacción actual. Cada instrucción `BEGIN TRANSACTION` incrementa `@@TRANCOUNT` en uno. Cada instrucción `COMMIT TRANSACTION` o `COMMIT WORK` disminuye `@@TRANCOUNT` en uno. Una instrucción `ROLLBACK WORK` o `ROLLBACK TRANSACTION` que no tiene un nombre de la transacción revierte todas las transacciones anidadas y reduce `@@TRANCOUNT` a 0. Un `ROLLBACK TRANSACTION` que usa el nombre de transacción de la transacción más extrema en un conjunto de transacciones anidadas revierte todas las transacciones anidadas y reduce `@@TRANCOUNT` a 0. Si no está seguro de si se encuentra en una transacción, use `SELECT @@TRANCOUNT` para determinar si es 1 o más. Si `@@TRANCOUNT` es 0, no está en una transacción.  
  
### <a name="using-bound-sessions"></a>Usar sesiones enlazadas  
 Las sesiones enlazadas facilitan la coordinación de las acciones entre varias sesiones iniciadas en un mismo servidor. Permiten que dos o más sesiones compartan la misma transacción y los mismos bloqueos, además de trabajar con los mismos datos sin que surjan conflictos de bloqueo. Se pueden crear sesiones enlazadas a partir de varias sesiones con la misma aplicación o desde varias aplicaciones con sesiones independientes.  
  
 Para participar en una sesión enlazada, una sesión llama a `sp_getbindtoken` o `srv_getbindtoken` (mediante Servicios de datos abiertos) para obtener un token de enlace. Un token de enlace es una cadena de caracteres que identifica de forma única cada transacción enlazada. El token de enlace se envía a las otras sesiones que se van a enlazar a la sesión actual. Las demás sesiones se enlazan con la transacción llamando a **sp_bindsession** con el token de enlace recibido de la primera sesión.  
  
> [!NOTE]  
> Las sesiones deben tener una transacción de usuario activa para que `sp_getbindtoken` o `srv_getbindtoken` funcionen correctamente.  
  
 Los tokens de enlace deben transmitirse desde el código de la aplicación que establece la primera sesión al código de la aplicación que enlaza posteriormente sus sesiones a la primera sesión. No existe ninguna una instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] o función de API que pueda utilizar una aplicación para obtener el token de enlace de una transacción iniciada por otro proceso. A continuación se indican algunos métodos que se pueden utilizar para transmitir un token de enlace:  
  
-   Si se han iniciado todas las sesiones desde el mismo proceso de aplicación, se pueden guardar los tokens de enlace en la memoria global, o bien se pueden pasar como un parámetro a las funciones.  
  
-   Si se han establecido las sesiones desde procesos de aplicación independientes, los tokens de enlace se pueden transmitir mediante la comunicación entre procesos (IPC), como una llamada a un procedimiento remoto (RPC) o el intercambio dinámico de datos (DDE).  
  
-   Los tokens de enlace se pueden guardar en una tabla de una instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] que puedan leer los procesos que deseen enlazar a la primera sesión.  
  
 Solo puede haber una sesión activa a la vez en un conjunto de sesiones enlazadas. Si una sesión ejecuta una instrucción en la instancia o tiene resultados pendientes de la instancia, ninguna otra sesión enlazada podrá tener acceso a la instancia hasta que la sesión actual finalice el procesamiento o cancele la instrucción actual. Si la instancia está ocupada procesando una instrucción de otra de las sesiones enlazadas, se producirá un error que indica que el espacio de la transacción está en uso y que la sesión debería volver a intentarlo más tarde.  
  
 Cuando se enlazan sesiones, cada una de ellas mantiene su nivel de aislamiento. Si se utiliza SET TRANSACTION ISOLATION LEVEL para cambiar el valor del nivel de aislamiento de una sesión, no se verán afectados los valores de las sesiones enlazadas a ella.  
  
#### <a name="types-of-bound-sessions"></a>Tipos de sesiones enlazadas  
 Los dos tipos de sesiones enlazadas son local y distribuido.  
  
-   **Sesión enlazada local**  
    Permite enlazar sesiones para compartir el espacio de transacciones de una sola transacción en una única instancia del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  
  
-   **Sesión enlazada distribuida**  
    Permite que las sesiones enlazadas compartan la misma transacción entre dos o más instancias hasta que toda la transacción se haya confirmado o revertido mediante el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../includes/msconame-md.md)] (MS DTC).  
  
 Las sesiones enlazadas distribuidas no se identifican mediante el token de enlace de una cadena de caracteres, sino mediante números de identificación de transacciones distribuidas. Si una sesión enlazada está implicada en una transacción local y ejecuta un RPC en un servidor remoto con `SET REMOTE_PROC_TRANSACTIONS ON`, MS DTC promueve automáticamente el nivel de la transacción enlazada local a transacción enlazada distribuida y se inicia una sesión de MS DTC.  
  
#### <a name="when-to-use-bound-sessions"></a>Cuándo utilizar sesiones enlazadas  
 En versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], las sesiones enlazadas se utilizaban básicamente para desarrollar procedimientos almacenados extendidos que tenían que ejecutar instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] en nombre del proceso que los llamaba. Pasar el proceso que realiza la llamada en un token de enlace como un parámetro del procedimiento almacenado extendido permite al procedimiento combinar el espacio de transacciones del proceso que realiza la llamada y, por ello, integrar el procedimiento almacenado extendido con el proceso que realiza la llamada.  
  
 En el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], los procedimientos almacenados escritos mediante CLR son más seguros, escalables y estables que los procedimientos almacenados extendidos. Los procedimientos almacenados CLR utilizan el objeto **SqlContext** para combinar el contexto de la sesión de llamada en lugar de `sp_bindsession`.  
  
 Se pueden utilizar sesiones enlazadas para desarrollar aplicaciones de tres niveles en las que la lógica comercial se incorpora mediante programas independientes que funcionan en colaboración en una sola transacción comercial. Estos programas deben codificarse de forma que coordinen con precisión su acceso a la base de datos. Dado que las dos sesiones comparten los mismos bloqueos, ambos programas deben evitar intentar modificar los mismos datos a la vez. En cualquier momento solo puede haber una sesión que realice el trabajo como parte de la transacción. No se permiten ejecuciones en paralelo. La transacción solo se puede cambiar entre sesiones y en puntos de rendimiento bien definidos, como el momento en que han finalizado todas las instrucciones DML y se han recuperado todos los resultados.  
  
### <a name="coding-efficient-transactions"></a>Codificar transacciones eficaces  
 Es importante que las transacciones sean tan cortas como sea posible. Cuando se inicia una transacción, un sistema de administración de bases de datos (DBMS) debe contener muchos recursos hasta el final de la transacción para proteger las propiedades de atomicidad, coherencia, aislamiento y durabilidad (ACID) de la transacción. Si se modifican datos, se deben proteger las filas modificadas con bloqueos exclusivos que impidan que otra transacción lea las filas, y se deben mantener bloqueos exclusivos hasta que se confirme o se revierta la transacción. Dependiendo de la configuración del nivel de aislamiento de la transacción, las instrucciones `SELECT` pueden adquirir bloqueos que deben mantenerse hasta que la transacción se confirme o se revierta. Especialmente en sistemas con muchos usuarios, las transacciones deben ser tan cortas como sea posible para reducir el conflicto de bloqueos de recursos entre conexiones simultáneas. Es posible que las transacciones de larga duración y poco eficaces no constituyan un problema cuando hay un pequeño número de usuarios, pero son intolerables en sistemas con miles de usuarios. A partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] admite las transacciones durables diferidas. Las transacciones durables diferidas no garantizan la durabilidad. Vea el tema [Durabilidad de las transacciones](../relational-databases/logs/control-transaction-durability.md) para obtener más información.  
  
#### <a name="coding-guidelines"></a><a name="guidelines"></a> Instrucciones de codificación  
 A continuación se muestran las instrucciones para codificar transacciones eficaces:  
  
-   No pida entradas de los usuarios durante una transacción.  
    Obtenga todas las entradas necesarias de los usuarios antes de iniciar la transacción. Si se necesita una entrada adicional del usuario durante una transacción, revierta la transacción actual y reinicie la transacción después de que el usuario proporcione la entrada. Aunque los usuarios respondan inmediatamente, los tiempos de reacción de las personas son mucho menores que la velocidad del equipo. Todos los recursos que mantiene la transacción se conservan durante un tiempo extremadamente largo que, en potencia, puede provocar problemas de bloqueos. Si los usuarios no responden, la transacción permanece activa y bloquea recursos críticos hasta que lo hagan, lo que puede tardar en suceder varios minutos o incluso horas.  
  
-   No abra una transacción mientras examina los datos si es posible.  
    No puede iniciar las transacciones hasta que haya completado todos los análisis preliminares de datos.  
  
-   Haga la transacción lo más corta posible.  
    Una vez que sepa las modificaciones que debe realizar, inicie una transacción, ejecute las instrucciones de modificación e, inmediatamente, confirme o revierta las operaciones. No abra la transacción antes de que sea necesario.  
  
-   Para reducir el bloqueo, considere la posibilidad de utilizar un nivel de aislamiento basado en versiones de fila para las consultas de solo lectura.  
  
-   Haga un uso inteligente de los niveles más bajos de aislamiento de las transacciones.  
    Se pueden codificar rápidamente muchas aplicaciones para que utilicen un nivel de aislamiento de lectura confirmada de las transacciones. No todas las transacciones necesitan el nivel de aislamiento serializable de las transacciones.  
  
-   Haga un uso inteligente de las opciones de simultaneidad más bajas de los cursores, como las opciones de simultaneidad optimista.  
    En un sistema que tenga pocas probabilidades de que se produzcan actualizaciones simultáneas, la sobrecarga que produce encontrar el error ocasional "alguien cambió los datos después de su lectura" puede ser mucho menor que la sobrecarga que produce bloquear siempre las filas a medida que se leen.  
  
-   Tenga acceso a la menor cantidad de datos posible en una transacción.  
    Así reduce el número de filas bloqueadas y, por lo tanto, disminuye el conflicto entre transacciones.  
    
-   Evite las sugerencias de bloqueo pesimistas, como "holdlock", siempre que sea posible. 
    Las sugerencias como HOLDLOCK o el nivel de aislamiento SERIALIZABLE pueden provocar que los procesos esperen incluso en bloqueos compartidos y que se reduzca la simultaneidad.

-   Evite el uso de transacciones implícitas cuando tales transacciones implícitas puedan presentar un comportamiento imprevisible debido a su naturaleza. Consulte [Transacciones implícitas y prevención de problemas de simultaneidad y de recursos](#implicit-transactions-and-avoiding-concurrency-and-resource-problems).

-   Diseñe índices con un [factor de relleno](indexes/specify-fill-factor-for-an-index.md) reducido. El hecho de reducir el factor de relleno puede ayudarle a evitar o disminuir la fragmentación de las páginas de índice y, por tanto, reducir los tiempos de búsqueda de índices, especialmente cuando se recuperan del disco. Para ver información sobre la fragmentación de los datos e índices de una tabla o vista, puede usar sys.dm_db_index_physical_stats. 
  
#### <a name="implicit-transactions-and-avoiding-concurrency-and-resource-problems"></a>Transacciones implícitas y prevención de problemas de simultaneidad y de recursos  
 Para evitar los problemas de simultaneidad y de recursos, administre cuidadosamente las transacciones implícitas. Cuando utilice transacciones implícitas, la siguiente instrucción [!INCLUDE[tsql](../includes/tsql-md.md)] después de `COMMIT` o `ROLLBACK` inicia automáticamente una nueva transacción. Esto puede hacer que se abra una nueva transacción mientras la aplicación examina los datos o, incluso, cuando pide una entrada del usuario. Tras concluir la última transacción necesaria para proteger las modificaciones de los datos, desactive las transacciones implícitas hasta que se necesite de nuevo una transacción para proteger las modificaciones de los datos. Este proceso permite que el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilice el modo de confirmación automática mientras la aplicación examina los datos y obtiene la entrada del usuario.  
  
 Además, cuando el nivel de aislamiento de instantánea está habilitado, aunque una transacción nueva no mantenga bloqueos, una transacción de larga duración impedirá que las versiones anteriores se eliminen de `tempdb`.  
  
### <a name="managing-long-running-transactions"></a>Administrar las transacciones de ejecución prolongada  
 Una *transacción de larga duración* es una transacción activa que no se ha confirmado ni revertido puntualmente. Por ejemplo, si el usuario controla el inicio y la finalización de una transacción, una causa frecuente de las transacciones de ejecución prolongada es que un usuario inicie una transacción y se ausente mientras la transacción queda en espera de una respuesta suya.  
  
 Una transacción de larga duración puede provocar graves problemas para una base de datos de la siguiente manera:  
  
-   Si se cierra una instancia de servidor después de que una transacción activa haya realizado un gran número de modificaciones no confirmadas, la fase de recuperación del siguiente reinicio puede durar bastante más que el tiempo especificado en la opción de configuración del servidor **recovery interval** o la opción `ALTER DATABASE ... SET TARGET_RECOVERY_TIME`. Estas opciones controlan la frecuencia de puntos de comprobación activos e indirectos, respectivamente. Para obtener más información sobre los tipos de puntos de comprobación, consulte [Puntos de comprobación de base de datos &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md).  
  
-   Y, lo que es más importante, aunque la transacción en espera genere muy poco registro, la transacción retiene el truncamiento del registro de manera indefinida y provoca el excesivo crecimiento y llenado del mismo. Si el registro de la transacción se llena, la base de datos no puede realizar más actualizaciones. Para obtener más información, vea [Guía de arquitectura y administración de registros de transacciones de SQL Server](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md), [Solucionar problemas de un registro de transacciones lleno &#40;Error 9002 de SQL Server)&#41;](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md) y [El registro de transacciones &#40;SQL Server&#41;](../relational-databases/logs/the-transaction-log-sql-server.md).  
  
#### <a name="discovering-long-running-transactions"></a>Descubrir transacciones de ejecución prolongada  
 Para buscar las transacciones de ejecución prolongada, use una de las opciones siguientes:  
  
-   **sys.dm_tran_database_transactions**  
  
    Esta vista de administración dinámica devuelve información sobre las transacciones en la base de datos. En una transacción de ejecución prolongada, las columnas de especial interés incluyen la hora de la primera entrada del registro (**database_transaction_begin_time**), el estado actual de la transacción (**database_transaction_state**) y el número de flujo de registro (LSN) del registro inicial del registro de transacciones (**database_transaction_begin_lsn**).  
  
    Para obtener más información, consulte [sys.dm_tran_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md).  
  
-   `DBCC OPENTRAN`  
  
    Esta instrucción permite identificar el Id. de usuario del propietario de la transacción, por lo que se puede realizar un seguimiento del origen de la misma para terminarla de forma más ordenada (confirmándola en lugar de revirtiéndola). Para obtener más información, vea [DBCC OPENTRAN &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-opentran-transact-sql.md).  
  
#### <a name="stopping-a-transaction"></a>Detener una transacción  
 Puede que deba utilizar la instrucción KILL. Sin embargo, utilice esta instrucción con sumo cuidado, especialmente cuando se estén ejecutando procesos críticos. Para obtener más información, consulte [KILL &#40;Transact-SQL&#41;](../t-sql/language-elements/kill-transact-sql.md).  
  
##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Lecturas adicionales   
[Overhead of Row Versioning](/archive/blogs/sqlserverstorageengine/overhead-of-row-versioning)  (Sobrecarga de las versiones de fila)  
[Eventos extendidos](../relational-databases/extended-events/extended-events.md)   
[sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)     
[Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)      
[Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)
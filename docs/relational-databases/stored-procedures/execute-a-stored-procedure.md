---
title: Ejecutar un procedimiento almacenado | Microsoft Docs
description: Obtenga información sobre cómo ejecutar un procedimiento almacenado en SQL Server 2019 (15.x) mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
f1_keywords:
- sql13.swb.executeprocedure.general.f1
- sql13.swb.executeprocedure.f1
helpviewer_keywords:
- stored procedures [SQL Server], parameters
- extended stored procedures [SQL Server], executing
- system stored procedures [SQL Server], executing
- stored procedures [SQL Server], executing
- user-defined stored procedures [SQL Server]
ms.assetid: a0b1337d-2059-4872-8c62-3f967d8b170f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 115220ef0fabbe98a0c4a1bf5f185409c5a82829
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250289"
---
# <a name="execute-a-stored-procedure"></a>Ejecutar un procedimiento almacenado
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo ejecutar un procedimiento almacenado en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Hay dos formas diferentes de ejecutar un procedimiento almacenado. El primer método y más común es que una aplicación o un usuario llame al procedimiento. El segundo método consiste en establecer el procedimiento para que se ejecute automáticamente cuando se inicie una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando una aplicación o un usuario llama a un procedimiento, la palabra clave EXECUTE o EXEC de [!INCLUDE[tsql](../../includes/tsql-md.md)] se indica explícitamente en la llamada. Como alternativa, se puede llamar al procedimiento y ejecutarlo sin la palabra clave si el procedimiento es la primera instrucción del lote de [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para ejecutar un procedimiento almacenado, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   La intercalación de base de datos de llamada se usa al comparar los nombres de los procedimientos del sistema. Por tanto, en las llamadas a procedimientos use siempre el modelo exacto de mayúsculas y minúsculas de los nombres de procedimientos del sistema. Por ejemplo, este código generará un error si se ejecuta en el contexto de una base de datos que tenga una intercalación que distinga mayúsculas de minúsculas:  
  
    ```sql  
    EXEC SP_heLP; -- Will fail to resolve because SP_heLP does not equal sp_help  
    ```  
  
     Para mostrar los nombres exactos de los procedimientos del sistema, consulte las vistas de catálogo [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) y [sys.system_parameters](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md) .  
  
-   Si un procedimiento definido por el usuario tiene el mismo nombre que un procedimiento del sistema, puede que el procedimiento definido por el usuario no se ejecute nunca.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Ejecutar procedimientos almacenados del sistema  
  
     Los procedimientos del sistema comienzan por el prefijo **sp_** . Puesto que aparecen lógicamente en todas las bases de datos definidas por el usuario y por el sistema, se pueden ejecutar desde cualquier base de datos sin necesidad de calificar totalmente el nombre del procedimiento. Pero se recomienda calificar como de esquema todos los nombres de procedimientos del sistema con el nombre de esquema **sys** para evitar conflictos de nombres. En el ejemplo siguiente se muestra el método recomendado para llamar a un procedimiento del sistema.  
  
    ```sql  
    EXEC sys.sp_who;  
    ```  
  
-   Ejecutar procedimientos almacenados definidos por el usuario  
  
     Al ejecutar un procedimiento definido por el usuario, se recomienda calificar el nombre del procedimiento con el nombre de esquema. Esta práctica proporciona un pequeño aumento del rendimiento porque el [!INCLUDE[ssDE](../../includes/ssde-md.md)] no tiene que buscar en varios esquemas. También evita la ejecución del procedimiento incorrecto si una base de datos tiene procedimientos con el mismo nombre en varios esquemas.  
  
     En el ejemplo siguiente se muestra el método recomendado para ejecutar un procedimiento definido por el usuario. Observe que el procedimiento acepta un parámetro de entrada. Para obtener más información sobre cómo especificar parámetros de entrada y salida, vea [Especificar parámetros](../../relational-databases/stored-procedures/specify-parameters.md).  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    EXEC dbo.uspGetEmployeeManagers @BusinessEntityID = 50;  
    ```  
  
     -O bien-  
  
    ```sql  
    EXEC AdventureWorks2012.dbo.uspGetEmployeeManagers 50;  
    GO  
    ```  
  
     Si se especifica un procedimiento definido por el usuario no calificado, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] busca el procedimiento siguiendo este orden:  
  
    1.  El esquema **sys** de la base de datos actual.  
  
    2.  El esquema predeterminado del autor de la llamada si se ejecuta en un lote o en SQL dinámico. O bien, si el nombre del procedimiento no calificado aparece dentro del cuerpo de otra definición de procedimiento, a continuación se busca en el esquema que contiene este otro procedimiento.  
  
    3.  El esquema **dbo** de la base de datos actual.  
  
-   Ejecutar procedimientos almacenados automáticamente  
  
     Los procedimientos marcados para su ejecución automática se ejecutan cada vez que se inicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y la base de datos **maestra** se recupera durante ese proceso de inicio. Puede ser útil configurar procedimientos para que se ejecuten automáticamente a la hora de realizar operaciones de mantenimiento de bases de datos o para tener procedimientos que se ejecutan continuamente como procesos en segundo plano. Otra forma de usar la ejecución automática consiste en que el procedimiento realice tareas del sistema o de mantenimiento en **tempdb**, como crear una tabla temporal global. De este modo, se garantiza que esa tabla temporal existirá siempre cuando se vuelva a crear **tempdb** durante el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Un procedimiento que se ejecuta automáticamente funciona con los mismos permisos que los miembros del rol fijo de servidor **sysadmin** . Todos los mensajes de error generados por el procedimiento se escriben en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     No existe límite en cuanto al número de procedimientos de inicio que se pueden crear, aunque debe tener en cuenta que cada uno consume un subproceso de trabajo mientras se ejecuta. Si es necesario ejecutar múltiples procedimientos en el inicio, pero no es necesario que se ejecuten en paralelo, haga que un procedimiento sea el procedimiento de inicio y que éste llame a los restantes. De este modo, solo se utiliza un subproceso de trabajo.  
  
    > [!TIP]  
    >  No se devuelve ningún conjunto de resultados de un procedimiento que se ejecuta automáticamente. Puesto que el responsable de ejecutar el procedimiento es [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y no una aplicación o un usuario, no existe ningún destino para el conjunto de resultados.  
  
-   Establecer, borrar y controlar la ejecución automática  
  
     Solo el administrador del sistema (**sa**) puede marcar un procedimiento para que se ejecute automáticamente. Además, el procedimiento debe encontrarse en la base de datos **maestra** , pertenecer a **sa** y no incluir parámetros de entrada ni de salida.  
  
     Use [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) para:  
  
    1.  Designar un procedimiento existente como procedimiento de inicio.  
  
    2.  Detener la ejecución de un procedimiento al iniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Para obtener más información, vea [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md) y [EXECUTE AS &#40;Cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para obtener más información, vea la sección "Permisos" de [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-execute-a-stored-procedure"></a>Para ejecutar un procedimiento almacenado  
  
1.  En el **Explorador de objetos**, conéctese a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], expándala y, a continuación, expanda **Bases de datos**.  
  
2.  Expanda la base de datos que desee, expanda **Programación** y, a continuación, expanda **Procedimientos almacenados**.  
  
3.  Haga clic con el botón derecho en el procedimiento almacenado definido por el usuario que quiera y, luego, haga clic en **Ejecutar procedimiento almacenado**.  
  
4.  En el cuadro de diálogo **Ejecutar procedimiento** , especifique un valor para cada parámetro y si debe pasar o no un valor NULL.  
  
     **Parámetro**  
     Indica el nombre del parámetro.  
  
     **Tipo de datos**  
     Indica el tipo de datos del parámetro.  
  
     **Parámetro de salida**  
     Indica si se trata de un parámetro de salida.  
  
     **Pasar valor NULL**  
     Pase un valor NULL como valor del parámetro.  
  
     **Valor**  
     Escriba el valor del parámetro cuando llame al procedimiento.  
  
5.  Para ejecutar el procedimiento almacenado, haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-execute-a-stored-procedure"></a>Para ejecutar un procedimiento almacenado  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo ejecutar un procedimiento almacenado que espera un parámetro. En el ejemplo se ejecuta el procedimiento almacenado `uspGetEmployeeManagers` con el valor  `6` como parámetro `@EmployeeID` .  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC dbo.uspGetEmployeeManagers 6;  
GO  
```  
  
#### <a name="to-set-or-clear-a-procedure-for-executing-automatically"></a>Para establecer o borrar un procedimiento para que se ejecute automáticamente  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) para establecer un procedimiento de manera que se ejecute automáticamente.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_procoption @ProcName = '<procedure name>'   
    , @OptionName = 'startup'   
    , @OptionValue = 'on';  
```  
  
#### <a name="to-stop-a-procedure-from-executing-automatically"></a>Para que un procedimiento deje de ejecutarse automáticamente  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) para que un procedimiento deje de ejecutarse automáticamente.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_procoption @ProcName = '<procedure name>'   
    , @OptionValue = 'off';  
```  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
  
## <a name="see-also"></a>Consulte también  
 [Especificar parámetros](../../relational-databases/stored-procedures/specify-parameters.md)   
 [Establecer la opción de configuración del servidor Buscar procedimientos de inicio](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Procedimientos almacenados &#40;motor de base de datos&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)  
  
  

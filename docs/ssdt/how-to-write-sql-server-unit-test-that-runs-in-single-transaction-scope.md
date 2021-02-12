---
title: Escritura de una prueba unitaria de SQL Server que se ejecuta en el ámbito de una única transacción
description: Obtenga información sobre cómo iniciar el servicio Coordinador de transacciones distribuidas, escribir una prueba unitaria de SQL Server de una sola transacción y revertir los cambios de las pruebas.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cb241e94-d81c-40e9-a7ae-127762a6b855
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6c03450778defd73b21c19d72aa1e0acc6af342b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017945"
---
# <a name="how-to-write-a-sql-server-unit-test-that-runs-within-the-scope-of-a-single-transaction"></a>Procedimientos: Escritura de una prueba unitaria de SQL Server que se ejecuta en el ámbito de una única transacción

Puede modificar las pruebas unitarias para ejecutarlas en el ámbito de una única transacción. Si elige este enfoque, puede revertir los cambios activados por la prueba una vez finalizada esta. Los procedimientos siguientes explican cómo:  
  
-   Cree una transacción en el script de prueba Transact\-SQL que usa **BEGIN TRANSACTION** y **ROLLBACK TRANSACTION**.  
  
-   Crear una transacción para un único método de prueba en una clase de prueba.  
  
-   Crear una transacción para todos los métodos de prueba en una clase de prueba determinada.  
  
**Requisitos previos**  
  
Para algunos procedimientos de este tema, el servicio Coordinador de transacciones distribuidas se debe ejecutar en el equipo en el que se ejecutan las pruebas unitarias. Para obtener más información, vea el procedimiento al final de este tema.  
  
## <a name="to-create-a-transaction-using-transact-sql"></a>Para crear una transacción mediante Transact\-SQL  
  
#### <a name="to-create-a-transaction-using-transact-sql"></a>Para crear una transacción mediante Transact\-SQL  
  
1.  Abra una prueba unitaria en el Diseñador de pruebas unitarias de SQL Server. (Haga doble clic en el archivo de código fuente de la prueba unitaria para mostrar el diseñador).  
  
2.  Especifique el tipo de script para el que desea crear la transacción. Por ejemplo, puede especificar anterior a la prueba, prueba o posterior a la prueba.  
  
3.  Escriba un script de prueba en el editor de Transact\-SQL.  
  
4.  Inserte las instrucciones `BEGIN TRANSACTION` y `ROLLBACK TRANSACTION`, tal como se muestra en este ejemplo simple. En el ejemplo se usa una tabla de base de datos denominada OrderDetails que contiene 50 filas de datos:  
  
    ```  
    BEGIN TRANSACTION TestTransaction  
    UPDATE "OrderDetails" set Quantity = Quantity + 10  
    IF @@ROWCOUNT!=50  
    RAISERROR('Row count does not equal 50',16,1)  
    ROLLBACK TRANSACTION TestTransaction  
    ```  
  
    > [!NOTE]  
    > No se puede revertir una transacción después de ejecutar una instrucción COMMIT TRANSACTION.  
  
    Para más información sobre cómo funciona ROLLBACK TRANSACTION con los procedimientos almacenados y los desencadenadores, consulte esta página en el sitio web de Microsoft: [ROLLBACK TRANSACTION (Transact-SQL)](../t-sql/language-elements/rollback-transaction-transact-sql.md).  
  
## <a name="to-create-a-transaction-for-a-single-test-method"></a>Para crear una transacción para un único método de prueba  
En este ejemplo se usa una transacción ambiente cuando se usa el tipo [System.Transactions.TransactionScope](/dotnet/api/system.transactions.transactionscope). De forma predeterminada, las conexiones de ejecución y privilegiadas no usarán la transacción ambiente, dado que las conexiones se crearon antes de ejecutar el método. SqlConnection tiene un método [System.Data.SqlClient.SqlConnection.EnlistTransaction](/dotnet/api/system.data.sqlclient.sqlconnection.enlisttransaction), el que asocia una conexión activa a una transacción. Cuando se crea una transacción ambiente, se registra como la transacción actual y se puede tener acceso a ella a través de la propiedad [System.Transactions.Transaction.Current](/dotnet/api/system.transactions.transaction.current). En este ejemplo, la transacción se revierte cuando se elimina la transacción ambiente. Si quiere confirmar cualquier cambio realizado cuando se ejecutó la prueba unitaria, debe llamar al método [System.Transactions.TransactionScope.Complete](/dotnet/api/system.transactions.transactionscope.complete).  
  
#### <a name="to-create-a-transaction-for-a-single-test-method"></a>Para crear una transacción para un único método de prueba  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en el nodo **Referencias** del proyecto de prueba y, a continuación, haga clic en **Agregar referencia**.  
  
    Aparecerá el cuadro de diálogo **Agregar referencia**.  
  
2.  Haga clic en la pestaña **.NET**.  
  
3.  En la lista de ensamblados, haga clic en **System.Transactions** y en **Aceptar**.  
  
4.  Abra el archivo de Visual Basic o C# de la prueba unitaria.  
  
5.  Ajuste las acciones anteriores a la prueba, de prueba y posteriores a la prueba como se muestra en el siguiente ejemplo de código de Visual Basic:  
  
    ```  
    <TestMethod()> _  
    Public Sub dbo_InsertTable1Test()  
  
        Using ts as New System.Transactions.TransactionScope( System.Transactions.TransactionScopeOption.Required)  
            ExecutionContext.Connection.EnlistTransaction(Transaction.Current)  
            PrivilegedContext.Connection.EnlistTransaction(Transaction.Current)  
  
            Dim testActions As DatabaseTestActions = Me.dbo_InsertTable1TestData  
            'Execute the pre-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PretestAction) Is Nothing), "Executing pre-test script...")  
            Dim pretestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PretestAction)  
            'Execute the test script  
  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.TestAction) Is Nothing), "Executing test script...")  
            Dim testResults() As ExecutionResult = TestService.Execute(ExecutionContext, Me.PrivilegedContext, testActions.TestAction)  
  
            'Execute the post-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PosttestAction) Is Nothing), "Executing post-test script...")  
            Dim posttestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PosttestAction)  
  
            'Because the transaction is not explicitly committed, it  
            'is rolled back when the ambient transaction is   
            'disposed.  
            'To commit the transaction, remove the comment delimiter  
            'from the following statement:  
            'ts.Complete()  
  
    End Sub  
    Private dbo_InsertTable1TestData As DatabaseTestActions  
    ```  
  
    > [!NOTE]  
    > Si usa Visual Basic, debe agregar `Imports System.Transactions` (además de `Imports Microsoft.VisualStudio.TestTools.UnitTesting`, `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTesting` y `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTest.Conditions`). Si usa Visual C#, debe agregar `using System.Transactions` (además de las instrucciones `using` para Microsoft.VisualStudio.TestTools, Microsoft.VisualStudio.TeamSystem.Data.UnitTesting y Microsoft.VisualStudio.TeamSystem.Data.UnitTesting.Conditions). También debe agregar una referencia al proyecto a esos ensamblados.  
  
## <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>Para crear una transacción para todos los métodos de prueba en una clase de prueba  
  
#### <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>Para crear una transacción para todos los métodos de prueba en una clase de prueba  
  
1.  Abra el archivo de Visual Basic o C# de la prueba unitaria.  
  
2.  Cree la transacción en TestInitialize y elimínela en TestCleanup, tal como se muestra en el siguiente ejemplo de código de Visual C#:  
  
    ```  
    TransactionScope _trans;  
  
            [TestInitialize()]  
            public void Init()  
            {  
                _trans = new TransactionScope();  
                base.InitializeTest();  
            }  
  
            [TestCleanup()]  
            public void Cleanup()  
            {  
                base.CleanupTest();  
                _trans.Dispose();  
            }  
  
            [TestMethod()]  
            public void TransactedTest()  
            {  
                DatabaseTestActions testActions = this.DatabaseTestMethod1Data;  
                // Execute the pre-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PretestAction != null), "Executing pre-test script...");  
                ExecutionResult[] pretestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PretestAction);  
                // Execute the test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.TestAction != null), "Executing test script...");  
                ExecutionResult[] testResults = TestService.Execute(this.ExecutionContext, this.PrivilegedContext, testActions.TestAction);  
                // Execute the post-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PosttestAction != null), "Executing post-test script...");  
                ExecutionResult[] posttestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PosttestAction);  
  
            }  
    ```  
  
## <a name="to-start-the-distributed-transaction-coordinator-service"></a>Para iniciar el servicio Coordinador de transacciones distribuidas  
En algunos procedimientos de este tema se usan los tipos del ensamblado System.Transactions. Antes de seguir estos procedimientos, debe asegurarse de que el servicio Coordinador de transacciones distribuidas se ejecuta en el equipo donde se ejecutan las pruebas unitarias. De lo contrario, se producirá un error en las pruebas y se mostrará el siguiente mensaje de error: "El método de prueba *NombreDeProyecto*.*NombreDePrueba*.*NombreDeMétodo* produjo una excepción: System.Data.SqlClient.SqlException: MSDTC en el servidor '*NombreDeEquipo*' no está disponible".  
  
#### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Para iniciar el servicio Coordinador de transacciones distribuidas  
  
1.  Abra el **Panel de control**.  
  
2.  En el **Panel de control**, abra **Herramientas administrativas**.  
  
3.  En **Herramientas administrativas**, abra **Servicios**.  
  
4.  En el panel **Servicios**, haga clic con el botón derecho en el servicio **Controlador de transacciones distribuidas** y haga clic en **Iniciar**.  
  
    El estado del servicio debe actualizarse a **Iniciado**. Ahora debería poder ejecutar las pruebas unitarias que usan System.Transactions.  
  
> [!IMPORTANT]  
> El siguiente error podría aparecer incluso si ha iniciado el servicio Controlador de transacciones distribuidas: `System.Transactions.TransactionManagerCommunicationException: Network access for Distributed Transaction Manager (MSDTC) has been disabled. Please enable DTC for network access in the security configuration for MSDTC using the Component Services Administrative tool. ---> System.Runtime.InteropServices.COMException: The transaction manager has disabled its support for remote/network transactions. (Exception from HRESULT: 0x8004D024)`. Si aparece este error, debe configurar el Controlador de transacciones distribuidas para el acceso de red. Para más información, consulte [Habilitación del acceso a DTC desde la red](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753510(v=ws.10)).  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  

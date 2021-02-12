---
title: Ampliación de la implementación del proyecto de base de datos para modificar el plan de implementación
description: Cree un colaborador de implementación de tipo DeploymentPlanModifier que programe la nueva ejecución de lotes de script de implementación si se producen errores durante la ejecución.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 22b077b1-fa25-49ff-94f6-6d0d196d870a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bdb9831caf2145b43340fdca9af3108ba20d3c34
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064960"
---
# <a name="walkthrough-extend-database-project-deployment-to-modify-the-deployment-plan"></a>Tutorial: Ampliación de la implementación del proyecto de base de datos para modificar el plan de implementación

Puede crear colaboradores de implementación para realizar acciones personalizadas al implementar un proyecto de SQL. Puede crear un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) o un [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). Utilice un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) para cambiar el plan antes de ejecutarlo y un [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor) para realizar operaciones mientras se ejecuta el plan. En este tutorial, creará un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) denominado SqlRestartableScriptContributor que agrega instrucciones IF a los lotes del script de implementación para permitir volver a ejecutar el script hasta que se completen en el caso de que se produzca un error durante la ejecución.  
  
En este tutorial, realizará las principales tareas siguientes:  
  
-   [Crear el tipo de colaborador de implementación DeploymentPlanModifier](#CreateDeploymentContributor)  
  
-   [Instalar el colaborador de implementación](#InstallDeploymentContributor)  
  
-   [Ejecutar o comprobar su colaborador de implementación](#TestDeploymentContributor)  
  
## <a name="prerequisites"></a>Requisitos previos  
Necesitará los componentes siguientes para completar este tutorial:  
  
-   Debe haber instalado una versión de Visual Studio que incluya SQL Server Data Tools y admita el desarrollo de C# o VB.  
  
-   Debe disponer de un proyecto de SQL que contenga los objetos SQL.  
  
-   Una instancia de SQL Server a la que pueda implementar un proyecto de base de datos.  
  
> [!NOTE]  
> Este tutorial está destinado a usuarios que ya están familiarizados con las características de SQL de SQL Server Data Tools. También se espera que esté familiarizado con los conceptos básicos de Visual Studio, como cómo crear una biblioteca de clases y cómo utilizar el editor de código para agregar código a una clase.  
  
## <a name="create-a-deployment-contributor"></a><a name="CreateDeploymentContributor"></a>Crear un colaborador de implementación  
Para crear un colaborador de implementación, debe realizar las siguientes tareas:  
  
-   Cree un proyecto de biblioteca de clases y agregue las referencias necesarias.  
  
-   Defina una clase denominada SqlRestartableScriptContributor que herede de [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier).  
  
-   Invalide el método [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute).  
  
-   Agregue métodos del asistente privados.  
  
-   Compile el ensamblado resultante.  
  
#### <a name="to-create-a-class-library-project"></a>Para crear un proyecto de biblioteca de clases  
  
1.  Cree un proyecto de bibliotecas de clases de Visual C# o Visual Basic llamado MyOtherDeploymentContributor.  
  
2.  Cambie el nombre del archivo “Class1.cs” a “SqlRestartableScriptContributor.cs”.  
  
3.  En el Explorador de soluciones, haga clic con el botón derecho en el nodo de proyecto y, a continuación, haga clic en **Agregar referencia**.  
  
4.  En la pestaña Frameworks, seleccione **System.ComponentModel.Composition**.  
  
5.  Haga clic en **Examinar** y vaya al directorio **C:\Archivos de programa (x86)\Microsoft SQL Server\110\SDK\Assemblies**, seleccione **Microsoft.SqlServer.TransactSql.ScriptDom.dll** y haga clic en **Aceptar**.  
  
6.  Agregue las referencias SQL necesarias: haga clic con el botón derecho en el nodo de proyecto y, a continuación, haga clic en **Agregar referencia**. Haga clic en **Examinar** y navegue a la carpeta **C:\Archivos de programa (x86)\Microsoft SQL Server\110\DAC\Bin**. Elija las entradas **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** y **Microsoft.Data.Tools.Schema.Sql.dll** y haga clic en **Agregar** y, a continuación, haga clic en **Aceptar**.  
  
A continuación, empiece a agregar el código a la clase.  
  
#### <a name="to-define-the-sqlrestartablescriptcontributor-class"></a>Para definir la clase SqlRestartableScriptContributor  
  
1.  En el editor de código, actualice el archivo class1.cs para que coincidan las siguientes instrucciones **using**:  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;  
    using System.Text;  
    using Microsoft.SqlServer.Dac.Deployment;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    ```  
  
2.  Actualice la definición de clases para que coincida con el ejemplo siguiente:  
  
    ```csharp  
        /// <summary>  
    /// This deployment contributor modifies a deployment plan by adding if statements  
    /// to the existing batches in order to make a deployment script able to be rerun to completion  
    /// if an error is encountered during execution  
    /// </summary>  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
    public class SqlRestartableScriptContributor : DeploymentPlanModifier  
    {  
    }  
  
    ```  
  
    Ahora ha definido un colaborador de implementación que hereda del [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier). Durante los procesos de compilación e implementación, los colaboradores personalizados se cargan desde un directorio de la extensión estándar. Los colaboradores que modifican el plan de implementación se identifican con un atributo [ExportDeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.exportdeploymentplanmodifierattribute). Este atributo es necesario para poder detectar colaboradores. Este atributo deberá ser parecido al de la ilustración siguiente:  
  
    ```csharp  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
  
    ```  
  
3.  Agregue las declaración de miembro siguientes:  
  
    ```vb  
         private const string BatchIdColumnName = "BatchId";  
            private const string DescriptionColumnName = "Description";  
  
            private const string CompletedBatchesVariableName = "CompletedBatches";  
            private const string CompletedBatchesVariable = "$(CompletedBatches)";  
            private const string CompletedBatchesSqlCmd = @":setvar " + CompletedBatchesVariableName + " __completedBatches_{0}_{1}";  
            private const string TotalBatchCountSqlCmd = @":setvar TotalBatchCount {0}";  
            private const string CreateCompletedBatchesTable = @"  
    if OBJECT_ID(N'tempdb.dbo." + CompletedBatchesVariable + @"', N'U') is null  
    begin  
    use tempdb  
    create table [dbo].[$(CompletedBatches)]  
    (  
    BatchId int primary key,  
    Description nvarchar(300)  
    )  
    use [$(DatabaseName)]  
    end  
    ";  
  
    ```  
  
    A continuación, invalide el método de OnExecute para agregar el código que desea ejecutar cuando se implementa un proyecto de base de datos.  
  
#### <a name="to-override-onexecute"></a>Para invalidar OnExecute  
  
1.  Agregue el método siguiente a la clase SqlRestartableScriptContributor:  
  
    ```csharp  
    /// <summary>  
    /// You override the OnExecute method to do the real work of the contributor.  
    /// </summary>  
    /// <param name="context"></param>  
    protected override void OnExecute(DeploymentPlanContributorContext context)  
    {  
         // Replace this with the method body  
    }  
  
    ```  
  
    Invalide el método [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute) de la clase base, [DeploymentPlanContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor), que es la clase base tanto para [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) como para [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). El método OnExecute se pasa a un objeto [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext) que proporciona acceso a cualquier argumento especificado, al modelo de base de datos de origen y de destino, al plan de implementación y a las opciones de implementación. En este ejemplo, obtenemos el plan de implementación y el nombre de la base de datos de destino.  
  
2.  Ahora agregue los principios de un cuerpo el método OnExecute:  
  
    ```vb  
    // Obtain the first step in the Plan from the provided context  
    DeploymentStep nextStep = context.PlanHandle.Head;  
    int batchId = 0;  
    BeginPreDeploymentScriptStep beforePreDeploy = null;  
  
    // Loop through all steps in the deployment plan  
    while (nextStep != null)  
    {  
        // Increment the step pointer, saving both the current and next steps  
        DeploymentStep currentStep = nextStep;  
        nextStep = currentStep.Next;  
  
        // Add additional step processing here  
    }  
  
    // if we found steps that required processing, set up a temporary table to track the work that you are doing  
    if (beforePreDeploy != null)  
    {  
        // Add additional post-processing here  
    }  
  
    // Cleanup and drop the table   
    DeploymentScriptStep dropStep = new DeploymentScriptStep(DropCompletedBatchesTable);  
    base.AddAfter(context.PlanHandle, context.PlanHandle.Tail, dropStep);  
  
    ```  
  
    En este código, definimos algunas variables locales, y configuramos el bucle que controlará el procesamiento de todos los pasos del plan de implementación. Después de que el bucle se complete, tendremos que hacer procesamiento posterior, y, a continuación, quitaremos la tabla temporal que creamos durante la implementación para realizar el seguimiento del progreso mientras el plan se ejecuta. Los tipos de clave aquí son: [DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep) y [DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep). Un método clave es AddAfter.  
  
3.  Ahora agregue el procesamiento del paso adicional, para reemplazar el comentario “Agregar el procesamiento del paso adicional aquí”:  
  
    ```csharp  
    // Look for steps that mark the pre/post deployment scripts  
    // These steps will always be in the deployment plan even if the  
    // user's project does not have a pre/post deployment script  
    if (currentStep is BeginPreDeploymentScriptStep)  
    {  
        // This step marks the beginning of the predeployment script.  
        // Save the step and move on.  
        beforePreDeploy = (BeginPreDeploymentScriptStep)currentStep;  
        continue;  
    }  
    if (currentStep is BeginPostDeploymentScriptStep)  
    {  
        // This is the step that marks the beginning of the post deployment script.    
        // We do not continue processing after this point.  
        break;  
    }  
    if (currentStep is SqlPrintStep)  
    {  
        // We do not need to put if statements around these  
        continue;  
    }  
  
    // if we have not yet found the beginning of the pre-deployment script steps,   
    // skip to the next step.  
    if (beforePreDeploy == null)  
    {  
        // We only surround the "main" statement block with conditional  
        // statements  
        continue;  
    }  
  
    // Determine if this is a step that we need to surround with a conditional statement  
    DeploymentScriptDomStep domStep = currentStep as DeploymentScriptDomStep;  
    if (domStep == null)  
    {  
        // This step is not a step that we know how to modify,  
        // so skip to the next step.  
        continue;  
    }  
  
    TSqlScript script = domStep.Script as TSqlScript;  
    if (script == null)  
    {  
        // The script dom step does not have a script with batches - skip  
        continue;  
    }  
  
        // Loop through all the batches in the script for this step.  All the statements  
        // in the batch will be enclosed in an if statement that will check the  
        // table to ensure that the batch has not already been executed  
        TSqlObject sqlObject;  
        string stepDescription;  
        GetStepInfo(domStep, out stepDescription, out sqlObject);  
        int batchCount = script.Batches.Count;  
  
    for (int batchIndex = 0; batchIndex < batchCount; batchIndex++)  
    {  
        // Add batch processing here  
    }  
  
    ```  
  
    Los comentarios de código explican el procesamiento. En un nivel superior, este código busca los pasos que le interesan, saltándose otros y deteniéndose cuando se alcanza el principio de los pasos posteriores a la implementación. Si el paso contiene instrucciones que debemos rodear con condicionales, realizaremos el procesamiento adicional. Entre las propiedades, métodos y tipos clave se encuentran los siguientes: [BeginPreDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpredeploymentscriptstep), [BeginPostDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpostdeploymentscriptstep), [TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject), [TSqlScript](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlscript), Script, [DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep) y [SqlPrintStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlprintstep).  
  
4.  Ahora, agregue el código de procesamiento por lotes reemplazando el comentario “Agregar procesamiento por lotes aquí”:  
  
    ```csharp  
        // Create the if statement that will contain the batch's contents  
        IfStatement ifBatchNotExecutedStatement = CreateIfNotExecutedStatement(batchId);  
        BeginEndBlockStatement statementBlock = new BeginEndBlockStatement();  
        ifBatchNotExecutedStatement.ThenStatement = statementBlock;  
        statementBlock.StatementList = new StatementList();  
  
        TSqlBatch batch = script.Batches[batchIndex];  
        int statementCount = batch.Statements.Count;  
  
        // Loop through all statements in the batch, embedding those in an sp_execsql  
        // statement that must be handled this way (schemas, stored procedures,   
        // views, functions, and triggers).  
        for (int statementIndex = 0; statementIndex < statementCount; statementIndex++)  
        {  
            // Add additional statement processing here  
        }  
  
        // Add an insert statement to track that all the statements in this  
        // batch were executed.  Turn on nocount to improve performance by  
        // avoiding row inserted messages from the server  
        string batchDescription = string.Format(CultureInfo.InvariantCulture,  
            "{0} batch {1}", stepDescription, batchIndex);  
  
        PredicateSetStatement noCountOff = new PredicateSetStatement();  
        noCountOff.IsOn = false;  
        noCountOff.Options = SetOptions.NoCount;  
  
        PredicateSetStatement noCountOn = new PredicateSetStatement();  
        noCountOn.IsOn = true;  
        noCountOn.Options = SetOptions.NoCount;   
        InsertStatement batchCompleteInsert = CreateBatchCompleteInsert(batchId, batchDescription);  
        statementBlock.StatementList.Statements.Add(noCountOn);  
    statementBlock.StatementList.Statements.Add(batchCompleteInsert);  
        statementBlock.StatementList.Statements.Add(noCountOff);  
  
        // Remove all the statements from the batch (they are now in the if block) and add the if statement  
        // as the sole statement in the batch  
        batch.Statements.Clear();  
        batch.Statements.Add(ifBatchNotExecutedStatement);  
  
        // Next batch  
        batchId++;  
  
    ```  
  
    Este código crea una instrucción IF junto con un bloque BEGIN/END. A continuación realizamos el procesamiento adicional en las instrucciones del lote. Una vez que realizado, agregamos una instrucción INSERT para agregar información a la tabla temporal que realiza el seguimiento del progreso de la ejecución del script. Finalmente, actualice el lote, reemplazando las instrucciones que solían estar con el nuevo IF que contiene esas instrucciones. Los tipos, métodos y propiedades clave son: [IfStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.ifstatement), [BeginEndBlockStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.beginendblockstatement), [StatementList](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.statementlist), [TSqlBatch](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlbatch), [PredicateSetStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.predicatesetstatement), [SetOptions](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.setoptions)y [InsertStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.insertstatement).  
  
5.  Ahora, agregue el cuerpo del bucle de procesamiento de instrucciones. Reemplace el comentario “Agregar el procesamiento de la instrucción adicional aquí”:  
  
    ```csharp  
    TSqlStatement smnt = batch.Statements[statementIndex];  
  
    if (IsStatementEscaped(sqlObject))  
    {  
        // "escape" this statement by embedding it in a sp_executesql statement  
        string statementScript;  
        domStep.ScriptGenerator.GenerateScript(smnt, out statementScript);  
        ExecuteStatement spExecuteSql = CreateExecuteSql(statementScript);  
        smnt = spExecuteSql;  
    }  
  
    statementBlock.StatementList.Statements.Add(smnt);  
  
    ```  
  
    Para cada instrucción del lote, si la instrucción es de un tipo que deba ajustarse a una instrucción sp_executesql, modifique la instrucción en consecuencia. El código agrega, a continuación, la instrucción a la lista de instrucciones para el bloque BEGIN/END que ha creado. Los tipos, métodos y propiedades clave son [TSqlStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlstatement) y [ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement).  
  
6.  Por último, agregue la sección del procesamiento posterior en lugar del comentario “Agregar el procesamiento posterior aquí”:  
  
    ```csharp  
    // Declare a SqlCmd variables.  
    //  
    // CompletedBatches variable - defines the name of the table in tempdb that will track  
    // all the completed batches.  The temporary table's name has the target database name and  
    // a guid embedded in it so that:  
    // * Multiple deployment scripts targeting different DBs on the same server  
    // * Failed deployments with old tables do not conflict with more recent deployments  
    //  
    // TotalBatchCount variable - the total number of batches surrounded by if statements.  Using this  
    // variable pre/post deployment scripts can also use the CompletedBatches table to make their  
    // script rerunnable if there is an error during execution  
    StringBuilder sqlcmdVars = new StringBuilder();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, CompletedBatchesSqlCmd,  
        context.Options.TargetDatabaseName, Guid.NewGuid().ToString("D"));  
    sqlcmdVars.AppendLine();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, TotalBatchCountSqlCmd, batchId);  
  
    DeploymentScriptStep completedBatchesSetVarStep = new DeploymentScriptStep(sqlcmdVars.ToString());  
    base.AddBefore(context.PlanHandle, beforePreDeploy, completedBatchesSetVarStep);  
  
    // Create the temporary table we will use to track the work that we are doing  
    DeploymentScriptStep createStatusTableStep = new DeploymentScriptStep(CreateCompletedBatchesTable);  
    base.AddBefore(context.PlanHandle, beforePreDeploy, createStatusTableStep);  
  
    ```  
  
    Si el procesamiento detecta uno o varios pasos que rodeamos con una instrucción condicional, debemos agregar instrucciones al script de implementación para definir variables SQLCMD. La primera variable, CompletedBatches, contiene un nombre único para la tabla temporal que el script de implementación utiliza para realizar un seguimiento de los lotes que se completaron correctamente cuando el script se ejecutó. La segunda variable, TotalBatchCount, contiene el número total de lotes en el script de implementación.  
  
    Los tipos, propiedades y métodos adicionales de interés son:  
  
    StringBuilder, [DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep) y AddBefore.  
  
    A continuación, define los métodos del asistente llamados por este método.  
  
#### <a name="to-add-the-helper-methods"></a>Para agregar los métodos del asistente  
  
-   Se deben definir varios métodos del asistente. Los métodos importantes son:  
  
    |**Método**|**Descripción**|  
    |--------------|-------------------|  
    |CreateExecuteSQL|Define el método CreateExecuteSQL para rodear una instrucción determinada mediante una instrucción sp_executesql EXEC. Los tipos, métodos, y propiedades clave son los siguientes: [ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement), [ExecutableProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executableprocedurereference), [SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname), [ProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.procedurereference)y [ExecuteParameter](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executeparameter).|  
    |CreateCompletedBatchesName|Define el método CreateCompletedBatchesName. Este método crea el nombre que se insertará en la tabla temporal para un lote. Los tipos, métodos, y propiedades clave son: [SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname).|  
    |IsStatementEscaped|Define el método IsStatementEscaped. Este método determina si el tipo de elemento de modelo requiere que la instrucción se ajuste en una instrucción sp_executesql EXEC antes de que se pueda incluir dentro de una instrucción IF. Entre las propiedades, métodos y tipos clave se encuentran los siguientes: TSqlObject.ObjectType, ModelTypeClass, and the TypeClass property for the following model types: Schema, Procedure, View,  TableValuedFunction, ScalarFunction, DatabaseDdlTrigger, DmlTrigger, ServerDdlTrigger.|  
    |CreateBatchCompleteInsert|Define el método CreateBatchCompleteInsert. Este método crea la instrucción INSERT que se agregará al script de implementación para realizar el seguimiento del progreso de la ejecución del script. Entre las propiedades, métodos y tipos clave se encuentran los siguientes: InsertStatement, NamedTableReference, ColumnReferenceExpression, ValuesInsertSource y RowValue.|  
    |CreateIfNotExecutedStatement|Define el método CreateIfNotExecutedStatement. Este método produce la instrucción IF que comprueba si los lotes temporales ejecutan la tabla que indica que el lote ya se ha ejecutado. Entre las propiedades, métodos y tipos clave se encuentran los siguientes: IfStatement, ExistsPredicate, ScalarSubquery, NamedTableReference, WhereClause, ColumnReferenceExpression, IntegerLiteral, BooleanComparisonExpression y BooleanNotExpression.|  
    |GetStepInfo|Defina el método GetStepInfo. Este método extrae la información sobre el elemento modelo usado para crear el script del paso, además del nombre del paso. Los tipos y métodos de interés son los siguientes: [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext), [DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep), [TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject), [CreateElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.createelementstep), [AlterElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.alterelementstep) y [DropElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.dropelementstep).|  
    |GetElementName|Crea un nombre con formato para un TSqlObject.|  
  
1.  Agregue el código siguiente para definir los métodos del asistente:  
  
    ```csharp  
  
    /// <summary>  
    /// The CreateExecuteSql method "wraps" the provided statement script in an "sp_executesql" statement  
    /// Examples of statements that must be so wrapped include: stored procedures, views, and functions  
    /// </summary>  
    private static ExecuteStatement CreateExecuteSql(string statementScript)  
    {  
        // define a new Exec statement  
        ExecuteStatement executeSp = new ExecuteStatement();  
        ExecutableProcedureReference spExecute = new ExecutableProcedureReference();  
        executeSp.ExecuteSpecification = new ExecuteSpecification { ExecutableEntity = spExecute };  
  
        // define the name of the procedure that you want to execute, in this case sp_executesql  
        SchemaObjectName procName = new SchemaObjectName();  
        procName.Identifiers.Add(CreateIdentifier("sp_executesql", QuoteType.NotQuoted));  
        ProcedureReference procRef = new ProcedureReference { Name = procName };  
  
        spExecute.ProcedureReference = new ProcedureReferenceName { ProcedureReference = procRef };  
  
        // add the script parameter, constructed from the provided statement script  
        ExecuteParameter scriptParam = new ExecuteParameter();  
        spExecute.Parameters.Add(scriptParam);  
        scriptParam.ParameterValue = new StringLiteral { Value = statementScript };  
        scriptParam.Variable = new VariableReference { Name = "@stmt" };  
        return executeSp;  
    }  
  
    /// <summary>  
    /// The CreateIdentifier method returns a Identifier with the specified value and quoting type  
    /// </summary>  
    private static Identifier CreateIdentifier(string value, QuoteType quoteType)  
    {  
        return new Identifier { Value = value, QuoteType = quoteType };  
    }  
  
    /// <summary>  
    /// The CreateCompletedBatchesName method creates the name that will be inserted  
    /// into the temporary table for a batch.  
    /// </summary>  
    private static SchemaObjectName CreateCompletedBatchesName()  
    {  
        SchemaObjectName name = new SchemaObjectName();  
        name.Identifiers.Add(CreateIdentifier("tempdb", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier("dbo", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier(CompletedBatchesVariable, QuoteType.SquareBracket));  
        return name;  
    }  
  
    /// <summary>  
    /// Helper method that determins whether the specified statement needs to  
    /// be escaped  
    /// </summary>  
    /// <param name="sqlObject"></param>  
    /// <returns></returns>  
    private static bool IsStatementEscaped(TSqlObject sqlObject)  
    {  
        HashSet<ModelTypeClass> escapedTypes = new HashSet<ModelTypeClass>  
        {  
            Schema.TypeClass,  
            Procedure.TypeClass,  
            View.TypeClass,  
            TableValuedFunction.TypeClass,  
            ScalarFunction.TypeClass,  
            DatabaseDdlTrigger.TypeClass,  
            DmlTrigger.TypeClass,  
            ServerDdlTrigger.TypeClass  
        };  
        return escapedTypes.Contains(sqlObject.ObjectType);  
    }  
  
    /// <summary>  
    /// Helper method that creates an INSERT statement to track a batch being completed  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <param name="batchDescription"></param>  
    /// <returns></returns>  
    private static InsertStatement CreateBatchCompleteInsert(int batchId, string batchDescription)  
    {  
        InsertStatement insert = new InsertStatement();  
        NamedTableReference batchesCompleted = new NamedTableReference();  
        insert.InsertSpecification = new InsertSpecification();  
        insert.InsertSpecification.Target = batchesCompleted;  
        batchesCompleted.SchemaObject = CreateCompletedBatchesName();  
  
        // Build the columns inserted into  
        ColumnReferenceExpression batchIdColumn = new ColumnReferenceExpression();  
        batchIdColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        batchIdColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(BatchIdColumnName, QuoteType.NotQuoted));  
  
        ColumnReferenceExpression descriptionColumn = new ColumnReferenceExpression();  
        descriptionColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        descriptionColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(DescriptionColumnName, QuoteType.NotQuoted));  
  
        insert.InsertSpecification.Columns.Add(batchIdColumn);  
        insert.InsertSpecification.Columns.Add(descriptionColumn);  
  
        // Build the values inserted  
        ValuesInsertSource valueSource = new ValuesInsertSource();  
        insert.InsertSpecification.InsertSource = valueSource;  
  
        RowValue values = new RowValue();  
        values.ColumnValues.Add(new IntegerLiteral { Value = batchId.ToString() });  
        values.ColumnValues.Add(new StringLiteral { Value = batchDescription });  
        valueSource.RowValues.Add(values);  
  
        return insert;  
    }  
  
    /// <summary>  
    /// This is a helper method that generates an if statement that checks the batches executed  
    /// table to see if the current batch has been executed.  The if statement will look like this  
    ///   
    /// if not exists(select 1 from [tempdb].[dbo].[$(CompletedBatches)]   
    ///                where BatchId = batchId)  
    /// begin  
    /// end  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <returns></returns>  
    private static IfStatement CreateIfNotExecutedStatement(int batchId)  
    {  
        // Create the exists/select statement  
        ExistsPredicate existsExp = new ExistsPredicate();  
        ScalarSubquery subQuery = new ScalarSubquery();  
        existsExp.Subquery = subQuery;  
  
        subQuery.QueryExpression = new QuerySpecification  
        {  
            SelectElements =  
            {  
                new SelectScalarExpression  { Expression = new IntegerLiteral { Value ="1" } }  
            },  
            FromClause = new FromClause  
            {  
                TableReferences =  
                    {  
                        new NamedTableReference() { SchemaObject = CreateCompletedBatchesName() }  
                    }  
            },  
            WhereClause = new WhereClause  
            {  
                SearchCondition = new BooleanComparisonExpression  
                {  
                    ComparisonType = BooleanComparisonType.Equals,  
                    FirstExpression = new ColumnReferenceExpression  
                    {  
                        MultiPartIdentifier = new MultiPartIdentifier  
                        {  
                            Identifiers = { CreateIdentifier(BatchIdColumnName, QuoteType.SquareBracket) }  
                        }  
                    },  
                    SecondExpression = new IntegerLiteral { Value = batchId.ToString() }  
                }  
            }  
        };  
  
        // Put together the rest of the statement  
        IfStatement ifNotExists = new IfStatement  
        {  
            Predicate = new BooleanNotExpression  
            {  
                Expression = existsExp  
            }  
        };  
  
        return ifNotExists;  
    }  
  
    /// <summary>  
    /// Helper method that generates a useful description of the step.  
    /// </summary>  
    private static void GetStepInfo(  
        DeploymentScriptDomStep domStep,  
        out string stepDescription,  
        out TSqlObject element)  
    {  
        element = null;  
  
        // figure out what type of step we've got, and retrieve  
        // either the source or target element.  
        if (domStep is CreateElementStep)  
        {  
            element = ((CreateElementStep)domStep).SourceElement;  
        }  
        else if (domStep is AlterElementStep)  
        {  
            element = ((AlterElementStep)domStep).SourceElement;  
        }  
        else if (domStep is DropElementStep)  
        {  
            element = ((DropElementStep)domStep).TargetElement;  
        }  
  
        // construct the step description by concatenating the type and the fully qualified  
        // name of the associated element.  
        string stepTypeName = domStep.GetType().Name;  
        if (element != null)  
        {  
            string elementName = GetElementName(element);  
  
            stepDescription = string.Format(CultureInfo.InvariantCulture, "{0} {1}",  
                stepTypeName, elementName);  
        }  
        else  
        {  
            // if the step has no associated element, just use the step type as the description  
            stepDescription = stepTypeName;  
        }  
    }  
  
    private static string GetElementName(TSqlObject element)  
    {  
        StringBuilder name = new StringBuilder();  
        if (element.Name.HasExternalParts)  
        {  
            foreach (string part in element.Name.ExternalParts)  
            {  
                if (name.Length > 0)  
                {  
                    name.Append('.');  
                }  
                name.AppendFormat("[{0}]", part);  
            }  
        }  
  
        foreach (string part in element.Name.Parts)  
        {  
            if (name.Length > 0)  
            {  
                name.Append('.');  
            }  
            name.AppendFormat("[{0}]", part);  
        }  
  
        return name.ToString();  
    }  
  
    ```  
  
2.  Guarde los cambios de SqlRestartableScriptContributor.cs.  
  
A continuación, compile la biblioteca de clases.  
  
#### <a name="to-sign-and-build-the-assembly"></a>Para firmar y compilar el ensamblado  
  
1.  En el menú **Proyecto**, haga clic en **Propiedades MyOtherDeploymentContributor**.  
  
2.  Haga clic en la pestaña **Firma** .  
  
3.  Haga clic en **Firmar el ensamblado**.  
  
4.  En **Elegir un archivo de clave de nombre seguro**, haga clic en **<New>** .  
  
5.  En el cuadro de diálogo **Crear clave de nombre seguro** , en el **Nombre del archivo clave**, escriba **MyRefKey**.  
  
6.  (opcional) Puede especificar una contraseña para el archivo de clave de nombre seguro.  
  
7.  Haga clic en **OK**.  
  
8.  En el menú **Archivo** , haga clic en **Guardar todo**.  
  
9. En el menú **Compilar**, haga clic en **Compilar solución**.  
  
    A continuación, debe instalar el ensamblado para que se cargue cuando implemente proyectos de SQL.  
  
## <a name="install-a-deployment-contributor"></a><a name="InstallDeploymentContributor"></a>Instalar un colaborador de implementación  
Para instalar un colaborador de implementación, debe copiar el ensamblado y el archivo .pdb asociado en la Carpeta de extensiones.  
  
#### <a name="to-install-the-myotherdeploymentcontributor-assembly"></a>Para instalar el ensamblado MyOtherDeploymentContributor  
  
1.  Después, copiará la información del ensamblado en el directorio Extensions. Cuando se inicie Visual Studio, identificará cualquier extensión del directorio y subdirectorios %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions, y hará que estén disponibles para su uso.  
  
2.  Copie el archivo de ensamblado **MyOtherDeploymentContributor.dll** del directorio de salida en el directorio %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions. De manera predeterminada, la ruta de acceso del archivo .dll compilado es YourSolutionPath\YourProjectPath\bin\Debug o YourSolutionPath\YourProjectPath\bin\Release.  
  
## <a name="run-or-test-your-deployment-contributor"></a><a name="TestDeploymentContributor"></a>Ejecutar o comprobar su colaborador de implementación  
Para ejecutar o comprobar su colaborador de implementación, debe realizar las siguientes tareas:  
  
-   Agregue propiedades al archivo .sqlproj que quiera compilar.  
  
-   Implemente el proyecto de base de datos utilizando MSBuild y proporcionando los parámetros apropiados.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Agregue Propiedades al archivo de Proyecto SQL (.sqlproj)  
Debe actualizar siempre el archivo de proyecto SQL para especificar el identificador de los colaboradores que desee ejecutar. Puede hacerlo de una de las maneras siguientes:  
  
1.  Puede modificar manualmente el archivo .sqlproj para agregar los argumentos necesarios. Debe hacerlo si su colaborador no tiene ningún argumento de colaborador necesario para la configuración o si no pretende utilizar el colaborador de compilación en un gran número de proyectos. Si elige esta opción, agregue las instrucciones siguientes en el archivo .sqlproj después del primer nodo de Importación en el archivo:  
  
    ```  
    <PropertyGroup>  
      <DeploymentContributors>  
        $(DeploymentContributors); MyOtherDeploymentContributor.RestartableScriptContributor  
      </DeploymentContributors>  
    </PropertyGroup>  
    ```  
  
2.  El segundo método es crear un archivo de destino que contenga los argumentos de colaborador necesarios. Esto es útil si está utilizando el mismo colaborador para varios proyectos y tiene argumentos de colaborador necesarios, ya que incluirá los valores predeterminados. En este caso, cree un archivo de destino en la ruta de extensiones MSBuild:  
  
    1.  Navegue a %Program Files%\MSBuild.  
  
    2.  Cree una carpeta nueva “MyContributors” donde se almacenarán los archivos de destino.  
  
    3.  Cree un archivo nuevo “MyContributors.targets” en este directorio, agréguele el siguiente texto y guarde el archivo:  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <DeploymentContributors>$(DeploymentContributors);MyOtherDeploymentContributor.RestartableScriptContributor</DeploymentContributors>  
          </PropertyGroup>  
        </Project>  
  
        ```  
  
    4.  En el archivo .sqlproj de cualquier proyecto en el que quiera ejecutar colaboradores, importe el archivo de destinos mediante la adición de la siguiente instrucción al archivo .sqlproj después del nodo \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> en el archivo:  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
  
        ```  
  
Después de haber seguido uno de estos métodos, puede usar MSBuild con el fin de pasar los parámetros para compilaciones de línea de comandos.  
  
> [!NOTE]  
> Debe actualizar siempre la propiedad “DeploymentContributors” para especificar el identificador de colaborador. Es el mismo identificador utilizado en el atributo “ExportDeploymentPlanModifier” en el archivo de origen del colaborador. Sin él su colaborador no se ejecutará al compilar el proyecto. Únicamente es necesario actualizar la propiedad “ContributorArguments” si tiene argumentos necesarios para ejecutar su colaborador.  
  
## <a name="deploy-the-database-project"></a>Implementar el proyecto de base de datos  
  
#### <a name="to-deploy-your-sql-project-and-generate-a-deployment-report"></a>Para implementar el proyecto de SQL y generar un informe de implementación  
  
-   El proyecto se puede publicar o implementar de manera habitual en Visual Studio. Abra simplemente una solución que contenga el proyecto de SQL y elija la opción del menú contextual del botón secundario “Publicar…” para el proyecto, o utilice F5 para una implementación de depuración en LocalDB. En este ejemplo utilizaremos el diálogo “Publicar…” para generar un script de implementación.  
  
    1.  Abra Visual Studio y abra la solución que contiene el proyecto de SQL.  
  
    2.  Haga clic con el botón derecho en el proyecto del Explorador de soluciones y elija la opción **Publicar…** .  
  
    3.  Establezca el nombre del servidor y de la base de datos en los que vaya a publicar.  
  
    4.  Elija **Generar script** de las opciones en la parte inferior del diálogo. Esto creará un script que se puede utilizar para la implementación. Examinaremos esta opción para comprobar que las instrucciones IF se han agregado para hacer que el script sea reiniciable.  
  
    5.  Examine el script de implementación resultante. Justo antes de la sección con la etiqueta “Plantilla de script anterior a la implementación”, debería ver algo similar a la siguiente sintaxis Transact-SQL:  
  
        ```  
        :setvar CompletedBatches __completedBatches_CompareProjectDB_cd1e348a-8f92-44e0-9a96-d25d65900fca  
        :setvar TotalBatchCount 17  
        GO  
  
        if OBJECT_ID(N'tempdb.dbo.$(CompletedBatches)', N'U') is null  
        begin  
        use tempdb  
        create table [dbo].[$(CompletedBatches)]  
        (  
        BatchId int primary key,  
        Description nvarchar(300)  
        )  
        use [$(DatabaseName)]  
        end  
  
        ```  
  
        Posteriormente en el script de implementación, alrededor de cada lote, verá la instrucción IF que rodea la instrucción original. Por ejemplo, debe aparecer lo siguiente para una instrucción CREATE SCHEMA:  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 0)  
            BEGIN  
                EXECUTE sp_executesql @stmt = N'CREATE SCHEMA [Sales]  
            AUTHORIZATION [dbo]';  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (0, N'CreateElementStep Sales batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        Observe que CREATE SCHEMA es una de las instrucciones que se deben incluir en una instrucción sp_executesql EXECUTE dentro de una instrucción IF. Instrucciones como CREATE TABLE no requieren la instrucción sp_executesql EXECUTE y se asemejarán al ejemplo siguiente:  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 1)  
            BEGIN  
                CREATE TABLE [Sales].[Customer] (  
                    [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
                    [CustomerName] NVARCHAR (40) NOT NULL,  
                    [YTDOrders]    INT           NOT NULL,  
                    [YTDSales]     INT           NOT NULL  
                );  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (1, N'CreateElementStep Sales.Customer batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        > [!NOTE]  
        > Si implementa un proyecto de base de datos idéntico a la base de datos de destino, el informe resultante no será muy significativo. Para resultados más significativos, implemente los cambios en una base de datos o implemente una nueva base de datos.  
  
## <a name="command-line-deployment-using-generated-dacpac-file"></a>Implementación de la línea de comandos mediante el archivo dacpac generado  
Una vez que se ha compilado un proyecto de SQL, se crea un archivo dacpac que se puede utilizar para implementar el esquema desde la línea de comandos, y que puede habilitar la implementación desde otro equipo como un equipo de compilación. SqlPackage es un programa de línea de comandos que habilita la implementación de dacpacs con una gama completa de opciones que permiten a los usuarios implementar un dacpac o generar un script de implementación, entre otras acciones. Para más información, consulte [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md).  
  
> [!NOTE]  
> Para implementar correctamente los dacpacs compilados desde proyectos con la propiedad DeploymentContributors definida, el (los) DLL que contiene(n) su(s) colaborador(es) de implementación se deben instalar en el equipo que se está utilizando. Esto es así porque se han marcado como necesarios para que la implementación se complete correctamente.  
>   
> Para evitar este requisito, excluya el colaborador de implementación del archivo .sqlproj. En su lugar, especifique que los colaboradores se ejecuten durante la implementación mediante SqlPackage con el parámetro **AdditionalDeploymentContributors**. Esto es útil si solo desea utilizar un colaborador para circunstancias especiales, como la implementación en un servidor concreto.  
  
## <a name="next-steps"></a>Pasos siguientes  
Puede experimentar con otros tipos de modificaciones en los planes de implementación antes de que se ejecuten. Otros tipos de modificaciones que puede ser conveniente realizar son:  
  
-   Agregar una propiedad extendida a todos los objetos de base de datos que les asocien un número de versión.  
  
-   Agregar o quitar instrucciones o comentarios de impresión de diagnóstico adicionales desde los scripts de implementación.  
  
## <a name="see-also"></a>Consulte también  
[Personalizar la compilación de bases de datos y la implementación con colaboradores de implementación y compilación](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[Tutorial: Ampliación de la compilación del proyecto de base de datos para generar estadísticas de modelo](../ssdt/walkthrough-extend-database-project-build-to-generate-model-statistics.md)  
[Tutorial: Ampliación de la implementación del proyecto de base de datos para analizar el plan de implementación](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  

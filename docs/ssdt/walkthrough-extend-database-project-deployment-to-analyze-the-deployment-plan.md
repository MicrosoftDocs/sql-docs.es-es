---
title: Ampliación de la implementación del proyecto de base de datos para analizar el plan de implementación
description: Cree un tipo de colaborador de implementación DeploymentPlanExecutor. Configure un colaborador que mantenga un registro de los eventos que se producen al implementar un proyecto de base de datos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9ead8470-93ba-44e3-8848-b59322e37621
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d389d9f71b08914385cdf3bedef5bf613e6700d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016675"
---
# <a name="walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan"></a>Tutorial: Ampliación de la implementación del proyecto de base de datos para analizar el plan de implementación

Puede crear colaboradores de implementación para realizar acciones personalizadas al implementar un proyecto de SQL. Puede crear un DeploymentPlanModifier o un DeploymentPlanExecutor. Utilice un DeploymentPlanModifier para cambiar el plan antes de ejecutarlo y un DeploymentPlanExecutor para realizar operaciones mientras se ejecuta el plan. En este tutorial, se crea un DeploymentPlanExecutor denominado DeploymentUpdateReportContributor que crea un informe sobre las acciones que se realizan al implementar un proyecto de base de datos. Dado que este colaborador de compilación acepta un parámetro para controlar si el informe se genera, debe efectuar un paso necesario adicional.  
  
En este tutorial, realizará las principales tareas siguientes:  
  
-   [Crear el tipo de colaborador de implementación DeploymentPlanExecutor](#CreateDeploymentContributor)  
  
-   [Instalar el colaborador de implementación](#InstallDeploymentContributor)  
  
-   [Comprobar su colaborador de implementación](#TestDeploymentContributor)  
  
## <a name="prerequisites"></a>Requisitos previos  
Necesitará los componentes siguientes para completar este tutorial:  
  
-   Debe haber instalado una versión de Visual Studio que incluya las herramientas (SSDT) de datos de SQL Server y admita el desarrollo de C# o VB.  
  
-   Debe disponer de un proyecto de SQL que contenga los objetos SQL.  
  
-   Una instancia de SQL Server a la que pueda implementar un proyecto de base de datos.  
  
> [!NOTE]  
> Este tutorial está destinado a usuarios que ya están familiarizados con las características de SQL de SSDT. También se espera que esté familiarizado con los conceptos básicos de Visual Studio, como cómo crear una biblioteca de clases y cómo utilizar el editor de código para agregar código a una clase.  
  
## <a name="create-a-deployment-contributor"></a><a name="CreateDeploymentContributor"></a>Crear un colaborador de implementación  
Para crear un colaborador de implementación, debe realizar las siguientes tareas:  
  
-   Cree un proyecto de biblioteca de clases y agregue las referencias necesarias.  
  
-   Defina una clase llamada DeploymentUpdateReportContributor que herede de [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor).  
  
-   Invalide el método OnExecute.  
  
-   Agregue una clase del asistente privada.  
  
-   Compile el ensamblado resultante.  
  
#### <a name="to-create-a-class-library-project"></a>Para crear un proyecto de biblioteca de clases  
  
1.  Cree un proyecto de bibliotecas de clases de Visual Basic o Visual C# llamado MyDeploymentContributor.  
  
2.  Cambie el nombre del archivo “Class1.cs” a “DeploymentUpdateReportContributor.cs”.  
  
3.  En el Explorador de soluciones, haga clic con el botón derecho en el nodo de proyecto y, a continuación, haga clic en **Agregar referencia**.  
  
4.  En la pestaña Frameworks, seleccione **System.ComponentModel.Composition**.  
  
5.  Agregue las referencias SQL necesarias: haga clic con el botón derecho en el nodo de proyecto y, a continuación, haga clic en **Agregar referencia**. Haga clic en **Examinar** y navegue a la carpeta **C:\Archivos de programa (x86)\Microsoft SQL Server\110\DAC\Bin**. Elija las entradas **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** y **Microsoft.Data.Tools.Schema.Sql.dll**, haga clic en **Agregar** y, a continuación, haga clic en **Aceptar**.  
  
    A continuación, empiece a agregar el código a la clase.  
  
#### <a name="to-define-the-deploymentupdatereportcontributor-class"></a>Para definir la clase DeploymentUpdateReportContributor  
  
1.  En el editor de código, actualice el archivo DeploymentUpdateReportContributor.cs para que coincidan las siguientes instrucciones **using**:  
  
    ```csharp  
    using System;  
    using System.IO;  
    using System.Text;  
    using System.Xml;  
    using Microsoft.SqlServer.Dac.Deployment;  
    using Microsoft.SqlServer.Dac.Extensibility;  
    using Microsoft.SqlServer.Dac.Model;  
  
    ```  
  
2.  Actualice la definición de clases para que coincida con la siguiente:  
  
    ```csharp  
    /// <summary>  
        /// An executor that generates a report detailing the steps in the deployment plan. Will only run  
        /// if a "GenerateUpdateReport=true" contributor argument is set in the project file, in a targets file or  
        /// passed as an additional argument to the DacServices API. To set in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);DeploymentUpdateReportContributor.GenerateUpdateReport=true;  
        ///     </ContributorArguments>  
        /// <PropertyGroup>  
        ///   
        /// </summary>  
        [ExportDeploymentPlanExecutor("MyDeploymentContributor.DeploymentUpdateReportContributor", "1.0.0.0")]  
        public class DeploymentUpdateReportContributor : DeploymentPlanExecutor  
        {  
        }  
  
    ```  
  
    Ahora ha definido un colaborador de implementación que hereda del DeploymentPlanExecutor. Durante los procesos de compilación e implementación, los colaboradores personalizados se cargan desde un directorio de la extensión estándar. Los colaboradores del ejecutor del plan de implementación se identifican con un atributo [ExportDeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportdeploymentplanexecutorattribute).  
  
    Este atributo es necesario para poder detectar colaboradores. Debería tener un aspecto similar al siguiente:  
  
    ```csharp  
    [ExportDeploymentPlanExecutor("MyDeploymentContributor.DeploymentUpdateReportContributor", "1.0.0.0")]  
  
    ```  
  
    En este caso el primer parámetro del atributo debe ser un identificador único que se utilizará para identificar su colaborador en archivos de proyecto. Una práctica recomendada consiste en combinar el espacio de nombres de la biblioteca (en este tutorial, MyDeploymentContributor) con el nombre de clase (en este tutorial, DeploymentUpdateReportContributor) para generar el identificador.  
  
3.  A continuación, agregue el miembro siguiente que utilizará para habilitar a este proveedor a fin de que acepte un parámetro de línea de comandos:  
  
    ```csharp  
    public const string GenerateUpdateReport = "DeploymentUpdateReportContributor.GenerateUpdateReport";  
    ```  
  
    Este miembro permite al usuario especificar si se debe generar el informe con la opción de GenerateUpdateReport.  
  
    A continuación, invalide el método de OnExecute para agregar el código que desea ejecutar cuando se implementa un proyecto de base de datos.  
  
#### <a name="to-override-onexecute"></a>Para invalidar OnExecute  
  
-   Agregue el método siguiente a la clase DeploymentUpdateReportContributor:  
  
    ```csharp  
    /// <summary>  
            /// Override the OnExecute method to perform actions when you execute the deployment plan for  
            /// a database project.  
            /// </summary>  
            protected override void OnExecute(DeploymentPlanContributorContext context)  
            {  
                // determine whether the user specified a report is to be generated  
                bool generateReport = false;  
                string generateReportValue;  
                if (context.Arguments.TryGetValue(GenerateUpdateReport, out generateReportValue) == false)  
                {  
                    // couldn't find the GenerateUpdateReport argument, so do not generate  
                    generateReport = false;  
                }  
                else  
                {  
                    // GenerateUpdateReport argument was specified, try to parse the value  
                    if (bool.TryParse(generateReportValue, out generateReport))  
                    {  
                        // if we end up here, the value for the argument was not valid.  
                        // default is false, so do nothing.  
                    }  
                }  
  
                if (generateReport == false)  
                {  
                    // if user does not want to generate a report, we are done  
                    return;  
                }  
  
                // We will output to the same directory where the deployment script  
                // is output or to the current directory  
                string reportPrefix = context.Options.TargetDatabaseName;  
                string reportPath;  
                if (string.IsNullOrEmpty(context.DeploymentScriptPath))  
                {  
                    reportPath = Environment.CurrentDirectory;  
                }  
                else  
                {  
                    reportPath = Path.GetDirectoryName(context.DeploymentScriptPath);  
                }  
                FileInfo summaryReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".summary.xml"));  
                FileInfo detailsReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".details.xml"));  
  
                // Generate the reports by using the helper class DeploymentReportWriter  
                DeploymentReportWriter writer = new DeploymentReportWriter(context);  
                writer.WriteReport(summaryReportFile);  
                writer.IncludeScripts = true;  
                writer.WriteReport(detailsReportFile);  
  
                string msg = "Deployment reports ->"  
                    + Environment.NewLine + summaryReportFile.FullName  
                    + Environment.NewLine + detailsReportFile.FullName;  
  
                ExtensibilityError reportMsg = new ExtensibilityError(msg, Severity.Message);  
                base.PublishMessage(reportMsg);  
            }  
        /// <summary>  
        /// Override the OnExecute method to perform actions when you execute the deployment plan for  
        /// a database project.  
        /// </summary>  
            protected override void OnExecute(DeploymentPlanContributorContext context)  
            {  
                // determine whether the user specified a report is to be generated  
                bool generateReport = false;  
                string generateReportValue;  
                if (context.Arguments.TryGetValue(GenerateUpdateReport, out generateReportValue) == false)  
                {  
                    // couldn't find the GenerateUpdateReport argument, so do not generate  
                    generateReport = false;  
                }  
                else  
                {  
                    // GenerateUpdateReport argument was specified, try to parse the value  
                    if (bool.TryParse(generateReportValue, out generateReport))  
                    {  
                        // if we end up here, the value for the argument was not valid.  
                        // default is false, so do nothing.  
                    }  
                }  
  
                if (generateReport == false)  
                {  
                    // if user does not want to generate a report, we are done  
                    return;  
                }  
  
                // We will output to the same directory where the deployment script  
                // is output or to the current directory  
                string reportPrefix = context.Options.TargetDatabaseName;  
                string reportPath;  
                if (string.IsNullOrEmpty(context.DeploymentScriptPath))  
                {  
                    reportPath = Environment.CurrentDirectory;  
                }  
                else  
                {  
                    reportPath = Path.GetDirectoryName(context.DeploymentScriptPath);  
                }  
                FileInfo summaryReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".summary.xml"));  
                FileInfo detailsReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".details.xml"));  
  
                // Generate the reports by using the helper class DeploymentReportWriter  
                DeploymentReportWriter writer = new DeploymentReportWriter(context);  
                writer.WriteReport(summaryReportFile);  
                writer.IncludeScripts = true;  
                writer.WriteReport(detailsReportFile);  
  
                string msg = "Deployment reports ->"  
                    + Environment.NewLine + summaryReportFile.FullName  
                    + Environment.NewLine + detailsReportFile.FullName;  
  
                DataSchemaError reportMsg = new DataSchemaError(msg, ErrorSeverity.Message);  
                base.PublishMessage(reportMsg);  
            }  
    ```  
  
    El método OnExecute se pasa a un objeto [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext) que proporciona acceso a cualquier argumento especificado, al modelo de base de datos de origen y de destino, a las propiedades de compilación y a los archivos de la extensión. En este ejemplo, obtenemos el modelo y, a continuación llamamos a las funciones del asistente para generar información sobre el modelo. Utilizamos el método del asistente PublishMessage de la clase base para notificar los errores que se produzcan.  
  
    Entre los tipos y métodos adicionales de interés se encuentran: [TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel), [ModelComparisonResult](/dotnet/api/microsoft.sqlserver.dac.deployment.modelcomparisonresult), [DeploymentPlanHandle](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanhandle) y [SqlDeploymentOptions](/dotnet/api/microsoft.sqlserver.dac.deployment.sqldeploymentoptions).  
  
    A continuación, defina la clase del asistente que examina más en profundidad los detalles del plan de implementación.  
  
#### <a name="to-add-the-helper-class-that-generates-the-report-body"></a>Para agregar la clase del asistente que genera el cuerpo del informe  
  
-   Agregue la clase del asistente y sus métodos agregando el código siguiente:  
  
    ```csharp  
    /// <summary>  
            /// This class is used to generate a deployment  
            /// report.   
            /// </summary>  
            private class DeploymentReportWriter  
            {  
                readonly TSqlModel _sourceModel;  
                readonly ModelComparisonResult _diff;  
                readonly DeploymentStep _planHead;  
  
                /// <summary>  
                /// The constructor accepts the same context info  
                /// that was passed to the OnExecute method of the  
                /// deployment contributor.  
                /// </summary>  
                public DeploymentReportWriter(DeploymentPlanContributorContext context)  
                {  
                    if (context == null)  
                    {  
                        throw new ArgumentNullException("context");  
                    }  
  
                    // save the source model, source/target differences,  
                    // and the beginning of the deployment plan.  
                    _sourceModel = context.Source;  
                    _diff = context.ComparisonResult;  
                    _planHead = context.PlanHandle.Head;  
                }  
                /// <summary>  
                /// Property indicating whether script bodies  
                /// should be included in the report.  
                /// </summary>  
                public bool IncludeScripts { get; set; }  
  
                /// <summary>  
                /// Drives the report generation, opening files,   
                /// writing the beginning and ending report elements,  
                /// and calling helper methods to report on the  
                /// plan operations.  
                /// </summary>  
                internal void WriteReport(FileInfo reportFile)  
                {// Assumes that we have a valid report file  
                    if (reportFile == null)  
                    {  
                        throw new ArgumentNullException("reportFile");  
                    }  
  
                    // set up the XML writer  
                    XmlWriterSettings xmlws = new XmlWriterSettings();  
                    // Indentation makes it a bit more readable  
                    xmlws.Indent = true;  
                    FileStream fs = new FileStream(reportFile.FullName, FileMode.Create, FileAccess.Write, FileShare.ReadWrite);  
                    XmlWriter xmlw = XmlWriter.Create(fs, xmlws);  
  
                    try  
                    {  
                        xmlw.WriteStartDocument(true);  
                        xmlw.WriteStartElement("DeploymentReport");  
  
                        // Summary report of the operations that  
                        // are contained in the plan.  
                        ReportPlanOperations(xmlw);  
  
                        // You could add a method call here  
                        // to produce a detailed listing of the   
                        // differences between the source and  
                        // target model.  
                        xmlw.WriteEndElement();  
                        xmlw.WriteEndDocument();  
                        xmlw.Flush();  
                        fs.Flush();  
                    }  
                    finally  
                    {  
                        xmlw.Close();  
                        fs.Dispose();  
                    }  
                }  
  
                /// <summary>  
                /// Writes details for the various operation types  
                /// that could be contained in the deployment plan.  
                /// Optionally writes script bodies, depending on  
                /// the value of the IncludeScripts property.  
                /// </summary>  
                private void ReportPlanOperations(XmlWriter xmlw)  
                {// write the node to indicate the start  
                    // of the list of operations.  
                    xmlw.WriteStartElement("Operations");  
  
                    // Loop through the steps in the plan,  
                    // starting at the beginning.  
                    DeploymentStep currentStep = _planHead;  
                    while (currentStep != null)  
                    {  
                        // Report the type of step  
                        xmlw.WriteStartElement(currentStep.GetType().Name);  
  
                        // based on the type of step, report  
                        // the relevant information.  
                        // Note that this procedure only handles   
                        // a subset of all step types.  
                        if (currentStep is SqlRenameStep)  
                        {  
                            SqlRenameStep renameStep = (SqlRenameStep)currentStep;  
                            xmlw.WriteAttributeString("OriginalName", renameStep.OldName);  
                            xmlw.WriteAttributeString("NewName", renameStep.NewName);  
                            xmlw.WriteAttributeString("Category", GetElementCategory(renameStep.RenamedElement));  
                        }  
                        else if (currentStep is SqlMoveSchemaStep)  
                        {  
                            SqlMoveSchemaStep moveStep = (SqlMoveSchemaStep)currentStep;  
                            xmlw.WriteAttributeString("OrignalName", moveStep.PreviousName);  
                            xmlw.WriteAttributeString("NewSchema", moveStep.NewSchema);  
                            xmlw.WriteAttributeString("Category", GetElementCategory(moveStep.MovedElement));  
                        }  
                        else if (currentStep is SqlTableMigrationStep)  
                        {  
                            SqlTableMigrationStep dmStep = (SqlTableMigrationStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(dmStep.SourceTable));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(dmStep.SourceElement));  
                        }  
                        else if (currentStep is CreateElementStep)  
                        {  
                            CreateElementStep createStep = (CreateElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(createStep.SourceElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(createStep.SourceElement));  
                        }  
                        else if (currentStep is AlterElementStep)  
                        {  
                            AlterElementStep alterStep = (AlterElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(alterStep.SourceElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(alterStep.SourceElement));  
                        }  
                        else if (currentStep is DropElementStep)  
                        {  
                            DropElementStep dropStep = (DropElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(dropStep.TargetElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(dropStep.TargetElement));  
                        }  
  
                        // If the script bodies are to be included,  
                        // add them to the report.  
                        if (this.IncludeScripts)  
                        {  
                            using (StringWriter sw = new StringWriter())  
                            {  
                                currentStep.GenerateBatchScript(sw);  
                                string tsqlBody = sw.ToString();  
                                if (string.IsNullOrEmpty(tsqlBody) == false)  
                                {  
                                    xmlw.WriteCData(tsqlBody);  
                                }  
                            }  
                        }  
  
                        // close off the current step  
                        xmlw.WriteEndElement();  
                        currentStep = currentStep.Next;  
                    }  
                    xmlw.WriteEndElement();  
                }  
  
                /// <summary>  
                /// Returns the category of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementCategory(TSqlObject element)  
                {  
                    return element.ObjectType.Name;  
                }  
  
                /// <summary>  
                /// Returns the name of the specified element  
                /// in the source model  
                /// </summary>  
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
            }        /// <summary>  
            /// This class is used to generate a deployment  
            /// report.   
            /// </summary>  
            private class DeploymentReportWriter  
            {  
                /// <summary>  
                /// The constructor accepts the same context info  
                /// that was passed to the OnExecute method of the  
                /// deployment contributor.  
                /// </summary>  
                public DeploymentReportWriter(DeploymentPlanContributorContext context)  
                {  
               }  
                /// <summary>  
                /// Property indicating whether script bodies  
                /// should be included in the report.  
                /// </summary>  
                public bool IncludeScripts { get; set; }  
  
                /// <summary>  
                /// Drives the report generation, opening files,   
                /// writing the beginning and ending report elements,  
                /// and calling helper methods to report on the  
                /// plan operations.  
                /// </summary>  
                internal void WriteReport(FileInfo reportFile)  
                {  
                }  
  
                /// <summary>  
                /// Writes details for the various operation types  
                /// that could be contained in the deployment plan.  
                /// Optionally writes script bodies, depending on  
                /// the value of the IncludeScripts property.  
                /// </summary>  
                private void ReportPlanOperations(XmlWriter xmlw)  
                {  
                }  
  
                /// <summary>  
                /// Returns the category of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementCategory(IModelElement element)  
                {  
                }  
  
                /// <summary>  
                /// Returns the name of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementName(IModelElement element)  
                {  
                }  
            }  
    ```  
  
-   Guarde los cambios en el archivo de clase. Hay varios tipos útiles referenciados en la clase del asistente:  
  
    |**Área de código**|**Tipos útiles**|  
    |-----------------|--------------------|  
    |Miembros de clase|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel), [ModelComparisonResult](/dotnet/api/microsoft.sqlserver.dac.deployment.modelcomparisonresult), [DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep)|  
    |Método WriteReport|XmlWriter y XmlWriterSettings|  
    |Método ReportPlanOperations|Entre los tipos de interés se encuentran: [DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep), [SqlRenameStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlrenamestep), [SqlMoveSchemaStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlmoveschemastep), [SqlTableMigrationStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqltablemigrationstep), [CreateElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.createelementstep), [AlterElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.alterelementstep), [DropElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.dropelementstep).<br /><br />Hay otros pasos, en la documentación de la API podrá obtener una lista completa de pasos.|  
    |GetElementCategory|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|  
    |GetElementName|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|  
  
    A continuación, compile la biblioteca de clases.  
  
#### <a name="to-sign-and-build-the-assembly"></a>Para firmar y compilar el ensamblado  
  
1.  En el menú **Proyecto**, haga clic en **Propiedades ColumnCountCondition**.  
  
2.  Haga clic en la pestaña **Firma** .  
  
3.  Haga clic en **Firmar el ensamblado**.  
  
4.  En **Elegir un archivo de clave de nombre seguro**, haga clic en **<New>** .  
  
5.  En el cuadro de diálogo **Crear clave de nombre seguro** , en el **Nombre del archivo clave**, escriba **MyRefKey**.  
  
6.  (opcional) Puede especificar una contraseña para el archivo de clave de nombre seguro.  
  
7.  Haga clic en **OK**.  
  
8.  En el menú **Archivo** , haga clic en **Guardar todo**.  
  
9. En el menú **Compilar**, haga clic en **Compilar solución**.  
  
A continuación, debe instalar el ensamblado para que se cargue cuando compile e implemente proyectos de SQL.  
  
## <a name="install-a-deployment-contributor"></a><a name="InstallDeploymentContributor"></a>Instalar un colaborador de implementación  
Para instalar un colaborador de implementación, debe copiar el ensamblado y el archivo .pdb asociado en la Carpeta de extensiones.  
  
#### <a name="to-install-the-mydeploymentcontributor-assembly"></a>Para instalar el ensamblado MyDeploymentContributor  
  
-   Después, copiará la información del ensamblado en el directorio Extensions. Cuando se inicie Visual Studio, identificará cualquier extensión del directorio y subdirectorios %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions, y hará que estén disponibles para su uso:  
  
-   Copie el archivo de ensamblado **MyDeploymentContributor.dll** del directorio de salida en el directorio %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions. De manera predeterminada, la ruta de acceso del archivo .dll compilado es YourSolutionPath\YourProjectPath\bin\Debug o YourSolutionPath\YourProjectPath\bin\Release.  
  
## <a name="test-your-deployment-contributor"></a><a name="TestDeploymentContributor"></a>Comprobar su colaborador de implementación  
Para comprobar su colaborador de implementación, debe realizar las siguientes tareas:  
  
-   Agregue propiedades al archivo .sqlproj que quiera implementar.  
  
-   Implemente el proyecto utilizando MSBuild y proporcionando el parámetro apropiado.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Agregue Propiedades al archivo de Proyecto SQL (.sqlproj)  
Debe actualizar siempre el archivo de proyecto SQL para especificar el identificador de los colaboradores que desee ejecutar. Además, dado que este colaborador espera un argumento “GenerateUpdateReport”, este se debe especificar como argumento de colaborador.  
  
Hay dos maneras de hacerlo. Puede modificar manualmente el archivo .sqlproj para agregar los argumentos necesarios. Debe hacerlo si su colaborador no tiene ningún argumento de colaborador necesario para la configuración o si no pretende utilizar el colaborador en un gran número de proyectos. Si elige esta opción, agregue las instrucciones siguientes en el archivo .sqlproj después del primer nodo de Importación en el archivo:  
  
```  
<PropertyGroup>  
    <DeploymentContributors>$(DeploymentContributors); MyDeploymentContributor.DeploymentUpdateReportContributor</DeploymentContributors>  
<ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);DeploymentUpdateReportContributor.GenerateUpdateReport=true;</ContributorArguments>  
  </PropertyGroup>  
```  
  
El segundo método es crear un archivo de destino que contenga los argumentos de colaborador necesarios. Esto es útil si está utilizando el mismo colaborador para varios proyectos y tiene argumentos de colaborador necesarios, ya que incluirá los valores predeterminados. En este caso, cree un archivo de destino en la ruta de extensiones MSBuild.  
  
1.  Navegue a %Program Files%\MSBuild.  
  
2.  Cree una carpeta nueva “MyContributors” donde se almacenarán los archivos de destino.  
  
3.  Cree un archivo nuevo “MyContributors.targets” en este directorio, agréguele el siguiente texto y guarde el archivo:  
  
    ```  
    <?xml version="1.0" encoding="utf-8"?>  
  
    <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
      <PropertyGroup>  
    <DeploymentContributors>$(DeploymentContributors);MyDeploymentContributor.DeploymentUpdateReportContributor</DeploymentContributors>  
    <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments); DeploymentUpdateReportContributor.GenerateUpdateReport=true;</ContributorArguments>  
      </PropertyGroup>  
    </Project>  
    ```  
  
4.  En el archivo .sqlproj de cualquier proyecto en el que quiera ejecutar colaboradores, importe el archivo de destinos mediante la adición de la siguiente instrucción al archivo .sqlproj después del nodo \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> en el archivo:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
    ```  
  
Después de haber seguido uno de estos métodos, puede usar MSBuild con el fin de pasar los parámetros para compilaciones de línea de comandos.  
  
> [!NOTE]  
> Debe actualizar siempre la propiedad “DeploymentContributors” para especificar el identificador de colaborador. Es el mismo identificador utilizado en el atributo “ExportDeploymentPlanExecutor” en el archivo de origen del colaborador. Sin él su colaborador no se ejecutará al compilar el proyecto. Únicamente es necesario actualizar la propiedad “ContributorArguments” si tiene argumentos necesarios para ejecutar su colaborador.  
  
### <a name="deploy-the-database-project"></a>Implementar el proyecto de base de datos  
El proyecto se puede publicar o implementar de manera habitual en Visual Studio. Abra simplemente una solución que contenga el proyecto de SQL y elija la opción del menú contextual del botón derecho “Publicar…” para el proyecto, o utilice F5 para una implementación de depuración en LocalDB. En este ejemplo utilizaremos el diálogo “Publicar…” para generar un script de implementación.  
  
##### <a name="to-deploy-your-sql-project-and-generate-a-deployment-report"></a>Para implementar el proyecto de SQL y generar un informe de implementación  
  
1.  Abra Visual Studio y abra la solución que contiene el proyecto de SQL.  
  
2.  Seleccione el proyecto y pulse “F5” para realizar una implementación de depuración. Nota: Como el elemento ContributorArguments está configurado para ser incluido solo si la configuración es “Debug” por ahora el informe de implementación solo se genera para las implementaciones de depuración. Para cambiar esto, elimine la instrucción Condition="'$(Configuration)' == 'Debug'" de la definición de ContributorArguments.  
  
3.  Un resultado como el siguiente debería encontrarse en la ventana de resultados:  
  
    ```  
    ------ Deploy started: Project: Database1, Configuration: Debug Any CPU ------  
    Finished verifying cached model in 00:00:00  
    Deployment reports ->  
  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyTargetDatabase.summary.xml  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyTargetDatabase.details.xml  
  
      Deployment script generated to:  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyDatabaseProject.sql  
  
    ```  
  
4.  Abra MyTargetDatabase.summary.xml y examine el contenido. El archivo es similar al siguiente ejemplo que muestra una nueva implementación de la base de datos:  
  
    ```  
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>  
    <DeploymentReport>  
      <Operations>  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <BeginPreDeploymentScriptStep />  
        <DeploymentScriptStep />  
        <EndPreDeploymentScriptStep />  
        <SqlBeginPreservationStep />  
        <SqlEndPreservationStep />  
        <SqlBeginDropsStep />  
        <SqlEndDropsStep />  
        <SqlBeginAltersStep />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales" Category="Schema" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Customer" Category="Table" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.PK_Customer_CustID" Category="Primary Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Orders" Category="Table" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.PK_Orders_OrderID" Category="Primary Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Customer_YTDOrders" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Customer_YTDSales" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Orders_OrderDate" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Orders_Status" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.FK_Orders_Customer_CustID" Category="Foreign Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.CK_Orders_FilledDate" Category="Check Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.CK_Orders_OrderDate" Category="Check Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspCancelOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspFillOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspNewCustomer" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspPlaceNewOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspShowOrderDetails" Category="Procedure" />  
        <SqlEndAltersStep />  
        <DeploymentScriptStep />  
        <BeginPostDeploymentScriptStep />  
        <DeploymentScriptStep />  
        <EndPostDeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
      </Operations>  
    </DeploymentReport>  
  
    ```  
  
    > [!NOTE]  
    > Si implementa un proyecto de base de datos idéntico a la base de datos de destino, el informe resultante no será muy significativo. Para resultados más significativos, implemente los cambios en una base de datos o implemente una nueva base de datos.  
  
5.  Abra MyTargetDatabase.details.xml y examine el contenido. Una pequeña sección de detalles muestra las entradas y el script que crea el Esquema de ventas, que imprime un mensaje acerca de cómo crear una tabla, y que crea la tabla:  
  
    ```  
    <CreateElementStep Name="Sales" Category="Schema"><![CDATA[CREATE SCHEMA [Sales]  
        AUTHORIZATION [dbo];  
  
    ]]></CreateElementStep>  
        <SqlPrintStep><![CDATA[PRINT N'Creating [Sales].[Customer]...';  
  
    ]]></SqlPrintStep>  
        <CreateElementStep Name="Sales.Customer" Category="Table"><![CDATA[CREATE TABLE [Sales].[Customer] (  
        [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
        [CustomerName] NVARCHAR (40) NOT NULL,  
        [YTDOrders]    INT           NOT NULL,  
        [YTDSales]     INT           NOT NULL  
    );  
  
    ]]></CreateElementStep>  
  
    ```  
  
    Analizando el plan de implementación que se ejecuta, puede notificar cualquier información contenida en la implementación y puede tomar medidas adicionales en función de los pasos de este plan.  
  
## <a name="next-steps"></a>Pasos siguientes  
Puede crear herramientas adicionales para realizar el procesamiento de los archivos XML de salida. Esto es solo un ejemplo de un [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). También podría crear un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) para cambiar un plan de implementación antes de que se ejecute.  
  
## <a name="see-also"></a>Consulte también  
[Tutorial: Ampliación de la compilación del proyecto de base de datos para generar estadísticas de modelo](/previous-versions/visualstudio/visual-studio-2010/ee461508(v=vs.100))  
[Tutorial: Ampliación de la implementación del proyecto de base de datos para modificar el plan de implementación](/previous-versions/visualstudio/visual-studio-2010/ee461507(v=vs.100))  
[Personalizar la compilación de bases de datos y la implementación con colaboradores de implementación y compilación](/previous-versions/visualstudio/visual-studio-2010/ee461505(v=vs.100))  

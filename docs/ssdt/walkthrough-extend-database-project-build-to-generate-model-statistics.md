---
title: Ampliación de la compilación del proyecto de base de datos para generar estadísticas de modelo
description: Obtenga información sobre cómo crear, instalar y probar un colaborador de compilación que genera estadísticas del modelo de base de datos SQL al compilar un proyecto de base de datos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: d44935ce-63bf-46df-976a-5a54866c8119
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5e3905a3ded61ac6ef99f9ada4e94ffaab77a36c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016685"
---
# <a name="walkthrough-extend-database-project-build-to-generate-model-statistics"></a>Tutorial: Ampliación de la compilación del proyecto de base de datos para generar estadísticas de modelo

Puede crear un colaborador de compilación para realizar acciones personalizadas al compilar un proyecto de base de datos. En este tutorial, se crea un colaborador de compilación llamado ModelStatistics que genera estadísticas del modelo de base de datos SQL al compilar un proyecto de base de datos. Dado que este colaborador de compilación acepta parámetros cuando se compila, son necesarios algunos pasos adicionales.  
  
En este tutorial, realizará las principales tareas siguientes:  
  
-   [Crear un colaborador de compilación](#CreateBuildContributor)  
  
-   [Instalar el colaborador de compilación](#InstallBuildContributor)  
  
-   [Comprobar su colaborador de compilación](#TestBuildContributor)  
  
## <a name="prerequisites"></a>Requisitos previos  
Necesitará los componentes siguientes para completar este tutorial:  
  
-   Debe haber instalado una versión de Visual Studio que incluya las herramientas (SSDT) de datos de SQL Server y admita el desarrollo de C# o VB.  
  
-   Debe disponer de un proyecto de SQL que contenga los objetos SQL.  
  
> [!NOTE]  
> Este tutorial está destinado a usuarios que ya están familiarizados con las características de SQL de SSDT. También se espera que esté familiarizado con los conceptos básicos de Visual Studio, como cómo crear una biblioteca de clases y cómo utilizar el editor de código para agregar código a una clase.  
  
## <a name="build-contributor-background"></a>Información previa del colaborador de compilación  
Los colaboradores de compilación se ejecutan durante la compilación del proyecto, después de que el modelo que representa el proyecto se haya generado pero antes de que el proyecto se guarde en el disco. Se pueden utilizar para varios escenarios, por ejemplo  
  
-   Validar el contenido del modelo e informar de errores de validación al autor de llamada. Esto puede hacerse agregando errores a la lista que se pasa como un parámetro al método OnExecute.  
  
-   Generar estadísticas del modelo e información para el usuario. Este es el ejemplo que se muestra a continuación.  
  
El punto de entrada principal para los colaboradores de compilación es el método OnExecute. Todas las clases que heredan del BuildContributor deben implementar este método. Un objeto BuildContributorContext se pasa a este método: este contiene todos los datos pertinentes para la compilación, como un modelo de la base de datos, las propiedades de compilación y los argumentos/archivos que usarán los colaboradores de compilación.  
  
**TSqlModel y la API del modelo de base de datos**  
  
El objeto más útil será el modelo de la base de datos, representado por un objeto TSqlModel. Esta es una representación lógica de una base de datos, que incluye todas las tablas, vistas y otros elementos, más las relaciones entre ellos. Existe un esquema fuertemente tipado que se puede utilizar para consultar tipos específicos de elementos y para recorrer relaciones interesantes. Verá ejemplos de cómo se utiliza en el código del tutorial.  
  
A continuación se muestran algunos de los comandos usados por el colaborador de ejemplo de este tutorial:  
  
|**Clase**|**Método/propiedad**|**Descripción**|  
|-------------|------------------------|-------------------|  
|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel)|GetObjects()|Consulta el modelo de objetos y es el punto de entrada principal a la API modelo. Solo se pueden consultar los tipos de nivel superior como Tabla o Vista: los tipos como Columna solo se pueden encontrar recorriendo el modelo. Si no se especifica ningún filtro ModelTypeClass, devolverá todos los tipos de nivel superior.|  
|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|GetReferencedRelationshipInstances()|Detecta relaciones con los elementos a los que hace referencia el TSqlObject actual. Por ejemplo, para una tabla devolverá objetos como columnas de la Tabla. En este caso, se puede utilizar un filtro ModelRelationshipClass para especificar relaciones exactas de la consulta (por ejemplo con el filtro “Table.Columns” se aseguraría de que solo devuelve columnas).<br /><br />Hay varios métodos similares, como GetReferencingRelationshipInstances, GetChildren y GetParent. Para obtener más información, vea la documentación de la API.|  
  
**Identificar su colaborador de forma exclusiva**  
  
Durante el proceso de compilación, los colaboradores personalizados se cargan desde un directorio de la extensión estándar. Los colaboradores de compilación se identifican con un atributo [ExportBuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportbuildcontributorattribute). Este atributo es necesario para poder detectar colaboradores. Este atributo deberá ser parecido al de la ilustración siguiente:  
  
```  
[ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
  
```  
  
En este caso el primer parámetro del atributo debe ser un identificador único que se utilizará para identificar su colaborador en archivos de proyecto. Una práctica recomendada es combinar el espacio de nombres de la biblioteca (en este tutorial, “ExampleContributors”) con el nombre de clase (en este tutorial, “ModelStatistics”) para generar el identificador. Verá cómo este espacio de nombres se utiliza para especificar que su colaborador se debe ejecutar posteriormente en el tutorial.  
  
## <a name="create-a-build-contributor"></a><a name="CreateBuildContributor"></a>Crear un colaborador de compilación  
Para crear un colaborador de compilación, debe realizar las siguientes tareas:  
  
-   Cree un proyecto de biblioteca de clases y agregue las referencias necesarias.  
  
-   Defina una clase llamada ModelStatistics que herede de [BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor).  
  
-   Invalide el método OnExecute.  
  
-   Agregue varios métodos del asistente privados.  
  
-   Compile el ensamblado resultante.  
  
#### <a name="to-create-a-class-library-project"></a>Para crear un proyecto de biblioteca de clases  
  
1.  Cree un proyecto de bibliotecas de clases de Visual Basic o Visual C# llamado MyBuildContributor.  
  
2.  Cambie el nombre del archivo de “Class1.cs” a “ModelStatistics.cs”.  
  
3.  En el Explorador de soluciones, haga clic con el botón derecho en el nodo de proyecto y, a continuación, haga clic en **Agregar referencia**.  
  
4.  Seleccione la entrada de **System.ComponentModel.Composition** y, a continuación, haga clic en **Aceptar**.  
  
5.  Agregue las referencias SQL necesarias: haga clic con el botón derecho en el nodo de proyecto y, a continuación, haga clic en **Agregar referencia**. Haga clic en el botón **Examinar** . Navegue a la carpeta **C:\Archivos de programa (x86)\Microsoft SQL Server\110\DAC\Bin**. Elija las entradas **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** y **Microsoft.Data.Tools.Schema.Sql.dll** y, a continuación, haga clic en **Aceptar**.  
  
    A continuación, empiece a agregar el código a la clase.  
  
#### <a name="to-define-the-modelstatistics-class"></a>Para definir la clase ModelStatistics  
  
1.  La clase ModelStatistics procesa el modelo de la base de datos que se pasa al método OnExecute, y genera un informe XML que detalla el contenido del modelo.  
  
    En el editor de código , actualice el archivo ModelStatistics.cs para que coincida con lo siguiente:  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.IO;  
    using System.Linq;  
    using System.Xml.Linq;  
    using Microsoft.Data.Schema;  
    using Microsoft.Data.Schema.Build;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema.SchemaModel;  
    using Microsoft.Data.Schema.Sql;  
  
    namespace ExampleContributors  
    {  
    /// <summary>  
        /// A BuildContributor that generates statistics about a model and saves this to the output directory.  
        /// Will only run if a "GenerateModelStatistics=true" contributor argument is set in the project file, or a targets file.   
        /// Statistics can be sorted by "none, "name" or "value", with "none" being the default sort behavior.  
        ///   
        /// To set contributor arguments in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
        ///     </ContributorArguments>  
        /// <PropertyGroup>      
        ///   
        /// This will generate model statistics when building in Debug mode only - remove the condition to generate in all build modes.  
        /// </summary>  
        [ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
        public class ModelStatistics : BuildContributor  
        {  
            public const string GenerateModelStatistics = "ModelStatistics.GenerateModelStatistics";  
            public const string SortModelStatisticsBy = "ModelStatistics.SortModelStatisticsBy";  
            public const string OutDir = "ModelStatistics.OutDir";  
            public const string ModelStatisticsFilename = "ModelStatistics.xml";  
            private enum SortBy { None, Name, Value };  
            private static Dictionary<string, SortBy> SortByMap = new Dictionary<string, SortBy>(StringComparer.OrdinalIgnoreCase)  
            {  
                { "none", SortBy.None },  
                { "name", SortBy.Name },  
                { "value", SortBy.Value },  
            };  
  
            private SortBy _sortBy = SortBy.None;  
  
            /// <summary>  
            /// Override the OnExecute method to perform actions when you build a database project.  
            /// </summary>  
            protected override void OnExecute(BuildContributorContext context, IList<ExtensibilityError> errors)  
            {  
                // handle related arguments, passed in as part of  
                // the context information.  
                bool generateModelStatistics;  
                ParseArguments(context.Arguments, errors, out generateModelStatistics);  
  
                // Only generate statistics if requested to do so  
                if (generateModelStatistics)  
                {  
                    // First, output model-wide information, such  
                    // as the type of database schema provider (DSP)  
                    // and the collation.  
                    StringBuilder statisticsMsg = new StringBuilder();  
                    statisticsMsg.AppendLine(" ")  
                                 .AppendLine("Model Statistics:")  
                                 .AppendLine("===")  
                                 .AppendLine(" ");  
                    errors.Add(new ExtensibilityError(statisticsMsg.ToString(), Severity.Message));  
  
                    var model = context.Model;  
  
                    // Start building up the XML that will later  
                    // be serialized.  
                    var xRoot = new XElement("ModelStatistics");  
  
                    SummarizeModelInfo(model, xRoot, errors);  
  
                    // First, count the elements that are contained   
                    // in this model.  
                    IList<TSqlObject> elements = model.GetObjects(DacQueryScopes.UserDefined).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "UserDefinedElements", xRoot, errors);  
  
                    // Now, count the elements that are defined in  
                    // another model. Examples include built-in types,  
                    // roles, filegroups, assemblies, and any   
                    // referenced objects from another database.  
                    elements = model.GetObjects(DacQueryScopes.BuiltIn | DacQueryScopes.SameDatabase | DacQueryScopes.System).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "OtherElements", xRoot, errors);  
  
                    // Now, count the number of each type  
                    // of relationship in the model.  
                    SurveyRelationships(model, xRoot, errors);  
  
                    // Determine where the user wants to save  
                    // the serialized XML file.  
                    string outDir;  
                    if (context.Arguments.TryGetValue(OutDir, out outDir) == false)  
                    {  
                        outDir = ".";  
                    }  
                    string filePath = Path.Combine(outDir, ModelStatisticsFilename);  
                    // Save the XML file and tell the user  
                    // where it was saved.  
                    xRoot.Save(filePath);  
                    ExtensibilityError resultArg = new ExtensibilityError("Result was saved to " + filePath, Severity.Message);  
                    errors.Add(resultArg);  
                }  
            }  
  
            /// <summary>  
            /// Examine the arguments provided by the user  
            /// to determine if model statistics should be generated  
            /// and, if so, how the results should be sorted.  
            /// </summary>  
            private void ParseArguments(IDictionary<string, string> arguments, IList<ExtensibilityError> errors, out bool generateModelStatistics)  
            {  
                // By default, we don't generate model statistics  
                generateModelStatistics = false;  
  
                // see if the user provided the GenerateModelStatistics   
                // option and if so, what value was it given.  
                string valueString;  
                arguments.TryGetValue(GenerateModelStatistics, out valueString);  
                if (string.IsNullOrWhiteSpace(valueString) == false)  
                {  
                    if (bool.TryParse(valueString, out generateModelStatistics) == false)  
                    {  
                        generateModelStatistics = false;  
  
                        // The value was not valid from the end user  
                        ExtensibilityError invalidArg = new ExtensibilityError(  
                            GenerateModelStatistics + "=" + valueString + " was not valid.  It can be true or false", Severity.Error);  
                        errors.Add(invalidArg);  
                        return;  
                    }  
                }  
  
                // Only worry about sort order if the user requested  
                // that we generate model statistics.  
                if (generateModelStatistics)  
                {  
                    // see if the user provided the sort option and  
                    // if so, what value was provided.  
                    arguments.TryGetValue(SortModelStatisticsBy, out valueString);  
                    if (string.IsNullOrWhiteSpace(valueString) == false)  
                    {  
                        SortBy sortBy;  
                        if (SortByMap.TryGetValue(valueString, out sortBy))  
                        {  
                            _sortBy = sortBy;  
                        }  
                        else  
                        {  
                            // The value was not valid from the end user  
                            ExtensibilityError invalidArg = new ExtensibilityError(  
                                SortModelStatisticsBy + "=" + valueString + " was not valid.  It can be none, name, or value", Severity.Error);  
                            errors.Add(invalidArg);  
                        }  
                    }  
                }  
            }  
  
            /// <summary>  
            /// Retrieve the database schema provider for the  
            /// model and the collation of that model.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SummarizeModelInfo(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // use a Dictionary to accumulate the information  
                // that will later be output.  
                var info = new Dictionary<string, string>();  
  
                // Two things of interest: the database schema  
                // provider for the model, and the language id and  
                // case sensitivity of the collation of that  
                // model  
                info.Add("Version", model.Version.ToString());  
  
                TSqlObject options = model.GetObjects(DacQueryScopes.UserDefined, DatabaseOptions.TypeClass).FirstOrDefault();  
                if (options != null)  
                {  
                    info.Add("Collation", options.GetProperty<string>(DatabaseOptions.Collation));  
                }  
  
                // Output the accumulated information and add it to   
                // the XML.  
                OutputResult("Basic model info", info, xContainer, errors);  
            }  
  
            /// <summary>  
            /// For a provided list of model elements, count the number  
            /// of elements for each class name, sorted as specified  
            /// by the user.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private void Summarize<T>(IList<T> set, Func<T, string> groupValue, string category, XElement xContainer, IList<ExtensibilityError> errors)  
            { // Use a Dictionary to keep all summarized information  
                var statistics = new Dictionary<string, int>();  
  
                // For each element in the provided list,  
                // count items based on the specified grouping  
                var groups =  
                    from item in set  
                    group item by groupValue(item) into g  
                    select new { g.Key, Count = g.Count() };  
  
                // order the groups as requested by the user  
                if (this._sortBy == SortBy.Name)  
                {  
                    groups = groups.OrderBy(group => group.Key);  
                }  
                else if (this._sortBy == SortBy.Value)  
                {  
                    groups = groups.OrderBy(group => group.Count);  
                }  
  
                // build the Dictionary of accumulated statistics  
                // that will be passed along to the OutputResult method.  
                foreach (var item in groups)  
                {  
                    statistics.Add(item.Key, item.Count);  
                }  
  
                statistics.Add("subtotal", set.Count);  
                statistics.Add("total items", groups.Count());  
  
                // output the results, and build up the XML  
                OutputResult(category, statistics, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Iterate over all model elements, counting the  
            /// styles and types for relationships that reference each   
            /// element  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SurveyRelationships(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // get a list that contains all elements in the model  
                var elements = model.GetObjects(DacQueryScopes.All);  
                // We are interested in all relationships that  
                // reference each element.  
                var entries =  
                    from element in elements  
                    from entry in element.GetReferencedRelationshipInstances(DacExternalQueryScopes.All)  
                    select entry;  
  
                // initialize our counting buckets  
                var composing = 0;  
                var hierachical = 0;  
                var peer = 0;  
  
                // process each relationship, adding to the   
                // appropriate bucket for style and type.  
                foreach (var entry in entries)  
                {  
                    switch (entry.Relationship.Type)  
                    {  
                        case RelationshipType.Composing:  
                            ++composing;  
                            break;  
                        case RelationshipType.Hierarchical:  
                            ++hierachical;  
                            break;  
                        case RelationshipType.Peer:  
                            ++peer;  
                            break;  
                        default:  
                            break;  
                    }  
                }  
  
                // build a dictionary of data to pass along  
                // to the OutputResult method.  
                var stat = new Dictionary<string, int>  
                {  
                    {"Composing", composing},  
                    {"Hierarchical", hierachical},  
                    {"Peer", peer},  
                    {"subtotal", entries.Count()}  
                };  
  
                OutputResult("Relationships", stat, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Performs the actual output for this contributor,  
            /// writing the specified set of statistics, and adding any   
            /// output information to the XML being constructed.  
            /// </summary>  
            private static void OutputResult<T>(string category, Dictionary<string, T> statistics, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                var maxLen = statistics.Max(stat => stat.Key.Length) + 2;  
                var format = string.Format("{{0, {0}}}: {{1}}", maxLen);  
  
                StringBuilder resultMessage = new StringBuilder();  
                //List<ExtensibilityError> args = new List<ExtensibilityError>();  
                resultMessage.AppendLine(category);  
                resultMessage.AppendLine("-----------------");  
  
                // Remove any blank spaces from the category name  
                var xCategory = new XElement(category.Replace(" ", ""));  
                xContainer.Add(xCategory);  
  
                foreach (var item in statistics)  
                {  
                    //Console.WriteLine(format, item.Key, item.Value);  
                    var entry = string.Format(format, item.Key, item.Value);  
                    resultMessage.AppendLine(entry);  
                    // Replace any blank spaces in the element key with  
                    // underscores.  
                    xCategory.Add(new XElement(item.Key.Replace(' ', '_'), item.Value));  
                }  
                resultMessage.AppendLine(" ");  
                errors.Add(new ExtensibilityError(resultMessage.ToString(), Severity.Message));  
            }  
        }  
    }  
  
    ```  
  
    A continuación, compile la biblioteca de clases.  
  
### <a name="to-sign-and-build-the-assembly"></a>Para firmar y compilar el ensamblado  
  
1.  En el menú **Proyecto** , haga clic en **Propiedades de MyBuildContributor**.  
  
2.  Haga clic en la pestaña **Firma** .  
  
3.  Haga clic en **Firmar el ensamblado**.  
  
4.  En **Elegir un archivo de clave de nombre seguro**, haga clic en **<New>** .  
  
5.  En el cuadro de diálogo **Crear clave de nombre seguro** , en el **Nombre del archivo clave**, escriba **MyRefKey**.  
  
6.  (opcional) Puede especificar una contraseña para el archivo de clave de nombre seguro.  
  
7.  Haga clic en **OK**.  
  
8.  En el menú **Archivo** , haga clic en **Guardar todo**.  
  
9. En el menú **Compilar**, haga clic en **Compilar solución**.  
  
    A continuación, debe instalar el ensamblado para que se cargue cuando compile proyectos de SQL.  
  
## <a name="install-a-build-contributor"></a><a name="InstallBuildContributor"></a>Instalar un colaborador de compilación  
Para instalar un colaborador de compilación, debe copiar el ensamblado y el archivo .pdb asociado en la Carpeta de extensiones.  
  
#### <a name="to-install-the-mybuildcontributor-assembly"></a>Para instalar el ensamblado MyBuildContributor  
  
1.  Después, copiará la información del ensamblado en el directorio Extensions. Cuando se inicie Visual Studio, identificará cualquier extensión del directorio y subdirectorios %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions, y hará que estén disponibles para su uso.  
  
2.  Copie el archivo de ensamblado **MyBuildContributor.dll** del directorio de salida en el directorio %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions.  
  
    > [!NOTE]  
    > De manera predeterminada, la ruta de acceso del archivo .dll compilado es YourSolutionPath\YourProjectPath\bin\Debug o YourSolutionPath\YourProjectPath\bin\Release.  
  
## <a name="run-or-test-your-build-contributor"></a><a name="TestBuildContributor"></a>Ejecutar o comprobar su colaborador de compilación  
Para ejecutar o comprobar su colaborador de compilación, debe realizar las siguientes tareas:  
  
-   Agregue propiedades al archivo .sqlproj que quiera compilar.  
  
-   Compile el proyecto de base de datos utilizando MSBuild y proporcionando los parámetros apropiados.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Agregue Propiedades al archivo de Proyecto SQL (.sqlproj)  
Debe actualizar siempre el archivo de proyecto SQL para especificar el identificador de los colaboradores que desee ejecutar. Además, porque este colaborador de compilación acepta parámetros de línea de comandos MSBuild, debe modificar el proyecto de SQL para que los usuarios puedan pasar los parámetros con MSBuild.  
  
Puede hacerlo de una de las maneras siguientes:  
  
-   Puede modificar manualmente el archivo .sqlproj para agregar los argumentos necesarios. Puede elegir hacer esto si no pretende utilizar el colaborador de compilación en un gran número de proyectos. Si elige esta opción, agregue las instrucciones siguientes en el archivo .sqlproj después del primer nodo de Importación en el archivo  
  
    ```  
    /// <PropertyGroup>  
    ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
    ///         $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
    ///     </ContributorArguments>  
    /// <PropertyGroup>  
  
    ```  
  
-   El segundo método es crear un archivo de destino que contenga los argumentos de colaborador necesarios. Esto es útil si está utilizando el mismo colaborador para varios proyectos, ya que incluirá los valores predeterminados.  
  
    En este caso, cree un archivo de destino en la ruta de extensiones MSBuild:  
  
    1.  Navegue a %Program Files%\MSBuild\\.  
  
    2.  Cree una carpeta nueva “MyContributors” donde se almacenarán los archivos de destino.  
  
    3.  Cree un nuevo archivo “MyContributors.targets” en este directorio, agréguele el siguiente texto y después guarde el archivo:  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <BuildContributors>$(BuildContributors);ExampleContributors.ModelStatistics</BuildContributors>  
            <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy=name;</ContributorArguments>  
          </PropertyGroup>  
        </Project>  
        ```  
  
    4.  En el archivo .sqlproj de cualquier proyecto en el que quiera ejecutar colaboradores, importe el archivo de destinos mediante la adición de la siguiente instrucción al archivo .sqlproj después del nodo \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> en el archivo:  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
        ```  
  
Después de haber seguido uno de estos métodos, puede usar MSBuild con el fin de pasar los parámetros para compilaciones de línea de comandos.  
  
> [!NOTE]  
> Debe actualizar siempre la propiedad “BuildContributors” para especificar el identificador de colaborador. Es el mismo identificador utilizado en el atributo “ExportBuildContributor” en el archivo de origen del colaborador. Sin esto, su colaborador no se ejecutará al compilar el proyecto. Únicamente es necesario actualizar la propiedad “ContributorArguments” si tiene argumentos necesarios para ejecutar su colaborador.  
  
### <a name="build-the-sql-project"></a>Compilar el proyecto de SQL  
  
##### <a name="to-rebuild-your-database-project-by-using-msbuild-and-generate-statistics"></a>Para recompilar el proyecto de base de datos mediante MSBuild y generar estadísticas  
  
1.  En Visual Studio, haga clic con el botón secundario en el proyecto y seleccione “Recompilar”. Se recompilará el proyecto, y debería ver las estadísticas del modelo generadas, con la salida incluida en la compilación de salida y guardada en ModelStatistics.xml. Observe que puede tener que elegir “Mostrar todos los archivos” en el Explorador de soluciones para ver el archivo XML.  
  
2.  Abra un símbolo del sistema de Visual Studio: en el menú **Inicio**, haga clic en **Todos los programas**, **Microsoft Visual Studio <Visual Studio Version>**, **Visual Studio Tools**, y, finalmente, en **Símbolo del sistema Visual Studio (<Visual Studio Version>)**.  
  
3.  En el símbolo del sistema, navegue a la carpeta que contiene el proyecto de SQL.  
  
4.  En el símbolo del sistema, escriba el siguiente comando:  
  
    ```  
    MSBuild /t:Rebuild MyDatabaseProject.sqlproj /p:BuildContributors=$(BuildContributors);ExampleContributors.ModelStatistics /p:ContributorArguments=$(ContributorArguments);GenerateModelStatistics=true;SortModelStatisticsBy=name;OutDir=.\;  
    ```  
  
    Reemplace *MyDatabaseProject* con el nombre del proyecto de base de datos que desea generar. Si ha cambiado el proyecto después de la última vez que lo compiló, puede usar /t:Build en lugar de /t:Rebuild.  
  
    En la salida debe ver información de compilación como la siguiente:  
  
```  
Model Statistics:  
===  
  
Basic model info  
-----------------  
    Version: Sql110  
  Collation: SQL_Latin1_General_CP1_CI_AS  
  
UserDefinedElements  
-----------------  
  DatabaseOptions: 1  
         subtotal: 1  
      total items: 1  
  
OtherElements  
-----------------  
                Assembly: 1  
       BuiltInServerRole: 9  
           ClrTypeMethod: 218  
  ClrTypeMethodParameter: 197  
         ClrTypeProperty: 20  
                Contract: 6  
                DataType: 34  
                Endpoint: 5  
               Filegroup: 1  
             MessageType: 14  
                   Queue: 3  
                    Role: 10  
                  Schema: 13  
                 Service: 3  
                    User: 4  
         UserDefinedType: 3  
                subtotal: 541  
             total items: 16  
  
Relationships  
-----------------  
     Composing: 477  
  Hierarchical: 6  
          Peer: 19  
      subtotal: 502  
  
```  
  
1.  Abra ModelStatistics.xml y examine el contenido.  
  
    Los resultados informados también se guardan en el archivo XML.  
  
## <a name="next-steps"></a>Pasos siguientes  
Puede crear herramientas adicionales para realizar el procesamiento del archivo XML de salida. Esto es solo un ejemplo de un colaborador de compilación. Podría, por ejemplo, crear un colaborador de compilación para generar un archivo de diccionario de datos como parte de la compilación.  
  
## <a name="see-also"></a>Consulte también  
[Personalizar la compilación de bases de datos y la implementación con colaboradores de implementación y compilación](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[Tutorial: Ampliación de la implementación del proyecto de base de datos para analizar el plan de implementación](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  

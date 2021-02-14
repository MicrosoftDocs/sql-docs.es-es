---
title: Creación de un ensamblado de regla de análisis de código estático personalizado para SQL Server
description: Obtenga información sobre cómo crear reglas de Code Analysis de SQL Server. Configure una regla para evitar instrucciones WAITFOR DELAY en procedimientos almacenados, desencadenadores y funciones.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f7b6ed8c-a4e0-4e33-9858-a8aa40aef309
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 3cae3183bef3d6bd40a9d30805f3923df8d70679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082346"
---
# <a name="walkthrough-authoring-a-custom-static-code-analysis-rule-assembly-for-sql-server"></a>Tutorial de creación de un ensamblado de regla de análisis de código estático personalizado para SQL Server

En este tutorial se muestran los pasos utilizados para crear una regla de análisis de código de SQL Server. La regla creada en este tutorial se utiliza para evitar las instrucciones WAITFOR DELAY en procedimientos almacenados, desencadenadores y funciones.  
  
En este tutorial, creará una regla personalizada de análisis de código estático de Transact\-SQL mediante el uso de los siguientes procesos:  
  
1. Cree una biblioteca de clases, habilite la firma para el proyecto y agregue las referencias necesarias.  
  
2. Cree una clase de regla personalizada de Visual C\#.  
  
3. Cree dos clases del asistente de Visual C\#.  
  
4. Copie el archivo DLL resultante que ha creado en el directorio Extensions para instalarlo.  
  
5. Compruebe que la nueva regla de análisis de código está en su lugar.  
  
**Requisitos previos**
  
Necesitará los componentes siguientes para completar este tutorial:  
  
- Debe haber instalado una versión de Visual Studio que incluya SQL Server Data Tools y admita el desarrollo de Visual C\# o Visual Basic.  
  
- Debe tener un proyecto de SQL que contenga objetos de SQL Server.  
  
- Una instancia de SQL Server a la que pueda implementar un proyecto de base de datos.  
  
> [!NOTE]  
> Este tutorial está destinado a usuarios que ya están familiarizados con las características de SQL Server de SQL Server Data Tools. También se espera que esté familiarizado con los conceptos de Visual Studio, como cómo crear una biblioteca de clases y cómo utilizar el editor de código para agregar código a una clase.  
  
## <a name="creating-a-custom-code-analysis-rule-for-sql-server"></a>Creación de una regla de análisis de código personalizada para SQL Server  

En primer lugar, cree una biblioteca de clases. Para crear un proyecto de biblioteca de clases:  
  
1. Cree un proyecto de bibliotecas de clases de Visual C\# o Visual Basic llamado SampleRules.  
  
2. Cambie el nombre del archivo Class1.cs por AvoidWaitForDelayRule.cs.  
  
3. En el Explorador de soluciones, haga clic con el botón derecho en el nodo de proyecto y, a continuación, haga clic en **Agregar referencia**.  
  
4. En la ficha Frameworks, seleccione System.ComponentModel.Composition.  
  
5. Haga clic en **Examinar** y navegue al directorio C:\Archivos de programa (x86)\\Microsoft SQL Server\120\SDK\Assemblies, seleccione Microsoft.SqlServer.TransactSql.ScriptDom.dll y, a continuación, haga clic en Aceptar.  
  
6. A continuación, instale las referencias de DACFx necesarias. Haga clic en **Examinar** y vaya al directorio <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120. Elija las entradas Microsoft.SqlServer.Dac.dll, Microsoft.SqlServer.Dac.Extensions.dll y Microsoft.Data.Tools.Schema.Sql.dll y haga clic en **Agregar** y, a continuación, haga clic en **Aceptar**.  
  
    Ahora se instalarán los archivos binarios de DACFx dentro del directorio de instalación de Visual Studio. Para Visual Studio 2012, <Visual Studio Install Dir> habitualmente será C:\Archivos de programa (x86)\\MicrosoftVisual Studio 11.0. Para Visual Studio 2013, habitualmente será C:\Archivos de programa (x86)\\MicrosoftVisual Studio 12.0.  
  
A continuación deberá agregar clases auxiliares que la regla utilizará.  
  
## <a name="creating-the-custom-code-analysis-rule-supporting-classes"></a>Creación de las clases auxiliares para la regla de análisis de código personalizada

Antes de crear la clase para la regla en sí, deberá agregar al proyecto una clase de visitante y una clase de atributos. Estas clases pueden ser útiles para crear reglas personalizadas adicionales.  
  
La primera clase que debe definir es la clase WaitForDelayVisitor, que se deriva de [TSqlConcreteFragmentVisitor](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlconcretefragmentvisitor). Esta clase proporciona acceso a las instrucciones WAITFOR DELAY en el modelo. Las clases de visitante hacen uso de la API [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom), proporcionada por SQL Server. En esta API, el código Transact\-SQL se representa como un árbol de sintaxis abstracta (AST) y las clases de visitante pueden ser útiles para buscar objetos de sintaxis específica, como instrucciones WAITFORDELAY. Puede ser difícil encontrar estos objetos utilizando el modelo de objetos, ya que no están asociados a una propiedad de objeto o relación específicas, pero es fácil encontrarlos mediante el patrón de visitante y la API [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom).  
  
### <a name="defining-the-waitfordelayvisitor-class"></a>Definición de la clase WaitForDelayVisitor  
  
1. En el **Explorador de soluciones**, seleccione el proyecto SampleRules.  
  
2. En el menú **Proyecto**, seleccione **Agregar clase**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento** .  
  
3. En el cuadro de texto **Nombre**, escriba WaitForDelayVisitor.cs y, a continuación, haga clic en el botón **Agregar**. El archivo WaitForDelayVisitor.cs se agrega al proyecto en el **Explorador de soluciones**.  
  
4. Abra el archivo WaitForDelayVisitor.cs y actualice el contenido para que coincida con el código siguiente:  
  
    ```  
    using System.Collections.Generic;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    namespace SampleRules {  
        class WaitForDelayVistor {}  
    }  
    ```  
  
5. En la declaración de clase, cambie el modificador de acceso a internal y derive la clase de TSqlConcreteFragmentVisitor:  
  
    ```  
    internal class WaitForDelayVisitor : TSqlConcreteFragmentVisitor {}  
    ```  
  
6. Agregue el código siguiente para definir la variable de miembro de lista:  
  
    ```  
    public IList<WaitForStatement> WaitForDelayStatements { get; private set; }  
    ```  
  
7. Defina el constructor de clase agregando el código siguiente:  
  
    ```  
    public WaitForDelayVisitor() {  
       WaitForDelayStatments = new List<WaitForStatement>();  
    }  
    ```  
  
8. Anule el método ExplicitVisit agregando el código siguiente:  
  
    ```  
    public override void ExplicitVisit(WaitForStatement node) {  
       // We are only interested in WAITFOR DELAY occurrences  
       if (node.WaitForOption == WaitForOption.Delay)  
          WaitForDelayStatments.Add(node);  
    }  
    ```  
  
    Este método pasa por las instrucciones WAITFOR del modelo y agrega las que tienen la opción DELAY especificada a la lista de instrucciones WAITFOR DELAY. La clase clave a la que se hace referencia aquí es [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement).  
  
9. En el menú **Archivo** , haga clic en **Guardar**.  
  
La segunda clase es LocalizedExportCodeAnalysisRuleAttribute.cs. Se trata de una extensión de la clase Microsoft.SqlServer.Dac.CodeAnalysis.ExportCodeAnalysisRuleAttribute integrada que proporciona el marco y admite la lectura en un archivo de recursos de los parámetros DisplayName y Description utilizados por la regla. Esta clase es útil si desea que las reglas puedan utilizarse en varios idiomas.  
  
### <a name="defining-the-localizedexportcodeanalysisruleattribute-class"></a>Definición de la clase LocalizedExportCodeAnalysisRuleAttribute  
  
1. En el **Explorador de soluciones**, seleccione el proyecto SampleRules.  
  
2. En el menú **Proyecto**, seleccione **Agregar clase**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento** .  
  
3. En el cuadro de texto **Nombre**, escriba LocalizedExportCodeAnalysisRuleAttribute.cs y, a continuación, haga clic en el botón **Agregar**. El archivo se agrega al proyecto en el **Explorador de soluciones**.  
  
4. Abra el archivo y actualiza el contenido para que coincida con el código siguiente:  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using System;  
    using System.Globalization;  
    using System.Reflection;  
    using System.Resources;  
  
    namespace SampleRules  
    {  
  
        internal class LocalizedExportCodeAnalysisRuleAttribute : ExportCodeAnalysisRuleAttribute  
        {  
            private readonly string _resourceBaseName;  
            private readonly string _displayNameResourceId;  
            private readonly string _descriptionResourceId;  
  
            private ResourceManager _resourceManager;  
            private string _displayName;  
            private string _descriptionValue;  
  
            /// <summary>  
            /// Creates the attribute, with the specified rule ID, the fully qualified  
            /// name of the resource file that will be used for looking up display name  
            /// and description, and the Ids of those resources inside the resource file.  
            /// </summary>  
            public LocalizedExportCodeAnalysisRuleAttribute(  
                string id,  
                string resourceBaseName,  
                string displayNameResourceId,  
                string descriptionResourceId)  
                : base(id, null)  
            {  
                _resourceBaseName = resourceBaseName;  
                _displayNameResourceId = displayNameResourceId;  
                _descriptionResourceId = descriptionResourceId;  
            }  
  
            /// <summary>  
            /// Rules in a different assembly would need to overwrite this  
            /// </summary>  
            /// <returns></returns>  
            protected virtual Assembly GetAssembly()  
            {  
                return GetType().Assembly;  
            }  
  
            private void EnsureResourceManagerInitialized()  
            {  
                var resourceAssembly = GetAssembly();  
  
                try  
                {  
                    _resourceManager = new ResourceManager(_resourceBaseName, resourceAssembly);  
                }  
                catch (Exception ex)  
                {  
                    var msg = String.Format(CultureInfo.CurrentCulture, RuleResources.CannotCreateResourceManager, _resourceBaseName, resourceAssembly);  
                    throw new RuleException(msg, ex);  
                }  
            }  
  
            private string GetResourceString(string resourceId)  
            {  
                EnsureResourceManagerInitialized();  
                return _resourceManager.GetString(resourceId, CultureInfo.CurrentUICulture);  
            }  
  
            /// <summary>  
            /// Overrides the standard DisplayName and looks up its value inside a resources file  
            /// </summary>  
            public override string DisplayName  
            {  
                get  
                {  
                    if (_displayName == null)  
                    {  
                        _displayName = GetResourceString(_displayNameResourceId);  
                    }  
                    return _displayName;  
                }  
            }  
  
            /// <summary>  
            /// Overrides the standard Description and looks up its value inside a resources file  
            /// </summary>  
            public override string Description  
            {  
                get  
                {  
                    if (_descriptionValue == null)  
                    {   
                        _descriptionValue = GetResourceString(_descriptionResourceId);  
                    }  
                    return _descriptionValue;  
                }  
            }  
        }  
    }  
    ```  
  
A continuación, agregue un archivo de recursos en el que se defina el nombre de regla, su descripción y la categoría en la que aparecerá la regla en la interfaz de configuración de reglas.  
  
### <a name="to-add-a-resource-file-and-three-resource-strings"></a>Para agregar un archivo de recursos y tres cadenas de recursos  
  
1. En el **Explorador de soluciones**, seleccione el proyecto SampleRules.  
  
2. En el menú **Proyecto**, seleccione **Agregar nuevo elemento**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento** .  
  
3. En la lista de **Plantillas instaladas**, haga clic en **General**.  
  
4. En el panel de detalles, haga clic en **Archivo de recursos**.  
  
5. En **Nombre**, escriba RuleResources.resx. Aparece el editor de recursos, sin recursos definidos.  
  
6. Defina cuatro cadenas de recursos, como sigue:  
  
    |Nombre|Value|  
    |--------|---------|  
    |AvoidWaitForDelay_ProblemDescription|La instrucción WAITFOR DELAY se encontró en {0}.|  
    |AvoidWaitForDelay_RuleName|Evite usar instrucciones WaitFor Delay en procedimientos, funciones y desencadenadores almacenados.|  
    |CategorySamples|SamplesCategory|  
    |CannotCreateResourceManager|No se puede crear ResourceManager para {0} desde {1}.|  
  
7. En el menú **Archivo**, haga clic en **Guardar RuleResources.resx**.  
  
A continuación, defina una clase que haga referencia a los recursos del archivo de recursos que Visual Studio utiliza para mostrar información acerca de la regla en la interfaz de usuario.  
  
### <a name="defining-the-sampleconstants-class"></a>Definición de la clase SampleConstants  
  
1. En el **Explorador de soluciones**, seleccione el proyecto SampleRules.  
  
2. En el menú **Proyecto**, seleccione **Agregar clase**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento** .  
  
3. En el cuadro de texto **Nombre**, escriba SampleRuleConstants.cs y haga clic en el botón **Agregar**. El archivo SampleRuleConstants.cs se agrega al proyecto en el **Explorador de soluciones**.  
  
4. Abra el archivo SampleRuleConstants.cs y agregue las siguientes instrucciones de uso al archivo:  
  
    ```  
    namespace SampleRules  
    {   
        internal static class RuleConstants  
        {  
            /// <summary>  
            /// The name of the resources file to use when looking up rule resources  
            /// </summary>  
            public const string ResourceBaseName = "Public.Dac.Samples.Rules.RuleResources";  
  
            /// <summary>  
            /// Lookup name inside the resources file for the select asterisk rule name  
            /// </summary>  
            public const string AvoidWaitForDelay_RuleName = "AvoidWaitForDelay_RuleName";  
            /// <summary>  
            /// Lookup ID inside the resources file for the select asterisk description  
            /// </summary>  
            public const string AvoidWaitForDelay_ProblemDescription = "AvoidWaitForDelay_ProblemDescription";  
  
            /// <summary>  
            /// The design category (should not be localized)  
            /// </summary>  
            public const string CategoryDesign = "Design";  
  
            /// <summary>  
            /// The performance category (should not be localized)  
            /// </summary>  
            public const string CategoryPerformance = "Design";  
        }  
    }  
    ```  
  
5. Haga clic en **Archivo** > **Guardar**.  
  
## <a name="creating-the-custom-code-analysis-rule-class"></a>Creación de la clase de regla de análisis de código personalizada

Ahora que ha agregado las clases del asistente que la regla de análisis de código personalizada utilizará, deberá crear una clase de reglas personalizada y asignarle el nombre AvoidWaitForDelayRule. La regla personalizada AvoidWaitForDelayRule se utilizará para ayudar a los desarrolladores de base de datos a evitar las instrucciones WAITFOR DELAY en los procedimientos, desencadenadores y funciones almacenados.  
  
### <a name="creating-the-avoidwaitfordelayrule-class"></a>Creación de la clase AvoidWaitForDelayRule  
  
1. En el **Explorador de soluciones**, seleccione el proyecto SampleRules.  
  
2. En el menú **Proyecto**, seleccione **Agregar clase**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento** .  
  
3. En el cuadro de texto **Nombre**, escriba AvoidWaitForDelayRule.cs y, a continuación, haga clic en **Agregar**. El archivo AvoidWaitForDelayRule.cs se agrega al proyecto en el **Explorador de soluciones**.  
  
4. Abra el archivo AvoidWaitForDelayRule.cs y agregue las siguientes instrucciones de uso al archivo:  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;   
    namespace SampleRules {  
        class AvoidWaitForDelayRule {}  
    }  
    ```  
  
5. En la declaración de clase AvoidWaitForDelayRule, cambie el modificador de acceso a public:  
  
    ```  
    /// <summary>  
    /// This is a rule that returns a warning message   
    /// whenever there is a WAITFOR DELAY statement appears inside a subroutine body.  
    /// This rule only applies to stored procedures, functions and triggers.  
    /// </summary>  
    public sealed class AvoidWaitForDelayRule  
    ```  
  
6. Derive la clase AvoidWaitForDelayRule de la clase base Microsoft.SqlServer.Dac.CodeAnalysis.SqlCodeAnalysisRule:  
  
    ```  
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    ```  
  
7. Agregue el atributo LocalizedExportCodeAnalysisRuleAttribute a la clase.  
  
    LocalizedExportCodeAnalysisRuleAttribute permite al servicio de análisis de código detectar reglas de análisis de código personalizado. Solo las clases marcadas con un atributo ExportCodeAnalysisRuleAttribute (o un atributo que hereda de este) pueden utilizarse en el análisis de código.  
  
    LocalizedExportCodeAnalysisRuleAttribute proporciona algunos metadatos necesarios que el servicio utiliza. Esto incluye un identificador único para esta regla, un nombre que se mostrará en la interfaz de usuario de Visual Studio y una descripción que la regla puede utilizar para identificar los problemas.  
  
    ```  
    [LocalizedExportCodeAnalysisRule(AvoidWaitForDelayRule.RuleId,  
        RuleConstants.ResourceBaseName,  
        RuleConstants.AvoidWaitForDelay_RuleName,   
        RuleConstants.AvoidWaitForDelay_ProblemDescription  
        Category = RuleConstants.CategoryPerformance,   
        RuleScope = SqlRuleScope.Element)]   
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    {  
       /// <summary>  
       /// The Rule ID should resemble a fully-qualified class name. In the Visual Studio UI  
       /// rules are grouped by "Namespace + Category", and each rule is shown using "Short ID: DisplayName".  
       /// For this rule, that means the grouping will be "Public.Dac.Samples.Performance", with the rule  
       /// shown as "SR1004: Avoid using WaitFor Delay statements in stored procedures, functions and triggers."  
       /// </summary>  
       public const string RuleId = "RuleSamples.SR1004";  
    }  
    ```  
  
    La propiedad RuleScope debería ser Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Element, ya que esta regla analizará elementos específicos. La regla se llamará una vez para cada elemento coincidente del modelo. Si quiere analizar un modelo entero, puede usar Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Model en su lugar.  
  
8. Agregue un constructor que configure Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.SupportedElementTypes. Esto es necesario para las reglas de ámbito de elemento. Define los tipos de elemento a los que se aplicará esta regla. En este caso, la regla se aplicará a los procedimientos, desencadenadores y funciones almacenados. Tenga en cuenta que la clase Microsoft.SqlServer.Dac.Model.ModelSchema enumera todos los tipos de elementos que se pueden analizar.  
  
    ```  
    public AvoidWaitForDelayRule()  
    {  
       // This rule supports Procedures, Functions and Triggers. Only those objects will be passed to the Analyze method  
       SupportedElementTypes = new[]  
       {  
          // Note: can use the ModelSchema definitions, or access the TypeClass for any of these types  
          ModelSchema.ExtendedProcedure,  
          ModelSchema.Procedure,  
          ModelSchema.TableValuedFunction,  
          ModelSchema.ScalarFunction,  
  
          ModelSchema.DatabaseDdlTrigger,  
          ModelSchema.DmlTrigger,  
          ModelSchema.ServerDdlTrigger  
       };  
    }  
    ```  
  
9. Agregue una invalidación para el método Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.Analyze(Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext), que usa Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext como parámetros de entrada. Este método devuelve una lista de posibles problemas.  
  
    El método obtiene Microsoft.SqlServer.Dac.Model.TSqlModel, Microsoft.SqlServer.Dac.Model.TSqlObject y [TSqlFragment](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlfragment) a partir del parámetro de contexto. A continuación se utiliza la clase WaitForDelayVisitor para obtener una lista de todas las instrucciones WAITFOR DELAY del modelo.  
  
    Por cada elemento [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement) de la lista, se crea un Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleProblem.  
  
    ```  
    /// <summary>  
    /// For element-scoped rules the Analyze method is executed once for every matching   
    /// object in the model.   
    /// </summary>  
    /// <param name="ruleExecutionContext">The context object contains the TSqlObject being   
    /// analyzed, a TSqlFragment  
    /// that's the AST representation of the object, the current rule's descriptor, and a   
    /// reference to the model being  
    /// analyzed.  
    /// </param>  
    /// <returns>A list of problems should be returned. These will be displayed in the Visual   
    /// Studio error list</returns>  
    public override IList<SqlRuleProblem> Analyze(  
        SqlRuleExecutionContext ruleExecutionContext)  
    {  
         IList<SqlRuleProblem> problems = new List<SqlRuleProblem>();  
  
         TSqlObject modelElement = ruleExecutionContext.ModelElement;  
  
         // this rule does not apply to inline table-valued function  
         // we simply do not return any problem in that case.  
         if (IsInlineTableValuedFunction(modelElement))  
         {  
             return problems;  
         }  
  
         string elementName = GetElementName(ruleExecutionContext, modelElement);  
  
         // The rule execution context has all the objects we'll need, including the   
         // fragment representing the object,  
         // and a descriptor that lets us access rule metadata  
         TSqlFragment fragment = ruleExecutionContext.ScriptFragment;  
         RuleDescriptor ruleDescriptor = ruleExecutionContext.RuleDescriptor;  
  
         // To process the fragment and identify WAITFOR DELAY statements we will use a   
         // visitor   
         WaitForDelayVisitor visitor = new WaitForDelayVisitor();  
         fragment.Accept(visitor);  
         IList<WaitForStatement> waitforDelayStatements = visitor.WaitForDelayStatements;  
  
         // Create problems for each WAITFOR DELAY statement found   
         // When creating a rule problem, always include the TSqlObject being analyzed. This   
         // is used to determine  
         // the name of the source this problem was found in and a best guess as to the   
         // line/column the problem was found at.  
         //  
         // In addition if you have a specific TSqlFragment that is related to the problem   
         //also include this  
         // since the most accurate source position information (start line and column) will   
         // be read from the fragment  
         foreach (WaitForStatement waitForStatement in waitforDelayStatements)  
         {  
            SqlRuleProblem problem = new SqlRuleProblem(  
                String.Format(CultureInfo.CurrentCulture,   
                    ruleDescriptor.DisplayDescription, elementName),  
                modelElement,  
                waitForStatement);  
            problems.Add(problem);  
        }  
        return problems;  
    }  
  
    private static string GetElementName(  
        SqlRuleExecutionContext ruleExecutionContext,   
        TSqlObject modelElement)  
    {  
        // Get the element name using the built in DisplayServices. This provides a number of   
        // useful formatting options to  
        // make a name user-readable  
        var displayServices = ruleExecutionContext.SchemaModel.DisplayServices;  
        string elementName = displayServices.GetElementName(  
            modelElement, ElementNameStyle.EscapedFullyQualifiedName);  
        return elementName;  
    }  
  
    private static bool IsInlineTableValuedFunction(TSqlObject modelElement)  
    {  
        return TableValuedFunction.TypeClass.Equals(modelElement.ObjectType)  
                       && FunctionType.InlineTableValuedFunction ==   
            modelElement.GetMetadata<FunctionType>(TableValuedFunction.FunctionType);  
    }  
    ```  
  
10. Haga clic en **Archivo** > **Guardar**.  
  
### <a name="building-the-class-library"></a>Creación de la biblioteca de clases  
  
1. En el menú **Proyecto**, haga clic en **Propiedades de SampleRules**.  
  
2. Haga clic en la pestaña **Firma** .  
  
3. Haga clic en **Firmar el ensamblado**.  
  
4. En **Elegir un archivo de clave de nombre seguro**, haga clic en **<New>** .  
  
5. En el cuadro de diálogo **Crear clave de nombre seguro**, en **Nombre del archivo clave**, escriba MyRefKey.  
  
6. (opcional) Puede especificar una contraseña para el archivo de clave de nombre seguro.  
  
7. Haga clic en **OK**.  
  
8. En el menú **Archivo** , haga clic en **Guardar todo**.  
  
9. En el menú **Compilar**, haga clic en **Compilar solución**.  
  
A continuación, debe instalar el ensamblado para que se cargue cuando compile e implemente proyectos de SQL Server.  
  
## <a name="install-a-static-code-analysis-rule"></a>Instalación de una regla de análisis de código estático

Para instalar una regla, debe copiar el ensamblado y el archivo .pdb asociado en la carpeta Extensions.  
  
### <a name="to-install-the-samplerules-assembly"></a>Para instalar al ensamblado SampleRules

Después, copiará la información del ensamblado en el directorio Extensions. Cuando se inicie Visual Studio, identificará todas las extensiones en el directorio <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions y sus subdirectorios y las dejará disponibles para su uso.  
  
Para Visual Studio 2012, <Visual Studio Install Dir> habitualmente será C:\Archivos de programa (x86)\\MicrosoftVisual Studio 11.0. Para Visual Studio 2013, habitualmente será C:\Archivos de programa (x86)\\MicrosoftVisual Studio 12.0.  
  
Copie el archivo de ensamblado SampleRules.dll del directorio de salida al directorio <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions. De manera predeterminada, la ruta de acceso del archivo .dll compilado es YourSolutionPath\YourProjectPath\bin\Debug o YourSolutionPath\YourProjectPath\bin\Release.  
  
La regla deberá entonces instalarse y aparecer al reiniciar Visual Studio. Después, iniciará una nueva sesión de Visual Studio y creará un proyecto de base de datos.  
  
### <a name="starting-a-new-visual-studio-session-and-creating-a-database-project"></a>Inicio de una nueva sesión de Visual Studio y creación de un proyecto de base de datos  
  
1. Inicie una segunda sesión de Visual Studio.  
  
2. Haga clic en **Archivo** > **Nuevo** > **Proyecto**.  
  
3. En el cuadro de diálogo **Nuevo proyecto**, en la lista **Plantillas instaladas**, expanda el nodo **SQL Server** y, a continuación, haga clic en **Proyecto de base de datos de SQL Server**.  
  
4. En el cuadro de texto **Nombre**, escriba SampleRulesDB y haga clic en **Aceptar**.  
  
Por último, verá la nueva regla en el proyecto SQL Server. Para ver la nueva regla de análisis de código AvoidWaitForRule:  
  
1. En el **Explorador de soluciones**, seleccione el proyecto SampleRulesDB.  
  
2. En el menú **Proyecto** , haga clic en **Propiedades**. Se abrirá la página de propiedades de SampleRulesDB.  
  
3. Haga clic en **Análisis de código**. Verá una nueva categoría denominada RuleSamples.CategorySamples.  
  
4. Expanda RuleSamples.CategorySamples. Verá SR1004: Evitar instrucción WAITFOR DELAY en procedimientos, desencadenadores y funciones almacenados.  
  
## <a name="see-also"></a>Consulte también

[Información general sobre extensibilidad para reglas de análisis de código de base de datos](../ssdt/overview-of-extensibility-for-database-code-analysis-rules.md)
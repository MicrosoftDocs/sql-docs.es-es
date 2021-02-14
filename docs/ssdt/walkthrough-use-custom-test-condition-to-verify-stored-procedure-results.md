---
title: Condición de prueba personalizada para comprobar los resultados de un procedimiento almacenado
description: Siga los pasos de configuración de una condición de prueba personalizada que comprueba si un procedimiento almacenado devuelve el número correcto de columnas.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 4c33b494-a85e-4dd2-97b6-c88ee858a99c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d434df88cae0a5fb28add27cc9785e659f059ced
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042495"
---
# <a name="walkthrough-using-a-custom-test-condition-to-verify-the-results-of-a-stored-procedure"></a>Tutorial: Uso de una condición de prueba personalizada para comprobar el resultado de un procedimiento almacenado

En el tutorial de esta extensión de característica, creará una condición de prueba y comprobará su funcionalidad creando una prueba unitaria de SQL Server. El proceso incluye la creación de un proyecto de biblioteca de clases para la condición de prueba, así como su firma y su instalación. Si ya tiene una condición de prueba que desea actualizar, consulte [Cómo: Actualizar una condición de prueba personalizada de Visual Studio 2010 desde una versión anterior a SQL Server Data Tools](../ssdt/how-to-upgrade-visual-studio-2010-custom-test-condition-to-ssdt.md).  
  
En este tutorial se muestran las tareas siguientes:  
  
-   Cómo crear una condición de prueba.  
  
-   Cómo firmar el ensamblado con un nombre seguro.  
  
-   Cómo agregar las referencias necesarias al proyecto.  
  
-   Cómo compilar una condición de prueba.  
  
-   Cómo instalar la nueva condición de prueba.  
  
-   Cómo probar la nueva condición de prueba.  
  
Para completar este tutorial, debe tener Visual Studio 2010 o Visual Studio 2012 con la versión más reciente de SQL Server Data Tools. Para más información, consulte [Instalar SQL Server Data Tools](./download-sql-server-data-tools-ssdt.md).  
  
## <a name="creating-a-custom-test-condition"></a>Crear una condición de prueba personalizada  
En primer lugar, creará una biblioteca de clases.  
  
1.  En el menú **Archivo**, haga clic en **Nuevo** y, a continuación, haga clic en **Proyecto**.  
  
2.  En el cuadro de diálogo **Nuevo proyecto**, en **Tipos de proyecto**, haga clic en Visual C\#.  
  
3.  En **Plantillas**, seleccione **Biblioteca de clases**.  
  
4.  En el cuadro de texto **Nombre**, escriba **ColumnCountCondition** y haga clic en **Aceptar**.  
  
Después, firme el proyecto.  
  
1.  En el menú **Proyecto**, haga clic en **Propiedades de ColumnCountCondition**.  
  
2.  En la pestaña **Firma**, active la casilla **Firmar el ensamblado**.  
  
3.  En el cuadro **Elegir un archivo de clave de nombre seguro**, haga clic en **\<New...>** .  
  
    Aparecerá el cuadro de diálogo **Crear clave de nombre seguro**.  
  
4.  En el cuadro **Nombre del archivo de clave**, escriba **SampleKey**.  
  
5.  Escriba y confirme una contraseña y, a continuación, haga clic en **Aceptar**. Al compilar su solución, el archivo de clave se usa para firmar el ensamblado.  
  
6.  En el menú **Archivo** , haga clic en **Guardar todo**.  
  
7.  En el menú **Compilar**, haga clic en **Compilar solución**.  
  
Después, agregará las referencias necesarias al proyecto.  
  
1.  En el **Explorador de soluciones**, seleccione el proyecto **ColumnCountCondition**.  
  
2.  En el menú **Proyecto**, haga clic en **Agregar referencia** para mostrar el cuadro de diálogo **Agregar referencia**.  
  
3.  Seleccione la pestaña **.NET**.  
  
4.  En la columna **Nombre de componente**, busque y seleccione el componente **System.ComponentModel.Composition**. Haga clic en **Aceptar** después de seleccionar el componente.  
  
5.  Agregue las referencias de ensamblado necesarias. Haga clic con el botón derecho en el nodo del proyecto y, a continuación, haga clic en **Agregar referencia**. Haga clic en **Examinar** y navegue a la carpeta C:\Archivos de programa (x86)\\MicrosoftSQL Server\110\DAC\Bin. Seleccione Microsoft.Data.Tools.Schema.Sql.dll, haga clic en Agregar y, a continuación, haga clic en Aceptar.  
  
6.  En el menú **Proyecto**, haga clic en **Descargar el proyecto**.  
  
7.  Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y elija **Editar <project name>.csproj**.  
  
8.  Agregue la instrucción Import siguiente después de la importación de **Microsoft.CSharp.targets**:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. Guarde el archivo y ciérrelo. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y elija **Volver a cargar el proyecto**.  
  
    Las referencias necesarias se mostrarán bajo el nodo **Referencias** del proyecto en el **Explorador de soluciones**.  
  
## <a name="creating-the-resultsetcolumncountcondition-class"></a>Crear la clase ResultSetColumnCountCondition  
Ahora, cambiará el nombre de **Class1** a **ResultSetColumnCountCondition** y lo derivará de [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). La clase **ResultSetColumnCountCondition** es una condición de prueba simple que comprueba el número de columnas devueltas en el ResultSet. Puede emplear esta condición para asegurarse de que el contrato de un procedimiento almacenado es correcto.  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en Class1.cs, haga clic en **Cambiar nombre** y escriba **ResultSetColumnCountCondition.cs**.  
  
2.  Haga clic en **Sí** para confirmar el cambio de nombre de todas las referencias a Class1.  
  
3.  Abra el archivo **ResultSetColumnCountCondition.cs** y agregue las siguientes instrucciones using al archivo:  
  
    ```  
    using System;  
    using System.ComponentModel;  
    using System.Data;  
    using System.Data.Common;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
    namespace ColumnCountCondition {  
        public class ResultSetColumnCountCondition  
    ```  
  
4.  Derive la clase de [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)):  
  
    ```  
    public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
5.  Agregue [ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103)). Consulte [Cómo: Crear condiciones de prueba para el Diseñador de pruebas unitarias de SQL Server](../ssdt/how-to-create-test-conditions-for-the-sql-server-unit-test-designer.md) para más información sobre UnitTesting.Conditions.ExportTestConditionAttribute.  
  
    ```  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
        public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
6.  Crear las variables miembro y el constructor:  
  
    ```  
            private int _resultSet;  
            private int _count;  
            private int _batch;  
  
            public ResultSetColumnCountCondition() {  
                _resultSet = 1;  
                _count = 0;  
                _batch = 1;  
            }  
    ```  
  
7.  Invalide el método **Assert**. El método incluye argumentos para **IDbConnection**, que representa la conexión con la base de datos, y **SqlExecutionResult**. El método usa **DataSchemaException** para el control de errores:  
  
    ```  
           //method you need to override  
            //to perform the condition verification  
            public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
            {  
                //call base for parameter validation  
                base.Assert(validationConnection, results);  
  
                //verify batch exists  
                if (results.Length < _batch)  
                    throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
                SqlExecutionResult result = results[_batch - 1];  
  
                //verify resultset exists  
                if (result.DataSet.Tables.Count < ResultSet)  
                    throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
                DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
                //actual condition verification  
                //verify resultset column count matches expected  
                if (table.Columns.Count != Count)  
                    throw new DataException(String.Format(  
                        "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                        ResultSet, table.Columns.Count, Count));  
            }  
  
    Add the following method, which overrides the ToString method:  
    C#  
            //this method is called to provide the string shown in the  
            //test conditions panel grid describing what the condition tests  
            public override string ToString()  
            {  
                return String.Format(  
                    "Condition fails if ResultSet {0} does not contain {1} columns",  
                    ResultSet, Count);  
            }  
    ```  
  
8.  Agregue las siguientes propiedades de condición de prueba mediante los atributos **CategoryAttribute**, **DisplayNameAttribute** y **DescriptionAttribute**:  
  
    ```  
            //below are the test condition properties  
            //that are exposed to the user in the property browser  
            #region Properties  
  
            //property specifying the resultset for which  
            //you want to check the column count  
            [Category("Test Condition")]  
            [DisplayName("ResultSet")]  
            [Description("ResultSet Number")]  
            public int ResultSet  
            {  
                get { return _resultSet; }  
  
                set  
                {  
                    //basic validation  
                    if (value < 1)  
                        throw new ArgumentException("ResultSet cannot be less than 1");  
  
                    _resultSet = value;  
                }  
            }  
  
            //property specifying  
            //expected column count  
            [Category("Test Condition")]  
            [DisplayName("Count")]  
            [Description("Column Count")]  
            public int Count  
            {  
                get { return _count; }  
  
                set  
                {  
                    //basic validation  
                    if (value < 0)  
                        throw new ArgumentException("Count cannot be less than 0");  
  
                    _count = value;  
                }  
            }  
             #endregion  
    ```  
  
El código final es el siguiente:  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace ColumnCountCondition  
{  
  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
    public class ResultSetColumnCountCondition : TestCondition  
    {  
        private int _resultSet;  
        private int _count;  
        private int _batch;  
  
        public ResultSetColumnCountCondition()  
        {  
            _resultSet = 1;  
            _count = 0;  
            _batch = 1;  
        }  
  
        //method you need to override  
        //to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            //call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            //verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            //verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            //actual condition verification  
            //verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        //this method is called to provide the string shown in the  
        //test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        //below are the test condition properties  
        //that are exposed to the user in the property browser  
        #region Properties  
  
        //property specifying the resultset for which  
        //you want to check the column count  
        [Category("Test Condition")]  
        [DisplayName("ResultSet")]  
        [Description("ResultSet Number")]  
        public int ResultSet  
        {  
            get { return _resultSet; }  
  
            set  
            {  
                //basic validation  
                if (value < 1)  
                    throw new ArgumentException("ResultSet cannot be less than 1");  
  
                _resultSet = value;  
            }  
        }  
  
        //property specifying  
        //expected column count  
        [Category("Test Condition")]  
        [DisplayName("Count")]  
        [Description("Column Count")]  
        public int Count  
        {  
            get { return _count; }  
  
            set  
            {  
                //basic validation  
                if (value < 0)  
                    throw new ArgumentException("Count cannot be less than 0");  
  
                _count = value;  
            }  
        }  
  
        #endregion  
    }  
}  
  
```  
  
Después, compilaremos el proyecto.  
  
## <a name="compiling-the-project-and-installing-your-test-condition"></a><a name="xxx"></a>Compilar el proyecto e instalar la condición de prueba  
En el menú **Compilar**, haga clic en **Compilar solución**.  
  
Después, copiará la información del ensamblado en el directorio Extensions. Cuando Visual Studio se inicie, identificará cualquier extensión del directorio %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions y sus subdirectorios, y hará que estén disponibles para su uso:  
  
Copie el archivo de ensamblado **ColumnCountCondition.dll** desde el directorio de resultados al directorio %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions.  
  
De manera predeterminada, la ruta de acceso del archivo .dll compilado es *YourSolutionPath*\\*YourProjectPath*\bin\Debug o *YourSolutionPath*\\*YourProjectPath*\bin\Release.  
  
Después, iniciará una nueva sesión de Visual Studio y creará un proyecto de base de datos. Para iniciar una nueva sesión de Visual Studio y crear un proyecto de base de datos:  
  
1.  Inicie una segunda sesión de Visual Studio.  
  
2.  En el menú **Archivo**, haga clic en **Nuevo** y, a continuación, haga clic en **Proyecto**.  
  
3.  En el cuadro de diálogo **Nuevo proyecto**, en la lista de plantillas instaladas, seleccione el nodo **SQL Server**.  
  
4.  En el panel de detalles, haga clic en **Proyecto de base de datos de SQL Server**.  
  
5.  En el cuadro de texto **Nombre**, escriba **SampleConditionDB** y haga clic en **Aceptar**.  
  
Después, necesitamos crear una prueba unitaria. Para crear una prueba unitaria de SQL Server en una nueva clase de prueba:  
  
1.  En el menú **Prueba**, haga clic en **Nueva prueba** para mostrar el cuadro de diálogo **Agregar nueva prueba**.  
  
    También puede abrir el **Explorador de soluciones**, hacer clic con el botón derecho en un proyecto de prueba, seleccionar **Agregar** y, a continuación, hacer clic en **Nueva prueba**.  
  
2.  En la lista de plantillas, haga clic en **Prueba unitaria de SQL Server**.  
  
3.  En **Nombre de la prueba**, escriba **SampleUnitTest**.  
  
4.  En **Agregar a proyecto de prueba**, haga clic en **Crear un nuevo proyecto de prueba de Visual C\#** . Después, haga clic en **Aceptar** para mostrar el cuadro de diálogo **Nuevo proyecto de prueba**.  
  
5.  Escriba **SampleUnitTest** como nombre del proyecto.  
  
6.  Haga clic en **Cancelar** para crear la prueba unitaria sin configurar proyecto de prueba para que use una conexión de base de datos. Aparecerá la prueba en blanco en el Diseñador de pruebas unitarias de SQL Server. Se agregará un archivo de código fuente de Visual C\# al proyecto de prueba.  
  
    Para obtener más información acerca de cómo crear y configurar pruebas unitarias de base de datos con conexiones de base de datos, vea [Cómo: Crear una prueba unitaria de SQL Server vacía](../ssdt/how-to-create-an-empty-sql-server-unit-test.md).  
  
7.  Haga clic en **Haga clic aquí para crear** para terminar de crear la prueba unitaria. Verá que la nueva condición de prueba aparece en el proyecto de SQL Server.  
  
> [!NOTE]  
> Para usar la condición de prueba personalizada con proyectos de prueba unitaria existentes, se debe crear al menos una nueva clase de prueba unitaria de SQL Server. La referencia necesaria al ensamblado de condición de prueba se agregará al proyecto de prueba durante la creación de la clase de prueba.  
  
Para ver la nueva condición de prueba:  
  
1.  En el **Diseñador de pruebas unitarias de SQL Server**, bajo **Condiciones de prueba**, en la columna **Nombre**, haga clic en la prueba inconclusiveCondition1.  
  
2.  Haga clic en el botón **Eliminar condición de prueba** de la barra de herramientas para borrar la prueba inconclusiveCondition1.  
  
3.  Haga clic en el desplegable **Condiciones de prueba** y seleccione **Número de columnas de ResultSet**.  
  
4.  Haga clic en el botón **Agregar condición de prueba** de la barra de herramientas para agregar la condición de prueba personalizada.  
  
5.  En la ventana **Propiedades**, configure las propiedades Count, Enabled y ResultSet.  
  
    Para más información, vea: [Cómo: Incorporar condiciones de prueba a pruebas unitarias de SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Consulte también  
[Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  

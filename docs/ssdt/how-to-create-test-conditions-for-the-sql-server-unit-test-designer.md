---
title: Creación de condiciones de prueba para el Diseñador de pruebas unitarias de SQL Server
description: Aprenda a extender la clase TestCondition para crear una condición de prueba personalizada para el Diseñador de pruebas unitarias de SQL Server. Vea un ejemplo de una condición de prueba personalizada.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 48076062-1ef5-419a-8a55-3c7b4234cc35
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d74fe60ff94d2893dc5a2dd899643cabfd0a4f30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018285"
---
# <a name="how-to-create-test-conditions-for-the-sql-server-unit-test-designer"></a>Procedimientos: Creación de condiciones de prueba para el Diseñador de pruebas unitarias de SQL Server

Puede usar la clase extensible [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) para crear nuevas condiciones de prueba. Por ejemplo, puede crear una nueva condición de prueba que compruebe el número de columnas o los valores de un conjunto de resultados.  
  
## <a name="to-create-a-test-condition"></a>Para crear una condición de prueba  
En este procedimiento se explica cómo crear una condición de prueba de manera que aparezca en el Diseñador de pruebas unitarias de SQL Server.  
  
1.  En Visual Studio, cree un proyecto de biblioteca de clases.  
  
2.  En el menú **Proyecto**, haga clic en **Agregar referencia**.  
  
3.  Haga clic en la pestaña **.NET**.  
  
4.  En la lista **Nombre de componente**, seleccione **System.ComponentModel.Composition** y, después, haga clic en **Aceptar**.  
  
5.  Agregue las referencias de ensamblado necesarias. Haga clic con el botón derecho en el nodo del proyecto y, a continuación, haga clic en **Agregar referencia**. Haga clic en **Examinar** y navegue a la carpeta C:\Archivos de programa (x86)\\MicrosoftSQL Server\110\DAC\Bin. Seleccione Microsoft.Data.Tools.Schema.Sql.dll, haga clic en Agregar y, a continuación, haga clic en Aceptar.  
  
6.  En el menú **Proyecto**, haga clic en **Descargar el proyecto**.  
  
7.  Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y elija **Editar <project name>.csproj**.  
  
8.  Agregue las siguientes instrucciones Import después de la importación de Microsoft.CSharp.targets:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. Guarde el archivo y ciérrelo. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y elija **Volver a cargar el proyecto**.  
  
10. Derive la clase de la clase [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)).  
  
11. Firme el ensamblado con un nombre seguro. Para más información, vea: [Cómo: Firmar un ensamblado con un nombre seguro](/dotnet/standard/assembly/sign-strong-name).  
  
12. Compile la biblioteca de clases.  
  
13. Antes de poder usar la nueva condición de prueba, debe copiar el ensamblado firmado a la carpeta %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions. Si esta carpeta no existe, créela. Necesita privilegios administrativos en el equipo para copiar archivos a este directorio.  
  
14. Instale la condición de prueba. Para más información, consulte [Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
15. Agregue una nueva prueba unitaria de SQL Server al proyecto para crear una referencia a la condición de prueba que se agregará al proyecto. Puede agregar manualmente una referencia al ensamblado de la condición de prueba en el proyecto. Después de este paso, vuelva a cargar el diseñador.  
  
    > [!NOTE]  
    > Se debe agregar una clase de prueba para crear la referencia. Puede eliminar la clase de prueba una vez agregada la referencia.  
  
En el ejemplo siguiente, se crea una condición de prueba simple que comprueba el número de columnas devueltas en el ResultSet. Puede emplear esta condición de prueba simple para asegurarse de que el contrato de un procedimiento almacenado es correcto.  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace Ssdt.Samples.SqlUnitTesting  
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
  
        // method you need to override  
        // to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            // call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            // verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            // verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            // actual condition verification  
            // verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        // this method is called to provide the string shown in the  
        // test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        // below are the test condition properties  
        // that are exposed to the user in the property browser  
        #region Properties  
  
        // property specifying the resultset for which  
        // you want to check the column count  
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
  
        // property specifying  
        // expected column count  
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
  
La clase de la condición de prueba personalizada hereda de la clase base [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). Debido a las propiedades adicionales de la condición de prueba personalizada, los usuarios pueden configurar la condición desde la ventana Propiedades después de haber instalado la condición.  
  
[ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103)) debe agregarse a clases que extiendan [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). Este atributo permite que SQL Server Data Tools detecte la clase y la use durante el diseño y la ejecución de pruebas unitarias. El atributo toma dos parámetros:  
  
|Parámetro del atributo|Posición|Descripción|  
|-----------------------|------------|---------------|  
|DisplayName|1|Identifica la cadena en el cuadro combinado “Condiciones de prueba”. El nombre debe ser único. Si hay dos condiciones con el mismo nombre para mostrar, se mostrará al usuario la primera condición encontrada y se mostrará una advertencia en el Administrador de errores de Visual Studio.|  
|ImplementingType|2|Se emplea para identificar la extensión de manera única. Necesita cambiar este parámetro para que coincida con el tipo en el que pone el atributo. Este ejemplo usa el tipo **ResultSetColumnCountCondition**, por tanto use **typeof(ResultSetColumnCountCondition)** . Si el tipo es **NewTestCondition**, utilice **typeof(NewTestCondition)** .|  
  
En este ejemplo, se agregan dos propiedades. Los usuarios de la condición de prueba personalizada pueden usar la propiedad ResultSet con el fin de especificar para qué conjunto de resultados se debe comprobar el recuento de columnas. Después, los usuarios pueden emplear la propiedad Count para especificar el recuento de columnas esperado.  
  
Se agregan tres atributos para cada propiedad:  
  
-   El nombre de categoría, que ayuda a organizar las propiedades.  
  
-   El nombre para mostrar de la propiedad.  
  
-   Una descripción de la propiedad.  
  
La validación se realiza sobre las propiedades para comprobar que el valor de la propiedad ResultSet no es menor que uno y que el valor de la propiedad Count es mayor que cero.  
  
El método Assert realiza la tarea principal de la condición de prueba. Invalide el método Assert para validar que se cumple la condición esperada. Este método proporciona dos parámetros:  
  
-   El primer parámetro es la conexión de base de datos empleada para validar la condición de prueba.  
  
-   El segundo parámetro, y el más importante, es la matriz de resultados, que devuelve un único elemento de matriz para cada lote ejecutado.  
  
Solo se admite un lote para cada script de prueba. Por tanto, las condiciones de prueba siempre examinarán el primer elemento de la matriz. El elemento de la matriz contiene un DataSet que, a su vez, contiene los conjuntos de resultados devueltos para el script de prueba. En este ejemplo, el código comprueba que la tabla de datos del DataSet contiene el número de columnas adecuado. Para obtener más información, vea DataSet.  
  
Debe establecer la biblioteca de clases que contiene la condición de prueba que se va a firmar, lo que puede hacer en la pestaña Firma de las propiedades del proyecto.  
  
## <a name="see-also"></a>Consulte también  
[Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  

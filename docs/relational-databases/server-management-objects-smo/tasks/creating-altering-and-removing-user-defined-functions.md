---
description: Crear, modificar y eliminar las funciones definidas por el usuario
title: Crear, modificar y eliminar las funciones definidas por el usuario
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- user-defined functions [SMO]
ms.assetid: 0ebebd3b-0775-41c2-989d-aa4cf81af12a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a5f2adc78d8885f534d852b8d7f51913e50f104a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468516"
---
# <a name="creating-altering-and-removing-user-defined-functions"></a>Crear, modificar y eliminar las funciones definidas por el usuario
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]
  El <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> objeto proporciona funcionalidad que permite a los usuarios administrar mediante programación las funciones definidas por el usuario en [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Las funciones definidas por el usuario admiten los parámetros de entrada y salida y también admiten las referencias directas a las columnas de la tabla.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiere registrar los ensamblados en una base de datos antes de que se puedan utilizar en los procedimientos almacenados, funciones definidas por el usuario, desencadenadores y los tipos de datos definidos por el usuario. SMO admite esta característica con el objeto <xref:Microsoft.SqlServer.Management.Smo.SqlAssembly>.  
  
 El objeto <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> hace referencia al ensamblado .NET con las propiedades <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.AssemblyName%2A>, <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.ClassName%2A> y <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.MethodName%2A>.  
  
 Cuando el objeto <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> hace referencia a un ensamblado .NET, debe registrar el ensamblado creando un objeto <xref:Microsoft.SqlServer.Management.Smo.SqlAssembly> y agregándolo al objeto <xref:Microsoft.SqlServer.Management.Smo.SqlAssemblyCollection>, que pertenece al objeto <xref:Microsoft.SqlServer.Management.Smo.Database>.  
  
## <a name="example"></a>Ejemplo  
 Para utilizar cualquier ejemplo de código que se proporcione, deberá elegir el entorno de programación, la plantilla de programación y el lenguaje de programación con los que crear su aplicación. Para obtener más información, vea [crear un proyecto de Visual C&#35; SMO en Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-a-scalar-user-defined-function-in-visual-basic"></a>Crear una función escalar definida por el usuario en Visual Basic  
 En este ejemplo de código se muestra cómo crear y quitar una función escalar definida por el usuario que tiene un parámetro de objeto <xref:System.DateTime> de entrada y un tipo de valor devuelto entero de [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]. La función definida por el usuario se crea en la base de datos de [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . En el ejemplo se crea una función definida por el usuario, ISOweek, que toma un argumento de fecha y calcula el número de semana ISO. Para que esta función realice los cálculos correctamente, la opción DATEFIRST de base de datos debe establecerse en 1 antes de que se llame a la función.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 2008R2 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Define a UserDefinedFunction object variable by supplying the parent database and the name arguments in the constructor.
Dim udf As UserDefinedFunction
udf = New UserDefinedFunction(db, "IsOWeek")
'Set the TextMode property to false and then set the other properties.
udf.TextMode = False
udf.DataType = DataType.Int
udf.ExecutionContext = ExecutionContext.Caller
udf.FunctionType = UserDefinedFunctionType.Scalar
udf.ImplementationType = ImplementationType.TransactSql
'Add a parameter.
Dim par As UserDefinedFunctionParameter
par = New UserDefinedFunctionParameter(udf, "@DATE", DataType.DateTime)
udf.Parameters.Add(par)
'Set the TextBody property to define the user defined function.
udf.TextBody = "BEGIN  DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;"
'Create the user defined function on the instance of SQL Server.
udf.Create()
'Remove the user defined function.
udf.Drop()
``` 
  
## <a name="creating-a-scalar-user-defined-function-in-visual-c"></a>Crear una función escalar definida por el usuario en Visual C#  
 En este ejemplo de código se muestra cómo crear y quitar una función escalar definida por el usuario que tiene un parámetro de objeto <xref:System.DateTime> de entrada y un tipo de valor devuelto entero de [!INCLUDE[csprcs](../../../includes/csprcs-md.md)]. La función definida por el usuario se crea en la base de datos de [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . En el ejemplo se crea la función definida por el usuario. `ISOweek`. Esta función toma un argumento de fecha para calcular el número de semana ISO. Para que esta función realice los cálculos correctamente, la opción `DATEFIRST` de base de datos debe establecerse en `1` antes de que se llame a la función.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
           Server srv = new Server();  
            //Reference the AdventureWorks2012 database.   
           Database db = srv.Databases["AdventureWorks2012"];  
  
            //Define a UserDefinedFunction object variable by supplying the parent database and the name arguments in the constructor.   
            UserDefinedFunction udf = new UserDefinedFunction(db, "IsOWeek");  
  
            //Set the TextMode property to false and then set the other properties.   
            udf.TextMode = false;  
            udf.DataType = DataType.Int;  
            udf.ExecutionContext = ExecutionContext.Caller;  
            udf.FunctionType = UserDefinedFunctionType.Scalar;  
            udf.ImplementationType = ImplementationType.TransactSql;  
  
            //Add a parameter.   
  
     UserDefinedFunctionParameter par = new UserDefinedFunctionParameter(udf, "@DATE", DataType.DateTime);  
            udf.Parameters.Add(par);  
  
            //Set the TextBody property to define the user-defined function.   
            udf.TextBody = "BEGIN DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;";  
  
            //Create the user-defined function on the instance of SQL Server.   
            udf.Create();  
  
            //Remove the user-defined function.   
            udf.Drop();  
        }  
```  
  
## <a name="creating-a-scalar-user-defined-function-in-powershell"></a>Crear una función escalar definida por el usuario en PowerShell  
 En este ejemplo de código se muestra cómo crear y quitar una función escalar definida por el usuario que tiene un parámetro de objeto <xref:System.DateTime> de entrada y un tipo de valor devuelto entero de [!INCLUDE[csprcs](../../../includes/csprcs-md.md)]. La función definida por el usuario se crea en la base de datos de [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . En el ejemplo se crea la función definida por el usuario. `ISOweek`. Esta función toma un argumento de fecha para calcular el número de semana ISO. Para que esta función realice los cálculos correctamente, la opción `DATEFIRST` de base de datos debe establecerse en `1` antes de que se llame a la función.  
  
```powershell   
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item Adventureworks2012  
  
# Define a user defined function object variable by supplying the parent database and name arguments in the constructor.   
$udf  = New-Object -TypeName Microsoft.SqlServer.Management.SMO.UserDefinedFunction `  
-argumentlist $db, "IsOWeek"  
  
# Set the TextMode property to false and then set the other properties.   
$udf.TextMode = $false  
$udf.DataType = [Microsoft.SqlServer.Management.SMO.DataType]::Int   
$udf.ExecutionContext = [Microsoft.SqlServer.Management.SMO.ExecutionContext]::Caller  
$udf.FunctionType = [Microsoft.SqlServer.Management.SMO.UserDefinedFunctionType]::Scalar  
$udf.ImplementationType = [Microsoft.SqlServer.Management.SMO.ImplementationType]::TransactSql  
  
# Define a Parameter object variable by supplying the parent function, name and type arguments in the constructor.  
$type = [Microsoft.SqlServer.Management.SMO.DataType]::DateTime  
$par  = New-Object -TypeName Microsoft.SqlServer.Management.SMO.UserDefinedFunctionParameter `  
-argumentlist $udf, "@DATE",$type  
  
# Add the parameter to the function  
$udf.Parameters.Add($par)  
  
#Set the TextBody property to define the user-defined function.   
$udf.TextBody = "BEGIN DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;"  
  
# Create the user-defined function on the instance of SQL Server.   
$udf.Create()  
  
# Remove the user-defined function.   
$udf.Drop()  
```  
  
## <a name="see-also"></a>Consulte también  
 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction>  
  
  

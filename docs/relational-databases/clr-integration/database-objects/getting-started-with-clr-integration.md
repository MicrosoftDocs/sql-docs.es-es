---
title: Introducción con la integración CLR | Microsoft Docs
description: En este artículo se describen los espacios de nombres y las bibliotecas necesarios para compilar objetos de base de datos mediante la integración de Microsoft SQL Server con el .NET Framework CLR.
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: quickstart
dev_langs:
- TSQL
- VB
- CSharp
helpviewer_keywords:
- database objects [CLR integration]
- namespaces [CLR integration]
- database objects [CLR integration], about CLR integration
- stored procedures [CLR integration]
- common language runtime [SQL Server], about CLR integration
- building database objects [CLR integration], about CLR integration
- common language runtime [SQL Server], stored procedures
- common language runtime [SQL Server], namespaces
- Hello World example [CLR integration]
- library [CLR integration]
ms.assetid: c73e628a-f54a-411a-bfe3-6dae519316cc
author: rothja
ms.author: jroth
ms.openlocfilehash: c307172d7bf8b258cbd56b4ef4abfe6704750358
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192365"
---
# <a name="getting-started-with-clr-integration"></a>Introducción a la integración CLR

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

En este tema se proporciona información general sobre los espacios de nombres y las bibliotecas necesarios para compilar objetos de base de datos mediante la [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] integración de con el Common Language Runtime de .NET Framework (CLR). En este tema también se muestra cómo escribir, compilar y ejecutar un procedimiento almacenado CLR simple escrito en [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#.  
  
## <a name="required-namespaces"></a>Espacios de nombres necesarios  

Los componentes necesarios para desarrollar objetos de base de datos CLR básicos se instalan con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La funcionalidad de la integración CLR se expone en un ensamblado denominado system.data.dll, que forma parte de .NET Framework. Este ensamblado puede encontrarse en la caché de ensamblados global (GAC) o en el directorio de .NET Framework. Normalmente, se agrega una referencia a este ensamblado mediante las herramientas de la línea de comandos y [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio, por lo que no es necesario agregarla manualmente.  
  
El ensamblado system.data.dll contiene los espacios de nombres siguientes, que son necesarios para compilar objetos de base de datos CLR:  
  
- `System.Data`  
- `System.Data.Sql`  
- `Microsoft.SqlServer.Server`  
- `System.Data.SqlTypes`  

> [!TIP]
> Se admite la carga de objetos de base de datos de CLR en Linux, pero deben compilarse con la .NET Framework (SQL Server la integración CLR no es compatible con .NET Core). Además, los ensamblados CLR con el conjunto de permisos EXTERNAL_ACCESS o Unsafe no se admiten en Linux.

## <a name="writing-a-simple-hello-world-stored-procedure"></a>Escribir un procedimiento almacenado "Hello World" simple  

Copie y pegue el siguiente código de Visual C# o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic en un editor de texto y guárdelo en un archivo denominado "helloworld.cs" o "helloworld.vb".  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Server;  
using System.Data.SqlTypes;  
  
public class HelloWorldProc  
{  
    [Microsoft.SqlServer.Server.SqlProcedure]  
    public static void HelloWorld(out string text)  
    {  
        SqlContext.Pipe.Send("Hello world!" + Environment.NewLine);  
        text = "Hello world!";  
    }  
}  
```  
  
```vb  
Imports System  
Imports System.Data  
Imports Microsoft.SqlServer.Server  
Imports System.Data.SqlTypes  
Imports System.Runtime.InteropServices  
  
Public Class HelloWorldProc  
    \<Microsoft.SqlServer.Server.SqlProcedure> _   
    Public Shared  Sub HelloWorld(\<Out()> ByRef text as String)  
        SqlContext.Pipe.Send("Hello world!" & Environment.NewLine)  
        text = "Hello world!"  
    End Sub  
End Class  
  
```  
  
Este programa simple contiene un solo método estático en una clase pública. Este método usa dos nuevas clases, **[SqlContext](/dotnet/api/microsoft.sqlserver.server.sqlcontext)** y **[SqlPipe](/dotnet/api/microsoft.sqlserver.server.sqlpipe)**, para crear objetos de base de datos administrados con el fin de generar un mensaje de texto simple. El método también asigna la cadena "Hello World!" como el valor de un parámetro de salida. Este método puede declararse como un procedimiento almacenado en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y, a continuación, ejecutarse del mismo modo que un procedimiento almacenado de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
Compile este programa como una biblioteca, cárguelo en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y ejecútelo como un procedimiento almacenado.  
  
## <a name="compile-the-hello-world-stored-procedure"></a>Compilar el procedimiento almacenado "Hola mundo"  

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] instala los archivos de redistribución de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework de forma predeterminada. Estos archivos incluyen csc.exe y vbc.exe, los compiladores de línea de comandos de Visual C# y Visual Basic. Para compilar el ejemplo, debe modificar la variable de ruta de acceso para que apunte al directorio que contiene los archivos csc.exe o vbc.exe. Ésta es la ruta de instalación predeterminada de .NET Framework.  
  
`C:\Windows\Microsoft.NET\Framework\(version)`  
  
La versión incluye el número de versión del componente .NET Framework redistribuible instalado. Por ejemplo:  
  
`C:\Windows\Microsoft.NET\Framework\v4.6.1`

Una vez agregado el directorio .NET Framework a la ruta de acceso, puede compilar el procedimiento almacenado de ejemplo en un ensamblado con el siguiente comando. La opción **/target** le permite compilarlo en un ensamblado.  
  
Para los archivos de origen de Visual C#:  
  
`csc /target:library helloworld.cs`  
  
 Para los archivos de origen de Visual Basic:  
  
`vbc /target:library helloworld.vb`  
  
Estos comandos inician el compilador de Visual C# o Visual Basic mediante la opción /target para especificar la creación de un archivo DLL de biblioteca.  
  
## <a name="loading-and-running-the-hello-world-stored-procedure-in-sql-server"></a>Cargar y ejecutar el procedimiento almacenado "Hello World" en SQL Server  

Una vez que el procedimiento de ejemplo se haya compilado correctamente, podrá probarlo en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para ello, abra [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] y cree una nueva consulta, conectándose a una base de datos de prueba adecuada, como la base de datos de ejemplo AdventureWorks.  
  
La capacidad de ejecutar el código de Common Language Runtime (CLR) está establecida de forma predeterminada en OFF en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El código CLR se puede habilitar mediante el **sp_configure** procedimiento almacenado del sistema. Para más información, consulte [Enabling CLR Integration](../../../relational-databases/clr-integration/clr-integration-enabling.md).  
  
Tendremos que crear el ensamblado de modo que podamos obtener acceso al procedimiento almacenado. Para este ejemplo, vamos a suponer que ha creado el ensamblado helloworld.dll en el directorio C:\. Agregue la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] a su consulta.  
  
`CREATE ASSEMBLY helloworld from 'c:\helloworld.dll' WITH PERMISSION_SET = SAFE`  
  
Una vez creado el ensamblado, podremos obtener acceso a nuestro método HelloWorld mediante la instrucción CREATE PROCEDURE. Llamaremos a nuestro procedimiento almacenado "hello":  
  
```sql
CREATE PROCEDURE hello  
@i nchar(25) OUTPUT  
AS  
EXTERNAL NAME helloworld.HelloWorldProc.HelloWorld  
-- if the HelloWorldProc class is inside a namespace (called MyNS),  
-- the last line in the create procedure statement would be  
-- EXTERNAL NAME helloworld.[MyNS.HelloWorldProc].HelloWorld  
```  
  
Una vez creado el procedimiento, podrá ejecutarse como un procedimiento almacenado normal escrito en [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Ejecute el comando siguiente:  
  
```sql
DECLARE @J nchar(25)  
EXEC hello @J out  
PRINT @J  
```  
  
Este comando debería dar lugar a la siguiente salida en la ventana de mensajes de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
```
Hello world!  
Hello world!  
```  
  
## <a name="removing-the-hello-world-stored-procedure-sample"></a>Quitar el ejemplo de procedimiento almacenado "Hello World"  

Cuando haya terminado de ejecutar el procedimiento almacenado de ejemplo, puede quitar el procedimiento y el ensamblado de la base de datos de prueba.  
  
En primer lugar, quite el procedimiento mediante el comando de drop procedure.  
  
```sql
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'hello')  
   drop procedure hello  
```  
  
Una vez quitado el procedimiento, puede quitar el ensamblado que contiene su código de ejemplo.  
  
```sql
IF EXISTS (SELECT name FROM sys.assemblies WHERE name = 'helloworld')  
   drop assembly helloworld  
```  
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la integración de CLR en SQL Server, consulte los siguientes artículos:

- [Procedimientos almacenados de CLR](/dotnet/framework/data/adonet/sql/clr-stored-procedures)
- [Extensiones específicas en proceso de SQL Server a ADO.NET](../../../relational-databases/clr-integration-data-access-in-process-ado-net/sql-server-in-process-specific-extensions-to-ado-net.md)
- [Depurar objetos de base de datos CLR](../../../relational-databases/clr-integration/debugging-clr-database-objects.md)
- [Seguridad de la integración CLR](../../../relational-databases/clr-integration/security/clr-integration-security.md)
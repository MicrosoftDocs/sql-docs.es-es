---
description: MSSQLSERVER_6522
title: MSSQLSERVER_6522
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 6522 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 99c0a18330de144cc2a20ead150b1c0c5b7a3fc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199018"
---
# <a name="mssqlserver_6522"></a>MSSQLSERVER_6522
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|6522|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|SQLCLR_UDF_EXEC_FAILED|
|Texto del mensaje|Error de .NET Framework durante la ejecución de la rutina o agregado definido por el usuario "%.*ls": %ls.|
||

## <a name="explanation"></a>Explicación

Considere los siguientes escenarios.

### <a name="scenario-1"></a>Escenario 1

Cree una rutina de Common Language Runtime (CLR) que haga referencia a un ensamblado de Microsoft .NET Framework. El ensamblado de .NET Framework no se documenta en [922672](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). A continuación, instale la corrección basada en .NET Framework 3.5 o en .NET Framework 2.0.

### <a name="scenario-2"></a>Escenario 2

Cree un ensamblado y, a continuación, registre el ensamblado en una base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. A continuación, instale una versión diferente del ensamblado en la caché global de ensamblados (GAC).

Al ejecutar la rutina de CLR o usar el ensamblado de cualquiera de estos escenarios en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], recibirá un mensaje de error similar al siguiente:

> Servidor:  Mensaje 6522, nivel 16, estado 2, línea 1  
Error de .NET Framework durante la ejecución de la rutina o agregado definido por el usuario "getsid":
>
> System.IO.FileLoadException: No se puede cargar el archivo o ensamblado 'System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' ni una de sus dependencias. El ensamblado del almacén de host tiene una signatura diferente a la del ensamblado de GAC. (Excepción de HRESULT: 0x80131050)

## <a name="possible-cause"></a>Causa posible

Cuando CLR carga un ensamblado, CLR comprueba que el mismo ensamblado se encuentra en la GAC. Si el mismo ensamblado se encuentra en la GAC, CLR comprueba que los identificadores de versión del módulo (MVID) de estos ensamblados coinciden. Si los MVID de estos ensamblados no coinciden, recibirá el mensaje de error que se menciona en la sección [Explicación](#explanation).

Cuando se vuelve a compilar un ensamblado, cambia el MVID del mismo. Por lo tanto, si actualiza el .NET Framework, los ensamblados de .NET Framework tienen MVID diferentes porque esos ensamblados se vuelven a compilar. Además, si actualiza su propio ensamblado, este se volverá a compilar. Por lo tanto, el ensamblado también tiene un MVID diferente.

## <a name="user-action"></a>Acción del usuario

### <a name="action-1"></a>Acción 1

Para solucionar el escenario 1 de la sección [Explicación](#explanation), debe actualizar manualmente los ensamblados de .NET Framework en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para ello, use la instrucción `ALTER ASSEMBLY` de modo que apunte a la nueva versión del ensamblado de .NET Framework en la siguiente carpeta:

`%Windir%\Microsoft.NET\Framework\Version`

> [!NOTE]
> **Versión** representa la versión de .NET Framework que instaló o actualizó.

### <a name="action-2"></a>Acción 2

Para solucionar el escenario 2 de la sección [Explicación](#explanation), utilice la instrucción `ALTER ASSEMBLY` para actualizar el ensamblado en la base de datos.

Si el problema sigue existiendo después de hacerlo, coloque el ensamblado de la base de datos y, a continuación, registre la nueva versión del ensamblado en la base de datos.

## <a name="more-information"></a>Más información

No se recomienda usar ensamblados de .NET Framework que no estén documentados en [Directiva de soporte técnico para los ensamblados de .NET Framework que no se hayan probado en el entorno alojado en CLR de SQL Server](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). Muestra los ensamblados que se prueban en el entorno alojado en CLR de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="description-of-clr-routines"></a>Descripción de las rutinas CLR

Las rutinas CLR incluyen los siguientes objetos que se implementan mediante la integración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con CLR de .NET Framework:

- Funciones definidas por el usuario con valores escalares (UDF escalares)
- Funciones definidas por el usuario con valores de tabla (TVF)
- Procedimientos definidos por el usuario (UDP)
- Desencadenadores definidos por el usuario
- Tipos de datos definidos por el usuario
- Agregados definidos por el usuario

### <a name="assemblies-to-update-after-you-install-the-net-framework-35"></a>Ensamblados que se van a actualizar después de instalar .NET Framework 3.5

Después de instalar .NET Framework 3.5, debe usar la instrucción ALTER ASSEMBLY para actualizar los siguientes ensamblados:

- Accessibility.dll
- AspNetMMCExt.dll
- Cscompmgd.dll
- IEExecRemote.dll
- IEHost.dll
- IIEHost.dll
- Microsoft.Build.Conversion.dll
- Microsoft.Build.Engine.dll
- Microsoft.Build.Framework.dll
- Microsoft.Build.Tasks.dll 
- Microsoft.Build.Utilities.dll 
- Microsoft.CompactFramework.Build.Tasks.dll 
- Microsoft.JScript.dll 
- Microsoft.VisualBasic.Vsa.dll 
- Microsoft.Vsa.dll 
- Microsoft.Vsa.Vb.CodeDOMProcessor.dll 
- Microsoft_VsaVb.dll 
- Sysglobl.dll 
- System.Configuration.Install.dll 
- System.Design.dll 
- System.DirectoryServices.dll 
- System.DirectoryServices.Protocols.dll 
- System.Drawing.dll 
- System.Drawing.Design.dll 
- System.EnterpriseServices.dll 
- System.Management.dll 
- System.Messaging.dll 
- System.Runtime.Serialization.Formatters.Soap.dll 
- System.ServiceProcess.dll 
- System.Web.dll 
- System.Web.Mobile.dll 
- System.Web.RegularExpressions.dll 

Estos ensamblados se encuentran en la siguiente carpeta:

`%Windir%\Microsoft.NET\Framework\v2.0.50727`

### <a name="how-to-preserve-the-data-from-user-defined-data-types-after-you-drop-an-assembly"></a>Cómo conservar los datos de los tipos de datos definidos por el usuario después de colocar un ensamblado

Si coloca un ensamblado que usa un tipo de datos definido por el usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede utilizar uno de los métodos siguientes para conservar los datos.

Supongamos que lo siguiente es el escenario:

- Cree un ensamblado cuyo nombre sea *MyAssembly.dll*
- El ensamblado MyAssembly hace referencia al ensamblado `System.DirectoryServices.dll`.
- Tiene un tipo de datos definido por el usuario cuyo nombre es *MyDateTime*.
- El tipo de datos *MyDateTime* utiliza el ensamblado *MyAssembly.dll*.
- Cree una tabla cuyo nombre sea *MyTable*.
- La tabla *MyTable* contiene los datos del tipo de datos *MyDateTime*.

#### <a name="method-1-use-the-bcpexe-utility"></a>Método 1: Uso de la utilidad bcp.exe

1. Use la utilidad Bcp.exe junto con el modificador -n para copiar los datos de la tabla MyTable en un archivo. Por ejemplo, ejecute el comando siguiente en el símbolo del sistema:

    ```console
    bcp MyDatabase.dbo.MyTable out C:\MyFile.bcp -n -SSQLServerName -T
    ```

2. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, siga estos pasos:
   1. Coloque la tabla MyTable.
   2. Coloque el tipo de datos MyDateTime.
   3. Coloque el ensamblado `System.DirectoryServices.dll`.
   4. Coloque el ensamblado MyAssembly.
3. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, siga estos pasos:

   1. Registre el ensamblado `System.DirectoryServices.dll`.
   2. Registre el ensamblado MyAssembly.
   3. Cree el tipo de datos MyDateTime.
   4. Cree una nueva tabla que tenga la misma estructura de tabla que la tabla MyTable.
4. Use la utilidad Bcp.exe junto con el modificador -n para importar los datos del archivo en la tabla MyTable. Por ejemplo, ejecute el comando siguiente en el símbolo del sistema:
  
    ```console
    bcp MyDatabase.dbo.MyTable in C:\MyFile.bcp -n -SSQLServerName -T
    ```

#### <a name="method-2-use-the-insert--select-statement"></a>Método 2: Uso de la instrucción INSERT... Instrucción SELECT

Supongamos que el tipo de datos MyDateTime ocupa 9 bytes en el almacenamiento.

1. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, cree una nueva tabla que contenga una columna del tipo de datos `VARBINARY(9)` mediante la ejecución de la siguiente instrucción:

    ```sql
    CREATE TABLE TempTable (c1 VARBINARY(9));
    ```

2. Ejecute la siguiente instrucción INSERT... Instrucción SELECT para rellenar la tabla TempTable:

    ```sql
    INSERT INTO TempTable SELECT CAST(c1 as VARBINARY(9)) FROM MyTable;
    ```

3. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, siga estos pasos:
   1. Coloque la tabla MyTable.
   2. Coloque el tipo de datos MyDateTime.
   3. Coloque el ensamblado System.DirectoryServices.dll.
   4. Coloque el ensamblado MyAssembly.
4. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, siga estos pasos:
   1. Registre el ensamblado System.DirectoryServices.dll.
   2. Registre el ensamblado MyAssembly.
   3. Cree el tipo de datos MyDateTime.
   4. Cree una nueva tabla que tenga la misma estructura de tabla que la tabla MyTable.
5. Ejecute la siguiente instrucción INSERT... Instrucción SELECT para rellenar la tabla MyTable:

    ```sql
    INSERT INTO MyTable SELECT c1 FROM TempTable;
    ```

## <a name="references"></a>Referencias

- Para obtener más información sobre la versión del ensamblado, consulte la [documentación retirada de Visual Studio 2005](https://www.microsoft.com/download/details.aspx?id=55984).
- Para obtener más información sobre cómo actualizar un ensamblado, consulte [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md).
- Para obtener más información sobre cómo colocar un ensamblado, consulte [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md).
- Para obtener más información sobre cómo registrar un ensamblado en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consulte [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)
- Para más información acerca de la utilidad Bcp.exe, consulte [https://msdn2.microsoft.com/library/ms162802.aspx](../../tools/bcp-utility.md).
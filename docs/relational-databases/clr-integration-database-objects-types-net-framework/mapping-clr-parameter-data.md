---
title: Asignar datos de parámetros CLR | Microsoft Docs
description: En este artículo se enumeran los tipos de datos de Microsoft SQL Server, los equivalentes en CLR para SQL Server y los equivalentes de CLR nativos en el .NET Framework.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- SqlBinary data type
- SqlInt16 data type
- SqlMoney data type
- SqlString data type
- SqlSingle data type
- data types [CLR integration]
- SqlInt64 data type
- SqlDateTime data type
- SqlXml data type
- SqlBoolean data type
- SqlDecimal data type
- SqlBytes data type
- mapping data types [CLR integration]
- SqlChars data type
- SqlInt32 data type
ms.assetid: 89b43ee9-b9ad-4281-a4bf-c7c8d116daa2
author: rothja
ms.author: jroth
ms.openlocfilehash: bed0d2f1b133d3d155c3afd59a86652f010316ea
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250644"
---
# <a name="mapping-clr-parameter-data"></a>Asignar datos de parámetros CLR
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En la tabla siguiente se enumeran los [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos de datos, sus equivalentes en el Common Language Runtime (CLR) para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el espacio de nombres **System. Data. SqlTypes** y sus equivalentes de CLR nativos en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework.  
  
||||  
|-|-|-|  
|**Tipos de datos de SQL Server**|Tipo (en System.Data.SqlTypes o Microsoft.SqlServer.Types)|**Tipo de datos CLR (.NET Framework)**|  
|**bigint**|**SqlInt64**|**Int64, que admite valores NULL\<Int64>**|  
|**binary**|**SqlBytes, SqlBinary**|**Byte []**|  
|**bit**|**SqlBoolean**|**Booleano, acepta valores NULL\<Boolean>**|  
|**char**|None|None|  
|**cursor**|None|None|  
|**date**|**SqlDateTime**|**DateTime, que admite valores NULL\<DateTime>**|  
|**datetime**|**SqlDateTime**|**DateTime, que admite valores NULL\<DateTime>**|  
|**datetime2**|None|**DateTime, que admite valores NULL\<DateTime>**|  
|**DATETIMEOFFSET**|**None**|**DateTimeOffset, que admite valores NULL\<DateTimeOffset>**|  
|**decimal**|**SqlDecimal**|**Decimal, admite valores NULL\<Decimal>**|  
|**float**|**SqlDouble**|**Double, que admite valores NULL\<Double>**|  
|**geography**|**SqlGeography**<br /><br /> **SqlGeography** se define en Microsoft.SqlServer.Types.dll, que se instala con SQL Server y se puede descargar desde el [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [Feature Pack](https://www.microsoft.com/download/details.aspx?id=100430).|None|  
|**geometry**|**SqlGeometry**<br /><br /> **SqlGeometry** se define en Microsoft.SqlServer.Types.dll, que se instala con SQL Server y se puede descargar desde el [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [Feature Pack](https://www.microsoft.com/download/details.aspx?id=100430).|None|  
|**hierarchyid**|**SqlHierarchyId**<br /><br /> **SqlHierarchyId** se define en Microsoft.SqlServer.Types.dll, que se instala con SQL Server y se puede descargar desde el [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [Feature Pack](https://www.microsoft.com/download/details.aspx?id=100430).|None|  
|**image**|None|None|  
|**int**|**SqlInt32**|**Int32, que admite valores NULL\<Int32>**|  
|**money**|**SqlMoney**|**Decimal, admite valores NULL\<Decimal>**|  
|**nchar**|**SqlChars, SqlString**|**String, Char[]**|  
|**ntext**|None|None|  
|**numeric**|**SqlDecimal**|**Decimal, admite valores NULL\<Decimal>**|  
|**nvarchar**|**SqlChars, SqlString**<br /><br /> **SQLChars** es una mejor coincidencia para la transferencia de datos y el acceso, y **SQLString** es una coincidencia mejor para realizar operaciones de cadena.|**String, Char[]**|  
|**nvarchar (1), NCHAR (1)**|**SqlChars, SqlString**|**Char, String, Char [], que admite valores NULL\<char>**|  
|**real**|**SqlSingle** (el intervalo de **SqlSingle**, sin embargo, es mayor que **real**)|**Single, que admite valores NULL\<Single>**|  
|**rowversion**|None|**Byte []**|  
|**smallint**|**SqlInt16**|**Int16, que admite valores NULL\<Int16>**|  
|**smallmoney**|**SqlMoney**|**Decimal, admite valores NULL\<Decimal>**|  
|**sql_variant**|None|**Object**|  
|**table**|None|None|  
|**text**|None|None|  
|**time**|None|**TimeSpan, que admite valores NULL\<TimeSpan>**|  
|**timestamp**|None|None|  
|**tinyint**|**SqlByte**|**Byte, que admite valores NULL\<Byte>**|  
|**uniqueidentifier**|**SqlGuid**|**GUID, que admite valores NULL\<Guid>**|  
|**Tipo definido por el usuario (UDT)**|None|La misma clase que está enlazada al tipo definido por el usuario en el mismo ensamblado o en un ensamblado dependiente.|  
|**varbinary**|**SqlBytes, SqlBinary**|**Byte []**|  
|**varbinary (1), binario (1)**|**SqlBytes, SqlBinary**|**byte, Byte [], que admite valores NULL\<byte>**|  
|**varchar**|None|None|  
|**xml**|**SqlXml**|None|  
  
## <a name="automatic-data-type-conversion-with-out-parameters"></a>Conversión automática de tipos de datos con parámetros OUT  
 Un método CLR puede devolver información al código o programa que realiza la llamada marcando un parámetro de entrada con el modificador **out** (Microsoft Visual C#) o **\<Out()> ByRef** (Microsoft Visual Basic) si el parámetro de entrada es un tipo de datos de CLR en el sistema. el espacio de nombres **Data. SqlTypes** y el programa que realiza la llamada especifican su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos equivalente como parámetro de entrada, se produce una conversión de tipos automáticamente cuando el método CLR devuelve el tipo de datos.  
  
 Por ejemplo, el siguiente procedimiento almacenado CLR tiene un parámetro de entrada de tipo de datos de CLR **SqlInt32** que está marcado con **out** (C#) o **\<Out()> ByRef** (Visual Basic):  
  
```csharp  
[Microsoft.SqlServer.Server.SqlProcedure]  
public static void PriceSum(out SqlInt32 value)  
{ ... }  
```  
  
```vb  
\<Microsoft.SqlServer.Server.SqlProcedure> _  
Public Shared Sub PriceSum( \<Out()> ByRef value As SqlInt32)  
...  
End Sub  
```  
  
 Una vez generado y creado el ensamblado en la base de datos, el procedimiento almacenado se crea en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el siguiente Transact-SQL, que especifica un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos **int** como parámetro de salida:  
  
```  
CREATE PROCEDURE PriceSum (@sum int OUTPUT)  
AS EXTERNAL NAME TestStoredProc.StoredProcedures.PriceSum  
```  
  
 Cuando se llama al procedimiento almacenado CLR, el tipo de datos **SqlInt32** se convierte automáticamente en un tipo de datos **int** y se devuelve al programa que realiza la llamada.  
  
 Pero no todos los tipos de datos CLR se pueden convertir automáticamente en sus tipos de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] equivalentes mediante un parámetro OUT. En la tabla siguiente se enumeran estas excepciones.  
  
|||  
|-|-|  
|**Tipo de datos CLR (SQL Server)**|**Tipos de datos de SQL Server**|  
|**Decimal**|SMALLMONEY|  
|**SqlMoney**|SMALLMONEY|  
|**Decimal**|money|  
|**DateTime**|smalldatetime|  
|**SQLDateTime**|smalldatetime|  
  
## <a name="change-history"></a>Historial de cambios  
  
|Contenido actualizado|  
|---------------------|  
|Se han agregado los tipos **SqlGeography**, **SqlGeometry** y **SqlHierarchyId** a la tabla de asignación.|  
  
## <a name="see-also"></a>Consulte también  
 [Tipos de datos de SQL Server en .NET Framework](../../relational-databases/clr-integration-database-objects-types-net-framework/sql-server-data-types-in-the-net-framework.md)  
  
  

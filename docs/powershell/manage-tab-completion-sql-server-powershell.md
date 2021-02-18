---
title: Administrar la finalización mediante tabulador (SQL Server PowerShell)
description: Obtenga información sobre cómo controlar la finalización con tabulación de Windows PowerShell mediante el uso correcto de tres variables en los módulos de SQL Server PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 6296848a-890f-4ad3-8d9f-92ed6a79aa00
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: cca82925c95b360bff522bc813798485628d4382
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339040"
---
# <a name="manage-tab-completion-with-sql-server-powershell"></a>Administrar la finalización mediante tabulador con SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 Los complementos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell incluyen tres variables (**$SqlServerMaximumTabCompletion**, **$SqlServerMaximumChildItems** y **$SqlServerIncludeSystemObjects**) para controlar la finalización mediante el tabulador de Windows PowerShell. La finalización mediante el tabulador reduce la cantidad de texto que se debe escribir al devolver las tablas de elementos cuyos nombres empiezan por la cadena que está escribiendo.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Con la finalización mediante el tabulador de PowerShell, cuando se ha escrito parte de una ruta de acceso o nombre de cmdlet, se puede presionar la tecla Tab para obtener una lista de los elementos cuyos nombres coincidan con lo que ya se ha escrito. Se puede seleccionar a continuación el elemento que desee en la lista sin tener que escribir el resto del nombre.  

Si está trabajando en una base de datos que tiene varios objetos, las listas para finalización mediante tabulador pueden llegar a ser grandes. Algunos tipos de objeto de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , como las vistas, también tienen una gran cantidad de objetos de sistema.  

Los complementos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] presentan tres variables del sistema que se pueden usar para controlar la cantidad de información presentada por la finalización mediante tabulador y **Get-ChildItem**.

## <a name="sqlservermaximumtabcompletion--n"></a>$SqlServerMaximumTabCompletion =** *n*

Especifica el número máximo de objetos que se incluyen en una lista de finalización mediante tabulador. Si selecciona Tab en un nodo de ruta de acceso que tiene más de *n* objetos, la lista de finalización mediante tabulador se trunca en *n*. *n* es un entero. El valor predeterminado es 0, es decir, no hay límite para el número de objetos que se muestran.  

## <a name="sqlservermaximumchilditems--n"></a>$SqlServerMaximumChildItems =** *n*

Especifica el número máximo de objetos mostrados por **Get-ChildItem**. Si se ejecuta **Get-ChildItem** en un nodo de ruta de acceso que tiene más de *n* objetos, la lista se trunca en *n*. *n* es un entero. El valor predeterminado es 0, es decir, no hay límite para el número de objetos que se muestran.  

## <a name="sqlserverincludesystemobjects---true--false-"></a>$SqlServerIncludeSystemObjects =** { **$True** | **$False** }

Si es **$True**, los objetos del sistema se muestran con finalización mediante tabulador y **Get-ChildItem**. Si es **$False**, no se muestran objetos del sistema. El valor de configuración predeterminado es **$False**.  

## <a name="set-the-sql-server-tab-completion-variables"></a>Establecer las variables de finalización mediante tabulador de SQL Server

Para cualquiera de las variables a las que desee cambiar el valor predeterminado, establezca la variable en el nuevo valor.  

### <a name="example-powershell"></a>Ejemplo (PowerShell)

En el ejemplo siguiente se establecen las tres variables y se enumeran sus valores:  

```powershell
$SqlServerMaximumTabCompletion = 20  
$SqlServerMaximumChildItems = 10  
$SqlServerIncludeSystemObjects = $False  
dir variable:sqlserver*  
```

## <a name="see-also"></a>Consulte también

- [Instalación de SQL Server PowerShell](download-sql-server-ps-module.md)
- [SQL Server PowerShell](sql-server-powershell.md)
---
title: sqlps, utilidad
description: La utilidad sqlps inicia una sesión de Windows PowerShell con el proveedor de SQL Server PowerShell y los cmdlets cargados y registrados.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- sqlps utility
- PowerShell [SQL Server], sqlps utility
ms.assetid: 4b2515a6-12c3-44fb-b263-1c567681cd2b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 87fbc1760cecc26f98b4c6e6f610f32683015e66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338577"
---
# <a name="sqlps-utility"></a>sqlps, utilidad
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La utilidad **sqlps** inicia una sesión de Windows PowerShell 2.0 con el proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell y los cmdlets cargados y registrados. Puede escribir scripts o comandos de PowerShell que usen los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell para trabajar con instancias de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y sus objetos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../includes/ssnotedepfutureavoid-md.md)] En su lugar, use el módulo **sqlps** de PowerShell. Para más información acerca del módulo **sqlps** , consulte [Import the SQLPS Module](../powershell/download-sql-server-ps-module.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlps   
[ [ [ -NoLogo ][ -NoExit ][ -NoProfile ]  
    [ -OutPutFormat { Text | XML } ] [ -InPutFormat { Text | XML } ]  
  ]  
  [ -Command { -  
             | script_block [ -args argument_array ]  
             | string [ command_parameters ]  
             }  
  ]  
]  
[ -? | -Help ]  
```  
  
## <a name="arguments"></a>Argumentos  
 **-NoLogo**  
 Especifica que la utilidad **sqlps** debe ocultar el título de copyright cuando se inicia.  
  
 **-NoExit**  
 Especifica que la utilidad **sqlps** debe seguir ejecutándose después de que los comandos de inicio se hayan completado.  
  
 **-NoProfile**  
 Especifica que la utilidad **sqlps** no debe cargar un perfil de usuario. Los perfiles de usuario registran los alias, funciones y variables de uso frecuente para usarlos en las sesiones de PowerShell.  
  
 **-OutPutFormat** { **Text** | **XML** }  
 Especifica que se dé formato a la salida de la utilidad **sqlps** como cadenas de texto (**Text**) o en un formato CLIXML serializado (**XML**).  
  
 **-InPutFormat** { **Text** | **XML** }  
 Especifica que se dé formato a la entrada de la utilidad **sqlps** como cadenas de texto (**Text**) o en un formato CLIXML serializado (**XML**).  
  
 **-Command**  
 Especifica el comando que debe ejecutar la utilidad **sqlps** . La utilidad **sqlps** ejecuta el comando y luego se cierra, a menos que también se especifique **-NoExit** . No especifique ningún otro modificador después de **-Command**; se leerían como parámetros del comando.  
  
 **-**  
 **-Command-** especifica que la utilidad **sqlps** debe leer la entrada desde la entrada estándar.  
  
 *script_block* [ **-args**_argument\_array_ ]  
 Especifica un bloque de comandos de PowerShell que se han de ejecutar; el bloque debe incluirse entre llaves: {}. *Script_block* solo se puede especificar cuando se llama a la utilidad **sqlps** desde **PowerShell** o desde otra sesión de la utilidad **sqlps** . *argument_array* es una matriz de variables de PowerShell que contiene los argumentos de los comandos de PowerShell en *script_block*.  
  
 *string* [ *command_parameters* ]  
 Especifica una cadena que contiene los comandos de PowerShell que se han de ejecutar. Use el formato **"&{** _command_ **}"** . Las comillas indican una cadena y el operador de invocación (&) hace que la utilidad **sqlps** ejecute el comando.  
  
 [ **-?** |  **-Help** ]  
 Muestra el resumen de la sintaxis de las opciones de la utilidad **sqlps** .  
  
## <a name="remarks"></a>Observaciones  
 La utilidad **sqlps** inicia el entorno de PowerShell (PowerShell.exe) y carga el módulo de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell. El módulo, también denominado **sqlps**, carga y registra estos complementos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell:  
  
-   Microsoft.SqlServer.Management.PSProvider.dll  
  
     Implementa el proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell y cmdlets asociados como **Encode-SqlName** y **Decode-SqlName**.  
  
-   Microsoft.SqlServer.Management.PSSnapin.dll  
  
     Implementa los cmdlets **Invoke-Sqlcmd** e **Invoke-PolicyEvaluation** .  
  
 Puede usar la utilidad **sqlps** para realizar las tareas siguientes:  
  
-   Ejecutar interactivamente comandos de PowerShell.  
  
-   Ejecutar archivos de script de PowerShell.  
  
-   Ejecutar cmdlets de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
-   Usar las rutas de acceso del proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para navegar por la jerarquía de objetos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 De forma predeterminada, la utilidad **sqlps** se ejecuta con la directiva de ejecución de scripting establecida en **Restricted**. Esto evita la ejecución de cualquier script de PowerShell. Puede usar el cmdlet **Set-ExecutionPolicy** para habilitar la ejecución de scripts firmados o de cualquier script. Ejecute solo scripts de orígenes de confianza y proteja todos los archivos de entrada y salida con los permisos NTFS adecuados. Para obtener más información sobre cómo habilitar los scripts de PowerShell, vea [Running Windows PowerShell Scripts](/previous-versions/system-center/virtual-machine-manager-2008-r2/cc917925(v=technet.10)).  
  
 La versión de la utilidad **sqlps** en [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] se implementó como un shell mínimo de Windows PowerShell 1.0. Los shells mínimos tienen ciertas restricciones, como no permitir que los usuarios carguen complementos distintos de los que ha cargado el shell mínimo. Estas restricciones no se aplican a la versión [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] y posteriores de la utilidad, que se han cambiado para usar el módulo **sqlps** .  
  
## <a name="examples"></a>Ejemplos  
 **A. Ejecutar la utilidad sqlps en modo predeterminado e interactivo sin el título de copyright**  
  
```  
sqlps -NoLogo  
```  
  
 **B. Ejecutar un script de SQL Server PowerShell desde el símbolo del sistema**  
  
```  
sqlps -Command "&{.\MyFolder.MyScript.ps1}"  
```  
  
 **C. Ejecutar un script de SQL Server PowerShell desde el símbolo del sistema y continuar la ejecución una vez que el script se complete**  
  
```  
sqlps -NoExit -Command "&{.\MyFolder.MyScript.ps1}"  
```  
  
## <a name="see-also"></a>Consulte también  
 [Habilitar o deshabilitar un protocolo de red de servidor](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [SQL Server PowerShell](../powershell/sql-server-powershell.md)  
  

---
description: SSMS (Utilidad)
title: SSMS (Utilidad)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], opening
- command prompt utilities [SQL Server], sqlwb
- sqlwb utility
- Management Studio command line
- opening SQL Server Management Studio
ms.assetid: aafda520-9e2a-4e1e-b936-1b165f1684e8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/24/2020
ms.openlocfilehash: 102a07f1b26b8c1e31807227686dd8f1f6856237
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344181"
---
# <a name="ssms-utility"></a>SSMS (Utilidad)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

La utilidad **SSMS** abre [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. Si se especifica, **Ssms** también establece una conexión con un servidor y abre consultas, scripts, archivos, proyectos y soluciones.

Puede especificar archivos que contienen consultas, proyectos o soluciones. Los archivos que contienen consultas se conectan automáticamente con un servidor si se proporciona información de conexión y el tipo de archivo está asociado con ese tipo de servidor. Por ejemplo, los archivos .sql abren una ventana del Editor de consultas de SQL en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], y los archivos .mdx abren una ventana del Editor de consultas MDX en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. Las **Soluciones y proyectos de SQL Server** se abren en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)].

> [!NOTE]
> La utilidad **Ssms** no ejecuta consultas. Para ejecutar consultas desde la línea de comandos, utilice la utilidad **sqlcmd** . 

## <a name="syntax"></a>Sintaxis

```syntaxsql
Ssms
[scriptfile] [projectfile] [solutionfile] 
[-S servername] [-d databasename] [-G] [-U username] [-E] [-nosplash] [-log [filename]?] [-?] 
```

## <a name="arguments"></a>Argumentos

*scriptfile* Especifica uno o más archivos de script para abrirlos. El parámetro debe contener la ruta completa a los archivos. 

*projectfile* Especifica un proyecto de script para abrirlo. El parámetro debe contener la ruta completa al archivo del proyecto de script. 

*solutionfile* Especifica una solución para abrirla. El parámetro debe contener la ruta completa al archivo de solución. 

[ **-S** _servername_] Nombre de servidor

[ **-d** _databasename_] Nombre de base de datos

[ **-G**] Conexión con la autenticación de Azure Active Directory. El tipo de conexión se determina en función de si se incluye **-U**.

> [!Note]
> **Active Directory - Universal compatible con MFA** no se admite actualmente.

[ **-U** _username_] Nombre de usuario cuando se conecta con "Autenticación de SQL".

> [!Note]
> **-P** se quitó en la versión 18.0 de SSMS.
>
> Solución alternativa: Intente conectarse al servidor una vez con la interfaz de usuario y guarde la contraseña.

[ **-E**] Conectarse mediante la autenticación de Windows.

[ **-nosplash**] Impide que [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] muestre el gráfico de la pantalla de presentación mientras se abre. Utilice esta opción cuando se conecte con un equipo que ejecute [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] mediante Terminal Services en una conexión con ancho de banda limitado. Este argumento no distingue entre mayúsculas y minúsculas y puede aparecer antes o después de otros argumentos.

[ **-log** _[filename]?_ ] Registra la actividad de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] en el archivo especificado para la solución de problemas.

[ **-?** ] Muestra la ayuda de la línea de comandos.

## <a name="remarks"></a>Observaciones

Todos los modificadores son opcionales y están separados por un espacio, excepto los archivos, que están separados por comas. Si no especifica modificadores, **Ssms** abre [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] según se especifica en la configuración de **Opciones** del menú **Herramientas** . Por ejemplo, si en la página **Entorno/General**, la opción **Al iniciar** especifica **Abrir nueva ventana de consulta**, **Ssms** se abre con un Editor de consultas en blanco.

El modificador **-log** debe aparecer al final de la línea de comandos, después de todos los demás modificadores. El argumento de nombre de archivo es opcional. Si se especifica un nombre de archivo y el archivo no existe, este se crea. Si no es posible crear el archivo, por ejemplo, debido a un acceso de escritura insuficiente, el registro se escribe en la ubicación APPDATA no localizada (ver más abajo). Si no se especifica el argumento de nombre de archivo, se escriben dos archivos en la carpeta de datos de la aplicación no localizada del usuario actual. La carpeta de datos de la aplicación no localizada para SQL Server se puede obtener de la variable de entorno APPDATA. Por ejemplo, para SQL Server 2012, la carpeta es \<system drive>:\Users\\<nombreDeUsuario\>\AppData\Roaming\Microsoft\AppEnv\10.0\\. De forma predeterminada, los dos archivos se denominan ActivityLog.xml y ActivityLog.xsl. El primero contiene los datos del registro de actividad y el segundo es una hoja de estilos XML que permite visualizar correctamente el archivo XML. Siga estos pasos para ver el archivo de registro en el visor XML predeterminado, como Internet Explorer: haga clic en Inicio y, después, en Ejecutar...; luego, escriba "\<system drive>:\Users\\<nombreDeUsuario\>\AppData\Roaming\Microsoft\AppEnv\10.0\ActivityLog.xml" en el campo proporcionado y, por último, presione Entrar.

Los archivos que contienen consultas solicitan la conexión con un servidor si se proporciona información de conexión y el tipo de archivo está asociado con ese tipo de servidor. Por ejemplo, los archivos .sql abren una ventana del Editor de consultas de SQL en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], y los archivos .mdx abren una ventana del Editor de consultas MDX en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. Las **Soluciones y proyectos de SQL Server** se abren en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)].

La siguiente tabla asigna tipos de servidores a extensiones de archivos.

| Tipo de servidor | Extensión |
|-------------|-----------|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|.sql|
|SQL Server Analysis Services|.mdx<br /><br /> .xmla|

## <a name="examples"></a>Ejemplos

El siguiente script abre [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] desde un símbolo del sistema con la configuración predeterminada:

```console
  Ssms
```

Los siguientes scripts abren [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] desde un símbolo del sistema por medio de *Active Directory - Integrado*:

```console
Ssms.exe -S servername.database.windows.net -G
```

El siguiente script abre [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] desde un símbolo del sistema, con autenticación de Windows, con el Editor de código establecido en el servidor `ACCTG and the database AdventureWorks2012,` sin mostrar la pantalla de presentación:

```console
Ssms -E -S ACCTG -d AdventureWorks2012 -nosplash
```

El siguiente script abre [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] desde un símbolo del sistema y abre el script de MonthEndQuery:

```console
Ssms "C:\Documents and Settings\username\My Documents\SQL Server Management Studio Projects\FinanceScripts\FinanceScripts\MonthEndQuery.sql"
```

El siguiente script abre [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] desde un símbolo del sistema y abre el proyecto NewReportsProject en el equipo denominado `developer`:

```console
Ssms "\\developer\fin\ReportProj\ReportProj\NewReportProj.ssmssqlproj"
```

El siguiente script abre [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] desde un símbolo del sistema y abre la solución MonthlyReports: 

```console
Ssms "C:\solutionsfolder\ReportProj\MonthlyReports.ssmssln"
```

## <a name="see-also"></a>Consulte también

[Usar SQL Server Management Studio](./sql-server-management-studio-ssms.md)
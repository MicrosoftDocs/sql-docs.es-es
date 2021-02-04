---
title: SqlLocalDB (utilidad)
description: La herramienta de la línea de comandos de la utilidad SqlLocalDB permite a los usuarios y desarrolladores crear y administrar una instancia de LocalDB de SQL Server Express.
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- SqlLocalDB utility [SQL Server]
- local database runtime utility
- LocalDB, SqlLocalDB Utility
ms.assetid: d785cdb7-1ea0-4871-bde9-1ae7881190f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 269bdcc45622fdf0c422e1aae50537bbd9275d46
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076067"
---
# <a name="sqllocaldb-utility"></a>SqlLocalDB (utilidad)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  Utilice la utilidad **SqlLocalDB** para crear una instancia de [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssExpCurrent](../includes/ssexpcurrent-md.md)] **LocalDB**. La utilidad **SqlLocalDB** (SqlLocalDB.exe) es una herramienta de línea de comandos simple que permite a los usuarios y desarrolladores crear y administrar una instancia de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] **LocalDB**. Para obtener más información sobre cómo usar **LocalDB**, vea [SQL Server 2016 Express LocalDB](../database-engine/configure-windows/sql-server-express-localdb.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
SqlLocalDB.exe   
{  
      [ create   | c ] \<instance-name>  \<instance-version> [-s ]  
    | [ delete   | d ] \<instance-name>  
    | [ start    | s ] \<instance-name>  
    | [ stop     | p ] \<instance-name>  [ -i ] [ -k ]  
    | [ share    | h ] [" <user_SID> " | " <user_account> " ] " \<private-name> " " \<shared-name> "  
    | [ unshare  | u ] " \<shared-name> "  
    | [ info     | i ] \<instance-name>  
    | [ versions | v ]  
    | [ trace    | t ] [ on | off ]  
    | [ help     | -? ]  
}  
```  
  
## <a name="arguments"></a>Argumentos  
 [ **create** | **c** ] *\<instance-name>* *\<instance-version>* [ **-s** ]  
 Crea una instancia de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**. **SqlLocalDB** utiliza la versión de los archivos binarios de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] especificados por el argumento *\<instance-version>* . El número de versión se especifica en formato numérico con al menos un decimal. Los números de versión secundaria (Service Pack) son opcionales. Como los siguientes dos números de versión son aceptables: 11.0 u 11.0.1186. La versión especificada se debe instalar en el equipo. Si no se especifica, el número de versión tiene como valor predeterminado el de la versión de la utilidad **SqlLocalDB** . Al agregar **-s**, se inicia la nueva instancia de **LocalDB**.  
  
 [ **share** | **h** ]  
 Comparte la instancia privada especificada de **LocalDB** por medio del nombre compartido especificado. Si se omite el SID del usuario o el nombre de la cuenta, usa de forma predeterminada el usuario actual.  
  
 [ **unshared** | **u** ]  
 Detiene el uso compartido de la instancia compartida especificada de **LocalDB**.  
  
 [ **delete** | **d** ] *\<instance-name>*  
 Elimina la instancia especificada de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**.  
  
 [ **start** | **s** ] " *\<instance-name>* "  
 Inicia la instancia especificada de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**. Cuando se realiza correctamente, la instrucción devuelve la dirección de la canalización con nombre de **LocalDB**.  
  
 [ **stop** | **p** ] *\<instance-name>* [ **-i** ] [ **-k** ]  
 Detiene la instancia especificada de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**. Al agregar **-i**, se solicita el cierre de la instancia con la opción **NOWAIT**. Al agregar **-k**, se elimina el proceso de la instancia sin ponerse en contacto con ella.  
  
 [ **info** | **i** ] [ *\<instance-name>* ]  
 Muestra toda la instancia de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** que pertenece al usuario actual.  
  
 *\<instance-name>* devuelve el nombre, la versión, el estado (En ejecución o Detenido), la última hora de inicio de la instancia especificada de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** y el nombre de la canalización local de **LocalDB**.  
  
 [ **trace** | **t** ] **on** | **off**  
 **trace on** habilita el seguimiento de las llamadas a la API de **SqlLocalDB** para el usuario actual. **trace off** deshabilita el seguimiento.  
  
 **-?**  
 Devuelve una descripción breve de cada opción de **SqlLocalDB** .  
  
## <a name="remarks"></a>Observaciones  
 El argumento *instance name* debe cumplir las reglas de los identificadores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o debe incluirse entre comillas.  
  
 La ejecución de SqlLocalDB sin argumentos devuelve el texto de ayuda.  
  
 Las operaciones distintas de la operación de inicio solo se pueden realizar en una instancia perteneciente al usuario que ha iniciado la sesión. Una instancia de SQLLOCALDB, cuando se comparte, solo se puede iniciar y detener por el propietario de la instancia.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-an-instance-of-localdb"></a>A. Crear una instancia de LocalDB  
 En el ejemplo siguiente se crea una instancia de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** denominada `DEPARTMENT` utilizando los binarios de [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] y se inicia la instancia.  
  
```  
SqlLocalDB.exe create "DEPARTMENT" 12.0 -s  
```  
  
### <a name="b-working-with-a-shared-instance-of-localdb"></a>B. Trabajar con una instancia compartida de LocalDB  
 Abra un símbolo del sistema utilizando los permisos de administrador.  
  
```  
SqlLocalDB.exe create "DeptLocalDB"  
SqlLocalDB.exe share "DeptLocalDB" "DeptSharedLocalDB"  
SqlLocalDB.exe start "DeptLocalDB"  
SqlLocalDB.exe info "DeptLocalDB"  
REM The previous statement outputs the Instance pipe name for the next step  
sqlcmd -S np:\\.\pipe\LOCALDB#<use your pipe name>\tsql\query  
CREATE LOGIN NewLogin WITH PASSWORD = 'Passw0rd!!@52';   
GO  
CREATE USER NewLogin;  
GO  
EXIT  
```  
  
 Ejecute el código siguiente para conectarse a la instancia compartida de **LocalDB** mediante el inicio de sesión de `NewLogin` .  
  
```  
sqlcmd -S (localdb)\.\DeptSharedLocalDB -U NewLogin -P Passw0rd!!@52  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server 2016 Express LocalDB](../database-engine/configure-windows/sql-server-express-localdb.md)  
[Herramienta de administración de la línea de comandos: SqlLocalDB.exe](../relational-databases/express-localdb-instance-apis/command-line-management-tool-sqllocaldb-exe.md)  

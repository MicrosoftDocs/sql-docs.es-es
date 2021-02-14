---
title: Habilitación o deshabilitación de la administración remota de paquetes de R
description: Habilitación de la administración remota de paquetes de R en SQL Server 2016 R Services o SQL Server Machine Learning Services (en base de datos)
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 728dc656d3fdb94abae0a5c2c0e2665af2c4807c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073931"
---
# <a name="enable-or-disable-remote-package-management-for-sql-server"></a>Habilitación o deshabilitación de la administración remota de paquetes para SQL Server
[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

En este artículo se describe cómo habilitar la administración remota de paquetes de R desde una estación de trabajo cliente o un servidor Machine Learning Server diferente. Una vez habilitada la característica de administración de paquetes en SQL Server, puede usar comandos de RevoScaleR en un cliente para instalar paquetes en SQL Server.

De forma predeterminada, la característica de administración de paquetes externos para SQL Server está deshabilitada. Debe ejecutar un script independiente para habilitar la característica como se describe en la sección siguiente.

## <a name="overview-of-process-and-tools"></a>Información general acerca del proceso y las herramientas

Para habilitar o deshabilitar la administración de paquetes en SQL Server, use la utilidad de línea de comandos **RegisterRExt.exe**, incluida en el paquete **RevoScaleR**.

La [habilitación](#bkmk_enable) de esta característica es un proceso de dos pasos que requiere un administrador de base de datos: habilite la administración de paquetes en la instancia de SQL Server (una vez por instancia de SQL Server) y, a continuación, habilite la administración de paquetes en la base de datos SQL (una vez por cada base de datos de SQL Server).

La [deshabilitación](#bkmk_disable) de la característica de administración de paquetes también requiere varios pasos: quite los permisos y los paquetes de nivel de base de datos (una vez por base de datos) y, luego, quite los roles del servidor (una vez por instancia).

## <a name="enable-package-management"></a><a name="bkmk_enable"></a> Habilitación de la administración de paquetes

1. En SQL Server, abra un símbolo del sistema con privilegios elevados y navegue hasta la carpeta donde se encuentra la utilidad, RegisterRExt.exe. La ubicación predeterminada es `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExe.exe`.

2. Ejecute el siguiente comando y proporcione los argumentos adecuados para su entorno:

    `RegisterRExt.exe /install pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Este comando crea objetos de nivel de instancia en el equipo de SQL Server que son necesarios para la administración de paquetes. También reinicia Launchpad para la instancia.

    Si no especifica una instancia, se usa la instancia predeterminada. Si no especifica un usuario, se usa el contexto de seguridad actual. Por ejemplo, el siguiente comando habilita la administración de paquetes en la instancia predeterminada, con las credenciales del usuario que abrió el símbolo del sistema:

    `REgisterRExt.exe /install pkgmgmt`

3. Para agregar la administración a una base de datos específica, ejecute el comando siguiente desde un símbolo del sistema con privilegios elevados:

    `RegisterRExt.exe /install pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`
   
    Este comando crea algunos artefactos de base de datos, incluidos los siguientes roles de base de datos que se usan para controlar los permisos de usuario: `rpkgs-users`, `rpkgs-private` y `rpkgs-shared`.

    Por ejemplo, el siguiente comando habilita la administración de paquetes en la base de datos, en la instancia predeterminada. Si no especifica un usuario, se usa el contexto de seguridad actual.

    `RegisterRExt.exe /install pkgmgmt /database:TestDB`

4. Repita el comando para cada base de datos en la que se deben instalar los paquetes.

5. Para comprobar que los nuevos roles se han creado correctamente, en SQL Server Management Studio, haga clic en la base de datos, expanda **Seguridad** y expanda **Roles de base de datos**.

    También puede ejecutar una consulta en sys.database_principals como la siguiente:

    ```sql
    SELECT pr.principal_id, pr.name, pr.type_desc,   
        pr.authentication_type_desc, pe.state_desc,   
        pe.permission_name, s.name + '.' + o.name AS ObjectName  
    FROM sys.database_principals AS pr  
    JOIN sys.database_permissions AS pe  
        ON pe.grantee_principal_id = pr.principal_id  
    JOIN sys.objects AS o  
        ON pe.major_id = o.object_id  
    JOIN sys.schemas AS s  
        ON o.schema_id = s.schema_id;
    ```

Después de habilitar esta característica, puede usar la función RevoScaleR para instalar o desinstalar paquetes desde un cliente remoto de R.

## <a name="disable-package-management"></a><a name="bkmk_disable"></a> Deshabilitación de la administración de paquetes

1. Desde un símbolo del sistema con privilegios elevados, ejecute de nuevo la utilidad RegisterRExt y deshabilite la administración de paquetes en el nivel de base de datos:

    `RegisterRExt.exe /uninstall pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`

    Este comando quita los objetos de base de datos relacionados con la administración de paquetes de la base de datos especificada. También quita todos los paquetes que se instalaron desde la ubicación del sistema de archivos protegido en el equipo de SQL Server.

2. Repita este comando en cada base de datos en la que se usara la administración de paquetes.

3.  (Opcional) Después de quitar los paquetes de todas las bases de datos mediante el paso anterior, ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados:

    `RegisterRExt.exe /uninstall pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Este comando quita la característica de administración de paquetes de la instancia. Es posible que tenga que reiniciar manualmente el servicio Launchpad una vez más para ver los cambios.

## <a name="next-steps"></a>Pasos siguientes

+ [Uso de RevoScaleR para instalar paquetes de R](install-r-packages-with-revoscaler.md)
+ [Obtención de información de paquetes de R](r-package-information.md)
+ [Sugerencias para usar paquetes de R](tips-for-using-r-packages.md)

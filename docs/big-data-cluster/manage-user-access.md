---
title: Administración del acceso al clúster de macrodatos en el modo de Active Directory
description: Administración del acceso al clúster de macrodatos
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c40a136f748126bd7baa6892d5cdb5317363ad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046345"
---
# <a name="manage-big-data-cluster-access-in-active-directory-mode"></a>Administración del acceso al clúster de macrodatos en el modo de Active Directory

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe cómo agregar nuevos grupos de Active Directory con roles *bdcUser* además de los proporcionados durante la implementación a través de la opción de configuración *clusterUsers*.

>[!IMPORTANT]
>No use este procedimiento para agregar nuevos grupos de Active Directory con el rol *bdcAdmin*. Los componentes de Hadoop, como HDFS y Spark, solo permiten un grupo de Active Directory como grupo de superusuarios: el equivalente del rol *bdcAdmin* en BDC. Para conceder a grupos de Active Directory adicionales permisos *bdcAdmin* al clúster de macrodatos después de la implementación, debe agregar usuarios y grupos adicionales a los grupos ya designados durante la implementación. Puede seguir el mismo procedimiento para actualizar la pertenencia al grupo que tiene el rol *bdcUsers*.

## <a name="two-overarching-roles-in-the-big-data-cluster"></a>Dos roles generales en el clúster de macrodatos

Los grupos de Active Directory se pueden proporcionar en la sección de seguridad del perfil de implementación como parte de dos roles generales para la autorización dentro del clúster de macrodatos:

* `clusterAdmins`: este parámetro toma un grupo de Active Directory. Los miembros de este grupo tienen el rol *bdcAdmin*, lo que significa que obtienen permisos de administrador para todo el clúster. Tienen permisos de *sysadmin* en SQL Server, permisos de *superuser* en el Sistema de archivos distribuido Hadoop (HDFS) y Spark y derechos de *administrador* en el controlador.

* `clusterUsers`: Estos grupos de Active Directory se asignan a *bdcUsers* en BDC. Son usuarios normales, sin permisos de administrador en el clúster. Tienen permisos para iniciar sesión en la instancia maestra de SQL Server pero, de forma predeterminada, no tienen permisos para los objetos o los datos. Son usuarios normales de HDFS y Spark, sin permisos de *superusuario*. Al conectarse al punto de conexión del controlador, estos usuarios solo pueden consultar los puntos de conexión (mediante la *lista de puntos de conexión de BDC de azdata*).

Para conceder a los grupos de Active Directory adicionales permisos *bdcUser* sin modificar las pertenencias a grupos en Active Directory, complete los procedimientos que aparecen en las secciones siguientes.

## <a name="grant-bdcuser-permissions-to-additional-active-directory-groups"></a>Concesión de permisos *bdcUser* a grupos de Active Directory adicionales

### <a name="create-a-login-for-the-active-directory-user-or-group-in-the-sql-server-master-instance"></a>Creación de un inicio de sesión para el usuario o el grupo de Active Directory en la instancia maestra de SQL Server

1. Conéctese al punto de conexión de SQL maestro mediante su cliente SQL favorito. Use cualquier inicio de sesión de administrador (por ejemplo, `AZDATA_USERNAME`, que se proporcionó durante la implementación). Como alternativa, podría ser cualquier cuenta de Active Directory que pertenezca al grupo de Active Directory que se proporciona como `clusterAdmins` en la configuración de seguridad.

1. Para crear un inicio de sesión para el usuario o grupo de Active Directory, ejecute el comando TSQL siguiente:

   ```sql
   CREATE LOGIN [<domain>\<principal>] FROM WINDOWS;
   ```

   Conceda los permisos deseados en la instancia de SQL Server:

   ```sql
   ALTER SERVER ROLE <server role> ADD MEMBER [<domain>\<principal>];
   GO
   ```

Para obtener una lista completa de los roles de servidor, consulte el tema de seguridad de SQL Server correspondiente [aquí](../relational-databases/security/authentication-access/server-level-roles.md).

### <a name="add-the-active-directory-user-or-group-to-the-roles-table-in-the-controller-database"></a>Incorporación del usuario o grupo de Active Directory a la tabla de roles en la base de datos del controlador

1. Ejecute estos comandos para obtener las credenciales de SQL Server del controlador:

   a. Ejecute este comando como administrador de Kubernetes:

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Descodifique el secreto en Base64:

   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. En una ventana de comandos independiente, exponga el puerto del servidor de base de datos del controlador:

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```

1. Use la conexión anterior para insertar una nueva fila en los *roles* y tablas *active_directory_principals*. Escriba el valor *REALM* en mayúsculas.

   ```sql
   USE controller;
   GO

   INSERT INTO [controller].[auth].[roles] VALUES (N'<user or group name>@<REALM>', 'bdcUser')
   GO

   INSERT INTO [controller].[auth].[active_directory_principals] VALUES (N'<user or group name>@<REALM>', N'<SID>')
   GO
   ```

   Para buscar el SID del usuario o el grupo que se va a agregar, puede usar los comandos de PowerShell [Get-ADUser](/powershell/module/addsadministration/get-aduser/) o  [Get-ADGroup](/powershell/module/addsadministration/get-adgroup/).

2. Compruebe que los miembros del grupo que ha agregado tienen los permisos *bdcUser* esperados; para ello, inicie sesión en el punto de conexión del controlador o en la autenticación de la instancia maestra de SQL Server. Por ejemplo:

   ```bash
   azdata login
   azdata bdc endpoints list
   ```

## <a name="next-steps"></a>Pasos siguientes

- [Conceptos de seguridad de los Clústeres de macrodatos de SQL Server 2019](concept-security.md)

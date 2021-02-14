---
title: Actualización de AZDATA_PASSWORD
description: Actualización manual de `AZDATA_PASSWORD`
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 12/19/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7fe856ae92c198d44ed0b11d1d2ce808e87fb4eb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039705"
---
# <a name="manually-update-azdata_password"></a>Actualización manual de `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Tanto si el clúster está funcionando con la integración de Active Directory como si no es así, `AZDATA_PASSWORD` se establece durante la implementación. Proporciona una autenticación básica al controlador de clúster y la instancia maestra. En este documento se describe cómo actualizar `AZDATA_PASSWORD` manualmente.

## <a name="change-azdata_password-for-controller"></a>Cambio de `AZDATA_PASSWORD` para el controlador

Si el clúster está funcionando en modo no Active Directory, haga lo siguiente para actualizar la contraseña de la puerta de enlace de Apache Knox:

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
 
1. Use la contraseña de administrador del sistema, que acaba de obtener, para conectarse al servidor de base de datos del controlador desde una herramienta de cliente SQL.

1. Genere una contraseña compleja nueva para `AZDATA_USERNAME` con el fin de reemplazar la `AZDATA_PASSWORD` existente.

   Para simplificar el ejemplo, en los pasos siguientes se usa "newPassword" porque la contraseña generada es "newPassword". 

1. Obtiene `hexsalt` de la tabla de usuarios:

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` devuelve una cadena hexadecimal aleatoria (por ejemplo, `64FC59DF31244FFEE02F457BC0750226`).

1. Cifre la contraseña compleja nueva mediante `hexsalt`:

   Para su comodidad, se proporciona una herramienta pregenerada `pbkdf2` para cifrar la contraseña. Descargue la aplicación .NET Core adecuada para la plataforma para [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries).

   La aplicación es independiente y no requiere ningún requisito previo, como los entornos de ejecución de .NET. Para cifrar la contraseña, ejecute:

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. Actualice la contraseña en la tabla de usuarios:

   ```SQL
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Cambio de `AZDATA_PASSWORD` en la instancia maestra de SQL Server

1. Conéctese al punto de conexión de SQL maestro con cualquier usuario administrador.

1. Para cambiar la contraseña de las credenciales de inicio de sesión que definió durante la implementación en el parámetro `AZDATA_USERNAME`, ejecute el comando TSQL siguiente:

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```

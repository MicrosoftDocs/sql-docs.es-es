---
title: Actualización de AZDATA_PASSWORD
description: Actualización manual de `AZDATA_PASSWORD`
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 03/01/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 062e574772c2a44b78772da4a979c81ed3deb959
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836302"
---
# <a name="manually-update-azdata_password"></a>Actualización manual de `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Tanto si [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] está funcionando con la integración de Active Directory como si no es así, `AZDATA_PASSWORD` se establece durante la implementación. Proporciona una autenticación básica al controlador de clúster y la instancia maestra. En este documento se describe cómo actualizar `AZDATA_PASSWORD` manualmente.

## <a name="change-azdata_password-for-controller"></a>Cambio de `AZDATA_PASSWORD` para el controlador

Si el clúster está funcionando en modo no Active Directory, haga lo siguiente para actualizar la contraseña de la puerta de enlace de Apache Knox:

1. Ejecute estos comandos para obtener las credenciales de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] del controlador:

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

   ```sql
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Cambio de `AZDATA_PASSWORD` en la instancia maestra de SQL Server

1. Conéctese al punto de conexión de SQL maestro con cualquier usuario administrador.

1. Para cambiar la contraseña de las credenciales de inicio de sesión que definió durante la implementación en el parámetro `AZDATA_USERNAME`, ejecute el comando TSQL siguiente:

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```

## <a name="manually-updating-password-for-grafana-and-kibana"></a>Actualización manual de la contraseña para Grafana y Kibana

Después de seguir los pasos para actualizar AZDATA_PASSWORD, verá que [Grafana](app-monitor.md) y [Kibana](cluster-logging-kibana.md) siguen aceptando la contraseña anterior. Esto se debe a que Grafana y Kibana no tienen visibilidad para el nuevo secreto de Kubernetes. Debe actualizar manualmente la contraseña de Grafana y Kibana por separado.

## <a name="update-grafana-password"></a>Actualización de la contraseña de Grafana

Siga estas opciones para actualizar manualmente la contraseña de [Grafana](app-monitor.md).

1. La utilidad htpasswd es obligatoria. Puede instalarla en cualquier equipo cliente.
    
    #### <a name="for-ubuntu"></a>[Para Ubuntu](#tab/ubuntu): 
    ```bash
    sudo apt install apache2-utils
    ```
    
    #### <a name="for-rhel"></a>[Para RHEL](#tab/rhel): 
    ```bash
    sudo yum install httpd-tools
    ```
    
    ---

2. Genere la nueva contraseña. 
    
    ```bash
    htpasswd -nbs <username> <password>
    admin:{SHA}<secret>
    ```
    
    Reemplace los valores de /<username/>, /<password/>, /<secret/> según corresponda, por ejemplo:
    
    ```bash
    htpasswd -nbs admin Test@12345
    admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=
    ```

3. Ahora codifique la contraseña:
    
    ```bash
    echo "admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=" | base64
    ```             
    
    Conserve la cadena en base64 de salida para más adelante.
    
4. A continuación, edite el elemento mgmtproxy-secret:
    
    ```bash
    kubectl edit secret -n mssql-cluster mgmtproxy-secret
    ```
         
5. Actualice el elemento controller-login-htpasswd con la nueva cadena en base64 de contraseña codificada generada anteriormente:
    
    ```console
    # Please edit the object below. Lines beginning with a '#' will be ignored,
    # and an empty file will abort the edit. If an error occurs while saving this file will be
    # reopened with the relevant failures.
    #
    apiVersion: v1
    data:
       controller-login-htpasswd: <base64 string from before>
       mssql-internal-controller-password: <password>
       mssql-internal-controller-username: <username>
    ```         

6. Identifique y elimine el pod mgmtproxy. 
     
    Si es necesario, identifique el nombre del pod mgmtproxy.
    
    #### <a name="for-windows"></a>[Para Windows](#tab/windows): 
    En un servidor de Windows puede utilizar lo siguiente:
    
    ```bash 
    kubectl get pods -n <namespace> -l app=mgmtproxy
    ```
    
    #### <a name="for-linux"></a>[Para Linux](#tab/linux): 
    En Linux puede utilizar lo siguiente:
    
    ```bash
    kubectl get pods -n <namespace> | grep 'mgmtproxy'
    ```
    
    ---
    
    Quite el pod mgmtproxy:
    ```bash
    kubectl delete pod mgmtproxy-xxxxx -n mssql-clutser
    ```

7. Espere a que el pod mgmtproxy se conecte y se inicie el panel de Grafana.  
 
    La espera no es significativa y el pod debe estar en línea en cuestión de segundos. Para comprobar el estado del pod, puede usar el mismo comando `get pods` que se utilizó en el paso anterior. 
    Si ve que el pod mgmtproxy no vuelve a tener el estado Listo, use kubectl para describir el pod:
    
    ```bash
    kubectl describe pods mgmtproxy-xxxxx  -n <namespace>
    ```
    
    Para solucionar los problemas y recopilar más registros, use el comando `[azdata bdc debug copy-logs](../azdata/reference/reference-azdata-bdc-debug.md)` de la CLI de datos de Azure.
    
8. Ahora inicie sesión en Grafana con la nueva contraseña. 


## <a name="update-the-kibana-password"></a>Actualización de la contraseña de Kibana

Siga estas opciones para actualizar manualmente la contraseña de [Kibana](cluster-logging-kibana.md).

> [!NOTE]
> El explorador Microsoft Edge más antiguo no es compatible con Kibana; deberá usar el explorador basado en Edge Chromium para que el panel se muestre correctamente. Si se usa un explorador no compatible, aparecerá una página en blanco al cargar los paneles. Consulte los [exploradores admitidos para Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

1. Abra la dirección URL de Kibana.
    
    Puede encontrar la dirección URL del punto de conexión de servicio de Kibana en [Azure Data Studio](manage-with-controller-dashboard#controller-dashboard), o bien utilice el siguiente comando **azdata**:
    
    ```azurecli
    azdata login
    azdata bdc endpoint list -e logsui -o table
    ```
    
    Por ejemplo: https://11.111.111.111:30777/kibana/app/kibana#/discover

2. En el panel izquierdo, haga clic en la opción **Seguridad**.
    
    ![Captura de pantalla del menú en el panel izquierdo de Kibana, con la opción Security (Seguridad) elegida](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-1.jpg)

3. En la página de seguridad, en el encabezado Authentication Backends (Back-end de autenticación), haga clic en **Internal User Database** (Base de datos de usuarios internos).

    ![Una captura de pantalla de la página de seguridad, con el cuadro Internal User Database (Base de datos de usuarios internos) elegido.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-2.jpg)

4. Ahora verá la lista de usuarios debajo del encabezado Internal User Database (Base de datos de usuarios internos). Use esta página para agregar, modificar y quitar usuarios para el acceso del punto de conexión de Kibana. Para el usuario que necesita la contraseña actualizada, haga clic en el botón **Edit** (Editar) situado a la derecha del usuario.

    ![Captura de pantalla de la página Internal User Database (Base de datos de usuarios internos) En la lista de usuarios, para el usuario KubeAdmin, se elige el botón Edit (Editar).](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-3.jpg)

5. Escriba la nueva contraseña dos veces y haga clic en **Submit** (Enviar):

    ![Captura de pantalla del formulario de edición de usuarios internos. Se ha especificado una nueva contraseña en los campos Password (Contraseña) y Repeat password (Repetir contraseña).](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-4.jpg)

6. Cierre el explorador y vuelva a conectarse a la dirección URL de Kibana con la contraseña actualizada.

> [!Note]
> Después de iniciar sesión con la nueva contraseña, si ve páginas en blanco en Kibana, cierre la sesión manualmente con la opción Logout (Cerrar sesión) en la esquina superior derecha e inicie sesión de nuevo.

## <a name="see-also"></a>Consulte también

* [azdata bdc (CLI de datos de Azure)](../../sql/azdata/reference/reference-azdata-bdc.md) 
* [Supervisión de aplicaciones con el panel de azdata y Grafana](app-monitor.md)  
* [Consulta de los registros de clúster con el panel de Kibana](cluster-logging-kibana.md)  
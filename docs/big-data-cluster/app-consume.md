---
title: Consumo de aplicaciones
titleSuffix: SQL Server Big Data Clusters
description: Consuma una aplicación implementada en clústeres de macrodatos de SQL Server con un servicio web RESTful.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.metadata: seo-lt-2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 78d353130bfa4240c1d3eacb09c8e50d08a4c350
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039815"
---
# <a name="consume-an-app-deployed-on-big-data-clusters-2019-using-a-restful-web-service"></a>Consumo de una aplicación implementada en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] con un servicio web RESTful

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se explica cómo consumir una aplicación implementada en un clúster de macrodatos de SQL Server con un servicio web RESTful.

## <a name="prerequisites"></a>Prerequisites

- [Clúster de macrodatos de SQL Server](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- Una aplicación implementada mediante [azdata](app-create.md) o la [extensión de implementación de aplicaciones](app-deployment-extension.md)

> [!NOTE]
> Cuando el archivo de especificación YAML de la aplicación especifica una programación, la aplicación se desencadenará a través de un trabajo Cron. Si el clúster de macrodatos se implementa en OpenShift, el inicio del trabajo Cron necesita funcionalidades adicionales. Vea los detalles relativos a las [consideraciones de seguridad en OpenShift](concept-application-deployment.md#app-deploy-security) para obtener instrucciones específicas.

## <a name="capabilities"></a>Capacidades

Después de haber implementado una aplicación en el [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], puede acceder a esa aplicación y consumirla mediante un servicio web RESTful. Esto permite la integración de esa aplicación desde otras aplicaciones o servicios (por ejemplo, una aplicación móvil o un sitio web). En la tabla siguiente se describen los comandos de implementación de aplicaciones que se pueden usar con **azdata** para obtener información sobre el servicio web RESTful de la aplicación.

|Comando |Descripción |
|:---|:---|
|`azdata app describe` | Describe una aplicación. |

Puede obtener ayuda con el parámetro `--help`, como en el ejemplo siguiente:

```bash
azdata app describe --help
```

En las secciones siguientes se explica cómo recuperar un punto de conexión de una aplicación y cómo trabajar con el servicio web RESTful para la integración de aplicaciones.

## <a name="retrieve-the-endpoint"></a>Recuperación del punto de conexión

Los clústeres de macrodatos (BDC) proporcionan puntos de conexión a los que puede acceder y usar la aplicación con un servicio web RESTful; el objetivo principal es ofrecer la interacción con otras aplicaciones web o móviles y ser más proactiva para esa arquitectura de microservicios. El comando **azdata app describe** proporciona información detallada sobre la aplicación, incluido el punto final del clúster. Lo suelen usar los desarrolladores de aplicaciones para compilar una aplicación con el cliente de Swagger y con el servicio web para interactuar con la aplicación de manera RESTful.

Describa la aplicación al ejecutar un comando similar al siguiente ejemplo:

```bash
azdata app describe --name add-app --version v1
```

```json
{
  "input_param_defs": [
    {
      "name": "x",
      "type": "int"
    },
    {
      "name": "y",
      "type": "int"
    }
  ],
  "links": {
    "app": "https://10.1.1.3:30080/app/addpy/v1",
    "swagger": "https://10.1.1.3:30080/app/addpy/v1/swagger.json"
  },
  "name": "add-app",
  "output_param_defs": [
    {
      "name": "result",
      "type": "int"
    }
  ],
  "state": "Ready",
  "version": "v1"
}
```

Anote la dirección IP (`10.1.1.3` en este ejemplo) y el número de puerto (`30080`) de la salida.

Otra de las diversas formas de obtener esta información es hacer clic con el botón derecho en Administrar en el servidor de Azure Data Studio, donde encontrará los puntos de conexión de los servicios enumerados.

![Punto de conexión de ADS](media/big-data-cluster-consume-apps/ads_end_point.png)

## <a name="generate-a-jwt-access-token"></a>Generar un token de acceso JWT

Para acceder al servicio web RESTful de la aplicación que ha implementado, primero debe generar un token de acceso JWT. La dirección URL del token de acceso depende de la versión del clúster de macrodatos. 

|Versión |URL|
|------------|------|
|GDR1|  `https://[IP]:[PORT]/docs/swagger.json`|
|CU1 y versiones posteriores| `https://[IP]:[PORT]/api/v1/swagger.json`|

 En la salida del ejemplo anterior, la versión CU4 y la dirección IP del controlador (10.1.1.3 en el ejemplo) y el número de puerto (30080), la dirección URL sería similar a la siguiente: 
 
 ```bash
    https://10.1.1.3 :30080/api/v1/swagger.json
```
 
> Para información sobre la versión, consulte el [historial de versiones](release-notes-big-data-cluster.md#release-history).

Abra la dirección URL adecuada en el explorador con la dirección IP y el puerto que ha recuperado al ejecutar el comando [`describe`](#retrieve-the-endpoint) arriba. Inicie sesión con las mismas credenciales que ha usado para `azdata login`.

Pegue el contenido de `swagger.json` en el [editor de Swagger](https://editor.swagger.io) para conocer qué métodos están disponibles:

![API de Swagger](media/big-data-cluster-consume-apps/api_swagger.png)

Observe que `app` es el método GET y para obtener `token` se usaría el método POST. Dado que la autenticación de aplicaciones usa tokens JWT, necesita obtener un token mediante su herramienta favorita para realizar una llamada POST al método `token`. Con el mismo ejemplo, la dirección URL para obtener el token de JWT tendría este aspecto:

 ```bash
    https://10.1.1.3 :30080/api/v1/token
```


Este es un ejemplo de cómo hacer justamente eso en [Postman](https://www.getpostman.com/):

![Token de Postman](media/big-data-cluster-consume-apps/postman_token.png)


La salida de esta solicitud proporcionará un objeto `access_token` de JWT, que necesitará para llamar a la dirección URL a fin de ejecutar la aplicación.

## <a name="execute-the-app-using-the-restful-web-service"></a>Ejecutar la aplicación mediante el servicio web RESTful

Hay varias maneras de consumir una aplicación en BDC; puede elegir usar el [comando azdata app run](app-create.md). En esta sección se muestra cómo usar herramientas de desarrollo comunes como Postman para ejecutar la aplicación. 

Puede abrir la dirección URL de `swagger` que se ha devuelto al ejecutar `azdata app describe --name [appname] --version [version]` en el explorador, que debe ser similar a `https://[IP]:[PORT]/app/[appname]/[version]/swagger.json`. 

> [!NOTE]
> Tiene que iniciar sesión con las mismas credenciales que ha usado para `azdata login`. Con el mismo ejemplo, el comando tendría este aspecto:

 ```bash
    azdata app describe --name add-app --version v1
```

Puede pegar el contenido de `swagger.json` en el [editor de Swagger](https://editor.swagger.io). Verá que el servicio web expone el método `run` y, por debajo, ha pasado a través del proxy de aplicación, que es una API web que autentica a los usuarios y, después, enruta las solicitudes a través de las aplicaciones. Observe la dirección URL base que se muestra en la parte superior. Puede usar la herramienta que prefiera para llamar al método `run` (`https://[IP]:30778/api/app/[appname]/[version]/run`), y pasar los parámetros en el cuerpo de la solicitud POST como código JSON. 


En este ejemplo usaremos [Postman](https://www.getpostman.com/). Antes de realizar la llamada, deberá establecer `Authorization` en `Bearer Token` y pegar el token recuperado anteriormente. Esto establece un encabezado en la solicitud. Vea la captura de pantalla siguiente.

![Encabezados de ejecución de Postman](media/big-data-cluster-consume-apps/postman_run_1.png)

Después, en el cuerpo de las solicitudes, pase los parámetros a la aplicación a la que llama y establezca `content-type` en `application/json`:

![Cuerpo de ejecución de Postman](media/big-data-cluster-consume-apps/postman_run_2.png)

Cuando envía la solicitud, obtendrá el mismo resultado que cuando ejecuta la aplicación mediante `azdata app run`:

![Resultado de ejecución de Postman](media/big-data-cluster-consume-apps/postman_result.png)

Ahora ha llamado correctamente a la aplicación a través del servicio web. Puede seguir pasos similares para integrar este servicio web en la aplicación.


## <a name="next-steps"></a>Pasos siguientes

Explore cómo [supervisar las aplicaciones en clústeres de macrodatos](app-monitor.md) para más información. También puede ver otros ejemplos en [Ejemplos de implementación de aplicaciones](https://aka.ms/sql-app-deploy).

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)?
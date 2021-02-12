---
title: Implementación en el modo de Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Obtenga información sobre cómo actualizar los clústeres de macrodatos de SQL Server en un dominio Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2a79c82f2c3fd443d7237fc3b0a1f7c51102bceb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048020"
---
# <a name="deploy-sql-server-big-data-cluster-in-active-directory-mode"></a>Implementación del clúster de macrodatos de SQL Server en el modo de Active Directory

En este artículo se describe cómo implementar el clúster de macrodatos de SQL Server en el modo de Active Directory. Para hacer los pasos descritos en este artículo se necesita acceso a un dominio de Active Directory. Antes de continuar, debe completar los requisitos descritos en [Implementar[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el modo de Active Directory](active-directory-prerequisites.md).

## <a name="prepare-deployment"></a>Preparación de la implementación

Para la implementación del BDC con la integración de AD, hay cierta información adicional que se debe proporcionar para la creación de los objetos relacionados con el BDC en AD.

Con el uso del perfil `kubeadm-prod`, (o `openshift-prod` a partir de la versión CU5), tendrá automáticamente los marcadores de posición para la información relacionada con la seguridad y con el punto de conexión que se necesitan para la integración de AD.

Además, debe proporcionar las credenciales que [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] usará para crear los objetos necesarios en AD. Estas credenciales se proporcionan como variables de entorno.

## <a name="set-security-environment-variables"></a>Establecimiento de variables de entorno de seguridad

Las siguientes variables de entorno proporcionan las credenciales para la cuenta de servicio de dominio del BDC, que se usarán para configurar la integración de AD. El BDC también usa esta cuenta para mantener en el futuro los objetos de AD relacionados con el BDC.

```bash
export DOMAIN_SERVICE_ACCOUNT_USERNAME=<AD principal account name>
export DOMAIN_SERVICE_ACCOUNT_PASSWORD=<AD principal password>
```

## <a name="provide-security-and-endpoint-parameters"></a>Suministro de seguridad y parámetros de punto de conexión

Además de las variables de entorno para las credenciales, también debe proporcionar información de seguridad y de punto de conexión para que la integración de AD funcione. Los parámetros necesarios forman parte automáticamente del [perfil de implementación](deployment-guidance.md#configfile) de `kubeadm-prod`/`openshift-prod`.

La integración de AD necesita los parámetros siguientes. Agregue estos parámetros a los archivos `control.json` y `bdc.json` con los comandos `config replace` que se muestran más adelante en este artículo. Todos los ejemplos siguientes utilizan `contoso.local` de dominio de ejemplo.

- `security.activeDirectory.ouDistinguishedName`: nombre distintivo de una unidad organizativa (UO) en la que se agregarán todas las cuentas de AD que cree la implementación del clúster. Si se llama al dominio `contoso.local`, el nombre distintivo de la UO será `OU=BDC,DC=contoso,DC=local`.

- `security.activeDirectory.dnsIpAddresses`: contiene la lista de direcciones IP de los servidores DNS del dominio. 

- `security.activeDirectory.domainControllerFullyQualifiedDns`: lista de FQDN del controlador de dominio. El FQDN contiene el nombre de host o de la máquina del controlador de dominio. Si tiene varios controladores de dominio, aquí se puede proporcionar una lista. Ejemplo: `HOSTNAME.CONTOSO.LOCAL`.

  > [!IMPORTANT]
  > Cuando hay varios controladores de dominio que atienden a un dominio, utilice el controlador de dominio principal (PDC) como primera entrada en la lista de `domainControllerFullyQualifiedDns` de la configuración de seguridad. Para obtener el nombre del PDC, escriba `netdom query fsmo` en el símbolo del sistema y luego presione **Entrar**.

- **Parámetro opcional** `security.activeDirectory.realm`: en la mayoría de casos, el dominio es igual al nombre de dominio. En los casos en los que no sean iguales, use este parámetro para definir el nombre del dominio (por ejemplo, `CONTOSO.LOCAL`). El valor proporcionado para este parámetro debe ser completo.

  > [!IMPORTANT]
  > En este momento, el clúster de macrodatos no es compatible con una configuración en la que el nombre de dominio de Active Directory sea diferente del nombre de **NETBIOS** del dominio de Active Directory.

- `security.activeDirectory.domainDnsName`: Nombre del dominio DNS que se usará para el clúster (por ejemplo, `contoso.local`).

- `security.activeDirectory.clusterAdmins`: este parámetro toma un grupo de AD. El ámbito del grupo de AD debe ser universal o global. Los miembros de este grupo tendrán el rol de clúster `bdcAdmin`, que les concederá permisos de administrador en el clúster. Esto significa que tendrán [permisos de `sysadmin` en SQL Server](../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles), [permisos de `superuser` en HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#The_Super-User) y permisos de administrador cuando se conecten al punto de conexión del controlador.

  >[!IMPORTANT]
  >Cree este grupo en AD antes de comenzar la implementación. Si el ámbito de este grupo de AD es el dominio local, se produce un error en la implementación.

- `security.activeDirectory.clusterUsers`: lista de los grupos de AD que son usuarios normales (sin permisos de administrador) en el clúster de macrodatos. La lista puede incluir grupos de AD cuyo ámbito sean grupos universales o globales. No pueden ser grupos de dominio local.

Los grupos de AD de esta lista se asignan al rol de clúster de macrodatos `bdcUser` y se les debe conceder acceso a SQL Server (vea [Permisos de SQL Server](../relational-databases/security/permissions-hierarchy-database-engine.md)) o a HDFS (vea [Guía de permisos de HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#:~:text=Permission%20Checks%20%20%20%20Operation%20%20,%20%20N%2FA%20%2029%20more%20rows%20)). Al conectarse al punto de conexión del controlador, estos usuarios solo pueden mostrar los puntos de conexión disponibles en el clúster mediante el comando `azdata bdc endpoint list`.

A fin de obtener información detallada sobre cómo actualizar los grupos de AD para esta configuración, vea [Administración del acceso al Clúster de macrodatos en el modo de Active Directory](manage-user-access.md).

  >[!TIP]
  >Para habilitar la experiencia de exploración de HDFS cuando se conecta a SQL Server maestro en Azure Data Studio, se debe conceder a un usuario con el rol bdcUser permisos VIEW SERVER STATE, ya que Azure Data Studio usa la DMV `sys.dm_cluster_endpoints` para obtener el punto de conexión de la puerta de enlace de Knox requerido para conectarse a HDFS.

  >[!IMPORTANT]
  >Cree estos grupos en AD antes de comenzar la implementación. Si el ámbito de cualquiera de estos grupos de AD es el dominio local, se produce un error en la implementación.

  >[!IMPORTANT]
  >Si los usuarios del dominio cuentan con muchas pertenencias a grupos, debe ajustar los valores de la configuración de puerta de enlace `httpserver.requestHeaderBuffer` (el valor predeterminado es `8192`) y la configuración de HDFS `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` (el valor predeterminado es `10`), mediante el archivo de configuración personalizado de implementación *bdc.json*. Se trata de un procedimiento recomendado para evitar los tiempos de espera de conexión a las respuestas de puerta de enlace o HTTP con un código de estado 431 (*Campos del encabezado de solicitud demasiado grandes*). Esta es una sección del archivo de configuración que muestra cómo definir los valores de esta configuración y cuáles son los valores recomendados para un número mayor de pertenencias de grupo:

```json
{
    ...
    "spec": {
        "resources": {
            ...
            "gateway": {
                "spec": {
                    "replicas": 1,
                    "endpoints": [{...}],
                    "settings": {
                        "gateway-site.gateway.httpserver.requestHeaderBuffer": "65536"
                    }
                }
            },
            ...
        },
        "services": {
            ...
            "hdfs": {
                "resources": [...],
                "settings": {
                  "core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels": "4"
                }
            },
            ...
        }
    }
}
```

  >[!IMPORTANT]
  >Cree los grupos proporcionados para la configuración siguiente en AD antes de comenzar la implementación. Si el ámbito de cualquiera de estos grupos de AD es el dominio local, se produce un error en la implementación.

- **Parámetro opcional** `security.activeDirectory.appOwners`: lista de grupos de AD que tienen permisos para crear, eliminar y ejecutar cualquier aplicación. La lista puede incluir grupos de AD cuyo ámbito sean grupos universales o globales. No pueden ser grupos de dominio local.

- **Parámetro opcional** `security.activeDirectory.appReaders`: lista de grupos de AD que tienen permisos para ejecutar cualquier aplicación. La lista puede incluir grupos de AD cuyo ámbito sean grupos universales o globales. No pueden ser grupos de dominio local.

En la tabla siguiente se muestra el modelo de autorización para la administración de aplicaciones:

|   Roles autorizados   |   El comando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]   |
|----------------------|--------------------|
|   appOwner           | azdata app create  |
|   appOwner           | azdata app update  |
|   appOwner, appReader| azdata app list    |
|   appOwner, appReader| azdata app describe|
|   appOwner           | azdata app delete  |
|   appOwner, appReader| azdata app run     |

- `security.activeDirectory.subdomain`: **Parámetro opcional** Este parámetro se incluye en la versión SQL Server 2019 CU5 para admitir la implementación de varios clústeres de macrodatos en el mismo dominio. Con esta opción, puede especificar nombres DNS diferentes para cada uno de los clústeres de macrodatos implementados. Si el valor de este parámetro no se especifica en la sección Active Directory del archivo `control.json`, de forma predeterminada se usará el nombre del clúster de macrodatos (igual que el nombre del espacio de nombres de Kubernetes) para calcular el valor de la configuración de subdominio. 

  >[!NOTE]
  >El valor que se pasa a través de la configuración de subdominio no es un nuevo dominio de AD, sino solo un dominio DNS que el clúster de BDC usa internamente.

  >[!IMPORTANT]
  >Debe instalar o actualizar la versión más reciente de **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** a partir de la versión SQL Server 2019 CU5 para aprovechar estas capacidades nuevas e implementar varios clústeres de macrodatos en el mismo dominio.

  Vea [Concepto: Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en modo de Active Directory](active-directory-deployment-background.md) para obtener más información sobre la implementación de varios clústeres de macrodatos en el mismo dominio de Active Directory.

- `security.activeDirectory.accountPrefix`: **Parámetro opcional** Este parámetro se incluye en la versión SQL Server 2019 CU5 para admitir la implementación de varios clústeres de macrodatos en el mismo dominio. Esta configuración garantiza la exclusividad de los nombres de cuenta de varios servicios de clúster de macrodatos, que deben ser diferentes entre cualquier par de clústeres. La personalización del nombre del prefijo de la cuenta es opcional; de forma predeterminada, el nombre del subdominio se usa como prefijo de la cuenta. Si el nombre de subdominio es mayor de 12 caracteres, los primeros 12 caracteres del nombre de subdominio se usan como el prefijo de la cuenta.  

  >[!NOTE]
  >Active Directory requiere que los nombres de cuenta se limiten a 20 caracteres. El clúster de macrodatos debe usar 8 de estos caracteres para distinguir entre pods y StatefulSets. Esto nos deja 12 caracteres como límite para el prefijo de la cuenta.

[Compruebe el ámbito del grupo de AD](/powershell/module/addsadministration/get-adgroup), para determinar si es DomainLocal.

Si aún no ha inicializado el archivo de configuración de la implementación, puede ejecutar este comando para obtener una copia de la configuración. En los ejemplos siguientes se usa el perfil de `kubeadm-prod` y lo mismo se aplica a `openshift-prod`.

```bash
azdata bdc config init --source kubeadm-prod  --target custom-prod-kubeadm
```

Para establecer los parámetros anteriores en el archivo `control.json`, use los siguientes comandos de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Los comandos reemplazan la configuración y proporcionan sus propios valores antes de que se realice la implementación.

> [!IMPORTANT]
> En la versión SQL Server 2019 CU2, la estructura de la sección de configuración de seguridad del perfil de implementación ha cambiado ligeramente y todos los valores relacionados con Active Directory están en el nuevo directorio `activeDirectory` en el árbol de JSON, bajo `security`, en el archivo `control.json`.

>[!NOTE]
> Además de proporcionar valores diferentes para el subdominio, tal y como se describe en esta sección, también debe usar números de puerto diferentes para los puntos de conexión de BDC al implementar varios BDC en el mismo clúster de Kubernetes. Estos números de puerto son configurables en el momento de la implementación a través de los perfiles de [configuración de implementación](deployment-custom-configuration.md).

El ejemplo siguiente se basa en el uso de SQL Server 2019 CU2. Se muestra cómo reemplazar los valores de parámetros relacionados con AD en la configuración de implementación. Los detalles del dominio siguientes son valores de ejemplo.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Opcionalmente, solo a partir de la versión SQL Server 2019 CU5, puede invalidar los valores predeterminados para la configuración de `subdomain` y `accountPrefix`.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.subdomain=[\"bdctest\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.accountPrefix=[\"bdctest\"]"
```

Del mismo modo, en las versiones anteriores a SQL Server 2019 CU2, puede ejecutar lo siguiente:

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Además de la información anterior, también debe proporcionar nombres de DNS para los distintos puntos de conexión del clúster. Las entradas DNS que usan los nombres de DNS proporcionados se crearán automáticamente en el servidor DNS tras la implementación. Usará estos nombres al conectarse a los diferentes puntos de conexión del clúster. Por ejemplo, si el nombre DNS de la instancia maestra de SQL es `mastersql` y se tiene en cuenta que el subdominio usará el valor predeterminado del nombre del clúster en `control.json`, usará `mastersql.contoso.local,31433` o `mastersql.mssql-cluster.contoso.local,31433` (en función de los valores que se han proporcionado en los archivos de configuración de implementación para los nombres DNS del punto de conexión) a fin de conectarse a la instancia maestra desde las herramientas. 

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[1].dnsName=<monitoring services DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=<SQL Master Primary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=<SQL Master Secondary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=<Gateway (Knox) DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=<app proxy DNS name>.<Domain name. e.g. contoso.local>"
```

> [!IMPORTANT]
> Puede usar los nombres DNS del punto de conexión que prefiera, siempre y cuando estén completos y no entren en conflicto entre dos clústeres de macrodatos implementados en el mismo dominio. Opcionalmente, puede usar el valor del parámetro `subdomain` para garantizar que los nombres DNS son diferentes en los clústeres. Por ejemplo:

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.<subdomain e.g. mssql-cluster>.contoso.local"
```

Puede encontrar un script de ejemplo aquí para la [implementación de un clúster de macrodatos de SQL Server en un clúster de Kubernetes de un solo nodo (kubeadm) con integración de AD](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm-ad).

> [!Note]
> Puede haber escenarios en los que no pueda adaptar el parámetro `subdomain` recientemente introducido. Por ejemplo, debe implementar una versión anterior a CU5 y ya se ha actualizado **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** . Esto es muy improbable, pero si necesita revertir al comportamiento anterior a CU5, puede establecer el parámetro `useSubdomain` en `false` en la sección Active Directory de `control.json`.  Este es el comando para hacerlo:

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false"
```

Ahora debe haber establecido todos los parámetros necesarios para una implementación del BDC con la integración de Active Directory.

Ahora puede implementar el clúster de BDC integrado con Active Directory mediante el comando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] y el perfil de implementación kubeadm-prod. Para obtener la documentación completa sobre cómo implementar [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)], visite [Procedimiento para implementar clústeres de macrodatos de SQL Server en Kubernetes](deployment-guidance.md).

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Comprobación de la entrada de DNS inverso para el controlador de dominio

Asegúrese de que haya una entrada de DNS inverso (registro PTR) para el propio controlador de dominio registrada en el servidor DNS. Puede comprobarlo ejecutando el elemento `nslookup` del nombre de dominio en el controlador de dominio para ver que se pueda resolver en la dirección IP del controlador de dominio.

## <a name="known-issues-and-limitations"></a>Limitaciones y problemas conocidos

**Limitaciones que deben tenerse en cuenta en SQL Server 2019 CU5**

- Actualmente, los paneles Búsqueda de registros y Métricas no admiten la autenticación de AD. El nombre de usuario y la contraseña básicos establecidos tras la implementación se pueden usar para la autenticación en estos paneles. El resto de puntos de conexión del clúster admiten la autenticación de AD.

- El modo de AD seguro solo funcionará en entornos de implementación de `kubeadm` y `openshift`, pero no en AKS o en ARO por el momento. Los perfiles de implementación de `kubeadm-prod` y `openshift-prod` incluyen las secciones de seguridad de forma predeterminada.

- Antes de la versión SQL Server 2019 CU5 solo se permite un BDC por dominio (Active Directory). La habilitación de varios BDC por dominio está disponible a partir de la versión CU5.

- Ninguno de los grupos de AD especificados en las configuraciones de seguridad puede tener definido el ámbito DomainLocal. Puede comprobar el ámbito de un grupo de AD siguiendo [estas instrucciones](/powershell/module/addsadministration/get-adgroup).

- Se permite el uso de la cuenta de AD que se puede usar para iniciar sesión en BDC en el mismo dominio en el que se ha configurado para BDC. No se admite la habilitación de inicios de sesión desde otro dominio de confianza.

## <a name="next-steps"></a>Pasos siguientes

[Conexión[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: modo de Active Directory](active-directory-connect.md)

[Solución de problemas de integración de Active Directory de un Clúster de macrodatos de SQL Server](troubleshoot-active-directory.md)

[Concepto: Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en modo de Active Directory](active-directory-deployment-background.md)

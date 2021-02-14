---
title: Implementación múltiple en el dominio de Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Aprenda a implementar varios clústeres de macrodatos de SQL Server en un único dominio de Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8b82b4a5802467841ca345ac24c83e1356a79f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040135"
---
# <a name="deploy-multiple-big-data-clusters-2019-in-the-same-active-directory-domain"></a>Implementación de varios [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el mismo dominio de Active Directory

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se explican las actualizaciones de SQL Server 2019 CU 5 que permiten la implementación e integración de varios Clústeres de macrodatos de SQL Server 2019 con el mismo dominio de Active Directory.

Antes de SQL 2019 CU5 había dos problemas que impedían la implementación de varios clústeres de macrodatos en un dominio de AD.

- Conflicto de nomenclatura para los nombres de entidades de seguridad de servicio y el dominio DNS
- Nombre principal de la cuenta de dominio

## <a name="object-name-collisions"></a>Colisiones de nombres de objeto

### <a name="service-principal-names-spn-and-dns-domain-naming-conflict"></a>Conflicto de nombres de entidades de seguridad de servicio(SPN) y nombres de dominio DNS

El nombre de dominio proporcionado en el momento de la implementación se usa como dominio DNS de AD. Esto significa que los pods pueden conectarse entre sí en la red interna mediante este dominio DNS. Además, los usuarios se conectan a los puntos de conexión del clúster de macrodatos mediante este dominio DNS. Como resultado, cualquier nombre de entidad de seguridad de servicio (SPN) creado para un servicio dentro del clúster de macrodatos va a tener el nombre del punto de conexión, servicio o pod de Kubernetes calificado con este dominio DNS de AD. Si un usuario implementa un segundo clúster en el dominio, los SPN generados tendrán el mismo FQDN, ya que los nombres del pod y el nombre de dominio DNS no difieren entre los dos clústeres. Como ejemplo, considere un caso en el que el dominio DNS de AD es `contoso.local`. Uno de los SPN generados para SQL Server del grupo maestro en el pod `master-0` sería `MSSQLSvc/master-0.contoso.local:1433`. En el segundo clúster que intentaría implementar el usuario, el nombre del pod para `master-0` es el mismo, y el usuario proporcionaría el mismo dominio DNS de AD (``contoso.local``), lo que daría lugar a la misma cadena de SPN. Active Directory prohibiría la creación de un SPN en conflicto que condujese a un error de implementación para el segundo clúster.

### <a name="domain-account-principal-names"></a>Nombre principal de la cuenta de dominio

Durante una implementación de un clúster de macrodatos con un dominio de Active Directory se generan varias entidades de seguridad de cuenta para los servicios que se ejecutan dentro del clúster de macrodatos. Son esencialmente cuentas de usuario de AD. Antes de SQL 2019 CU5, los nombres de dichas cuentas no serían únicos entre los clústeres. Este manifiesto intenta crear el mismo nombre de cuenta de usuario para un servicio determinado en un clúster de macrodatos en dos clústeres diferentes. El clúster que se está implementando en segundo lugar entrará en conflicto en AD y no podrá crear su cuenta.

## <a name="resolution-for-collisions"></a>Resolución de colisiones

### <a name="solution-to-solve-the-problem-with-spns-and-dns-domain---sql-2019-cu5"></a>Solución para resolver el problema con los SPN y el dominio DNS: SQL 2019 CU5

Dado que los SPN deben ser diferentes en dos clústeres, el nombre de dominio DNS que se pasa en el momento de la implementación debe ser diferente. Puede especificar diferentes nombres DNS mediante la configuración que acaba de introducir en el archivo de configuración de la implementación: `subdomain`. Si el subdominio es diferente en dos clústeres y se puede producir una comunicación interna a través de este subdominio, los SPN incluirán el subdominio que consigue la exclusividad necesaria.

>[!NOTE]
>El valor que se pasa a través de la configuración de subdominio no es un nuevo dominio de AD, sino un dominio DNS que se usa internamente.

Como ejemplo, vuelva a considerar el caso de un SPN de SQL Server de un grupo maestro. Si el subdominio es `bdc`, el SPN descrito anteriormente cambiará a `MSSQLSvc/master-0.bdc.contoso.local:1433`.  

La personalización del valor del parámetro del subdominio recién introducido en las especificaciones de configuración de Active Directory es opcional. De forma predeterminada, se usará el nombre del clúster de macrodatos o el nombre del espacio de nombres para calcular el valor de la configuración del subdominio. Cuando los usuarios quieran reemplazar el nombre del subdominio, podrán hacerlo mediante el nuevo parámetro de subdominio que se introduce en las especificaciones de configuración de Active Directory.

### <a name="solution-to-solve-the-problem-regarding-account-names-uniqueness"></a>Solución para resolver el problema relacionado con la exclusividad de los nombres de cuenta

El concepto de prefijo de cuenta se presentó con el fin de actualizar los nombres de cuenta a un esquema que garantice la exclusividad. El prefijo de cuenta es una parte del nombre de cuenta que es única entre dos clústeres cualesquiera. La parte restante del nombre de cuenta es constante para un servicio determinado. El nuevo formato del nombre de cuenta tendrá el siguiente aspecto: `<prefix>-<name>-<podId>`. 

>[!NOTE]
>Active Directory requiere que los nombres de cuenta se limiten a 20 caracteres. El clúster de macrodatos debe usar 8 de estos caracteres para distinguir entre pods y StatefulSets. Esto nos deja 12 caracteres como límite para el prefijo de la cuenta.

La personalización del nombre de la cuenta es opcional. Use el parámetro `accountPrefix` en las especificaciones de configuración de Active Directory. SQL Server 2019 CU5 incorpora `accountPrefix` en las especificaciones de configuración. De forma predeterminada, el nombre del subdominio se usa como prefijo de la cuenta. Si el nombre de subdominio es mayor de 12 caracteres, la subcadena inicial de 12 caracteres del nombre de subdominio se usan como prefijo de la cuenta.

El subdominio solo se aplica a DNS. Por lo tanto, el nombre de la nueva cuenta de usuario de LDAP es `bdc-ldap@contoso.local`. El nombre de cuenta no debería ser `bdc-ldap@bdc.contoso.local`.

## <a name="semantics"></a>Semántica

En resumen, la semántica de los parámetros agregados en SQL 2019 CU5 para varios clústeres en un dominio es la siguiente:

### `subdomain`

- Campo opcional
- Tipo de datos: cadena
- Definición: un subdominio DNS único que se usará para este clúster de macrodatos. Este valor debe ser diferente para cada clúster que se implemente en el dominio de Active Directory.  
- Valor predeterminado: cuando no se proporciona, se usa el nombre del clúster como valor predeterminado.
- Longitud máxima: 63 caracteres por etiqueta (siendo la etiqueta cada cadena separada por un punto).
- Comentarios: los nombres DNS del punto de conexión deben usar el subdominio de su FQDN.

### `accountPrefix`

- Campo opcional
- Tipo de datos: cadena
- Definición: se generará un prefijo único para el clúster de macrodatos de cuentas de AD. Este valor debe ser diferente para cada clúster que se implemente en el dominio de Active Directory.
- Valor predeterminado: cuando no se proporciona, se usa el nombre del subdominio como valor predeterminado. Cuando no se proporciona el subdominio, se usa el nombre del clúster como el nombre del subdominio y, por tanto, el nombre del clúster también se heredará como accountPrefix. Si se proporciona el subdominio y es un nombre de varias partes (contiene uno o más puntos), el usuario debe proporcionar un accountPrefix. 
- Longitud máxima: 12 caracteres 

## <a name="impact-on-ad-domain-and-dns-server"></a>Impacto en el dominio de AD y el servidor DNS 

No se requiere ningún cambio en el dominio de AD ni en el controlador de dominio para dar cabida a la implementación de varios clústeres de macrodatos en el mismo dominio de Active Directory. El subdominio DNS se creará automáticamente en el servidor DNS al registrar nombres DNS de punto de conexión externos. 

## <a name="impact-on-setting-up-the-deployment-configuration-file-used-for-the-bdc-deployment"></a>Impacto en la configuración del archivo de configuración de implementación que se usa para la implementación del clúster de macrodatos 

La sección *activeDirectory* de la configuración del plano de control *control.json* tendrá dos nuevos parámetros opcionales: `subdomain` y `accountPrefix`. Proporcione únicamente valores para esta configuración si quiere invalidar el comportamiento predeterminado, que consiste en usar el nombre de clúster para cada uno de ellos. El nombre del clúster es el mismo que el nombre del espacio de nombres.

Además, puede usar los nombres DNS del punto de conexión que prefiera, siempre y cuando estén completos y no entren en conflicto entre dos clústeres de macrodatos implementados en el mismo dominio. Opcionalmente, puede usar el valor del parámetro del subdominio para garantizar que los nombres DNS son diferentes en los clústeres.  Como ejemplo, considere el punto de conexión de puerta de enlace. Si quiere usar el nombre `gateway` para el punto de conexión y registrarlo en el servidor DNS automáticamente como parte de la implementación del clúster de macrodatos, use `gateway.bdc1.contoso.local` como nombre DNS. Si `bdc1` es el subdominio y `contoso.local` es el nombre de dominio DNS de AD. Otros valores aceptables son `gateway-bdc1.contoso.local` o simplemente `gateway.contoso.local`.

## <a name="examples"></a>Ejemplos

A continuación, se muestra un ejemplo de configuración de seguridad de Active Directory, en caso de que quiera invalidar subdomain y accountPrefix. 

```json
    "security": { 
        "activeDirectory": { 
            "ouDistinguishedName":"OU=contosoou,DC=contoso,DC=local", 
            "dnsIpAddresses": [ "10.10.10.10" ], 
            "domainControllerFullyQualifiedDns": [ "contoso-win2016-dc.contoso.local" ], 
            "domainDnsName":"contoso.local", 
            "subdomain": "bdc", 
            "accountPrefix": "myprefix", 
            "clusterAdmins": [ "contosoadmins" ], 
            "clusterUsers": [ "contosousers1", "contosousers2" ] 
        } 
    } 
  
```

A continuación, se muestra un ejemplo de especificación del punto de conexión para los puntos de conexión del plano de control. Puede usar cualquier valor para los nombres DNS, siempre que sean únicos y completos:
  
```json
        "endpoints": [ 
            { 
                "serviceType": "NodePort", 
                "port": 30080, 
                "name": "Controller", 
                "dnsName": "control-bdc1.contoso.local" 
            }, 
            { 
                "serviceType": "NodePort", 
                "port": 30777, 
                "name": "ServiceProxy", 
                "dnsName": "monitor-bdc1.contoso.local" 
            } 
        ] 
  
```

## <a name="questions"></a>Preguntas

### <a name="do-you-need-to-create-separate-ous-for-different-clusters"></a>¿Necesita crear unidades organizativas independientes para diferentes clústeres?

No es necesario, pero es recomendable. Proporcionar unidades organizativas independientes para clústeres independientes ayuda a administrar las cuentas de usuario generadas.

### <a name="how-to-revert-back-to-the-pre-cu5-behavior-in-sql-2019"></a>¿Cómo revertir al comportamiento anterior a CU5 en SQL 2019?

Puede haber escenarios en los que no pueda adaptar el parámetro `subdomain` recientemente introducido. Por ejemplo, debe implementar una versión anterior a CU5 y ya se ha actualizado [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Esto es muy improbable, pero si necesita revertir al comportamiento anterior a CU5, puede establecer el parámetro `useSubdomain` en `false` en la sección de Active Directory de `control.json`.

En el siguiente ejemplo se establece `useSubdomain` en `false` para este escenario.

```console
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false" 
```

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas de integración de Active Directory de un clúster de macrodatos de SQL Server](troubleshoot-active-directory.md)

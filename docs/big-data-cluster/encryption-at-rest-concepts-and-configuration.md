---
title: Cifrado en reposo
titleSuffix: SQL Server Big Data Clusters
description: Obtenga toda la información que necesita sobre el cifrado en reposo en una instancia de Clústeres de macrodatos de SQL Server 2019.
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69749c0da2a5f7ef672a4673b5bc857898e9964f
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489189"
---
# <a name="encryption-at-rest-concepts-and-configuration-guide"></a>Conceptos y guía de configuración del cifrado en reposo

A partir de Clústeres de macrodatos de SQL Server CU8, hay disponible un conjunto completo de características de cifrado en reposo para proporcionar cifrado de nivel de aplicación a todos los datos almacenados en la plataforma. En esta guía se documentan los conceptos, la arquitectura y la configuración del conjunto de características de cifrado en reposo para Clústeres de macrodatos de SQL Server.

La plataforma Clústeres de macrodatos de SQL Server almacena datos en estas dos ubicaciones:

* Instancia maestra de __SQL Server__
* __HDFS__ usado por bloque de almacenamiento y Spark.

Hay dos enfoques posibles para poder cifrar datos de manera transparente en Clústeres de macrodatos de SQL Server:

* __Cifrado de volumen__. Este enfoque es compatible con la plataforma de Kubernetes y se espera como un procedimiento recomendado para las implementaciones de Clústeres de macrodatos. En esta guía no se trata el cifrado de volumen. Consulte la documentación del dispositivo o de la plataforma de Kubernetes para ver guías sobre cómo cifrar correctamente los volúmenes que se usarán para Clústeres de macrodatos de SQL Server.
* __Cifrado de nivel de aplicación__. Esta arquitectura hace referencia al cifrado de datos por parte de la aplicación que controla los datos antes de que se escriban en el disco. En caso de que se expongan los volúmenes, un atacante no podrá restaurar los artefactos de datos en ningún otro lugar, a menos que el sistema de destino también se haya configurado con las mismas claves de cifrado.

El __conjunto de características de cifrado en reposo de Clústeres de macrodatos de SQL Server__ admite el escenario principal de __cifrado de nivel de aplicación__ para los componentes de __SQL Server__ y __HDFS__.

Se proporcionan las funcionalidades siguientes:

* __Cifrado de reposo administrado por el sistema__. Esta funcionalidad está disponible en CU8.
* __Cifrado en reposo administrado por el usuario (BYOK)__ , con integraciones de proveedor de claves externas y administradas por el servicio. Actualmente, solo se admiten las claves creadas por el usuario administradas por el servicio.

## <a name="key-definitions"></a>Definiciones clave

### <a name="sql-server-big-data-clusters-key-management-service-kms"></a>Servicios de administración de claves (KMS) de Clústeres de macrodatos de SQL Server

Un servicio hospedado de controlador responsable de administrar claves y certificados para el conjunto de características de cifrado en reposo del clúster de BDC de SQL Server. Es un servicio que admite estas características:

* Administración y almacenamiento seguros de claves y certificados usados para el cifrado en reposo.
* Compatibilidad con KMS de Hadoop. Actúa como el servicio de administración de claves para el componente HDFS en BDC.
* Administración de certificados de TDE de SQL Server.

Durante el resto de este documento, haremos referencia a este servicio como __KMS de BDC__. Además, el término __BDC__ se usa para hacer referencia a la plataforma informática __Clústeres de macrodatos de SQL Server__.

### <a name="system-managed-keys-and-certificates"></a>Certificados y claves administrados por el sistema

El servicio KMS de BDC administrará todas las claves y certificados para SQL Server y HDFS.

### <a name="user-provided-certificates"></a>Certificados proporcionados por el usuario

Claves y certificados proporcionados por el usuario que van a ser administrados por KMS de BDC, comúnmente conocido Bring Your Own Key (BYOK).

### <a name="external-providers"></a>Proveedores externos

Soluciones de claves externas compatibles con KMS de BDC para delegación externa. Esta funcionalidad no se admite en este momento.

## <a name="encryption-at-rest-on-sql-server-big-data-clusters"></a>Cifrado en reposo en Clústeres de macrodatos de SQL Server

Lea con atención este documento para evaluar completamente el escenario.

El conjunto de características presenta el __servicio de controlador KMS de BDC__ para brindar claves y certificados administrados por el sistema para el cifrado de datos en reposo tanto en SQL Server como en HDFS. Esas claves y certificados los administra el servicio y en esta documentación se proporcionan los lineamientos operativos sobre cómo interactuar con el servicio.

* Las instancias de __SQL Server__ aprovechan la funcionalidad [Cifrado de datos transparente (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md).
* __HDFS__ usa KMS de Hadoop nativo dentro de cada pod para interactuar con KMS de BDC en el controlador. Esto habilita las zonas de cifrado de HDFS, las que proporcionan rutas de acceso seguras en HDFS.

### <a name="sql-server-instances"></a>Instancias de SQL Server

* Se instalará un certificado generado por el sistema en los pods de SQL Server que se van a usar con los comandos de TDE. El estándar de nomenclatura de los certificados administrados por el sistema es `TDECertificate` + `timestamp`. Por ejemplo, `TDECertificate2020_09_15_22_46_27`.
* Las bases de datos de usuario y las bases de datos aprovisionadas por BDC de la instancia maestra no se cifrarán de manera automática. Los administradores de bases de datos pueden usar el certificado instalado para cifrar cualquier base de datos.
* El grupo de proceso y el bloque de almacenamiento se cifrarán automáticamente mediante el certificado generado por el sistema.
* Si bien es técnicamente posible con el comando `EXECUTE AT` de T-SQL, no se recomienda el cifrado de grupo de datos, ni tampoco se admite en este momento. El uso de esta técnica para cifrar las bases de datos del grupo de datos podría no ser efectivo y es posible que el cifrado no se realice en el estado deseado. También se crea una ruta de actualización incompatible con respecto a las versiones siguientes.
* La rotación de claves de SQL Server se consigue mediante los comandos administrativos estándar de T-SQL. Para obtener instrucciones completas, consulte la [guía de uso del cifrado de datos transparente (TDE) en reposo de Clústeres de macrodatos de SQL Server](encryption-at-rest-sql-server-tde.md).
* La supervisión del cifrado se produce a través de las DMV de SQL Server estándar existentes para TDE.
* Se puede hacer copias de seguridad y restaurar una base de datos habilitada para TDE en el clúster.
* Se admite la alta disponibilidad. Si una base de datos de la instancia principal de SQL Server está cifrada, también se cifrará toda la réplica secundaria de la base de datos.

### <a name="hdfs-encryption-zones"></a>Zonas de cifrado HDFS

* La [integración de Active Directory](active-directory-prerequisites.md) es necesaria para habilitar la característica de zonas de cifrado para HDFS.
* Una clave generada por el sistema se aprovisionará en KMS de Hadoop. El nombre de la clave es `securelakekey`. En CU8, la clave predeterminada es 256 bits y se admite el cifrado AES de 256 bits.
* Una zona de cifrado predeterminada se aprovisionará con la clave generada por el sistema anterior en una ruta de acceso denominada `/securelake`.
* Los usuarios pueden crear zonas de cifrado y claves adicionales si siguen las instrucciones específicas que aparecen en esta guía. Los usuarios podrán elegir el tamaño de clave de 128, 192 o 256 durante la creación de las claves.
* La rotación de claves de las zonas de cifrado de HDFS se consigue mediante azdata. Para obtener instrucciones completas, consulte la [guía de uso de las zonas de cifrado de HDFS de Clústeres de macrodatos de SQL Server](encryption-at-rest-hdfs-encryption-zones.md).
* No se admite el montaje en niveles de HDFS sobre una zona de cifrado.

## <a name="encryption-at-rest-administration"></a>Administración del cifrado en reposo

La lista siguiente contiene las funcionalidades de administración para el cifrado en reposo:

* La administración del [cifrado de datos transparente (TDE) de SQL Server](encryption-at-rest-sql-server-tde.md) se realiza mediante los comandos estándar de T-SQL.
* La administración de las [zonas de cifrado de HDFS](encryption-at-rest-hdfs-encryption-zones.md) y de las claves de HDFS se realiza mediante los comandos de azdata.
* Las siguientes características de administración se llevan a cabo mediante el uso de [cuadernos operativos](cluster-manage-notebooks.md):
    - Copia de seguridad y recuperación de claves de HDFS
    - Eliminación de claves de HDFS

## <a name="configuration-guide"></a>Guía de configuración

Durante las __implementaciones nuevas de Clústeres de macrodatos de SQL Server__, desde CU8 en adelante, el __cifrado en reposo estará habilitado y configurado de manera predeterminada__. Esto significa lo siguiente:

* El componente KMS de BDC se implementará en el controlador y generará un conjunto predeterminado de claves y certificados.
* SQL Server se implementará con TDE activado y el controlador instalará los certificados.
* KMS de Hadoop (para HDFS) se configurará para interactuar con KMS de BDC para las operaciones de cifrado. Las zonas de cifrado de HDFS están configuradas y listas para usarse.

Se aplican los requisitos y comportamientos predeterminados descritos en la sección anterior.

__Si instala una nueva implementación de BDC de SQL Server de CU8 en adelante, o bien actualiza directamente a CU9, no es necesario que realice ningún paso adicional__.

### <a name="upgrade-scenarios"></a>Actualización de los escenarios

En los clústeres existentes, el proceso de actualización no aplicará nuevo cifrado ni volverá a cifrar los datos de usuario que no se hayan cifrado. Este es el comportamiento predeterminado y se deben tener en cuenta las necesidades siguientes por componente:

* __SQL Server__

    1. __Instancia maestra de SQL Server__. El proceso de actualización no afectará las bases de datos de instancia maestra ni los certificados de TDE instalados, pero se recomienda hacer una copia de seguridad de las bases de datos y de los certificados TDE instalados manualmente antes del proceso de actualización. También se recomienda almacenar esos artefactos fuera del clúster de BDC de SQL Server.
    1. __Grupo de proceso y bloque de almacenamiento__. Esas bases de datos están administradas por el sistema, son volátiles, se volverán a crear y se cifrarán automáticamente en la actualización del clúster.
    1. __Grupo de datos__. La actualización no afecta las bases de datos de la parte de las instancias de SQL Server del grupo de datos.

* __HDFS__

    1. __HDFS__. El proceso de actualización no tocará los archivos ni las carpetas de HDFS fuera de las zonas de cifrado.

### <a name="upgrading-to-cu9-from-cu8-or-earlier"></a>Actualización a CU9 desde CU8 o versiones anteriores

No se necesita realizar ningún paso adicional.

### <a name="upgrading-to-cu8-from-cu6-or-earlier"></a>Actualización a CU8 desde CU6 o versiones anteriores

   > [!CAUTION]
   > Antes de actualizar a Clústeres de macrodatos de SQL Server CU8, haga una copia de seguridad completa de los datos.


__No se configurarán zonas de cifrado__. El componente KMS de Hadoop no se configurará para usar KMS de BDC. A fin de configurar y habilitar la característica de zonas de cifrado de HDFS después de la actualización, siga las instrucciones de la siguiente sección.

#### <a name="enable-hdfs-encryption-zones-after-upgrade-to-cu8"></a>Habilitación de zonas de cifrado de HDFS después de la actualización a CU8

Si actualizó el clúster a CU8 (`azdata upgrade`) y quiere habilitar zonas de cifrado de HDFS, tiene dos opciones:

* Ejecutar el [cuaderno operativo](cluster-manage-notebooks.md) de Azure Data Studio denominado __SOP0128: Habilitación de zonas de cifrado de HDFS en Clústeres de macrodatos__ para llevar a cabo la configuración.
* Ejecutar scripts, tal y como se describe a continuación.

Requisitos:

- Clúster integrado en [Active Directory](active-directory-prerequisites.md).

- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] configurada y conectada en el clúster en el modo de AD.

Siga el procedimiento siguiente para volver a configurar el clúster con compatibilidad con zonas de cifrado.

1. Después de realizar la actualización con `azdata`, guarde los scripts siguientes.

    Requisitos de ejecución de scripts:
        
    * Ambos scripts deben estar en el mismo directorio. 
    * `kubectl` en `PATH
    * Archivo ```config``` para Kubernetes en la carpeta ```$HOME/.kube```
    
    Este script debe tener el nombre __```run-key-provider-patch.sh```__ :

    ```console
    #!/bin/bash
    
    if [[ -z "${BDC_DOMAIN}" ]]; then
      echo "BDC_DOMAIN environment variable with the domain name of the cluster is not defined."
      exit 1
    fi
    
    if [[ -z "${BDC_CLUSTER_NS}" ]]; then
      echo "BDC_CLUSTER_NS environment variable with the cluster namespace is not defined."
      exit 1
    fi
    
    kubectl get configmaps -n test
    
    diff <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    diff <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    # Replace the config maps.
    #
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    # Restart the pods which need to have the necessary changes with the core-site.xml
    kubectl delete pods -n $BDC_CLUSTER_NS nmnode-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-1
    
    # Wait for sometime for pods to restart
    #
    sleep 300
    
    # Check for the KMS process status.
    #
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop nmnode-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-1 -- bash -c 'ps aux | grep kms'
    ```

    Este script debe tener el nombre __```updatekeyprovider.py```__ :

    ```python
    #!/usr/bin/env python3
    
    import json
    import re
    import sys
    import xml.etree.ElementTree as ET
    import os
    
    class CommentedTreeBuilder(ET.TreeBuilder):
        def comment(self, data):
            self.start(ET.Comment, {})
            self.data(data)
            self.end(ET.Comment)
    
    domain_name = os.environ['BDC_DOMAIN']
    
    parser = ET.XMLParser(target=CommentedTreeBuilder())
    
    core_site = 'core-site.xml'
    j = json.load(sys.stdin)
    cs = j['data'][core_site]
    csxml = ET.fromstring(cs, parser=parser)
    props = [prop.find('value').text for prop in csxml.findall(
        "./property/name/..[name='hadoop.security.key.provider.path']")]
    
    kms_provider_path=''
    
    for x in range(5):
        if len(kms_provider_path) != 0:
            kms_provider_path = kms_provider_path + ';'
        kms_provider_path = kms_provider_path + 'nmnode-0-0.' + domain_name
    
    if len(props) == 0:
        prop = ET.SubElement(csxml, 'property')
        name = ET.SubElement(prop, 'name')
        name.text = 'hadoop.security.key.provider.path'
        value = ET.SubElement(prop, 'value')
        value.text = 'kms://https@' + kms_provider_path + ':9600/kms'
        cs = ET.tostring(csxml, encoding='utf-8').decode('utf-8')
    
    j['data'][core_site] = cs
    
    kms_site = 'kms-site.xml.tmpl'
    ks = j['data'][kms_site]
    
    kp_uri_regex = re.compile('(<name>hadoop.kms.key.provider.uri</name>\s*<value>\s*)(.*)(\s*</value>)', re.MULTILINE)
    
    def replace_uri(match_obj):
        key_provider_uri = 'bdc://https@hdfsvault-svc.' + domain_name
        if match_obj.group(2) == 'jceks://file@/var/run/secrets/keystores/kms/kms.jceks' or match_obj.group(2) == key_provider_uri:
            return match_obj.group(1) + key_provider_uri + match_obj.group(3)
        return match_obj.group(0)
    
    ks = kp_uri_regex.sub(replace_uri, ks)
    
    j['data'][kms_site] = ks
    print(json.dumps(j, indent=4, sort_keys=True))
    ```

    Ejecute __```run-key-provider-patch.sh```__ con los parámetros adecuados. 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar eficazmente el cifrado en reposo de Clústeres de macrodatos de SQL Server, consulte estos artículos:

- [Cifrado en reposo: TDE de SQL Server](encryption-at-rest-sql-server-tde.md)
- [Cifrado en reposo: zonas de cifrado de HDFS](encryption-at-rest-hdfs-encryption-zones.md)

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea la siguiente introducción:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)

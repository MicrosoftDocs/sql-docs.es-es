---
title: Información general sobre la configuración de Clústeres de macrodatos de SQL Server
titleSuffix: SQL Server big data clusters
description: Información general sobre la configuración de Clústeres de macrodatos
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cda6e61e8f5f13f5fd414879f888c7ed72a1bd0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343983"
---
# <a name="configure-a-sql-server-big-data-cluster"></a>Configuración de un clúster de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
La administración de configuración permite a los administradores asegurarse de que su clúster de macrodatos siempre está preparado para sus necesidades de carga de trabajo. Con esta funcionalidad, los administradores de clústeres pueden modificar o ajustar varias partes del clúster de macrodatos en el momento de la implementación o después de esta y obtener una visión más profunda de las configuraciones que se ejecutan en su clúster de BDC. 

La administración de configuración permite a un administrador habilitar el Agente SQL, definir los recursos de línea de base para los trabajos de Spark de su organización o incluso ver qué opciones de configuración se pueden configurar en cada ámbito. En el momento de la implementación, las configuraciones se pueden definir mediante el archivo de implementación `bdc.json` y, después de la implementación, mediante la CLI de azdata.

## <a name="configuration-scopes"></a>Ámbitos de configuración
La configuración de los Clústeres de macrodatos tiene tres niveles de ámbito: `cluster`, `service` y `resource`. La jerarquía de la configuración también sigue este orden, de mayor a menor. Los componentes de BDC tomarán el valor de la configuración que se define en el ámbito inferior. Si la configuración no se define en un ámbito determinado, heredará el valor de su ámbito principal superior.

Por ejemplo, puede que quiera definir el número predeterminado de núcleos que el controlador de Spark usará en el bloque de almacenamiento y en los recursos de `Sparkhead`. Para definir el número predeterminado de núcleos, puede realizar una de las siguientes acciones:

- Especifique un valor predeterminado de núcleos en el ámbito de servicio `Spark`

- Especifique un valor predeterminado de núcleos en el ámbito de recursos `storage-0` y `sparkhead`

En el primer escenario, todos los recursos con ámbito inferior del servicio Spark (bloque de almacenamiento y `Sparkhead`) *heredarán* el número predeterminado de núcleos del valor predeterminado del servicio Spark.

En el segundo escenario, cada recurso usará el valor definido en su ámbito respectivo.

Si el número predeterminado de núcleos se configura tanto en el ámbito de servicio como en el de ámbito, el valor con ámbito de recurso reemplazará el valor con ámbito de servicio, porque se trata del ámbito **configurado por el usuario** inferior para la configuración determinada.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información específica sobre la configuración, vea los artículos siguientes:

- [Configuración de opciones de implementación de recursos y servicios de clúster](deployment-custom-configuration.md)
- [Procedimiento para configurar BDC después de la implementación](configure-bdc-postdeployment.md)
- [Configuración de un clúster de macrodatos de SQL Server (versiones anteriores a CU9)](configure-bdc-pre-configuration.md)

Referencia: 
- [Propiedades de configuración de Clústeres de macrodatos de SQL Server](reference-config-bdc-overview.md)
- [Propiedades de configuración de Apache Spark y Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Propiedades de configuración de la instancia maestra de SQL Server (versiones anteriores a CU9)](reference-config-master-instance.md)
- [azdata](../azdata/reference/reference-azdata.md)
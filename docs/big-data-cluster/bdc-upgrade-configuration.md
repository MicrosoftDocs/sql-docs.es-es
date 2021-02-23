---
title: Actualización a un clúster de macrodatos habilitado para la administración de configuración
titleSuffix: SQL Server big data clusters
description: Actualización a un clúster de macrodatos habilitado para la administración de configuración
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0a9f6addcf73e0c50d67f75f19fedd51b2f3dbc9
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344014"
---
# <a name="upgrading-to-a-configuration-management-enabled-cluster-cu8-or-lower-to-cu9"></a>Actualización a un clúster habilitado para la administración de configuración (CU8 o inferior a CU9 y versiones posteriores)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
A partir de CU9, la plataforma Clústeres de macrodatos (BDC, por sus siglas en inglés) incluye una característica de administración de configuración que permite a los administradores alterar o ajustar varias partes del clúster de macrodatos después de la implementación y obtener información más detallada sobre las configuraciones que se ejecutan en su BDC. Antes de CU9, las configuraciones de Clústeres de macrodatos solo se podían modificar en el momento de la implementación, con una solución alternativa para editar algunas configuraciones de SQL mediante un archivo `mssql-custom.conf` personalizado. Esta solución alternativa se ha resuelto y ahora los ajustes se puede configurar mediante la característica de administración de configuración.

## <a name="migrating-sql-configurations-in-mssql-customconf-to-the-configuration-management-system"></a>Migración de configuraciones de SQL en mssql-custom.conf al sistema de administración de configuración
Si ha creado un archivo `mssql-custom.conf` personalizado para las instancias maestras de SQL Server, siga las instrucciones que aparecen a continuación para administrar la configuración mediante el sistema de configuración y no mediante el archivo. Si no sigue estos pasos, la funcionalidad de administración de configuración no administrará esas configuraciones de SQL y los valores de `mssql-custom.conf` invalidarán los cambios que se realicen mediante la funcionalidad de administración de configuración.

Pasos:
1. Actualice su instancia de Clústeres de macrodatos a CU9.
> [!NOTE]
> La configuración definida mediante `mssql-custom.conf` no se modificará ni eliminará. Simplemente no se reflejará ni se administrará mediante el marco de trabajo de configuración.

2. Establezca y aplique cualquier valor de configuración definido previamente en `mssql-custom.conf` con la nueva funcionalidad de configuración. Consulte el artículo de [información general sobre la configuración de Clústeres de macrodatos de SQL Server después de la implementación](configure-bdc-postdeployment.md) para obtener una guía paso a paso de cómo cambiar la configuración. Consulte [Propiedades de configuración de Clústeres de macrodatos de SQL Server](reference-config-bdc-overview.md) para obtener una lista completa de las opciones de configuración disponibles para cada ámbito. Tenga en cuenta que algunos valores pueden haber cambiado de ámbito, pero siguen estando disponibles, como customerFeedback.
3. Cambie el nombre del archivo `mssql-custom.conf` a `deprecated-mssql-custom.conf` en el contenedor `mssql-server` de cada pod maestro. Solo tiene un pod maestro, use `master-0`. En caso de que necesite degradar o revertir a un clúster que no esté habilitado para la configuración (CU8 o inferior), puede volver a usar este archivo para aplicar estas configuraciones personalizadas de SQL.

## <a name="downgrading-from-a-configuration-management-enabled-cluster-to-a-non-configuration-management-enabled-cluster-cu9-to-cu8-or-lower"></a>Degradación de un clúster habilitado para la administración de configuración a un clúster que no tiene habilitada la administración de configuración (de CU9 y versiones posteriores a CU8 o una versión inferior)
La degradación de un clúster habilitado para la administración de configuración (CU9 y versiones posteriores) a un clúster que no está habilitado para la administración de configuración (CU8 o versiones anteriores) eliminará la capacidad de optimizar el clúster de macrodatos después de la implementación. También se requerirá el uso del archivo `mssql-custom.conf` opcional para establecer las configuraciones de SQL. Si cambió el nombre del archivo a `deprecated-mssql-custom.conf` al realizar la actualización a CU9 o a una versión posterior, vuelva a cambiarlo a `mssql-custom.conf`. Si eliminó el archivo o no lo creó previamente y ahora necesita definir estas configuraciones especiales de SQL, créelo siguiendo las instrucciones que se indican en el artículo [Propiedades de configuración de la instancia maestra de SQL Server (versiones anteriores a CU9)](reference-config-master-instance.md). Todas las configuraciones definidas y modificadas mediante la experiencia de administración de configuración se revertirán a las configuraciones anteriores o a los valores predeterminados del sistema. 

Una vez degradado el clúster, las configuraciones se revertirán a sus valores predeterminados o a los valores especificados en el archivo de implementación bdc.json. No es necesario realizar ningún otro paso después de la degradación.
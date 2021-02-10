---
description: Propiedades de RDS
title: Propiedades de RDS | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- RDS properties [ADO]
- properties [ADO], RDS
ms.assetid: e4e04cbd-21fc-44a1-9f21-49aa68746934
author: rothja
ms.author: jroth
ms.openlocfilehash: 641b2776efbd8b4e78348c45aecef3ace006dfdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049225"
---
# <a name="rds-properties"></a>Propiedades de RDS
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
|Propiedad|Descripción|  
|-|-|  
|[Conectar (RDS)](./connect-property-rds.md)|Indica el nombre de la base de datos desde la que se ejecutan las operaciones de consulta y actualización.|  
|[ExecuteOptions (RDS)](./executeoptions-property-rds.md)|Indica si la ejecución asincrónica está habilitada.|  
|[FetchOptions (RDS)](./fetchoptions-property-rds.md)|Indica el tipo de captura asincrónica.|  
|[FilterColumn (RDS)](./filtercolumn-property-rds.md)|Indica la columna en la que se van a evaluar los criterios de filtro.|  
|[FilterCriterion (RDS)](./filtercriterion-property-rds.md)|Indica el operador de evaluación que se va a usar en el valor de filtro.|  
|[FilterValue (RDS)](./filtervalue-property-rds.md)|Indica el valor para filtrar los registros.|  
|[Controlador (RDS)](./handler-property-rds.md)|Indica el nombre de un programa de personalización de servidor (*controlador*) que extiende la funcionalidad de **RDSServer. DataFactory** y cualquier parámetro utilizado por el *controlador*.|  
|[InternetTimeout (RDS)](./internettimeout-property-rds.md)|Indica el número de milisegundos que hay que esperar antes de que se agote el tiempo de espera de una solicitud.|  
|[ReadyState (RDS)](./readystate-property-rds.md)|Indica el progreso de un objeto **DataControl** a medida que captura los datos en su objeto de **conjunto de registros** .|  
|[Recordset y SourceRecordset (RDS)](./recordset-sourcerecordset-properties-rds.md)|Indica el objeto de **conjunto de registros** devuelto desde un objeto comercial personalizado.|  
|[Servidor (RDS)](./server-property-rds.md)|Indica el nombre del Internet Information Services (IIS) y el protocolo de comunicación.|  
|[SortColumn (RDS)](./sortcolumn-property-rds.md)|Indica la columna por la que se ordenarán los registros.|  
|[SortDirection (RDS)](./sortdirection-property-rds.md)|Indica si un criterio de ordenación es ascendente o descendente.|  
|[SQL (RDS)](./sql-property.md)|Indica la cadena de consulta utilizada para recuperar el **conjunto de registros**.|  
|[URL (RDS)](./url-property-rds.md)|Indica una cadena que contiene una dirección URL relativa o absoluta.|
---
title: Clasificación y detección de datos en SqlClient
description: Describe cómo comprobar si una base de datos de SQL Server admite la clasificación de datos y cómo obtener acceso a la información de clasificación de datos a través de un objeto SqlDataReader.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: bac8c09b862e43549ade3a6d98e34fdc97596131
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835772"
---
# <a name="data-discovery-and-classification-in-sqlclient"></a>Clasificación y detección de datos en SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

La [clasificación y detección de datos](../../../relational-databases/security/sql-data-discovery-and-classification.md) es un conjunto de servicios avanzados para detectar, clasificar, etiquetar y notificar información confidencial en las bases de datos. SqlClient proporciona una API que expone información de clasificación y detección de datos de solo lectura cuando el origen subyacente es compatible con la característica. Se obtiene acceso a esta información a través de SqlDataReader.

Microsoft.Data.SqlClient v2.1.0 incluye compatibilidad con la información `Sensitivity Rank` de la clasificación de datos. `Sensitivity Rank` es un identificador basado en un conjunto de valores predefinidos que definen el rango de confidencialidad. Lo pueden usar otros servicios como Advanced Threat Protection para detectar anomalías en función de su rango. Las siguientes API de clasificación de datos ahora están disponibles en el espacio de nombres Microsoft.Data.SqlClient.DataClassification:

```csharp
// New in Microsoft.Data.SqlClient v2.1.0
public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}

public sealed class SensitivityClassification
{
  // Returns the sensitivity rank for the query associated with the active 'SqlDataReader'.
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the labels collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<Label> Labels;

  // Returns the information types collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<InformationType> InformationTypes;

  // Returns the column sensitivity for this 'SensitivityClassification' Object
  public ReadOnlyCollection<ColumnSensitivity> ColumnSensitivities;
}

public sealed class SensitivityProperty
{
  // Returns the sensitivity rank for this 'SensitivityProperty' Object
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the label for this 'SensitivityProperty' Object
  public Label Label;

  // Returns the information type for this 'SensitivityProperty' Object
  public InformationType InformationType;
}

public sealed class Label
{
  // Gets the name for this 'Label' object
  public string Name;

  // Gets the ID for this 'Label' object
  public string Id;
}

public sealed class InformationType
{
  // Gets the name for this 'InformationType' object
  public string Name;

  // Gets the ID for this 'InformationType' object
  public string Id;
}

public sealed class ColumnSensitivity
{
  // Returns the list of sensitivity properties as received from Server for this 'ColumnSensitivity' information      
  public ReadOnlyCollection<SensitivityProperty> SensitivityProperties;
}
```

> [!NOTE]
> Microsoft.Data.SqlClient lee información `Sensitivity Rank` solo si SQL Server admite la clasificación de datos con rango. En el caso de los servidores que usan la versión anterior de la clasificación de datos sin rango, el valor de rango para las consultas es "NO DEFINIDO".

En esta aplicación de ejemplo se muestra cómo acceder a las propiedades de clasificación de datos de SqlDataReader.

[!code-csharp [SqlDataReader_DataDiscoveryAndClassification#1](~/../sqlclient/doc/samples/SqlDataReader_DataDiscoveryAndClassification.cs#1)]


**Consulte también**  

 - [Características de SQL Server y ADO.NET](sql-server-features-adonet.md)
 - [sys.sensitivity_classifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
 - [AGREGAR CLASIFICACIÓN DE SENSIBILIDAD](../../../t-sql/statements/add-sensitivity-classification-transact-sql.md)
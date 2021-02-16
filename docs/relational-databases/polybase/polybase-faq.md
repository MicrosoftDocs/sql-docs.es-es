---
title: Preguntas más frecuentes en PolyBase | Microsoft Docs
description: Compare PolyBase y los servidores vinculados, y compare PolyBase en clústeres de macrodatos y en instancias independientes. Descubra las novedades de PolyBase 2019.
ms.date: 12/02/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.openlocfilehash: 2e0f89137327deada6307147f8bd83a28f23e6a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351819"
---
# <a name="frequently-asked-questions"></a>Preguntas más frecuentes

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="polybase-vs-linked-servers"></a>PolyBase frente a servidores vinculados
En la tabla siguiente se resaltan las diferencias entre las características de servidor vinculado y PolyBase:

|PolyBase | Servidores vinculados|
|--------------------------|--------------------------|  
|Objeto con ámbito de base de datos.|Objeto con ámbito de instancia.|
|Usa controladores ODBC.|Usa proveedores OLEDB.|
|Admite operaciones de solo lectura en todos los orígenes de datos y operaciones de inserción solo para HADOOP y el origen de datos del grupo de datos.|Admite tanto operaciones de lectura como de escritura.|
|Las consultas al origen de datos remotos desde una sola conexión se pueden escalar horizontalmente |Las consultas al origen de datos remotos desde una única conexión no se pueden escalar horizontalmente|
|Se admite la aplicación de predicados.|Se admite la aplicación de predicados.|
|No se necesita ninguna configuración independiente para el grupo de disponibilidad|Se necesita una configuración independiente para cada instancia del grupo de disponibilidad|
|Solo autenticación básica|Autenticación básica e integrada|
|Adecuado para procesar un gran número de filas de consultas de análisis|Adecuado para consultas OLTP que devuelven algunas filas|
|Las consultas que usan tablas externas no pueden participar en transacciones distribuidas|Las consultas distribuidas pueden participar en transacciones distribuidas|

## <a name="whats-new-in-polybase-2019"></a>Novedades en PolyBase 2019 

PolyBase en [!INCLUDE[sssql19](../../includes/sssql19-md.md)] puede leer ahora datos de una mayor variedad de orígenes de datos. Los datos de estos orígenes de datos externos se pueden almacenar como tablas externas en SQL Server. PolyBase además admite el cálculo de aplicación en estos orígenes de datos externos, excepto los tipos de ODBC genéricos.

### <a name="compatible-data-sources"></a>Orígenes de datos compatibles

- SQL Server
- Oracle
- Teradata
- MongoDB
- Tipos genéricos de ODBC compatibles
  
> [!NOTE]
> PolyBase puede permitir la conexión a orígenes de datos externos mediante controladores ODBC de terceros. Estos controladores no se proporcionan con PolyBase y podrían no funcionar según lo previsto. Para más información, consulte la [guía](../../relational-databases/polybase/polybase-configure-odbc-generic.md) para la configuración genérica de ODBC de PolyBase.  

## <a name="polybase-in-big-data-clusters-vs-polybase-in-stand-alone-instances"></a>PolyBase en clústeres de macrodatos frente a PolyBase en instancias independientes

En la tabla siguiente se resaltan las características de PolyBase disponibles en la instalación independiente de [!INCLUDE[sssql19](../../includes/sssql19-md.md)] y el clúster de macrodatos de [!INCLUDE[sssql19](../../includes/sssql19-md.md)]:

|Característica |Clúster de macrodatos|Instancia independiente|
|--------------------------|--------------------------|---------|   
|Crear un origen de datos externos desde SQL Server, Oracle, Teradata y Mongo DB |X|X|
|Crear un origen de datos externos con un controlador ODBC de terceros compatible | | X|
|Crear un origen de datos externos para el origen de datos HADOOP | X| X|
|Crear un origen de datos externo para Azure Blob Storage | X| X|
|Crear una tabla externa en un grupo de datos de SQL Server | X| |
|Crear una tabla externa en un grupo de almacenamiento de SQL Server | X| |
|Escalar horizontalmente la ejecución de consultas | X| X (solo Windows) |

> [!NOTE]
>En la tabla no se describe la funcionalidad disponible en la versión más reciente de CTP de [!INCLUDE[sssql19](../../includes/sssql19-md.md)]. Para ver las características disponibles, consulte las notas de la versión. Para más información sobre las conexiones mediante el conector ODBC genérico, consulte la [guía para configurar tipos genéricos de ODBC](polybase-configure-odbc-generic.md).

---
title: Utilizar la ventana Propiedades en Management Studio
description: Obtenga información sobre cómo usar la ventana Propiedades para ver información sobre un elemento de SQL Server Management Studio, como una conexión, y sobre los objetos de base de datos.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing properties
- Properties window [SQL Server Management Studio]
- complex properties [SQL Server Management Studio]
ms.assetid: 903d4aca-f57c-43d9-a893-702eceaa7004
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2bd8af54c0178ac13f27dcb3ae23fc8e72b712a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346082"
---
# <a name="use-the-properties-window-in-management-studio"></a>Utilizar la ventana Propiedades en Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La ventana Propiedades describe el estado de un elemento de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], como una conexión o un operador de plan de presentación, además de proporcionar información acerca de objetos de la base de datos, como tablas, vistas y diseñadores.  
  
 La ventana Propiedades se puede utilizar para ver las propiedades de la conexión actual. Muchas propiedades son de solo lectura en la ventana Propiedades, aunque se pueden cambiar en otras ubicaciones de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Por ejemplo, la propiedad Database de una consulta es de solo lectura en la ventana Propiedades, aunque se puede cambiar en la barra de herramientas.  
  
### <a name="to-view-properties-using-the-properties-window"></a>Para ver propiedades mediante la ventana Propiedades  
  
1.  Si la ventana Propiedades no está visible, haga clic en **Ventana Propiedades** en el menú **Ver** o presione F4.  
  
2.  Establezca el foco en el objeto que desea ver.  
  
3.  Busque una propiedad específica en la ventana Propiedades.  
  
### <a name="to-view-connection-properties-of-a-query-window"></a>Para ver las propiedades de conexión de una ventana de consulta  
  
1.  Si la ventana Propiedades no está visible, haga clic en **Ventana Propiedades** en el menú **Ver** o presione F4.  
  
2.  En la ventana Propiedades podrá ver todas las propiedades de conexión.  
  
### <a name="to-view-the-properties-of-a-showplan-operator"></a>Para ver las propiedades de un operador de plan de presentación  
  
1.  En el menú **Consulta** , haga clic en **Incluir plan de ejecución real**.  
  
2.  En el Editor de consultas de SQL, escriba y ejecute una consulta.  
  
3.  Si la ventana Propiedades no está visible, haga clic en **Ventana Propiedades** en el menú **Ver** o presione F4.  
  
4.  En la pestaña **Plan de ejecución** del Editor de consultas de SQL, haga clic en los iconos de los operadores para ver información sobre éstos en la ventana Propiedades.  
  
## <a name="see-also"></a>Consulte también  
 [Ventana Propiedades &#40;Management Studio&#41;](../properties-window-management-studio.md)  
  

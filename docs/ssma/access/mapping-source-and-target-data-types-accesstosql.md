---
description: Asignación de tipos de datos de origen y de destino (AccessToSQL)
title: Asignar tipos de datos de origen y de destino (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- customizing data type mappings
- data types, mapping
- mapping, data types
- source data types
- target data types
ms.assetid: b362a075-16e7-423f-b63f-e1e9f02844a9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f5a725d0e2c6d9651884c399caabe2a8833826f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059058"
---
# <a name="mapping-source-and-target-data-types-accesstosql"></a>Asignación de tipos de datos de origen y de destino (AccessToSQL)
Los tipos de base de datos de Access se diferencian de los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos Al convertir objetos de base de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] datos de Access en objetos de, debe especificar cómo se asignan los tipos de datos de a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede aceptar las asignaciones de tipos de datos predeterminadas o puede personalizar las asignaciones tal y como se muestra en los procedimientos siguientes.  
  
## <a name="default-mappings"></a>Asignaciones predeterminadas  
SSMA tiene un conjunto predeterminado de asignaciones de tipos de datos. Para obtener la lista de asignaciones predeterminadas, vea [configuración del proyecto (asignación de tipos)](./project-settings-type-mapping-accesstosql.md).  
  
## <a name="customizing-data-type-mappings"></a>Personalizar asignaciones de tipos de datos  
Mediante el cuadro de diálogo **configuración del proyecto** , puede personalizar cómo se asignan los tipos para todas las bases de datos y los objetos de base de datos de un proyecto. Las asignaciones de tipos de un proyecto de se aplican a todas las bases de datos y a los objetos de base de datos que no tienen asignaciones de tipo personalizadas.  
  
También puede personalizar la asignación de tipos de datos en el nivel de base de datos o de tabla.  
  
En el procedimiento siguiente se muestra cómo asignar tipos de datos en el nivel de proyecto, base de datos u objeto de base de datos.  
  
**Para asignar tipos de datos**  
  
1.  Para personalizar la asignación de tipos de datos para todo el proyecto, abra el cuadro de diálogo **configuración del proyecto** :  
  
    1.  En el menú **herramientas** , seleccione **configuración del proyecto**.  
  
    2.  En el panel izquierdo, seleccione **asignación de tipos**.  
  
        El gráfico de asignación de tipos y los botones aparecen en el panel derecho.  
  
    O bien, para personalizar la asignación de tipos de datos en el nivel de base de datos o de tabla, seleccione la base de datos o tabla en el panel Explorador de metadatos de acceso:  
  
    1.  En el panel Explorador de metadatos de Access, expanda **Access-metabase** y, a continuación, expanda **bases** de datos.  
  
    2.  Seleccione la base de datos o la tabla para la que desea personalizar la asignación de tipos de datos.  
  
    3.  En el panel derecho, haga clic en **asignación de tipos**.  
  
2.  Para agregar una nueva asignación, haga lo siguiente:  
  
    1.  En el panel asignación de tipos, haga clic en **Agregar**.  
  
    2.  En el cuadro de diálogo **nueva asignación de tipos** , en **tipo de origen**, seleccione el tipo de datos de acceso que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique las longitudes de datos mínima y máxima para la asignación; para ello, active las casillas **desde** y **hasta** y, a continuación, escriba los valores.  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de datos en el cuadro **reemplazar con** y, a continuación, haga clic en **Aceptar**.  
  
3.  Para editar una asignación de tipo de datos, haga lo siguiente:  
  
    1.  En el panel asignación de tipos, haga clic en **Editar**.  
  
    2.  En el cuadro de diálogo **lista de asignación de tipos** , en tipo de **origen**, seleccione el tipo de datos de acceso que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique las longitudes de datos mínima y máxima para la asignación; para ello, active las casillas **desde** y **hasta** y, a continuación, escriba los valores.  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de datos en el cuadro **reemplazar con** y, a continuación, haga clic en **Aceptar**.  
  
4.  Para quitar una asignación de tipo de datos, haga lo siguiente:  
  
    1.  En el panel asignación de tipos, seleccione la fila de la lista asignación de tipos que contiene la asignación de tipo de datos que desea quitar.  
  
    2.  Haga clic en **Quitar**.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso del proceso de migración es [convertir los objetos de base de datos de Access en objetos de SQL Server](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  

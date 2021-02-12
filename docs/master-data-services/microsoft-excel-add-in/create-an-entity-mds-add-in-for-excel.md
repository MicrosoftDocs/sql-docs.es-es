---
description: Crear una entidad (Complemento MDS para Excel)
title: Crear una entidad
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d354abb3-88fe-4b40-a374-f6256b84ffae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b6a150ba658ff65991f0c6d73cbb557268075474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272416"
---
# <a name="create-an-entity-mds-add-in-for-excel"></a>Crear una entidad (Complemento MDS para Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], los administradores pueden crear nuevas entidades para almacenar datos. Cuando crea una entidad, debe cargar al menos una muestra de los datos que desea almacenar.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer del permiso para tener acceso a las áreas funcionales del **Explorador** y de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md).  
  
-   Debe tener un modelo existente en el que crear la entidad. Para obtener más información, consulte [Crear un modelo &#40;Master Data Services&#41;](../../master-data-services/create-a-model-master-data-services.md).  
  
-   Asegúrese de que los datos cumplen los siguientes requisitos:  
  
    -   Los datos deben tener una fila de encabezado.  
  
    -   Resulta útil tener las columnas **Nombre** y **Código** . **Código** es un identificador único para cada fila.  
  
    -   Debe tener al menos una fila de datos distinta del encabezado. Todas las columnas no necesitan valores, pero los datos deben ser representativos de los que estarán en la entidad.  
  
    -   Si tiene una columna que contiene un identificador único (conocido en MDS como **Código**), asegúrese de que los valores sean únicos. Si ninguna columna contiene identificadores, puede hacer que se generen automáticamente cuando se cree la entidad.  
  
    -   Asegúrese de que las celdas contienen fórmulas.  
  
    -   Asegúrese de que las celdas contienen valores de hora. Los valores de fecha se pueden guardar en MDS pero los valores de hora no.  
  
### <a name="to-create-an-entity-and-load-data"></a>Para crear una entidad y cargar datos  
  
1.  Abra o cree una hoja de cálculo de Excel que contenga los datos que desea cargar.  
  
2.  Seleccione las celdas que desea cargar en la entidad nueva.  
  
3.  En la pestaña **Datos maestros** , en el grupo **Generar modelo** , haga clic en **Crear entidad**.  
  
4.  Si se le solicita conectarse a un repositorio MDS, conéctese.  
  
5.  En el cuadro de diálogo **Crear entidad** , deje el intervalo predeterminado o cámbielo para aplicarlo a los datos que desea cargar.  
  
6.  No desactive la casilla **Mis tablas tienen encabezados** .  
  
7.  En la lista **Modelo** , seleccione un modelo.  
  
8.  En la lista **Versión** , seleccione una versión.  
  
9. En el cuadro **Nuevo nombre de entidad** , escriba un nombre para la entidad.  
  
10. En la lista **Código** , seleccione la columna que contiene identificadores únicos o haga que los códigos se generen automáticamente.  
  
11. Opcional. En la lista **Nombre** , seleccione una columna que contenga los nombres de cada miembro.  
  
12. Haga clic en **OK**. Cuando haya creado la entidad correctamente, aparecerá una nueva fila de encabezado, las celdas se resaltarán y el nombre de la hoja se actualizará para coincidir con el nombre de la entidad.  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Para ver los errores producidos, en el grupo **Publicar y validar** , haga clic en **Mostrar estado**. Se muestran las columnas ValidationStatus e InputStatus. Para obtener más información, consulte [Validar datos &#40;complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/validating-data-mds-add-in-for-excel.md).  
  
-   Confirme que los atributos se crearon con el tipo de datos que esperaba.  
  
## <a name="see-also"></a>Consulte también  
 [Crear un atributo basado en dominio &#40;complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/create-a-domain-based-attribute-mds-add-in-for-excel.md)  
  
  

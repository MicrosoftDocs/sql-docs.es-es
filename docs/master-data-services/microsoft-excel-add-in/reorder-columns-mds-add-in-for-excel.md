---
description: Reordenar columnas (complemento MDS para Excel)
title: Reordenar columnas
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ac00462e-c0f7-4b8d-86f2-d9eda2598a15
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 93d59286f93f890540f39a6e974dee4a2b5ecf31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355355"
---
# <a name="reorder-columns-mds-add-in-for-excel"></a>Reordenar columnas (complemento MDS para Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En el [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], puede reordenar las columnas si filtra la lista antes de cargar.  
  
 Cuando vuelva a ordenar atributos en el cuadro de diálogo **Filtro** , los datos se cargan en Excel con el orden nuevo. Sin embargo, la próxima vez filtre los datos de atributo, el orden revertirá al orden del diseño original. Para cambiar el orden de forma permanente, un administrador debe cambiar el orden en el área de **Administración del sistema** de Master Data Manager. Para más información, consulte [Change the Order of Attributes](../../master-data-services/change-the-order-of-attributes.md).  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso de acceso al área funcional **Explorador** .  
  
### <a name="to-reorder-mds-managed-columns"></a>Para reordenar columnas administradas por MDS  
  
1.  Abra Excel y, en la pestaña **Datos maestros** , conéctese a un repositorio MDS. Para obtener más información, consulte [Conectarse a un repositorio MDS &#40;complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md).  
  
2.  En el panel **Explorador de datos maestros** , seleccione un modelo y una versión. La lista de entidades se rellena.  
  
    -   Si el panel **Explorador de datos maestros** no está visible, en el grupo **Conectar y cargar** , haga clic en **Mostrar explorador**.  
  
    -   Si el panel **Explorador de datos maestros** está deshabilitado, se debe a que la hoja existente ya contiene datos administrados por MDS. Para habilitar el panel, abra una nueva hoja de cálculo.  
  
3.  En el panel **Explorador de datos maestros** , haga clic en una entidad.  
  
4.  En el grupo **Conectar y cargar** , haga clic en **Filtro**.  
  
5.  En el cuadro de diálogo **Filtro** , en la sección **Columnas** , en la lista de atributos, haga clic en el atributo que desea mover.  
  
6.  A la derecha de la lista, haga clic en la flecha **Arriba** o **Abajo** para mover el atributo a la derecha o a la izquierda en la hoja de cálculo.  
  
7.  Repita el paso 7 para cada atributo hasta que el orden de arriba abajo represente el orden de izquierda a derecha que desee en la hoja de cálculo.  
  
8.  Haga clic en **Cargar datos**. La hoja se rellena con datos administrados por MDS y las columnas se muestran en el orden especificado.  
  
## <a name="see-also"></a>Consulte también  
 [Información general: Exportar datos a Excel &#40;complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
  

---
title: Errores del proceso de almacenamiento provisional
description: En este artículo se explican los códigos de error agregados a todos los registros procesados durante el proceso de almacenamiento provisional en Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], error messages
ms.assetid: 0d9be0dd-638f-4dd4-92b2-253fda655455
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 84a5a66cbb8bacb3e1df896fedfa3a051e2f4dbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336264"
---
# <a name="staging-process-errors-master-data-services"></a>Errores del proceso de almacenamiento provisional (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cuando el proceso de ensayo ha finalizado, todos los registros procesados en las tablas de ensayo tienen un valor en la columna ErrorCode. Los valores se muestran en la tabla siguiente.  
  
|Código|Error|Se produce cuando/detalles|Se aplica a la tabla|  
|----------|-----------|--------------------------|----------------------|  
|210001|El mismo código de miembro existe varias veces en la tabla de ensayo.|El lote de almacenamiento provisional incluye el mismo código de miembro varias veces. No se crea ni actualiza ningún miembro.|Hoja<br /><br /> Consolidado<br /><br /> Relación|  
|210003|El atributo values hace referencia a un miembro que no existe o está inactivo.|Cuando almacena provisionalmente atributos basados en dominio, debe usar el código en lugar del nombre. Se aplica a **ImportType0**, **1** y **2**.|Hoja<br /><br /> Consolidado|  
|210006|El código de miembro está inactivo.|**ImportType**  =  **1** y especificó un código de miembro que no existe.|Hoja<br /><br /> Consolidado<br /><br /> Relación|  
|210032|El nombre de jerarquía falta o no es válido.|La jerarquía explícita no se encontró o el valor de **HierarchyName** estaba en blanco.|Consolidado<br /><br /> Relación|  
|210035|Dado que no existe una regla de negocios de generación de código, se requiere **MemberCode** .|Al crear o actualizar miembros, se requiere siempre **MemberCode** , a menos que esté utilizando la generación de código automática. Para obtener más información, consulte [creación automática de código &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Hoja<br /><br /> Consolidado|  
|210036|Dado que existe una regla de negocios de generación de código, no se requiere **MemberCode** .|Al crear o actualizar miembros, no se requiere **MemberCode** cuando se utilice la generación de código automática. Sin embargo, puede especificar un código si lo desea. Para obtener más información, consulte [creación automática de código &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Hoja<br /><br /> Consolidado|  
|210041|"ROOT" no es un código de miembro válido.|El valor de **MemberCode** contiene la palabra "root".|Hoja<br /><br /> Consolidado<br /><br /> Relación|  
|210042|"MDMUNUSED" no es un código de miembro válido.|El valor de **MemberCode** contiene la palabra "MDMUNUSED".|Hoja<br /><br /> Consolidado<br /><br /> Relación|  
|210052|MemberCode no puede estar desactivado porque se utiliza como valor de atributo basado en dominio.|Cuando **ImportType**  =  **3** o **4**, el almacenamiento provisional produce un error si el miembro se utiliza como valor de atributo para otros miembros. Use **ImportType5** o **6** para establecer el valor en NULL o cambiar los valores antes de ejecutar el proceso de almacenamiento provisional.|Hoja<br /><br /> Consolidado|  
|300002|El código de miembro no es válido.|Relaciones: el código de miembro primario o secundario no existe.<br /><br /> Hoja o consolidado: **ImportType**  =  **3** o **4** y el código de miembro no existe.|Hoja<br /><br /> Consolidado<br /><br /> Relación|  
|300004|El código de miembro ya existe.|**ImportType**  =  **1** y usó un código de miembro que ya existe en la entidad.|Hoja<br /><br /> Consolidado|  
|210011|Cuando **RelationshipType** es **1**, **ParentCode** no puede ser un miembro hoja.|Asegúrese de que el valor de **ParentCode** sea un código de miembro consolidado.|Relación|  
|210015|El código de miembro existe varias veces en la tabla de ensayo para una jerarquía y un lote.|Para una jerarquía explícita, especificó la ubicación del mismo miembro varias veces en el mismo lote.|Relación|  
|210016|No se pudo crear la relación porque produciría una referencia circular.|Esto ocurre si intenta asignar un elemento secundario como elemento primario.|Relación|  
|210046|El miembro no puede ser un elemento relacionado de Root.|Esto sucede si **RelationshipType**  =  **2** (relacionado) y **ParentCode** o **ChildCode** es **root**. Los miembros no pueden estar en el mismo nivel que el nodo raíz; solo pueden ser elementos secundarios.|Relación|  
|210047|El miembro no puede ser un elemento relacionado del nodo Unused.|Esto sucede si **RelationshipType**  =  **2** (hermano) y **ParentCode** o **ChildCode** no se **usan**. Los miembros solo pueden ser secundarios del nodo Unused.|Relación|  
|210048|**ParentCode** y **ChildCode** no pueden ser iguales.|El valor de **ParentCode** es igual que el valor de **ChildCode** . Estos valores deben ser diferentes.|Relación|  
  
## <a name="see-also"></a>Consulte también  
 [Ver los errores que se producen durante el Master Data Services de &#40;de almacenamiento provisional&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [Información general: importación de datos de tablas &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)  
  
  

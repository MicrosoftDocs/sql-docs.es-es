---
title: Tabla de ensayo de miembros hoja
description: Use la tabla de ensayo de miembros hoja en la base de datos Master Data Services para crear, actualizar, desactivar y eliminar miembros hoja.
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- members staging table [Master Data Services]
- database [Master Data Services], members staging table
ms.assetid: a8c953da-ec20-47dc-8656-ed5f0dfed89b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e38a38d14551b819db0e6ff7f8558448040a7049
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338226"
---
# <a name="leaf-member-staging-table-master-data-services"></a>Tabla de ensayo de miembros hoja (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Use la tabla de ensayo de miembros hoja (stg.name_Leaf) de la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] para crear, actualizar, desactivar y eliminar miembros hoja. También puede usarla para actualizar valores de atributos de miembros hoja.  
  
##  <a name="table-columns"></a><a name="TableColumns"></a> Columnas de la tabla  
 En la tabla siguiente se explica para qué se usa cada uno de los campos de la tabla de ensayo Leaf.  
  
|Nombre de columna|Descripción|Valores|  
|-----------------|-----------------|------------|  
|**Id**|Identificador asignado automáticamente.|No especifique ningún valor en este campo. Si no se ha procesado el lote, este campo está en blanco.|  
|**ImportType**<br /><br /> Obligatorio|Determina qué se debe hacer si los datos almacenados provisionalmente coinciden con datos que ya existen en la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|**0**: crear nuevos miembros. Reemplazar datos de MDS existentes con datos almacenados provisionalmente, pero solo si los datos almacenados provisionalmente no son NULL. Los valores NULL se pasan por alto. Para cambiar un valor de atributo de cadena a NULL, establézcalo en **~NULL~**. Para cambiar un valor de atributo de número a NULL, establézcalo en **-98765432101234567890**. Para cambiar un valor de atributo datetime a NULL, establézcalo en **5555-11-22T12:34:56**.<br /><br /> **1**: crear miembros nuevos únicamente. Todas las actualizaciones de datos MDS existentes producen un error.<br /><br /> **2**: crear nuevos miembros. Reemplazar los datos de MDS con datos almacenados provisionalmente. Si importa valores NULL, sobrescribirán los valores de MDS existentes.<br /><br /> **3**: desactivar el miembro, según el valor de Code. Se conservan todos los atributos, pertenencias a jerarquías y colecciones, y transacciones pero ya no están disponibles en la interfaz de usuario. Si se utiliza el miembro como valor de atributo basado en dominio de otro miembro, la desactivación producirá un error. Vea **ImportType5** para conocer una alternativa.<br /><br /> **4**: eliminar definitivamente el miembro, según el valor de Code. Todos los atributos, pertenencias a jerarquías y colecciones, y transacciones se eliminan de forma permanente. Si se utiliza el miembro como valor de atributo basado en dominio de otro miembro, la eliminación producirá un error. Vea **ImportType6** para conocer una alternativa.<br /><br /> **5**: desactivar el miembro, según el valor de **Code** . Se conservan todos los atributos, pertenencias a jerarquías y colecciones, y transacciones pero ya no están disponibles en la interfaz de usuario. Si se utiliza el miembro como valor de atributo basado en dominio de otros miembros, los valores relacionados se establecerán en NULL. ImportType 5 es para uso exclusivo en los miembros hoja.<br /><br /> **6**: eliminar definitivamente el miembro, según el valor de **Code** . Todos los atributos, pertenencias a jerarquías y colecciones, y transacciones se eliminan de forma permanente. Si se utiliza el miembro como valor de atributo basado en dominio de otros miembros, los valores relacionados se establecerán en NULL. ImportType 6 es para uso exclusivo en los miembros hoja.|  
|**ImportStatus_ID**<br /><br /> Obligatorio|Estado del proceso de importación.|**0**, que se especifica para indicar que el registro está listo para el almacenamiento provisional.<br /><br /> **1**, que se asigna e indica automáticamente que el proceso de almacenamiento provisional del registro ha sido correcto.<br /><br /> **2**, que se asigna automáticamente e indica que el proceso de almacenamiento provisional del registro no ha sido correcto.|  
|**Batch_ID**<br /><br /> Solo lo necesita el servicio web|Identificador asignado automáticamente que agrupa los registros para el almacenamiento provisional. A todos los miembros del lote se les asigna este identificador, que se muestra en la columna [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Id. **de la interfaz de usuario de** .|Si no se ha procesado el lote, este campo está en blanco.|  
|**BatchTag**<br /><br /> Obligatorio, excepto para el servicio web|Nombre único para el lote, de hasta 50 caracteres.||  
|**ErrorCode**|Muestra un código de error. Para todos los registros con un **ImportStatus_ID** de **2**, consulte [Errores del proceso de almacenamiento provisional &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md).||  
|**Código**<br /><br /> Obligatorio, excepto cuando los códigos se generan de forma automática para **ImportType1** o **2**; consulte [Creación automática de código &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md) para obtener más información|Código único del miembro.||  
|**Nombre**<br /><br /> Opcionales|Nombre del miembro.||  
|**NewCode**|Úselo solo si va a cambiar el código de miembro.||  
|\<Attribute name>|Existe una columna para cada atributo de la entidad. Úsela con un **ImportType** de **0** o **2**. Para los atributos de forma libre, especifique el nuevo texto o valor de cadena para el atributo. Para los atributos basados en dominio, especifique el código del miembro que será el atributo. Para los atributos de vínculo, la dirección URL debe comenzar con **https://**.<br /><br /> Nota: No puede almacenar de forma provisional los atributos de archivo.||  
  
## <a name="see-also"></a>Consulte también  
 [Información general: importación de datos de tablas &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)   
 [Ver los errores que se producen durante el Master Data Services de &#40;de almacenamiento provisional&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [Errores del proceso de almacenamiento provisional &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md)  
  
  

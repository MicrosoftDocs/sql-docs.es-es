---
description: Determinar el modo de edición
title: Determinar el modo de edición | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], edit mode
- ADO, editing data
ms.assetid: 4c7e010d-08cd-4e22-9b32-23c36f02f88c
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b0a6d6642e17841b8a974efa8d497cdc12203fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033255"
---
# <a name="determining-edit-mode"></a>Determinar el modo de edición
ADO mantiene un búfer de edición asociado al registro actual. La propiedad **EditMode** indica si se han realizado cambios en este búfer o si se ha creado un nuevo registro. Use **EditMode** para determinar el estado de edición del registro actual. Puede comprobar los cambios pendientes si se ha interrumpido un proceso de edición y determinar si necesita usar el método **Update** o **CancelUpdate** .  
  
 **EditMode** devuelve una de las constantes de **EditModeEnum** , que se enumeran en la tabla siguiente.  
  
|Constante|Descripción|  
|--------------|-----------------|  
|**adEditNone**|Indica que no hay ninguna operación de edición en curso.|  
|**adEditInProgress**|Indica que los datos del registro actual se han modificado pero no se han guardado.|  
|**adEditAdd**|Indica que se ha llamado al método **AddNew** y que el registro actual del búfer de copia es un registro nuevo que no se ha guardado en la base de datos.|  
|**adEditDelete**|Indica que se ha eliminado el registro actual.|  
  
 **EditMode** solo puede devolver un valor válido si hay un registro actual. **EditMode** devolverá un error si **BOF** o **EOF** es **true** o si se ha eliminado el registro actual.

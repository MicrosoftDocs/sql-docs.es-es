---
description: Editar los registros existentes
title: Editando registros existentes | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], existing records
ms.assetid: 17ce1263-5897-452a-9ea5-c7f96b33df65
author: rothja
ms.author: jroth
ms.openlocfilehash: 031d819e2c8eed63b9b8ff42aa58fb8e998a4273
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037505"
---
# <a name="editing-existing-records"></a>Editar los registros existentes
Para editar los registros existentes, desplácese a la fila que desea editar y cambie la propiedad **valor** de los campos que desee cambiar. Para obtener más información sobre la propiedad **Value** del objeto de **campo** , vea [examinar datos](./examining-data.md). Dependiendo del tipo de cursor, utilizará **Update** o **UpdateBatch** para devolver los cambios al origen de datos. Para obtener más información, vea [actualizar y conservar datos](./updating-and-persisting-data.md).  
  
 Normalmente es más eficaz usar un procedimiento almacenado con un objeto de comando para realizar actualizaciones, así como para realizar otras operaciones, porque un procedimiento almacenado no requiere la creación de un cursor. Para obtener más información sobre los cursores, vea Descripción de los [cursores y bloqueos](./understanding-cursors-and-locks.md).
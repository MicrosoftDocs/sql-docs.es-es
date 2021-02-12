---
description: Guardar metadatos (MySQLToSQL)
title: Guardar metadatos (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9bc6273f-e8b1-430b-81a5-14330a783562
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bc5f3e07439f79fd5746fc564f2b7c9fb87e69c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074526"
---
# <a name="save-metadata--mysqltosql"></a>Guardar metadatos (MySQLToSQL)
El cuadro de diálogo **guardar metadatos** le solicita que cargue los metadatos en el proyecto de SSMA antes de guardarlo. Esto le permite tener un archivo de proyecto completo que puede usar sin conexión y enviar a otras personas, como personal de soporte técnico.  
  
Para tener acceso al cuadro de diálogo **guardar metadatos** , guarde el proyecto. Si faltan metadatos, SSMA mostrará el cuadro de diálogo **guardar metadatos** .  
  
## <a name="options"></a>Opciones  
**Nombre**  
El nombre de cada base de datos del proyecto.  
  
**Estado**  
Indica si los metadatos se cargan en el proyecto de SSMA o si faltan metadatos.  
  
SSMA carga los metadatos en el proyecto según sea necesario. Los metadatos se cargan automáticamente al examinar los metadatos y convertir los esquemas.  
  
**Seleccionar todo**  
Selecciona todas las bases de datos de la lista.  
  
**Borrar**  
Desactiva la casilla para todas las bases de datos con metadatos que faltan. No puede desactivar la casilla si se han cargado los metadatos.  
  
**Guardar**  
Guarda el proyecto y carga los metadatos de las bases de datos seleccionadas que tienen metadatos que faltan.  
  
**Cancelar**  
Cancela la operación de guardar. Los metadatos que faltan no se cargan en el proyecto.  
  

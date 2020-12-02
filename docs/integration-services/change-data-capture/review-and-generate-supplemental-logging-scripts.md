---
description: Revisar y generar scripts de registro complementario
title: Revisar y generar scripts de registro complementario | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- scripts
ms.assetid: 5c858ae2-37d6-42e8-a252-7f6ed4e628a7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 77a6b4f11ea2e01f43f1be335fad893ee87f1e90
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88457691"
---
# <a name="review-and-generate-supplemental-logging-scripts"></a>Revisar y generar scripts de registro complementario

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Use la pestaña **Scripts** para ejecutar o volver a ejecutar un script en la base de datos de origen de Oracle que configura el registro complementario.  
  
 Antes de ejecutar los scripts, seleccione una de las opciones siguientes:  
  
 **Incluir cambios en esta sesión**  
 Seleccione esta opción para ejecutar el script en cualquier tabla nueva que se agregó a la instancia CDC o en tablas que se modificaron de cualquier forma mediante el editor de propiedades.  
  
> [!NOTE]  
>  Si no se ha realizado ningún cambio en las tablas de la instancia CDC, el área de script de registro complementario de Oracle estará vacío.  
  
 **Incluir todas las tablas e instancias de captura**  
 Seleccione esta opción para ejecutar el script en todas las tablas e instancias de captura de la instancia CDC.  
  
 Después de seleccionar una de estas opciones, ejecute el script de registro complementario.  
  
### <a name="to-run-the-supplemental-logging-scripts"></a>Para ejecutar los scripts de registro complementario  
  
1.  Haga clic en **Ejecutar script** para ejecutar el script de registro complementario en las tablas definidas para la instancia CDC. Este script indica a la base de datos de Oracle que escriba todas las columnas de interés en sus registros de transacciones al registrar operaciones UPDATE en tablas capturadas. Normalmente es un administrador del sistema de Oracle quien examina y ejecuta este script.  
  
2.  Al ejecutar los scripts de registro complementario, se abre el cuadro de diálogo Credenciales de Oracle para ejecutar script donde debe proporcionar un nombre de usuario y una contraseña válidos de Oracle. Para obtener información acerca de cómo proporcionar las credenciales adecuadas de Oracle, vea [Oracle Credentials for Running Script](../../integration-services/change-data-capture/oracle-credentials-for-running-script.md).  
  
 También puede ejecutar los scripts manualmente mediante SQL\*Plus, si es necesario.  
  
### <a name="to-run-the-scripts-manually"></a>Para ejecutar los scripts manualmente  
  
1.  Haga clic en **Copiar** para pegar el script en el Portapapeles. Abra SQL* Plus y vaya al directorio donde se encuentra la base de datos de origen de Oracle. Pegue el script en SQL\*Plus para ejecutarlo.  
  
### <a name="to-save-the-supplemental-logging-script-in-a-text-file"></a>Para guardar el script de registro complementario en un archivo de texto  
  
1.  Haga clic en **Guardar como** y vaya hasta la ubicación donde desee guardar el archivo.  
  
2.  Asigne un nombre al archivo y haga clic en **Guardar** para guardar el archivo.  
  
## <a name="see-also"></a>Vea también  
 [Cómo editar las propiedades de la instancia CDC](../../integration-services/change-data-capture/how-to-edit-the-cdc-instance-properties.md)   
 [Credenciales de Oracle para ejecutar script](../../integration-services/change-data-capture/oracle-credentials-for-running-script.md)  
  
  

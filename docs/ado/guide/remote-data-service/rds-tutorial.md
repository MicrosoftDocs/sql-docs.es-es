---
description: Tutorial de RDS
title: Tutorial de RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO]
ms.assetid: 6e3305a0-7bc7-40d1-9122-235c15d23ab2
author: rothja
ms.author: jroth
ms.openlocfilehash: b2b9294b3024a3d4a7dbfd51baadb2474b7715ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036415"
---
# <a name="rds-tutorial"></a>Tutorial de RDS
En este tutorial se muestra el uso del modelo de programación RDS para consultar y actualizar un origen de datos. En primer lugar, se describen los pasos necesarios para realizar esta tarea. A continuación, el tutorial se repite en Microsoft® Visual Basic Scripting Edition (que incluye ADO para Windows Foundation Classes (ADO/WFC)).  
  
 Este tutorial se codifica en distintos idiomas por dos motivos:  
  
-   La documentación de RDS presupone los códigos de lector en Visual Basic. Esto hace que la documentación sea cómoda para los programadores de Visual Basic, pero menos útil para los programadores que usan otros lenguajes.  
  
-   Si no está seguro de una característica concreta de RDS y sabe un poco de otro lenguaje, es posible que pueda resolver la pregunta buscando la misma característica expresada en otro lenguaje.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="how-the-tutorial-is-presented"></a>Cómo se presenta el tutorial  
 Este tutorial se basa en el modelo de programación de RDS. Describe cada paso del modelo de programación individualmente. Además, ilustra cada paso con un fragmento de código Visual Basic.  
  
 El ejemplo de código se repite en otros idiomas con una explicación mínima. Cada paso de un tutorial de lenguaje de programación determinado se marca con el paso correspondiente en el modelo de programación y el tutorial descriptivo. Use el número del paso para hacer referencia a la discusión en el tutorial descriptivo.  
  
 El modelo de programación RDS se indica en la sección siguiente. Úsela como guía a lo largo del tutorial.  
  
## <a name="rds-programming-model-with-objects"></a>Modelo de programación de RDS con objetos  
  
-   Especifique el programa que se va a invocar en el servidor y obtenga una forma (proxy) para hacer referencia a él desde el cliente.  
  
-   Invoque el programa de servidor. Pase los parámetros al programa de servidor que identifica el origen de datos y el comando que se va a emitir.  
  
-   El programa de servidor obtiene un objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) del origen de datos, normalmente mediante ADO. Opcionalmente, el objeto de **conjunto de registros** se procesa en el servidor.  
  
-   El programa de servidor devuelve el último objeto de **conjunto de registros** a la aplicación cliente.  
  
-   En el cliente, el objeto de **conjunto de registros** se coloca opcionalmente en un formulario que se puede usar fácilmente en los controles visuales.  
  
-   Los cambios en el objeto de **conjunto de registros** se devuelven al servidor y se usan para actualizar el origen de datos.  
  
 Este tutorial contiene los siguientes temas.  
  
-   [Paso 1: Especificación de un programa de servidor (Tutorial de RDS)](./step-1-specify-a-server-program-rds-tutorial.md)  
  
-   [Paso 2: Invocación de un programa de servidor (Tutorial de RDS)](./step-2-invoke-the-server-program-rds-tutorial.md)  
  
-   [Paso 3: Obtención de un conjunto de registros por parte del servidor (Tutorial de RDS)](./step-3-server-obtains-a-recordset-rds-tutorial.md)  
  
-   [Paso 4: Devolución del conjunto de registros por parte del servidor (Tutorial de RDS)](./step-4-server-returns-the-recordset-rds-tutorial.md)  
  
-   [Paso 5: Habilitación del uso del control de datos (Tutorial de RDS)](./step-5-datacontrol-is-made-usable-rds-tutorial.md)  
  
-   [Paso 6: Envío de los cambios al servidor (Tutorial de RDS)](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)  
  
-   [Tutorial de RDS (VBScript)](./rds-tutorial-vbscript.md)  
  
## <a name="see-also"></a>Consulte también  
 [Paso 1: especificar un programa de servidor (tutorial de RDS)](./step-1-specify-a-server-program-rds-tutorial.md)   
 [Tutorial de RDS (VBScript)](./rds-tutorial-vbscript.md)
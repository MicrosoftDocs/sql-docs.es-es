---
description: Soluciones de acceso a datos remotos
title: Soluciones para el acceso a datos remotos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS [ADO]
ms.assetid: d311cc67-7db7-4c43-9590-d465564695e4
author: rothja
ms.author: jroth
ms.openlocfilehash: 01516d04ce20fc1590ca41985078bcd47f47e47e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031777"
---
# <a name="solutions-for-remote-data-access"></a>Soluciones de acceso a datos remotos
## <a name="the-issue"></a>El problema  
 ADO permite a la aplicación obtener acceso directamente a los orígenes de datos y modificarlos (a veces denominado sistema de dos niveles). Por ejemplo, si la conexión es con el origen de datos que contiene los datos, es una conexión directa en un sistema de dos niveles.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Sin embargo, puede que desee tener acceso indirectamente a los orígenes de datos a través de un intermediario como Microsoft® Internet Information Services (IIS). Esta disposición a veces se denomina sistema de tres niveles. IIS es un sistema cliente/servidor que proporciona una manera eficaz de que una aplicación local o de cliente invoque un programa remoto, o servidor, a través de Internet o de una intranet. El programa de servidor obtiene acceso al origen de datos y, opcionalmente, procesa los datos adquiridos.  
  
 Por ejemplo, la Página Web de la intranet contiene una aplicación escrita en Microsoft® Visual Basic Scripting Edition (VBScript), que se conecta a IIS. IIS a su vez se conecta al origen de datos real, recupera los datos, los procesa de algún modo y, a continuación, devuelve la información procesada a la aplicación.  
  
 En este ejemplo, la aplicación nunca se conecta directamente al origen de datos; IIS. Y IIS obtienen acceso a los datos por medio de ADO.  
  
> [!NOTE]
>  No es necesario que la aplicación cliente/servidor se base en Internet o en una intranet (es decir, basada en Web); puede constar únicamente de programas compilados en una red de área local. Sin embargo, el caso típico es una aplicación basada en Web.  
  
 Dado que un control visual, como una cuadrícula, una casilla o una lista, puede usar la información devuelta, el control visual debe usar fácilmente la información devuelta.  
  
 Desea una interfaz de programación de aplicaciones sencilla y eficaz que admita sistemas de tres niveles, y devuelve información tan fácilmente como si se hubiera recuperado en un sistema de dos niveles. El servicio de datos remotos (RDS) es esta interfaz.  
  
## <a name="the-solution"></a>Solución  
 RDS define un modelo de programación: la secuencia de actividades necesarias para obtener acceso y actualizar un origen de datos, para obtener acceso a los datos a través de un intermediario, como Internet Information Services (IIS). El modelo de programación resume toda la funcionalidad de RDS.  
  
## <a name="see-also"></a>Consulte también  
 [Modelo de programación básica de RDS](./basic-rds-programming-model.md)   
 [Escenario de RDS](./rds-scenario.md)   
 [Tutorial de RDS](./rds-tutorial.md)   
 [Seguridad y uso de RDS](./rds-usage-and-security.md)
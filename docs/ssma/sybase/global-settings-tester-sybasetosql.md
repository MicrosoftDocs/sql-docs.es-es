---
description: Configuración global (evaluador) (SybaseToSQL)
title: Configuración global (evaluador) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 6f0b9cea-5a24-4e42-8bbf-c4516b00da23
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8f4584f8b4cb4cd7c8117a33e2894ad3f6382335
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078736"
---
# <a name="global-settings-tester-sybasetosql"></a>Configuración global (evaluador) (SybaseToSQL)
Use la página evaluador del cuadro de diálogo **configuración global** para especificar la configuración de SSMA Tester.  
  
Para obtener acceso a la configuración del evaluador, en el menú **herramientas** , seleccione **configuración global** y haga clic en **evaluador** en la parte inferior del panel izquierdo.  
  
## <a name="options"></a>Opciones  
**Análisis de objetos comprobables**  
Esta configuración especifica si se debe realizar el análisis de los objetos que se van a probar. Seleccione **sí** si desea que el evaluador de SSMA analice y compruebe automáticamente los objetos dependientes. El conjunto de opciones predeterminado es **sí**.  
  
Las siguientes opciones están disponibles para esta configuración:  
  
1.  Sí  
  
2.  No  
  
**Modo de guardado de tablas auxiliares**  
Esta configuración especifica cómo se guardan las tablas auxiliares internas creadas durante la ejecución del caso de prueba. Se pueden establecer las siguientes opciones para esta configuración determinada:  
  
1.  Eliminar siempre  
  
2.  Guardar siempre  
  
3.  Guardar si se produjo un error en la comparación de tabla  
  
4.  Preguntar al usuario si se produjo un error en la comparación de tabla  
  
El conjunto de opciones predeterminado es: **eliminar siempre**.  
  
**Realizar la reversión de los datos**  
Esta configuración especifica si se debe realizar una operación de reversión después de ejecutar cada caso de prueba. El conjunto de opciones predeterminado es **no**.  
  
Las siguientes opciones están disponibles para esta configuración:  
  
1.  Sí  
  
2.  No  
  
**Detener la ejecución de pruebas después del primer error**  
Esta configuración especifica si se debe detener el caso de prueba actual en ejecución, si se ha producido un error durante la ejecución. El conjunto de opciones predeterminado es **sí**.  
  
Las siguientes opciones están disponibles para esta configuración:  
  
1.  Sí  
  
2.  No  
  
## <a name="see-also"></a>Vea también  
[Finalizando la preparación del caso de prueba &#40;SybaseToSQL&#41;](../../ssma/sybase/finishing-test-case-preparation-sybasetosql.md)  
  

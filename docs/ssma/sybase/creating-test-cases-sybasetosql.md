---
description: Creación de casos de prueba (SybaseToSQL)
title: Crear casos de prueba (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester component,Test Case Wizard
ms.assetid: b52dfd93-95af-4299-bc8f-83f2a7a6a518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 460ea84af51b872841eb82cd405cc29ebfe59e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056690"
---
# <a name="creating-test-cases-sybasetosql"></a>Creación de casos de prueba (SybaseToSQL)
Use el Asistente para casos de prueba para crear una prueba. Este asistente le permite crear casos de prueba eligiendo objetos comprobados y comprobados y especificando los parámetros de prueba.  
  
## <a name="starting-the-test-case-wizard"></a>Iniciar el Asistente para casos de prueba  
Para iniciar el Asistente para casos de prueba, haga clic en **nuevo caso de prueba...** en el menú de **evaluador** .  
  
Cuando se inicia, el asistente busca la base de datos ssmatester2005db o ssmatester2008db (según el tipo de proyecto) en el servidor de Sybase de origen. Es el esquema de la extensión de evaluador que se usa para almacenar objetos auxiliares. Si el Asistente para casos de prueba no encuentra ssmatester2005db o ssmatester2008db, muestra una ventana de cuadro de diálogo que propone crear la base de datos de la extensión de evaluador. (Normalmente, esta situación se produce durante la primera ejecución de la herramienta SSMA Tester).  
  
Si obtiene la ventana de diálogo, haga clic en **sí** para crear la base de datos de prueba de Sybase en el servidor de origen. A continuación, aparecerá una nueva ventana de diálogo en la que debe agregar uno o varios dispositivos en los que ubicar la nueva base de datos de prueba. Haga clic en **Agregar** para agregar un dispositivo. En el cuadro de diálogo **espacio de asignación para base de datos de prueba** , elija el dispositivo y especifique el tamaño usado por la base de datos de prueba. Además, puede establecer el dispositivo independiente para los registros de base de datos. Tenga en cuenta que debe tener privilegios de Sybase para crear bases de datos.  
  
## <a name="overview-of-creating-test-cases-using-the-wizard"></a>Información general sobre la creación de casos de prueba mediante el asistente  
El proceso de creación de un caso de prueba consta de cinco pasos:  
  
1.  [Inicializar casos de prueba &#40;SybaseToSQL&#41;](../../ssma/sybase/initializing-test-cases-sybasetosql.md).  
  
2.  [Selección y configuración de objetos para probar &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md).  
  
3.  [Seleccionar y configurar objetos afectados &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md).  
  
4.  [Personalizar el orden de las llamadas &#40;SybaseToSQL&#41;](../../ssma/sybase/customizing-calls-order-sybasetosql.md).  
  
5.  [Finalizando la preparación del caso de prueba &#40;SybaseToSQL&#41;](../../ssma/sybase/finishing-test-case-preparation-sybasetosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Probar objetos de base de datos migrados &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  

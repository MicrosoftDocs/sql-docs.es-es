---
description: Conceptos básicos de ADO
title: Aspectos básicos de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: d6a66928-e68f-4c38-b87a-838c5de50a28
author: rothja
ms.author: jroth
ms.openlocfilehash: fdf7580f3a04a15fd75fb6d2c7712a694dfd2175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033765"
---
# <a name="ado-fundamentals"></a>Conceptos básicos de ADO
ADO ofrece a los desarrolladores un modelo de objetos lógico y eficaz para obtener acceso, editar y actualizar datos mediante programación desde una amplia variedad de orígenes de datos a través de interfaces del sistema OLE DB. El uso más común de ADO es consultar una tabla o tablas en una base de datos relacional, recuperar y mostrar los resultados en una aplicación y, quizás, permitir que los usuarios realicen y guarden los cambios en los datos. Entre otras tareas se incluyen las siguientes:  
  
-   Consultar una base de datos mediante SQL y mostrar los resultados.  
  
-   Obtener acceso a la información de un almacén de archivos a través de Internet.  
  
-   Manipular mensajes y carpetas en un sistema de correo electrónico.  
  
-   Guardar datos de una base de datos en un archivo XML.  
  
-   Ejecutar comandos descritos con XML y recuperar una secuencia XML.  
  
-   Guardar datos en una secuencia binaria o XML.  
  
-   Permitir a los usuarios revisar y cambiar los datos de las tablas de base de datos.  
  
-   Crear y volver a usar comandos de base de datos parametrizados.  
  
-   Ejecutar procedimientos almacenados.  
  
-   Crear dinámicamente una estructura flexible, denominada **conjunto de registros**, para contener, navegar y manipular los datos.  
  
-   Realización de operaciones de base de datos transaccionales.  
  
-   Filtrar y ordenar copias locales de información de base de datos en función de los criterios de tiempo de ejecución.  
  
-   Crear y manipular los resultados jerárquicos de las bases de datos.  
  
-   Enlazar campos de base de datos a componentes con datos.  
  
-   Crear **conjuntos de registros** desconectados remotos.  
  
 ADO expone una amplia variedad de opciones y configuraciones para proporcionar esta flexibilidad. Por lo tanto, es importante adoptar un enfoque metódico para aprender a usar ADO en una aplicación, desglosando cada uno de sus objetivos en piezas administrables.  
  
 Hay cuatro operaciones principales implicadas en la mayoría de las aplicaciones que usan ADO: obtener datos, examinar datos, editar datos y actualizar datos. Estas operaciones se examinan con más detalle más adelante en esta sección.  
  
 Sin embargo, antes de describir estos detalles, presentaremos información general sobre el modelo de objetos ADO y una sencilla aplicación ADO, que está escrita en Microsoft® Visual Basic® y realiza cada una de las cuatro operaciones principales de ADO:  
  
-   [Colecciones y los objetos ADO](./ado-objects-and-collections.md)  
  
-   [HelloData: una aplicación ADO sencilla](./hellodata-a-simple-ado-application.md)  
  
-   [Proveedores de OLE DB](./ole-db-providers-ado.md)  
  
-   [Errores](./errors-ado.md)
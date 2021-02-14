---
title: Cuadro de diálogo Agregar referencia de base de datos
description: Obtenga información sobre las referencias a bases de datos y cómo usarlas. Vea los procedimientos que puede realizar en el cuadro de diálogo Agregar referencia de base de datos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.adddatabasereference.dialog
- sql.data.tools.newdatabase.dialog
- sql.data.tools.criticalerror.dialog
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: feb7df193ea54f4c7184066bb38cc5cc700f9bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079666"
---
# <a name="add-database-reference-dialog-box"></a>Cuadro de diálogo Agregar referencia de base de datos

En este tema se describen los procedimientos que puede realizar en el cuadro de diálogo **Agregar referencia de base de datos**.  
  
Las referencias de base de datos le permiten:  
  
Obtener acceso a objetos en otras bases de datos.  
Un proyecto puede hacer referencia a otra base de datos de cualquier servidor con una resolución de nombres de tres o cuatro partes. Al utilizar un nombre de tres o cuatro partes para una referencia, puede usar variables SQLCMD para permitir que las referencias trabajen en diferentes servidores y bases de datos.  
  
Crear una solución compuesta de varios proyectos de base de datos.  
En un proyecto compuesto, las referencias de base de datos dividen una gran base de datos en varios proyectos independientes. Puede crear una referencia que no contenga variables o valores para la base de datos o el servidor (solo usando nombres de una o dos partes).  
  
Las referencias de base de datos se pueden realizar en un proyecto de base de datos de la solución actual o un DACPAC. Si se agrega una referencia de base de datos a un proyecto se modifican las dependencias y el orden de compilación de dicho proyecto.  
  
## <a name="selecting-the-database-to-reference"></a>Seleccionar la base de datos a la que se va a hacer referencia

Puede hacer referencia a otro proyecto de base de datos en la misma solución, una base de datos del sistema o una base de datos en un DACPAC.  
  
Si hay más de un proyecto de base de datos en la solución, se habilita **Proyectos de base de datos en la solución actual**. Puede hacer referencia a otra base de datos en la solución.  
  
Seleccione **Base de datos del sistema** si va a seleccionar una de las bases de datos del sistema como referencia de base de datos.  
  
Seleccione **Aplicación de capa de datos (archivo .dacpac)** para hacer referencia a una base de datos en un DACPAC y examine el directorio con el archivo DACPAC.  
  
## <a name="selecting-the-databases-relative-location"></a>Seleccionar la ubicación relativa de la base de datos

Una vez seleccionada la base de datos a la que desea hacer referencia, puede especificar la ubicación esperada de un objeto de base de datos, relativa al proyecto al que se hace referencia.  
  
Las referencias se pueden resolver para objetos en una de las siguientes ubicaciones:  
  
- En la base de datos de referencia.  
  
- En una base de datos diferente de la base de datos de referencia, pero en el mismo servidor.  
  
- En una base de datos diferente de la base de datos de referencia, pero en otro servidor.  
  
Especifique un nombre de base de datos. Si selecciona **Base de datos del sistema**, no debe modificar el literal de la base de datos de sistema. Si selecciona **Proyectos de base de datos en la solución actual**, el nombre predeterminado de la base de datos se basa en el nombre de la base de datos del proyecto.  
  
Si seleccionó **Base de datos diferente, servidor diferente**, especifique un nombre de servidor.  
  
Puede utilizar una variable de base de datos (SQLCMD). Si desea hacer referencia a la base de datos con una variable, en lugar del nombre literal, acepte o modifique el nombre de variable de base de datos predeterminado. Una variable de base de datos es útil si publica el proyecto de base de datos desde varios servidores y bases de datos. En esta situación, un desarrollador puede ir a **Variables SQLCMD** en las páginas de propiedad del proyecto y especificar el nombre local de la base de datos. Si deja **Variable de la base de datos** en blanco, solo puede hacer referencia a la base de datos por su nombre literal. Si especifica un nombre de variable de base de datos, no puede hacer referencia a la base de datos por su nombre literal.  
  
Si seleccionó **Base de datos diferente, servidor diferente**, se requiere una variable de servidor (SQLCMD). Una variable de servidor es útil si publica el proyecto de base de datos desde varios servidores y bases de datos. En esta situación, un desarrollador puede ir a **Variables SQLCMD** en las páginas de propiedad del proyecto y especificar el nombre local del servidor. Solo puede hacer referencia al servidor con la variable y no con el nombre del servidor.  
  
> [!IMPORTANT]  
> En algunas situaciones, puede crear una referencia de base de datos que tenga el mismo nombre que una referencia de base de datos existente. Dos referencias de base de datos con el mismo nombre pueden dar lugar a un comportamiento inesperado. En esta situación, elimine ambas referencias de base de datos.  
  
## <a name="common-procedures"></a>Procedimientos habituales

Lo siguiente son procedimientos habituales:  
  
### <a name="to-create-a-reference-to-a-database-on-the-same-server"></a>Para crear una referencia a una base de datos del mismo servidor  
  
1.  En el Explorador de soluciones, haga clic con el botón derecho en **Referencias** y seleccione **Agregar referencia de base de datos**.  
  
2.  En el cuadro de diálogo **Agregar referencia de base de datos**, especifique la **Ubicación de la base de datos** como **Base de datos diferente, mismo servidor**.  
  
3.  Especifique el nombre de la base de datos.  
  
4.  Decida si desea utilizar una variable de base de datos.  
  
5.  Copie el ejemplo de uso y péguelo en el archivo .sql. En el ejemplo de uso, cambie [Esquema1] por el nombre del esquema (por ejemplo, [dbo]). Asimismo, modifique el nombre del objeto de base de datos de la manera apropiada para su proyecto.  
  
6.  Compile la solución.  
  
### <a name="to-create-a-reference-to-a-database-on-another-server"></a>Para crear una referencia a una base de datos en otro servidor  
  
1.  En el Explorador de soluciones, haga clic con el botón derecho en **Referencias** y seleccione **Agregar referencia de base de datos**.  
  
2.  En el cuadro de diálogo **Agregar referencia de base de datos**, especifique la **Ubicación de la base de datos** como **Base de datos diferente, servidor diferente**.  
  
3.  Asegúrese de que el nombre de la base de datos es correcto.  
  
4.  Decida si desea utilizar una variable de base de datos.  
  
5.  Especifique el nombre del servidor y la variable de servidor.  
  
6.  Copie el ejemplo de uso y péguelo en el archivo .sql. En el ejemplo de uso, cambie [Esquema1] por el nombre del esquema (por ejemplo, [dbo]). Asimismo, modifique el nombre del objeto de base de datos de la manera apropiada para su proyecto.  
  
7.  Compile la solución.  
  
### <a name="to-create-a-composite-project"></a>Para crear un proyecto compuesto  
  
1.  En el Explorador de soluciones, haga clic con el botón derecho en **Referencias** y seleccione **Agregar referencia de base de datos**.  
  
2.  Seleccione el origen de la base de datos a la que va a hacer referencia (un proyecto en la solución o un DACPAC).  
  
3.  En el cuadro de diálogo **Agregar referencia de base de datos**, especifique la **Ubicación de la base de datos** como **Misma base de datos**.  
  
4.  Copie el ejemplo de uso y péguelo en el archivo .sql. En el ejemplo de uso, cambie [Esquema1] por el nombre del esquema. Asimismo, modifique el nombre del objeto de base de datos de la manera apropiada para su proyecto.  
  
5.  Compile la solución.  
  
Cuando publique este proyecto, puede implementar proyectos compuestos en la misma solución en un único destino:  
  
1.  Haga clic con el botón derecho en el nombre de proyecto **Explorador de soluciones** y seleccione **Publicar** para mostrar el cuadro de diálogo **Publicar base de datos**.  
  
2.  En el cuadro de diálogo **Publicar base de datos**, haga clic en **Opciones avanzadas**.  
  
3.  En el cuadro de diálogo **Configuración de publicación avanzada**, asegúrese de que selecciona **Incluir objetos compuestos** en la lista **Opciones de implementación avanzadas**.  
  
## <a name="see-also"></a>Consulte también

[Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md)
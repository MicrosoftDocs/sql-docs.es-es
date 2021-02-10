---
description: Usar ADO con Microsoft Visual Basic y Visual Basic para Aplicaciones
title: Usar ADO con Microsoft Visual Basic | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, Visual Basic
- Visual Basic [ADO]
ms.assetid: 9dfb6784-037d-4f9d-bb7f-b506b4498573
author: rothja
ms.author: jroth
ms.openlocfilehash: 229662bd5db68039202f1f39d7309c12ff6354a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028865"
---
# <a name="using-ado-with-microsoft-visual-basic-and-visual-basic-for-applications"></a>Usar ADO con Microsoft Visual Basic y Visual Basic para Aplicaciones
La configuración de un proyecto de ADO y la escritura de código ADO es similar si se utiliza Visual Basic o Visual Basic para Aplicaciones. En este tema se trata el uso de ADO con Visual Basic y Visual Basic para Aplicaciones y se indica cualquier diferencia.

## <a name="referencing-the-ado-library"></a>Referencia a la biblioteca de ADO
 El proyecto debe hacer referencia a la biblioteca de ADO.

#### <a name="to-reference-ado-from-microsoft-visual-basic"></a>Para hacer referencia a ADO desde Microsoft Visual Basic

1.  En Visual Basic, en el menú **proyecto** , seleccione **referencias..**..

2.  Seleccione **Microsoft objetos de datos ActiveX biblioteca x. x** de la lista. Compruebe que también están seleccionadas al menos las siguientes bibliotecas:

    -   Visual Basic para Aplicaciones

    -   Objetos y procedimientos en tiempo de ejecución de Visual Basic

    -   Visual Basic objetos y procedimientos

    -   OLE Automation

3.  Haga clic en **OK**.

 Puede usar ADO con la misma facilidad con Visual Basic para Aplicaciones, por ejemplo, con Microsoft Access.

#### <a name="to-reference-ado-from-microsoft-access"></a>Para hacer referencia a ADO desde Microsoft Access

1.  En Microsoft Access, seleccione o cree un módulo en la pestaña **módulos** de la ventana **base de datos** .

2.  En el menú **herramientas** , seleccione **referencias.**...

3.  Seleccione **Microsoft objetos de datos ActiveX biblioteca x. x** de la lista. Compruebe que también están seleccionadas al menos las siguientes bibliotecas:

    -   Visual Basic para Aplicaciones

    -   Biblioteca de objetos de Microsoft Access 8,0 (o posterior)

    -   Biblioteca de objetos de Microsoft DAO 3,5 (o posterior)

4.  Haga clic en **OK**.

## <a name="creating-ado-objects-in-visual-basic"></a>Crear objetos ADO en Visual Basic
 Para crear una variable de Automation y una instancia de un objeto para esa variable, puede usar dos métodos: **Dim** o **CreateObject**.

### <a name="dim"></a>Dim
 Puede usar la palabra clave **New** con **Dim** para declarar y crear instancias de objetos de ADO en un solo paso:

```
Dim conn As New ADODB.Connection
```

 Como alternativa, la declaración de la instrucción **Dim** y la creación de instancias de objeto también pueden ser dos pasos:

```
Dim conn As ADODB.Connection
Set conn = New ADODB.Connection
```

> [!NOTE]
>  No es necesario usar explícitamente el `ADODB` ProgID con la instrucción **Dim** , suponiendo que se haya hecho referencia correctamente a la biblioteca de ADO en el proyecto. Sin embargo, el uso de ti garantiza que no se producirán conflictos de nomenclatura con otras bibliotecas.

> [!NOTE]
>  Por ejemplo, si incluye referencias a ADO y a DAO en el mismo proyecto, debe incluir un calificador para especificar el modelo de objetos que se va a utilizar al crear instancias de objetos de **conjunto de registros** , como en el código siguiente:

```
Dim adoRS As ADODB.Recordset
Dim daoRS As DAO.Recordset
```

### <a name="createobject"></a>CreateObject
 Con el método **CreateObject** , la declaración y la creación de instancias de objeto deben ser dos pasos discretos:

```
Dim conn1
Set conn1 = CreateObject("ADODB.Connection") As Object
```

 Los objetos con instancias de **CreateObject** están enlazados en tiempo de ejecución, lo que significa que no tienen establecimiento inflexible de tipos y la finalización de la línea de comandos está deshabilitada. Sin embargo, permite omitir la referencia a la biblioteca de ADO del proyecto y permite crear instancias de versiones específicas de los objetos. Por ejemplo:

```
Set conn1 = CreateObject("ADODB.Connection.2.0") As Object
```

 También puede lograr esto mediante la creación específica de una referencia a la biblioteca de tipos de ADO versión 2,0 y la creación del objeto.

 La creación de instancias de objetos mediante el método **CreateObject** suele ser más lenta que la utilización de la instrucción **Dim** .

## <a name="handling-events"></a>Controlar eventos
 Para controlar los eventos de ADO en Microsoft Visual Basic, debe declarar una variable de nivel de módulo mediante la palabra clave **WithEvents** . La variable solo se puede declarar como parte de un módulo de clase y se debe declarar en el nivel de módulo. Para obtener una explicación más detallada sobre el control de eventos de ADO, vea [controlar eventos de ADO](../data/handling-ado-events.md).

## <a name="visual-basic-examples"></a>Ejemplos de Visual Basic
 En la documentación de ADO se incluyen muchos ejemplos de Visual Basic. Para obtener más información, vea [ejemplos de código ADO en Microsoft Visual Basic](../../reference/ado-api/ado-code-examples-in-visual-basic.md).

## <a name="see-also"></a>Consulte también
 [Microsoft objetos de datos ActiveX (ADO)](../../microsoft-activex-data-objects-ado.md) [usar ado con Microsoft Visual C++](./using-ado-with-microsoft-visual-c.md) [usar ado con lenguajes de scripting](./using-ado-with-scripting-languages.md)
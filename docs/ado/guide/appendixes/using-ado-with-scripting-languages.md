---
description: Usar ADO con lenguajes de Scripting
title: Usar ADO con lenguajes de scripting | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- scripting languages [ADO]
- ADO, scripting languages
ms.assetid: 76fc4d00-0c9f-422b-af5c-af6ed8fb29d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f1919f3524f05035a4376f30cf2e36ae731bdea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028860"
---
# <a name="using-ado-with-scripting-languages"></a>Usar ADO con lenguajes de Scripting
En un entorno de scripting, ADO permite exponer datos por medio de scripting del lado servidor. En este escenario, ADO, el proveedor de OLE DB subyacente que utiliza y cualquier otro componente necesario para hacer referencia a un almacén de datos determinado se instalan en un servidor que ejecuta Internet Information Services (IIS). Con Active Server páginas (ASP), ADO es un componente al que se hace referencia en un script que puede generar HTML, por ejemplo. Este contenido HTML se puede pasar a través de HTTP a un explorador Web del cliente. Mediante el uso de scripting, la página web puede enviar acciones de vuelta al script del lado servidor, lo que le permite actualizar, recorrer o ver datos específicos.  
  
 Antes de utilizar un objeto ActiveX en una página web, es importante saber si el objeto es seguro para el scripting. Cuando un objeto se considera seguro para el scripting, significa que el control no puede realizar ninguna acción dañina en el equipo del usuario y, por tanto, se puede ejecutar sin solicitar la aprobación del usuario. En la tabla siguiente se enumeran los objetos ADO y se indica si son seguros para el scripting.  
  
|Object|¿Seguro para scripting?|  
|------------|-------------------------|  
|Conexión ADO|Sí|  
|Comando de ADO|No|  
|Parámetro de ADO|No|  
|Conjunto de registros ADO|Sí|  
|Registro de ADO|Sí|  
|Secuencia de ADO|Sí|  
|Error de ADO.|No|  
|Catálogo de ADOX|No|  
|CellSet (CellSet)|No|  
|RDS (control)|Sí|  
|Espacio de los de RDS|Sí|  
|DataFactory de RDS|No|  
  
 En la tabla siguiente se enumeran los proveedores incluidos con Windows DAC/MDAC y se indica si son seguros para el scripting.  
  
|Proveedor|¿Seguro para scripting?|  
|--------------|-------------------------|  
|Forma|Sí|  
|Persist|Sí|  
|Control remoto|Sí|  
|Proveedor de OLE DB para SQL Server (SQLOLEDB)|No|  
|Proveedor de OLE DB para ODBC (MSDASQL)|No|  
  
## <a name="odbc-data-sources"></a>Orígenes de datos ODBC  
 Una diferencia importante entre el scripting y el código ADO sin script es el origen de datos ODBC, si se usa. En el caso de las aplicaciones que no son de scripting, puede crear un DSN de usuario en el administrador de orígenes de datos ODBC. En el caso de los scripts que se ejecutan en IIS, debe crear un DSN de sistema. en caso contrario, los scripts no reconocerán el origen de datos que ha creado. Esto se aplica a cualquier aplicación de scripting de ADO que use el proveedor de Microsoft OLE DB para ODBC a través de Microsoft IIS.  
  
## <a name="referencing-the-ado-library"></a>Referencia a la biblioteca de ADO  
 No es aplicable con lenguajes de scripting.  
  
## <a name="handling-events"></a>Control de eventos  
 No es aplicable con lenguajes de scripting.  
  
 Los temas siguientes contienen información más específica sobre el uso de ADO con lenguajes de scripting:  
  
-   [Programación ADO en VBScript](./vbscript-ado-programming.md)  
  
-   [Programación ADO con JScript](./jscript-ado-programming.md)  
  
## <a name="see-also"></a>Consulte también  
 [Microsoft Objetos de datos ActiveX (ADO)](../../microsoft-activex-data-objects-ado.md)   
 [Usar ADO con Microsoft Visual Basic](./using-ado-with-microsoft-visual-basic.md)   
 [Usar ADO con Microsoft Visual C++](./using-ado-with-microsoft-visual-c.md)
---
description: Introducción al proveedor simple de Microsoft OLE DB
title: Proveedor simple de Microsoft OLE DB | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- simple provider [ADO]
- providers [ADO], OLE DB simple provider
- OLE DB simple provider [ADO]
ms.assetid: 1e7dc6f0-482c-4103-8187-f890865e40fc
author: rothja
ms.author: jroth
ms.openlocfilehash: a07224d352f059b8aad3709c6d4f9605c16696b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029239"
---
# <a name="microsoft-ole-db-simple-provider-overview"></a>Introducción al proveedor simple de Microsoft OLE DB
El proveedor simple de Microsoft OLE DB (OSP) permite a ADO acceder a los datos para los que se ha escrito un proveedor mediante el [Kit de herramientas de proveedor simple OLE DB (OSP)](/previous-versions/windows/desktop/ms715822(v=vs.85)). Los proveedores simples están diseñados para tener acceso a orígenes de datos que solo requieren una compatibilidad con OLE DB fundamental, como matrices en memoria o documentos XML.

## <a name="connection-string-parameters"></a>Parámetros de la cadena de conexión
 Para conectarse al OLE DB DLL de proveedor simple, establezca el argumento de *proveedor* en la propiedad [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) en:

```vb
MSDAOSP
```

 Este valor también se puede establecer o leer mediante la propiedad [Provider](../../reference/ado-api/provider-property-ado.md) .

 Puede conectarse a proveedores simples registrados como proveedores de OLE DB completos mediante el nombre del proveedor registrado, determinado por el escritor del proveedor.

## <a name="typical-connection-string"></a>Cadena de conexión típica
 Una cadena de conexión típica para este proveedor es:

```vb
"Provider=MSDAOSP;Data Source=serverName"
```

 La cadena consta de estas palabras clave:

|Palabra clave|Descripción|
|-------------|-----------------|
|**Proveedor**|Especifica el proveedor de OLE DB para SQL Server.|
|**Data Source** (Origen de datos)|Especifica el nombre de un servidor.|

## <a name="xml-document-example"></a>Ejemplo de documento XML
 La OLE DB proveedor simple (OSP) en MDAC 2,7 o posterior y los componentes de Windows Data Access (DAC de Windows) se ha mejorado para admitir la apertura de **conjuntos de registros** de ADO jerárquicos en archivos XML arbitrarios. Estos archivos XML pueden contener el esquema de persistencia XML de ADO, pero no es necesario. Esto se ha implementado mediante la conexión de la OSP al **MSXML2.DLL**; por lo tanto, se requiere **MSXML2.DLL** o posterior.

 El archivo **portfolio.xml** utilizado en el ejemplo siguiente contiene el siguiente árbol:

```console
Portfolio
   Stock
      Shares
      Symbol
      Price
      Info
         Company Name
         WebSite
```

 El DSO XML utiliza heurísticas integradas para convertir los nodos de un árbol XML en capítulos de un **conjunto de registros** jerárquico.

 Con estas heurísticas integradas, el árbol XML se convierte en un **conjunto de registros** jerárquico de dos niveles de la forma siguiente:

```console
Parent Recordset
Shares, Symbol, Price, $Text
   Child Recordset
      Company Name, WebSite, $Text
```

 Tenga en cuenta que las etiquetas portfolio y info no se representan en el **conjunto de registros** jerárquico. Para obtener una explicación de cómo el DSO XML convierte árboles XML en conjuntos de **registros** jerárquicos, vea las siguientes reglas. La columna $Text se describe en la sección siguiente.

## <a name="rules-for-assigning-xml-elements-and-attributes-to-columns-and-rows"></a>Reglas para asignar elementos y atributos XML a columnas y filas
 El DSO XML sigue un procedimiento para asignar elementos y atributos a columnas y filas en aplicaciones enlazadas a datos. XML se modela como un árbol con una etiqueta que contiene toda la jerarquía. Por ejemplo, una descripción XML de un libro podría contener etiquetas de capítulo, etiquetas de figura y etiquetas de sección. En el nivel más alto, sería la etiqueta de libro que contiene los subelementos Chapter, Figure y Section. Cuando el DSO XML asigna elementos XML a filas y columnas, los subelementos, no el elemento de nivel superior, se convierten.

 El DSO XML utiliza este procedimiento para convertir los subelementos:

-   Cada subelemento y atributo corresponde a una columna de un **conjunto de registros** de la jerarquía.

-   El nombre de la columna es el mismo que el nombre del subelemento o atributo, a menos que el elemento primario tenga un atributo y un subelemento con el mismo nombre, en cuyo caso se antepone un "!" al nombre de la columna del subelemento.

-   Cada columna es una columna *simple* que contiene valores escalares (normalmente cadenas) o una columna de **conjunto de registros** que contiene **conjuntos de registros** secundarios.

-   Las columnas correspondientes a los atributos son siempre simples.

-   Las columnas correspondientes a los subelementos son columnas de **conjunto de registros** si el subelemento tiene sus propios subelementos o atributos (o ambos), o si el elemento primario del subelemento tiene más de una instancia del subelemento como elemento secundario. De lo contrario, la columna es sencilla.

-   Cuando hay varias instancias de un subelemento (en diferentes elementos primarios), su columna es una columna de **conjunto de registros** si *alguna* de las instancias implica una columna de **conjunto de registros** ; su columna es simple solo si *todas* las instancias implican una columna simple.

-   Todos los **conjuntos de registros** tienen una columna adicional denominada $text.

 El código que se necesita para construir un **conjunto de registros** es el siguiente:

```vb
Dim adoConn as ADODB.Connection
Dim adoRS as ADODB.Recordset

Set adoRS = New ADODB.Connection
Set adoRS = New ADODB.Recordset

adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
adoRS.Open "https://WebServer/VRoot/portfolio.xml, adoConn
```

> [!NOTE]
>  La ruta de acceso del archivo de datos se puede especificar mediante cuatro convenciones de nomenclatura diferentes.

```vb
'HTTP://
adoRS.Open "https://WebServer/VRoot/portfolio.xml", adoConn
'FILE://
adoRS.Open "file:/// C:\\Directory\\portfolio.xml", adoConn
'UNC Path
adoRS.Open "\\ComputerName\ShareName\portfolio.xml", adoConn
'Full DOS Path
adoRS.Open "C:\Directory\portfolio.xml", adoConn
```

 En cuanto se abre el **conjunto de registros** , se pueden usar los comandos de navegación de conjunto de **registros** de ADO habituales.

 Los **conjuntos de registros** generados por OSP tienen algunas limitaciones:

-   No se admiten los cursores de cliente (**adUseClient**).

-   Los **conjuntos de registros** jerárquicos creados sobre XML arbitrario no se pueden conservar mediante **Recordset. Save**.

-   Los **conjuntos de registros** creados con OSP son de solo lectura.

-   XMLDSO agrega una columna de datos adicional ($Text) a cada **conjunto de registros** de la jerarquía.

 Para obtener más información sobre el proveedor de OLE DB simple, vea [compilar un proveedor simple](/previous-versions/windows/desktop/ms721067(v=vs.85)).

## <a name="code-example"></a>Ejemplo de código
 En el siguiente código de Visual Basic se muestra cómo abrir un archivo XML arbitrario, construir un **conjunto de registros** jerárquico y escribir de forma recursiva cada registro de cada conjunto de **registros** en la ventana de depuración.

 Este es un archivo XML simple que contiene cotizaciones bursátiles. En el código siguiente se usa este archivo para construir un **conjunto de registros** jerárquico de dos niveles.

```xml
<portfolio>
   <stock>
      <shares>100</shares>
      <symbol>MSFT</symbol>
      <price>$70.00</price>
      <info>
         <companyname>Microsoft Corporation</companyname>
         <website>https://www.microsoft.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>AAPL</symbol>
      <price>$107.00</price>
      <info>
         <companyname>Apple Computer, Inc.</companyname>
         <website>https://www.apple.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>DELL</symbol>
      <price>$50.00</price>
      <info>
         <companyname>Dell Corporation</companyname>
         <website>https://www.dell.com</website>
      </info>
    </stock>
    <stock>
       <shares>100</shares>
       <symbol>INTC</symbol>
       <price>$115.00</price>
       <info>
          <companyname>Intel Corporation</companyname>
          <website>https://www.intel.com</website>
       </info>
   </stock>
</portfolio>
```

 A continuación se muestran dos procedimientos sub Visual Basic. El primero crea el **conjunto de registros** y lo pasa al procedimiento Sub *WalkHier* , que recorre la jerarquía de forma recursiva, escribiendo cada **campo** en cada registro de cada **conjunto de registros** en la ventana de depuración.

```vb
Private Sub BrowseHierRecordset()
' Add ADO 2.7 or later to Project/References
' No need to add MSXML2, ADO just passes the ProgID through to the OSP.

    Dim adoConn As ADODB.Connection
    Dim adoRS As ADODB.Recordset
    Dim adoChildRS As ADODB.Recordset

    Set adoConn = New ADODB.Connection
    Set adoRS = New ADODB.Recordset
    Set adoChildRS = ADODB.Recordset

    adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
    adoRS.Open "https://bwillett3/Kowalski/portfolio.xml", adoConn

    Dim iLevel As Integer
    iLevel = 0
    WalkHier iLevel, adoRS

End Sub

Sub WalkHier(ByVal iLevel As Integer, ByVal adoRS As ADODB.Recordset)
    iLevel = iLevel + 1
    PriorLevel = iLevel
    While Not adoRS.EOF
        For ndx = 0 To adoRS.Fields.Count - 1
            If adoRS.Fields(ndx).Name <> "$Text" Then
                If adoRS.Fields(ndx).Type = adChapter Then
                    Set adoChildRS = adoRS.Fields(ndx).Value
                    WalkHier iLevel, adoChildRS
                Else
                    Debug.Print iLevel & ": adoRS.Fields(" & ndx & _
                       ") = " & adoRS.Fields(ndx).Name & " = " & _
                       adoRS.Fields(ndx).Value
                End If
            End If
        Next ndx
        adoRS.MoveNext
    Wend
    iLevel = PriorLevel
End Sub
```
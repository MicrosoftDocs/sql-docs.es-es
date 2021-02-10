---
description: Programación ADO en Visual C++
title: Programación de Visual C++ ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- ADO, Visual C++
- Visual C++ [ADO]
ms.assetid: 11233b96-e05c-4221-9aed-5f20944b0f1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 420bfbba2eaa3946ce9a373c4ac31a88b540f7d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028710"
---
# <a name="visual-c-ado-programming"></a>Programación ADO en Visual C++
La referencia de la API de ADO describe la funcionalidad de la interfaz de programación de aplicaciones (API) de ADO con una sintaxis similar a la de Microsoft Visual Basic. Aunque el público previsto es todos los usuarios, los programadores de ADO emplean diversos lenguajes, como Visual Basic, Visual C++ (con y sin la Directiva de **#import** ) y Visual J++ (con el paquete de clases ADO/WFC).  

> [!NOTE]
> Microsoft ha finalizado la compatibilidad con Visual J++ en 2004.

 Para dar cabida a esta diversidad, [ado para los índices de sintaxis Visual C++](./using-ado-with-microsoft-visual-c.md) proporcionan Visual C++ sintaxis específica del lenguaje con vínculos a descripciones comunes de funcionalidad, parámetros, comportamientos excepcionales, etc., en la referencia de la API.  
  
 ADO se implementa con interfaces COM (modelo de objetos componentes). Sin embargo, es más fácil para los programadores trabajar con COM en determinados lenguajes de programación que otros. Por ejemplo, casi todos los detalles del uso de COM se administran implícitamente para los programadores de Visual Basic, mientras que los programadores de Visual C++ deben asistir a esos detalles.  
  
 En las secciones siguientes se resumen los detalles de los programadores de C y C++ mediante ADO y la directiva **#import** . Se centra en los tipos de datos específicos de COM (**Variant**, **BSTR** y **SAFEARRAY**) y el control de errores (_com_error).  
  
## <a name="using-the-import-compiler-directive"></a>Usar la Directiva de compilador #import  
 La Directiva del compilador de **#import** Visual C++ simplifica el trabajo con los métodos y las propiedades de ADO. La Directiva toma el nombre de un archivo que contiene una biblioteca de tipos, como ADO. dll (Msado15.dll), y genera archivos de encabezado que contienen declaraciones TypeDef, punteros inteligentes para interfaces y constantes enumeradas. Cada interfaz está encapsulada o ajustada en una clase.  
  
 Para cada operación dentro de una clase (es decir, una llamada de método o propiedad), hay una declaración para llamar a la operación directamente (es decir, la forma "sin formato" de la operación) y una declaración para llamar a la operación sin procesar y producir un error COM si la operación no se puede ejecutar correctamente. Si la operación es una propiedad, normalmente hay una directiva de compilador que crea una sintaxis alternativa para la operación que tiene una sintaxis como Visual Basic.  
  
 Las operaciones que recuperan el valor de una propiedad tienen nombres del formulario, **Get**_Property_. Las operaciones que establecen el valor de una propiedad tienen nombres con el formato, la _propiedad_ **Put**. Las operaciones que establecen el valor de una propiedad con un puntero a un objeto ADO tienen nombres con el formato, la _propiedad_ **PutRef**.  
  
 Puede obtener o establecer una propiedad con las llamadas de estas formas:  
  
```cpp
variable = objectPtr->GetProperty(); // get property value   
objectPtr->PutProperty(value);       // set property value  
objectPtr->PutRefProperty(&value);   // set property with object pointer  
```
  
## <a name="using-property-directives"></a>Usar directivas de propiedad  
 La Directiva del compilador **__declspec (propiedad...)** es una extensión del lenguaje C específica de Microsoft que declara una función utilizada como propiedad para tener una sintaxis alternativa. Como resultado, puede establecer u obtener los valores de una propiedad de forma similar a Visual Basic. Por ejemplo, puede establecer y obtener una propiedad de esta manera:  
  
```cpp
objectPtr->property = value;        // set property value  
variable = objectPtr->property;     // get property value  
```
  
 Tenga en cuenta que no tiene que codificar:  
  
```cpp
objectPtr->PutProperty(value);      // set property value  
variable = objectPtr->GetProperty;  // get property value  
```
  
 El compilador generará la llamada de propiedad **Get** _-_ , **Put**-o **PutRef** adecuada en función de la sintaxis alternativa que se declara y de si la propiedad se lee o se escribe.  
  
 La Directiva del compilador **__declspec (propiedad...)** solo puede declarar la sintaxis alternativa **Get**, **Put** o **Get** y **Put** para una función. Las operaciones de solo lectura solo tienen una declaración **Get** ; las operaciones de solo escritura solo tienen una declaración **Put** ; las operaciones de lectura y escritura tienen las declaraciones **Get** y **Put** .  
  
 Con esta directiva solo son posibles dos declaraciones: sin embargo, cada propiedad puede tener tres funciones de propiedad: **Get**_Property_, **Put**_Property_ y **PutRef**_Property_. En ese caso, solo dos formas de la propiedad tienen la sintaxis alternativa.  
  
 Por ejemplo, la propiedad **ActiveConnection** del objeto de **comando** se declara con una sintaxis alternativa para **Get**_ActiveConnection_ y **PutRef**_ActiveConnection_. La sintaxis de **PutRef** es una buena opción porque, en la práctica, normalmente querrá colocar un objeto de **conexión** abierto (es decir, un puntero de objeto de **conexión** ) en esta propiedad. Por otro lado, el objeto **de conjunto de registros** tiene operaciones _ActiveConnection_ **Get**-, **Put**-y **PutRef**, pero no una sintaxis alternativa.  
  
## <a name="collections-the-getitem-method-and-the-item-property"></a>Colecciones, el método GetItem y la propiedad Item  

 ADO define varias colecciones, incluidos **campos**, **parámetros**, **propiedades** y **errores**. En Visual C++, el método **GetItem (_Índice_)** devuelve un miembro de la colección. *Index* es una **variante**, cuyo valor es un índice numérico del miembro de la colección o una cadena que contiene el nombre del miembro.  
  
 La Directiva del compilador **__declspec (propiedad...)** declara la propiedad **Item** como una sintaxis alternativa para el método **GetItem ()** fundamental de cada colección. La sintaxis alternativa utiliza corchetes y es similar a una referencia de matriz. En general, las dos formas son similares a las siguientes:  
  
```cpp
  
      collectionPtr->GetItem(index);  
collectionPtr->Item[index];  
```
  
 Por ejemplo, asigne un valor a un campo de un objeto de **conjunto de registros** , denominado **_RS_**, derivado de la tabla **authors** de la base de datos **pubs** . Use la **propiedad Item ()** para tener acceso al **tercer campo** de la colección de **campos** del objeto de conjunto de **registros** (las colecciones se indizan desde cero; Supongamos que el tercer campo se denomina **_au \_ fname_**). A continuación, llame al método **Value ()** en el objeto **Field** para asignar un valor de cadena.  
  
 Esto se puede expresar en Visual Basic de las cuatro maneras siguientes (las dos últimas formas son únicas de Visual Basic; otros lenguajes no tienen equivalentes):  
  
```cpp
rs.Fields.Item(2).Value = "value"  
rs.Fields.Item("au_fname").Value = "value"  
rs(2) = "value"  
rs!au_fname = "value"  
```
  
 El equivalente en Visual C++ a las dos primeras formas anteriores es:  
  
```cpp
rs->Fields->GetItem(long(2))->PutValue("value");   
rs->Fields->GetItem("au_fname")->PutValue("value");  
```
  
 -o bien-(también se muestra la sintaxis alternativa para la propiedad **Value** )  
  
```cpp
rs->Fields->Item[long(2)]->Value = "value";  
rs->Fields->Item["au_fname"]->Value = "value";  
```
  
 Para obtener ejemplos de cómo recorrer en iteración una colección, vea la sección "colecciones de ADO" de "referencia de ADO".  
  
## <a name="com-specific-data-types"></a>COM-Specific tipos de datos  
 En general, los tipos de datos de Visual Basic que se encuentran en la referencia de la API de ADO tienen un Visual C++ equivalente. Entre ellos se incluyen los tipos de datos estándar, como **unsigned char** para un **byte** Visual Basic, **Short** para **Integer** y **Long** para **Long**. Busque en la sintaxis Indexesto vea exactamente lo que se necesita para los operandos de un método o propiedad determinados.  
  
 Las excepciones a esta regla son los tipos de datos específicos de COM: **Variant**, **BSTR** y **SAFEARRAY**.  
  
### <a name="variant"></a>Variante  
 Una **variante** es un tipo de datos estructurados que contiene un miembro de valor y un miembro de tipo de datos. Una **variante** puede contener una amplia gama de otros tipos de datos, entre los que se incluyen otro tipo variante, BSTR, booleano, IDispatch o IUnknown, moneda, fecha, etc. COM también proporciona métodos que facilitan la conversión de un tipo de datos a otro.  
  
 La clase **_variant_t** encapsula y administra el tipo de datos **Variant** .  
  
 Cuando la referencia de la API de ADO indica que un operando de método o propiedad toma un valor, normalmente significa que el valor se pasa en una **_variant_t**.  
  
 Esta regla es explícitamente true cuando la sección de **parámetros** de los temas de la referencia de la API de ADO indica que un operando es una **variante**. Una excepción es cuando la documentación indica explícitamente que el operando toma un tipo de datos estándar, como **Long** o **byte**, o una enumeración. Otra excepción es cuando el operando toma una **cadena**.  
  
### <a name="bstr"></a>BSTR  
 Un **BSTR** (**B** ASIC **Str** Ing) es un tipo de datos estructurados que contiene una cadena de caracteres y la longitud de la cadena. COM proporciona métodos para asignar, manipular y liberar un **BSTR**.  
  
 La clase **_bstr_t** encapsula y administra el tipo de datos **BSTR** .  
  
 Cuando la referencia de la API de ADO indica que un método o una propiedad toma un valor de **cadena** , significa que el valor tiene el formato de un **_bstr_t**.  
  
### <a name="casting-_variant_t-and-_bstr_t-classes"></a>Convertir _variant_t y _bstr_t clases  
 A menudo, no es necesario codificar explícitamente un **_variant_t** o **_bstr_t** en un argumento para una operación. Si la clase **_variant_t** o **_bstr_t** tiene un constructor que coincide con el tipo de datos del argumento, el compilador generará el **_variant_t** o **_bstr_t** adecuado.  
  
 Sin embargo, si el argumento es ambiguo, es decir, el tipo de datos del argumento coincide con más de un constructor, debe convertir el argumento con el tipo de datos adecuado para invocar el constructor correcto.  
  
 Por ejemplo, la declaración del método **Recordset:: Open** es:  
  
```cpp
    HRESULT Open (  
        const _variant_t & Source,  
        const _variant_t & ActiveConnection,  
        enum CursorTypeEnum CursorType,  
        enum LockTypeEnum LockType,  
        long Options );  
```
  
 El `ActiveConnection` argumento toma una referencia a un **_variant_t**, que puede codificar como una cadena de conexión o un puntero a un objeto de **conexión** abierto.  
  
 Los **_variant_t** correctos se construirán implícitamente si se pasa una cadena como " `DSN=pubs;uid=MyUserName;pwd=MyPassword;` ", o un puntero como " `(IDispatch *) pConn` ".  
  
> [!NOTE]
>  Si se va a conectar a un proveedor de origen de datos que admite la autenticación de Windows, debe especificar **Trusted_Connection = Yes** o **Integrated Security = SSPI** en lugar de la información de identificador de usuario y contraseña en la cadena de conexión.  
  
 O bien, puede codificar explícitamente una **_variant_t** que contiene un puntero como " `_variant_t((IDispatch *) pConn, true)` ". La conversión de tipos, `(IDispatch *)` , resuelve la ambigüedad con otro constructor que toma un puntero a una interfaz IUnknown.  
  
 Es un hecho fundamental, aunque rara vez mencionado, que ADO es una interfaz IDispatch. Cada vez que se debe pasar un puntero a un objeto ADO como **variante**, ese puntero debe convertirse en un puntero a una interfaz IDispatch.  
  
 En el último caso, se codifica explícitamente el segundo argumento booleano del constructor con su valor predeterminado opcional de `true` . Este argumento hace que el constructor **Variant** llame a su método **AddRef**(), que compensa a ADO llamando automáticamente al método **_variant_t:: Release**() cuando se completa la llamada al método o la propiedad de ADO.  
  
### <a name="safearray"></a>SafeArray  
 **SAFEARRAY** es un tipo de datos estructurado que contiene una matriz de otros tipos de datos. Un **SAFEARRAY** se denomina *Safe* porque contiene información sobre los límites de cada dimensión de la matriz y limita el acceso a los elementos de la matriz dentro de esos límites.  
  
 Cuando la referencia de la API de ADO indica que un método o una propiedad toma o devuelve una matriz, significa que el método o la propiedad toma o devuelve un **SAFEARRAY**, no una matriz de C/C++ nativa.  
  
 Por ejemplo, el segundo parámetro del método **OpenSchema** del objeto **Connection** requiere una matriz de valores **Variant** . Esos valores **Variant** se deben pasar como elementos de una **matriz SafeArray** y ese **SAFEARRAY** debe establecerse como el valor de otra **variante**. Es el otro **Variant** que se pasa como segundo argumento de **OpenSchema**.  
  
 Como ejemplo adicional, el primer argumento del método **Find** es una **variante** cuyo valor es un **SAFEARRAY** unidimensional; cada uno de los argumentos opcionales primero y segundo de **AddNew** es una **matriz** unidimensional; y el valor devuelto del método **GetRows** es una **variante** cuyo valor es una **SAFEARRAY** bidimensional.  
  
## <a name="missing-and-default-parameters"></a>Parámetros que faltan y predeterminados  
 Visual Basic permite los parámetros que faltan en los métodos. Por ejemplo, el método **Open** del objeto de **conjunto de registros** tiene cinco parámetros, pero se pueden omitir los parámetros intermedios y dejar los parámetros finales. Un valor **BSTR** o **Variant** predeterminado se sustituirá según el tipo de datos del operando que falta.  
  
 En C/C++, se deben especificar todos los operandos. Si desea especificar un parámetro que falta y cuyo tipo de datos es una cadena, especifique un **_bstr_t** que contenga una cadena nula. Si desea especificar un parámetro que falta y cuyo tipo de datos es una **variante**, especifique un **_variant_t** con un valor de DISP_E_PARAMNOTFOUND y un tipo de VT_ERROR. Como alternativa, especifique la constante **_variant_t** equivalente, **vtMissing**, proporcionada por la directiva **#import** .  
  
 Tres métodos son excepciones al uso típico de **vtMissing**. Estos son los métodos de **ejecución** de los objetos de **conexión** y de **comando** , y el método **NextRecordset** del objeto de **conjunto de registros** . A continuación se muestran sus firmas:  
  
```cpp
_RecordsetPtr <A HREF="mdmthcnnexecute.htm">Execute</A>( _bstr_t CommandText, VARIANT * RecordsAffected,   
        long Options );  // Connection  
_RecordsetPtr <A HREF="mdmthcmdexecute.htm">Execute</A>( VARIANT * RecordsAffected, VARIANT * Parameters,   
        long Options );  // Command  
_RecordsetPtr <A HREF="mdmthnextrec.htm">NextRecordset</A>( VARIANT * RecordsAffected );  // Recordset  
```
  
 Los parámetros, *RecordsAffected* y *Parameters*, son punteros a una **variante**. *Parameters* es un parámetro de entrada que especifica la dirección de una **variante** que contiene un parámetro único, o una matriz de parámetros, que modificará el comando que se está ejecutando. *RecordsAffected* es un parámetro de salida que especifica la dirección de una **variante**, donde se devuelve el número de filas afectadas por el método.  
  
 En el método de **ejecución** de objetos de **comando** , indique que no se especifica ningún parámetro estableciendo *los parámetros* en `&vtMissing` (que se recomienda) o en el puntero nulo (es decir, **null** o cero (0)). Si *los parámetros* se establecen en el puntero nulo, el método sustituye internamente el equivalente de **vtMissing** y, a continuación, completa la operación.  
  
 En todos los métodos, indique que el número de registros afectados no debe devolverse estableciendo *RecordsAffected* en el puntero NULL. En este caso, el puntero nulo no es tan bien un parámetro que falta como una indicación de que el método debe descartar el número de registros afectados.  
  
 Por lo tanto, para estos tres métodos, es válido codificar algo como:  
  
```cpp
pConnection->Execute("commandText", NULL, adCmdText);   
pCommand->Execute(NULL, NULL, adCmdText);  
pRecordset->NextRecordset(NULL);  
```
  
## <a name="error-handling"></a>Tratamiento de errores  
 En COM, la mayoría de las operaciones devuelven un código de retorno HRESULT que indica si una función se ha completado correctamente. La directiva **#import** genera código de contenedor alrededor de cada método o propiedad "RAW" y comprueba el HRESULT devuelto. Si el valor HRESULT indica un error, el código de contenedor produce un error COM llamando a _com_issue_errorex () con el código de retorno HRESULT como argumento. Los objetos de error com se pueden detectar en un bloque **try** - **catch** . (Por motivos de eficacia, Capture una referencia a un objeto **_com_error** ).  
  
 Recuerde que se trata de errores de ADO: son el resultado de un error en la operación de ADO. Los errores devueltos por el proveedor subyacente aparecen como objetos de **error** en la colección de **errores** del objeto de **conexión** .  
  
 La directiva **#import** solo crea rutinas de control de errores para los métodos y propiedades declarados en el archivo. dll de ADO. Sin embargo, puede aprovechar este mismo mecanismo de control de errores escribiendo su propia macro o función insertada de comprobación de errores. Vea el tema [Visual C++ extensiones](./visual-c-extensions-for-ado.md)o el código de las secciones siguientes para obtener ejemplos.  
  
## <a name="visual-c-equivalents-of-visual-basic-conventions"></a>Visual C++ equivalentes de las convenciones de Visual Basic  
 A continuación se encuentra un resumen de varias convenciones de la documentación de ADO, codificadas en Visual Basic, así como sus equivalentes en Visual C++.  
  
### <a name="declaring-an-ado-object"></a>Declarar un objeto ADO  
 En Visual Basic, una variable de objeto ADO (en este caso para un objeto de **conjunto de registros** ) se declara como sigue:  
  
```vb
Dim rst As ADODB.Recordset  
```
  
 La cláusula, " `ADODB.Recordset` ", es el ProgID del objeto de **conjunto de registros** tal y como se define en el registro. Una nueva instancia de un objeto de **registro** se declara de la siguiente manera:  
  
```vb
Dim rst As New ADODB.Recordset  
```
  
 o bien  
  
```vb
Dim rst As ADODB.Recordset  
Set rst = New ADODB.Recordset  
```
  
 En Visual C++, la Directiva de **#import** genera declaraciones de tipos de puntero inteligente para todos los objetos de ADO. Por ejemplo, una variable que apunta a un objeto **_Recordset** es de tipo **_RecordsetPtr** y se declara como sigue:  
  
```cpp
_RecordsetPtr  rs;  
```
  
 Una variable que apunta a una nueva instancia de un objeto **_Recordset** se declara de la siguiente manera:  
  
```cpp
_RecordsetPtr  rs("ADODB.Recordset");  
```
  
 o bien  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance("ADODB.Recordset");  
```
  
 o bien  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance(__uuidof(_Recordset));  
```
  
 Después de llamar al método **CreateInstance** , la variable se puede usar de la siguiente manera:  
  
```cpp
rs->Open(...);  
```
  
 Observe que en un caso, el `.` operador "" se utiliza como si la variable fuera una instancia de una clase ( `rs.CreateInstance` ) y, en otro caso, `->` se usa el operador "" como si la variable fuera un puntero a una interfaz ( `rs->Open` ).  
  
 Una variable se puede usar de dos maneras porque el `->` operador "" se sobrecarga para permitir que una instancia de una clase se comporte como un puntero a una interfaz. Un miembro de clase privada de la variable de instancia contiene un puntero a la interfaz **_Recordset** ; el `->` operador "" devuelve ese puntero y el puntero devuelto obtiene acceso a los miembros del objeto **_Recordset** .  
  
### <a name="coding-a-missing-parameter---string"></a>Codificación de una cadena de parámetros que faltan  
 Cuando necesite codificar un operando de **cadena** que falta en Visual Basic, simplemente omita el operando. Debe especificar el operando en Visual C++. Codifique un **_bstr_t** que tenga una cadena vacía como valor.  
  
```cpp
_bstr_t strMissing(L"");  
```
  
### <a name="coding-a-missing-parameter---variant"></a>Codificación de una variante de parámetro que falta  
 Cuando necesite codificar un operando de **variante** ausente en Visual Basic, simplemente omita el operando. Debe especificar todos los operandos en Visual C++. Codifique un parámetro de **variante** que falta con un **_variant_t** establecido en el valor especial, DISP_E_PARAMNOTFOUND y tipo VT_ERROR. Como alternativa, especifique **vtMissing**, que es una constante predefinida equivalente proporcionada por la directiva **#import** .  
  
```cpp
_variant_t  vtMissingYours(DISP_E_PARAMNOTFOUND, VT_ERROR);   
```
  
 o use-  
  
```cpp
...vtMissing...;  
```
  
### <a name="declaring-a-variant"></a>Declarar una variante  
 En Visual Basic, se declara una **variante** con la instrucción **Dim** de la siguiente manera:  
  
```vb
Dim VariableName As Variant  
```
  
 En Visual C++, declare una variable como tipo **_variant_t**. A continuación se muestran algunas declaraciones de **_variant_t** esquemáticas.  
  
> [!NOTE]
>  Estas declaraciones simplemente proporcionan una idea aproximada de lo que se debe codificar en su propio programa. Para obtener más información, vea los ejemplos siguientes y la documentación de Visual C++.  
  
```cpp
_variant_t  VariableName(value);  
_variant_t  VariableName((data type cast) value);  
_variant_t  VariableName(value, VT_DATATYPE);  
_variant_t  VariableName(interface * value, bool fAddRef = true);  
```
  
### <a name="using-arrays-of-variants"></a>Usar matrices de variantes  
 En Visual Basic, las matrices de **variantes** se pueden codificar con la instrucción **Dim** , o bien puede usar la función **array** , como se muestra en el código de ejemplo siguiente:  
  
```vb
Public Sub ArrayOfVariants  
Dim cn As ADODB.Connection  
Dim rs As ADODB.Recordset  
Dim fld As ADODB.Field  
  
    cn.Open "DSN=pubs"  
    rs = cn.OpenSchema(adSchemaColumns, _  
        Array(Empty, Empty, "authors", Empty))  
    For Each fld in rs.Fields  
        Debug.Print "Name = "; fld.Name  
    Next fld  
    rs.Close  
    cn.Close  
End Sub  
```
  
 En el siguiente ejemplo de Visual C++ se muestra el uso de una **matriz SafeArray** utilizada con un **_variant_t**.  
  
#### <a name="notes"></a>Notas  
 Las notas siguientes corresponden a las secciones comentadas en el ejemplo de código.  
  
1.  Una vez más, la función insertada TESTHR () se define para aprovechar el mecanismo de control de errores existente.  
  
2.  Solo se necesita una matriz unidimensional, por lo que puede usar **SafeArrayCreateVector**, en lugar de la declaración **SAFEARRAYBOUND** de uso general y la función **SafeArrayCreate** . A continuación se presenta el aspecto que tendría el código mediante **SafeArrayCreate**:  
  
    ```cpp
       SAFEARRAYBOUND   sabound[1];  
       sabound[0].lLbound = 0;  
       sabound[0].cElements = 4;  
       pSa = SafeArrayCreate(VT_VARIANT, 1, sabound);  
    ```
  
3.  El esquema identificado por la constante enumerada, **adSchemaColumns**, está asociado a cuatro columnas de restricción: TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME y Column_name. Por lo tanto, se crea una matriz de valores **Variant** con cuatro elementos. A continuación, se especifica un valor de restricción que corresponde a la tercera columna, TABLE_NAME.  
  
     El **conjunto de registros** que se devuelve consta de varias columnas, un subconjunto de las cuales son las columnas de restricción. Los valores de las columnas de restricción de cada fila devuelta deben ser los mismos que los valores de restricción correspondientes.  
  
4.  Los usuarios familiarizados con **SAFEARRAY** pueden sorprender que no se llama a **SafeArrayDestroy**() antes de la salida. De hecho, al llamar a **SafeArrayDestroy**() en este caso, se producirá una excepción en tiempo de ejecución. La razón es que el destructor de `vtCriteria` llamará a **VariantClear**() cuando la **_variant_t** salga del ámbito, lo que liberará la **matriz SafeArray**. La llamada a **SafeArrayDestroy**, sin borrar manualmente el **_variant_t**, haría que el destructor intentara borrar un puntero **SAFEARRAY** no válido.  
  
     Si se llamó a **SafeArrayDestroy** , el código tendría el siguiente aspecto:  
  
    ```cpp
          TESTHR(SafeArrayDestroy(pSa));  
       vtCriteria.vt = VT_EMPTY;  
          vtCriteria.parray = NULL;  
    ```
  
     Sin embargo, es mucho más sencillo permitir que el **_variant_t** administre el **SAFEARRAY**.  
  
```cpp
// Visual_CPP_ADO_Prog_1.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Note 1  
inline void TESTHR( HRESULT _hr ) {   
   if FAILED(_hr)   
      _com_issue_error(_hr);   
}  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _RecordsetPtr pRs("ADODB.Recordset");  
      _ConnectionPtr pCn("ADODB.Connection");  
      _variant_t vtTableName("authors"), vtCriteria;  
      long ix[1];  
      SAFEARRAY *pSa = NULL;  
  
      pCn->Provider = "sqloledb";  
      pCn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
      // Note 2, Note 3  
      pSa = SafeArrayCreateVector(VT_VARIANT, 1, 4);  
      if (!pSa)   
         _com_issue_error(E_OUTOFMEMORY);  
  
      // Specify TABLE_NAME in the third array element (index of 2).   
      ix[0] = 2;        
      TESTHR(SafeArrayPutElement(pSa, ix, &vtTableName));  
  
      // There is no Variant constructor for a SafeArray, so manually set the   
      // type (SafeArray of Variant) and value (pointer to a SafeArray).  
  
      vtCriteria.vt = VT_ARRAY | VT_VARIANT;  
      vtCriteria.parray = pSa;  
  
      pRs = pCn->OpenSchema(adSchemaColumns, vtCriteria, vtMissing);  
  
      long limit = pRs->GetFields()->Count;  
      for ( long x = 0 ; x < limit ; x++ )  
         printf( "%d: %s\n", x + 1, ((char*) pRs->GetFields()->Item[x]->Name) );  
      // Note 4  
      pRs->Close();  
      pCn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Error:\n");  
      printf("Code = %08lx\n", e.Error());  
      printf("Code meaning = %s\n", (char*) e.ErrorMessage());  
      printf("Source = %s\n", (char*) e.Source());  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   CoUninitialize();  
}  
```
  
### <a name="using-property-getputputref"></a>Usar property get/Put/PutRef  
 En Visual Basic, el nombre de una propiedad no está calificado por si se recupera, asigna o asigna una referencia.  
  
```vb
Public Sub GetPutPutRef  
Dim rs As New ADODB.Recordset  
Dim cn As New ADODB.Connection  
Dim sz as Integer  
cn.Open "Provider=sqloledb;Data Source=yourserver;" & _  
         "Initial Catalog=pubs;Integrated Security=SSPI;"  
rs.PageSize = 10  
sz = rs.PageSize  
rs.ActiveConnection = cn  
rs.Open "authors",,adOpenStatic  
' ...  
rs.Close  
cn.Close  
End Sub  
```
  
 En este ejemplo Visual C++ se muestra la propiedad **Get** / **Put** / **PutRef**.  
  
#### <a name="notes"></a>Notas  
 Las notas siguientes corresponden a las secciones comentadas en el ejemplo de código.  
  
1.  En este ejemplo se usan dos formas de un argumento de cadena que falta: una constante explícita, **strMissing**, y una cadena que el compilador usará para crear un **_bstr_t** temporal que existirá para el ámbito del método **Open** .  
  
2.  No es necesario convertir el operando de `rs->PutRefActiveConnection(cn)` en `(IDispatch *)` porque ya existe el tipo del operando `(IDispatch *)` .  
  
```cpp
// Visual_CPP_ado_prog_2.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _bstr_t strMissing(L"");  
      long oldPgSz = 0, newPgSz = 5;  
  
      // Note 1  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", strMissing, "", adConnectUnspecified);  
  
      oldPgSz = rs->GetPageSize();  
      // -or-  
      // oldPgSz = rs->PageSize;  
  
      rs->PutPageSize(newPgSz);  
      // -or-  
      // rs->PageSize = newPgSz;  
  
      // Note 2  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockReadOnly, adCmdTable);  
      printf("Original pagesize = %d, new pagesize = %d\n", oldPgSz, rs->GetPageSize());  
      rs->Close();  
      cn->Close();  
  
   }  
   catch (_com_error &e) {  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="using-getitemx-and-itemx"></a>Usar GetItem (x) y Item [x]  
 En este Visual Basic ejemplo se muestra la sintaxis estándar y alternativa del **elemento**().  
  
```vb
Public Sub GetItemItem  
Dim rs As New ADODB.Recordset  
Dim name as String  
rs = rs.Open "authors", "DSN=pubs;", adOpenDynamic, _  
         adLockBatchOptimistic, adTable  
name = rs(0)  
' -or-  
name = rs.Fields.Item(0)  
rs(0) = "Test"  
rs.UpdateBatch  
' Restore name  
rs(0) = name  
rs.UpdateBatch  
rs.Close  
End Sub  
```
  
 En este Visual C++ ejemplo se muestra el **elemento**.  
  
> [!NOTE]
>  La nota siguiente corresponde a las secciones comentadas en el ejemplo de código: cuando se tiene acceso a la colección con **Item**, el índice, **2**, debe convertirse en **Long** para que se invoque un constructor adecuado.  
  
```cpp
// Visual_CPP_ado_prog_3.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
void main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _variant_t vtFirstName;  
  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", "", "", adConnectUnspecified);  
  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockOptimistic, adCmdTable);  
      rs->MoveFirst();  
  
      // Note 1. Get a field.  
      vtFirstName = rs->Fields->GetItem((long)2)->GetValue();  
      // -or-  
      vtFirstName = rs->Fields->Item[(long)2]->Value;  
  
      printf( "First name = '%s'\n", (char*)( (_bstr_t)vtFirstName) );  
  
      rs->Fields->GetItem((long)2)->Value = L"TEST";  
      rs->Update(vtMissing, vtMissing);  
  
      // Restore name  
      rs->Fields->GetItem((long)2)->PutValue(vtFirstName);  
      // -or-  
      rs->Fields->GetItem((long)2)->Value = vtFirstName;  
      rs->Update(vtMissing, vtMissing);  
      rs->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="casting-ado-object-pointers-with-idispatch-"></a>Convertir punteros de objeto ADO con (IDispatch *)  
 En el siguiente ejemplo de Visual C++ se muestra el uso de (IDispatch *) para convertir punteros de objetos de ADO.  
  
#### <a name="notes"></a>Notas  
 Las notas siguientes corresponden a las secciones comentadas en el ejemplo de código.  
  
1.  Especifique un objeto de **conexión** abierto en una **variante** codificada explícitamente. Conviértalo con (IDispatch \* ) para que se invoque el constructor correcto. Además, establezca explícitamente el segundo parámetro **_variant_t** en el valor predeterminado de **true**, por lo que el recuento de referencias de objeto será correcto cuando finalice la operación **Recordset:: Open** .  
  
2.  La expresión, `(_bstr_t)` , no es una conversión, sino un operador **_variant_t** que extrae una cadena **_bstr_t** de la **variante** devuelta por **Value**.  
  
 La expresión, `(char*)` , no es una conversión, sino un operador **_bstr_t** que extrae un puntero a la cadena encapsulada en un objeto **_bstr_t** .  
  
 En esta sección de código se muestran algunos de los comportamientos útiles de los operadores **_variant_t** y **_bstr_t** .  
  
```cpp
// Visual_CPP_ado_prog_4.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr pConn("ADODB.Connection");  
      _RecordsetPtr pRst("ADODB.Recordset");  
  
      pConn->Provider = "sqloledb";  
      pConn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
  
      // Note 1.  
      pRst->Open("authors", _variant_t((IDispatch *) pConn, true), adOpenStatic, adLockReadOnly, adCmdTable);  
      pRst->MoveLast();  
  
      // Note 2.  
      printf("Last name is '%s %s'\n",   
         (char*) ((_bstr_t) pRst->GetFields()->GetItem("au_fname")->GetValue()),  
         (char*) ((_bstr_t) pRst->Fields->Item["au_lname"]->Value));  
  
      pRst->Close();  
      pConn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }     
   ::CoUninitialize();  
}  
```
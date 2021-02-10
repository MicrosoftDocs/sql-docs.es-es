---
description: Términos del glosario de ADO
title: Términos del glosario de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, glossary
ms.assetid: b0478836-4123-4357-969a-c5784fc28be5
author: rothja
ms.author: jroth
ms.openlocfilehash: 3a79a16b0fe5f0514b2d90333fcbe99e8e5b5023
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031057"
---
# <a name="ado-glossary-terms"></a>Términos del glosario de ADO
En este tema se definen los términos relevantes para ADO.

## <a name="a"></a>A
 Dirección URL absoluta es una dirección URL completa que especifica la ubicación de un recurso que reside en Internet o en una intranet. Vea también *dirección URL* y *dirección URL relativa*.

 Registro automático de controles ActiveX, componente COM en proceso que suele tener un elemento visual en tiempo de diseño o en tiempo de ejecución. Los controles ActiveX también tienen la capacidad de comunicarse con un contenedor de documentos activo, como Microsoft Internet Explorer.

 ADISAPI (interfaz de programación de aplicaciones de servidor de Internet Advanced Data) un archivo DLL ISAPI que proporciona análisis, control de automatización, serialización de conjuntos de registros y empaquetado de MIME. El componente ADISAPI funciona a través de la API proporcionada por Internet Information Services (IIS). Vea también *ISAPI*.

 función de agregado en una consulta, una función como COUNT, AVG o STDEV que calcula un valor mediante todas las filas de una columna de una tabla. En la escritura de expresiones y en la programación, puede usar funciones de agregado de SQL (incluidas las tres enumeradas anteriormente) y funciones de agregado de dominio para determinar diversas estadísticas.

 alias un nombre alternativo que se asigna a una columna o expresión en una instrucción SELECT de SQL, a menudo más corta o más significativa. Por ejemplo, BobSales es el alias de la siguiente instrucción SELECT: "Select WR-sales as BobSales from SalesDB". Se puede usar un alias para asignar dinámicamente columnas a los enlaces de control en el objeto DataControl.

 subprocesamiento de Apartamento un modelo de subprocesos COM en el que todas las llamadas a un objeto se producen en un subproceso. En los subprocesos de apartamento, COM sincroniza y calcula las referencias de las llamadas. Vea también *COMmddefcom*.

 operación asincrónica operación que devuelve el control al programa que realiza la llamada sin esperar a que se complete la operación. Antes de que se complete la operación, la ejecución del código continúa. Vea también *operación sincrónica*.

## <a name="b"></a>B
 entrada de enlace: una asignación entre un campo de una tabla y una variable. En las extensiones de ADO Visual C++, los campos de **conjunto de registros** se asignan a variables de C/C++.

 máscara de bits: un valor numérico diseñado para una comparación de valores bit a bit con otros valores numéricos, normalmente para marcar opciones en parámetros o valores devueltos. Normalmente, esta comparación se realiza con operadores lógicos bit a bit, como **and** y **or** en Visual Basic **&** y **&#124;** en C++.

 Por ejemplo, los valores de **FieldAttributeEnum** de ADO se pueden usar como máscaras de y para determinar los atributos de un campo. Supongamos que desea determinar si un campo es actualizable. Podría comprobarlo con la siguiente expresión en Visual Basic:`Field.Attributes AND adFldUpdatable`

 Si el resultado es TRUE, el campo es actualizable.

 marcador un marcador que identifica de forma única una fila dentro de un conjunto de filas para que un usuario pueda desplazarse rápidamente a ella.

 objeto comercial un objeto que realiza un conjunto definido de operaciones, como la validación de datos o la lógica de la regla de negocios. Los objetos comerciales suelen residir en el nivel intermedio.

 regla de negocios la combinación de ediciones de validación, comprobaciones de inicio de sesión, búsquedas en bases de datos, directivas y transformaciones algorítmicas que constituye una forma empresarial de hacer negocios. También conocido como *lógica de negocios*.

## <a name="c"></a>C
 expresión calculada expresión que no es constante, pero cuyo valor depende de otros valores. Para su evaluación, una expresión calculada debe obtener y calcular los valores de otros orígenes, normalmente en otros campos o filas.

 capítulo A referencia a un intervalo de filas de un origen de datos. En ADO, un capítulo suele ser una referencia a otro **conjunto de registros**.

 Las columnas Chapter permiten definir una relación *de elementos primarios y secundarios* en la que el *elemento primario* es el **conjunto de registros** que contiene la columna Chapter y el *elemento secundario* es el **conjunto de registros** representado por el capítulo.

 Chapter: alias de un alias que hace referencia a la columna anexada al elemento primario.

 juego de caracteres: asignación de un juego de caracteres a sus valores numéricos. Por ejemplo, Unicode es un juego de caracteres de 16 bits capaz de codificar todos los caracteres conocidos y usarse como un estándar de codificación de caracteres en todo el mundo.

 secundario el lado dependiente de una relación jerárquica. Un elemento secundario es un nodo de una estructura jerárquica que tiene otro nodo por encima de él (más cerca de la raíz). Vea también *alias secundario*, relación de elementos *primarios y secundarios*, *elemento primario*.

 alias secundario: alias que hace referencia al elemento secundario. Vea también *alias*, *secundario*.

 CLSID (identificador de clase) identificador único universal (UUID) que identifica un componente COM. Cada componente COM tiene su CLSID en el registro de Windows para que lo puedan cargar otras aplicaciones. Vea también *ProgID*, *com*.

 nivel de cliente una capa lógica de un sistema distribuido que normalmente presenta datos y procesa la entrada del usuario, que a veces se conoce como *front-end*. Normalmente, el nivel de cliente solicita datos de un servidor en función de la entrada y, a continuación, da formato y muestra el resultado. Vea también *nivel intermedio*, *nivel de origen de datos*, *aplicación distribuida*.

 COM (modelo de objetos componentes) un estándar binario que permite a los objetos interoperar en un entorno de red independientemente del idioma en el que se desarrollaron o en qué equipos residen. Las tecnologías basadas en COM incluyen controles ActiveX, automatización y vinculación e incrustación de objetos (OLE). COM permite que un objeto exponga su funcionalidad a otros componentes y a las aplicaciones host. Define el modo en que el objeto se expone a sí mismo y cómo funciona esta exposición entre los procesos y las redes. COM también define el ciclo de vida del objeto.

 Archivo binario del componente COM, como. dll,. ocx y algunos archivos. exe, que admite el estándar COM para proporcionar objetos. Este tipo de archivo contiene código para uno o más generadores de clases, clases COM, mecanismos de entrada del registro, código de carga, etc.

 operador de comparación: operador que compara dos expresiones y devuelve un valor booleano.

 Un parámetro de criterios que se puede expresar como ">" (mayor que), " \<" (less than), "=" (equal), "> =" (mayor o igual que), "<=" (menor o igual que), "<>" (no igual) o "like" (coincidencia de patrones).

 componente objeto que encapsula datos y código, y proporciona un conjunto bien especificado de servicios disponibles públicamente.

 archivo compuesto una implementación de almacenamiento estructurado COM para archivos. Un archivo compuesto almacena objetos independientes en un único archivo estructurado que consta de dos elementos principales: objetos de almacenamiento y objetos de secuencia. Juntos, funcionan como un sistema de archivos dentro de un archivo.

 Número de archivos individuales enlazados entre sí en un archivo físico. Se puede tener acceso a cada archivo individual de un archivo compuesto como si fuera un único archivo físico.

 constante: valor numérico o de cadena que no cambia. Las enumeraciones de ADO denominadas (constantes enumeradas) se pueden utilizar en el código en lugar de en valores reales; por ejemplo, **adUseClient** es una constante cuyo valor es 3. (Const adUseClient = 3). Vea también *enumeración*.

 cursor un elemento de base de datos que controla la navegación por los registros, la actualización de los datos y la visibilidad de los cambios realizados en la base de datos por parte de otros usuarios.

## <a name="d"></a>D
 enlace de datos el proceso de asociar los objetos o controles de una aplicación a un origen de datos. Un control asociado a un origen de datos se denomina *control enlazado a datos*.

 El contenido de un control enlazado a datos se asocia a los valores de una base de datos. Por ejemplo, un control de cuadrícula que está enlazado a un objeto de **conjunto de registros** puede actualizarse cuando se actualizan las filas del **conjunto de registros** . Cuando el **conjunto de registros** recupera nuevos valores, se muestran nuevos valores en la cuadrícula.

 Software del proveedor de datos que expone los datos a una aplicación ADO directamente o a través de un proveedor de servicios. Vea también proveedor de servicios.

 la forma de datos de una técnica que hace uso de una sintaxis formalizada (denominada **lenguaje de formas**) para definir un objeto de **conjunto de registros** especializado (denominado conjunto de *registros* con forma) que no solo contiene datos, sino también referencias a otros objetos de **conjunto de registros** o valores calculados basados en esos otros objetos de conjunto de **registros** .

 nivel de origen de datos capa lógica de un sistema distribuido que representa un equipo que ejecuta un DBMS, como una base de datos de SQL Server. Vea también *nivel de cliente*, *nivel intermedio*, *aplicación distribuida*.

 DCOM un protocolo de conexión que permite a los componentes COM comunicarse directamente entre sí a través de una red. Vea también *com*, *componente*.

 DDL (lenguaje de definición de datos) las instrucciones de SQL que definen, en lugar de manipular los datos. El esquema de una base de datos se crea o se modifica con DDL. Por ejemplo, **CREATE TABLE**, **Create index**, **Grant** y **REVOKE** son instrucciones DDL de SQL.

 secuencia predeterminada: secuencia de texto o binaria (representada por un objeto de **flujo** ) que está asociada a objetos de **registro** o de **conjunto de registros** cuando se usan ciertos proveedores de OLE DB, como el proveedor de Microsoft OLE DB para la publicación en Internet. Normalmente, el flujo predeterminado contiene el contenido de un archivo, como el código HTML de la raíz de un sitio Web.

 aplicación distribuida un programa escrito para que el procesamiento pueda dividirse entre varios equipos a través de una red. Normalmente, una aplicación distribuida se divide en la presentación, la lógica de negocios y las capas de almacén de datos, o los *niveles*. Vea también nivel de cliente, nivel intermedio, nivel de origen de datos.

 Conjunto de registros desconectado un objeto de **conjunto de registros** en una caché de cliente que ya no tiene una conexión dinámica con el servidor. Si es necesario tener acceso de nuevo al origen de datos original por alguna razón, como actualizar los datos, se debe volver a establecer la conexión. Sin embargo, todavía se puede tener acceso a las colecciones, propiedades y métodos de un **conjunto de registros** desconectado.

 DML (lenguaje de manipulación de datos) las instrucciones de SQL que manipulan, en lugar de definir los datos. Los valores de una base de datos se seleccionan y se modifican con DML. Por ejemplo, **Insert**, **Update**, **Delete** y **Select** son instrucciones DML de SQL.

 proveedor de origen de documento es una clase especial de proveedores que administran carpetas y documentos. Cuando un documento se representa mediante un objeto de **registro** , o una carpeta de documentos se representa mediante un objeto de **conjunto de registros** , el proveedor de origen del documento rellena esos objetos con un conjunto único de campos que describen las características del documento, en lugar del propio documento. Vea también registro de recursos.

 DSN (nombre del origen de datos) colección de información usada para conectar la aplicación a una base de datos ODBC determinada. El administrador de controladores ODBC utiliza esta información para crear una conexión a la base de datos. Un DSN se puede almacenar en un archivo (un DSN de archivo) o en el registro de Windows (un DSN de equipo).

 propiedad dinámica: propiedad específica de un proveedor de datos o del servicio de cursor. La colección de **propiedades** de un objeto se rellena automáticamente ("dinámicamente"). Un objeto no tiene ninguna propiedad dinámica hasta que se conecta a un origen de datos a través de un proveedor de datos determinado. Vea también proveedor de datos, cursor.

## <a name="e"></a>E
 Enumeración una lista de constantes con nombre. Los valores enumerados no deben ser únicos. Sin embargo, el nombre de cada valor debe ser único dentro del ámbito en el que se define la enumeración. En ADO, las enumeraciones se utilizan para los valores devueltos y los parámetros numéricos, para agregar significado al código ADO y para proteger al desarrollador de los valores numéricos (que pueden cambiar de una versión a una versión). Por ejemplo, para abrir un **conjunto de registros** estático, use el valor enumerado **adOpenStatic** : `Recordset.Open ,,adOpenStatic`

 También se conoce como *constante enumerada*. Vea también *constante*.

 evento que una acción reconoce un objeto para el que se puede escribir código para responder. Los eventos se pueden generar mediante la ejecución de comandos, la finalización de transacciones, la navegación por conjuntos de registros y las actualizaciones de datos, entre otras acciones. Vea también *controlador de eventos*.

 controlador de eventos un controlador de eventos es el código que se ejecuta cuando se produce un evento. Vea también evento.

## <a name="h"></a>H
 controlador una rutina que administra una condición u operación común y relativamente sencilla, como la recuperación de errores o la administración de datos.

 Conjunto de registros jerárquico un **conjunto** de registros que contiene otro **conjunto de registros**. Vea también forma de datos, capítulo.

 Para obtener más información, vea [obtener acceso a las filas de un conjunto de registros jerárquico](./guide/data/accessing-rows-in-a-hierarchical-recordset.md).

 En general, una jerarquía es una estructura clasificada con niveles de nivel superior y subordinado. En ADO, los **conjuntos de registros** jerárquicos se utilizan para representar la relación de elementos primarios y secundarios entre un registro y un capítulo. También en ADO, los objetos **Record** y **Stream** se pueden usar para tener acceso a estructuras jerárquicas de árbol, como una carpeta y documentos. ADO MD también incluye objetos **Hierarchy** para representar una relación entre los niveles de una dimensión en un cubo OLAP. Vea también conjuntos de registros jerárquicos, relación primario-secundario, capítulo, árbol.

## <a name="i-l"></a>I-L
 ISAPI (interfaz de programación de aplicaciones para servidores de Internet) conjunto de funciones para los servidores de Internet, como un servidor Windows NT &reg; Server/windows 2000 que ejecuta Microsoft &reg; Internet Information Services (IIS).

 Clave: columna o columnas de una tabla que identifican de forma única una fila; se utiliza a menudo para indizar una tabla.

## <a name="m"></a>M
 serialización del proceso de empaquetar, enviar y desempaquetar parámetros de método de interfaz en los límites de subprocesos o procesos.

 nivel intermedio la capa lógica en un sistema distribuido entre una interfaz de usuario o un cliente web y la base de datos. Normalmente, este es el lugar en el que se crean instancias de los objetos Business. El nivel intermedio es una colección de reglas de negocios y funciones que generan y funcionan al recibir información. Logran esto a través de reglas de negocios, que pueden cambiar con frecuencia y, por tanto, se encapsulan en componentes físicamente independientes de la propia lógica de la aplicación. También se conoce como *nivel de servidor de aplicaciones*. Vea también aplicación distribuida, nivel de cliente, nivel de origen de datos.

 MIME (extensión multipropósito de correo Internet): un protocolo de Internet desarrollado originalmente para permitir el intercambio de mensajes de correo electrónico con contenido enriquecido en entornos de red, equipos y correo electrónico heterogéneos. En la práctica, MIME también se ha adoptado y ampliado por aplicaciones que no son de correo.

 MIME es un estándar que permite que los datos binarios se publiquen y se lean en Internet. El encabezado de un archivo con datos binarios contiene el tipo MIME de los datos. Esto informa a los programas cliente (los exploradores Web y los paquetes de correo, por ejemplo) que necesitarán para administrar los datos de forma diferente a como se trata de texto directo. Por ejemplo, el encabezado de un documento Web que contiene un gráfico JPEG contiene el tipo MIME específico del formato de archivo JPEG. Esto permite que un explorador muestre el archivo con su visor JPEG, si hay alguno.

## <a name="n-o"></a>N-O
 nodo un elemento en una estructura jerárquica de árbol. Un nodo puede ser la raíz o el elemento secundario de otro nodo. Un nodo también puede ser el elemento primario de varios elementos secundarios. Vea también jerarquía, árbol, raíz, elemento secundario y primario.

 variable de objeto: variable que contiene una referencia a un objeto. Por ejemplo, `objCustomObject` es una variable que señala a un objeto de tipo CustomObject:`Set objCustomObject = CreateObject(adodb.Recordset)`

 ODBC (Conectividad abierta de bases de datos) interfaz de lenguaje de programación estándar que se usa para conectarse a diversos orígenes de datos. Normalmente se tiene acceso a él a través del panel de control, donde se pueden asignar nombres de orígenes de datos (DSN) para usar controladores ODBC específicos.

 OLE DB un conjunto de interfaces que exponen datos de diversos orígenes mediante COM. OLE DB interfaces proporcionan a las aplicaciones acceso uniforme a los datos almacenados en diversos orígenes de información. Estas interfaces admiten la cantidad de funcionalidad de DBMS adecuada para el origen de datos, lo que permite compartir sus datos. Vea también COM.

 bloquear de forma optimista un tipo de bloqueo en el que la página de datos que contiene uno o varios registros, incluido el registro que se está editando, no está disponible para otros usuarios mientras el registro se está actualizando mediante el método **Update** , pero está disponible antes y después de la llamada a **Update**.

 El bloqueo optimista se usa cuando se abre el objeto de **conjunto de registros** con el parámetro **LockType** o la propiedad establecida en **adLockOptimistic** o **adLockBatchOptimistic**. Vea también bloqueo pesimista.

 valor ordinal la ubicación numérica de un elemento dentro de un pedido. En una colección de ADO, el valor ordinal del primer elemento es cero (0). El siguiente elemento es uno (1), y así sucesivamente.

## <a name="p"></a>P
 comando con parámetros: consulta o comando que permite establecer los valores de los parámetros antes de que se ejecute el comando. Por ejemplo, una cadena SQL se puede parametrizar mediante la incrustación de marcadores de parámetros en la cadena de SQL (designada por el carácter '? '). A continuación, la aplicación especifica valores para cada parámetro y ejecuta el comando.

 elemento primario del lado de control de una relación jerárquica. En una estructura jerárquica, un elemento primario tiene uno o más nodos secundarios directamente debajo de él en la jerarquía. Vea también alias primario, relación primario-secundario y secundario.

 alias primario: alias que hace referencia al elemento primario. Vea también alias, principal.

 relación de elementos primarios y secundarios: relación en una estructura jerárquica en la que el elemento primario es un nivel superior y directamente asociado a uno o más elementos secundarios. Un elemento secundario es un nivel inferior y debe tener un elemento primario. Vea también elemento primario y secundario.

 bloqueo pesimista: un tipo de bloqueo en el que la página que contiene uno o varios registros, incluido el registro que se está editando, no está disponible para otros usuarios para garantizar que se realizará una actualización. El proveedor de OLE DB define el comportamiento de bloqueo pesimista. Normalmente, los registros se bloquean cuando se editan y permanecen no disponibles hasta que el método de **actualización** se ha completado.

 El bloqueo pesimista se habilita al abrir el objeto de **conjunto de registros** con el parámetro **LockType** o la propiedad establecida en **adLockPessimistic**. Vea también bloqueo optimista.

 agrupar una optimización de rendimiento basada en el uso de colecciones de recursos previamente asignados, como objetos o conexiones de bases de datos. Es más eficaz dibujar un recurso existente desde el grupo que crear un nuevo recurso.

 ProgID (identificador de programación) un nombre único asignado al registro de Windows mediante una aplicación COM. El ProgID para una conexión ADO es "ADODB. Conexión ". Vea también CLSID, COM.

 proxy un objeto específico de la interfaz que proporciona el cálculo de referencias y la comunicación de parámetros necesarios para que un cliente llame a un objeto de aplicación que se está ejecutando en un entorno de ejecución diferente, como en un subproceso diferente o en otro proceso. El proxy se encuentra en el cliente y se comunica con el código auxiliar correspondiente que se encuentra en el objeto de aplicación al que se está llamando. Vea también código auxiliar.

## <a name="r"></a>R
 URL relativa una dirección URL parcialmente calificada que especifica un recurso en Internet o una intranet cuya ubicación es relativa a un punto de partida especificado por una dirección URL absoluta o un objeto de conexión ADO equivalente. En efecto, las direcciones URL absolutas y relativas concatenadas consituten una dirección URL completa. Vea también dirección URL y dirección URL absoluta.

 origen de datos remoto: un origen de datos que existe en otro equipo, en lugar de en el sistema local (donde se ejecuta la aplicación cliente).

 registro de recursos un registro de un proveedor de origen de documento que contiene campos para la definición y la descripción de una carpeta o un documento. El documento en sí no está incluido en el registro de recursos, pero normalmente el flujo predeterminado o un campo del registro de recursos que contiene una dirección URL pueden acceder a él. Vea también proveedor de origen de documento, flujo predeterminado, dirección URL.

 conjunto de filas conjunto de filas de un origen de datos, que tienen el mismo esquema de campo. Un conjunto de filas puede representar todos o algunos campos de una tabla. Un conjunto de filas también puede representar una tabla virtual, creada por una consulta o una combinación de dos o más tablas. En ADO, los conjuntos de filas se representan mediante objetos de **conjunto de registros** .

## <a name="s"></a>S
 Ámbito el intervalo de referencia de un objeto o una variable, o un intervalo de registros de una vista o una tabla. Por ejemplo, solo se puede hacer referencia a las variables locales dentro del procedimiento en el que se definieron. Se puede acceder a las variables públicas desde cualquier lugar de la aplicación. Los objetos, como la base de datos actual, se encuentran en el ámbito si se encuentran en la ruta de búsqueda definida. Los intervalos de registro se pueden especificar con una cláusula Scope en muchos comandos.

 Software de proveedor de servicios que encapsula un servicio mediante la generación y el consumo de datos, lo que aumenta las características de las aplicaciones de ADO. Es un proveedor que no expone directamente los datos, sino que proporciona un servicio, como el procesamiento de consultas. El proveedor de servicios puede procesar los datos proporcionados por un proveedor de datos. Vea también proveedor de datos.

 Conjunto **de registros con** forma de conjunto cuyas columnas se han definido específicamente para contener no solo los datos, sino que también hace referencia a otros objetos de conjunto de **registros** o valores calculados basados en otros objetos de **conjunto** de registros.

 elemento relacionado: dos o más nodos de una estructura jerárquica que se encuentran en el mismo nivel de la jerarquía. El nodo raíz de una jerarquía no tiene elementos del mismo nivel.

 procedimiento almacenado: colección precompilada de código como instrucciones SQL e instrucciones de control de flujo opcionales almacenadas bajo un nombre y procesadas como una unidad. Los procedimientos almacenados se almacenan en una base de datos; se pueden ejecutar con una llamada desde una aplicación y permitir variables declaradas por el usuario, la ejecución condicional y otras características de programación eficaces.

 código auxiliar de un objeto específico de la interfaz que proporciona el cálculo de referencias y la comunicación de parámetros necesarios para que un objeto de aplicación reciba llamadas de un cliente que se ejecuta en un entorno de ejecución diferente, como en un subproceso diferente o en otro proceso. El código auxiliar se encuentra con el objeto de aplicación y se comunica con el proxy correspondiente que se encuentra en el cliente que lo llama. Vea también proxy.

 subnodo vea secundario.

 operación sincrónica una operación iniciada por el código que se completa antes de que se inicie la siguiente operación. Vea también operación asincrónica.

## <a name="t-z"></a>T-Z
 Árbol una estructura que representa una relación jerárquica entre elementos (nodos). Hay un nodo en el nivel superior de un árbol (la raíz). Debajo de la raíz, puede haber varios elementos secundarios. A su vez, cada elemento secundario puede ser el elemento primario de otros elementos secundarios, con lo que se bifurca como un árbol. Una carpeta que contiene documentos y otras carpetas es un ejemplo típico de una estructura de árbol. Vea también jerarquía, nodo, raíz, elemento secundario y primario.

 Servidor Web equipo que proporciona servicios web y páginas a los usuarios de la intranet y de Internet.
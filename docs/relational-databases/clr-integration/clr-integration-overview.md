---
title: Información general sobre la integración CLR | Microsoft Docs
description: Microsoft SQL Server hospedar CLR se denomina integración CLR. La creación en código administrado puede mejorar el rendimiento. SQL Server utiliza CAS para ayudar a proteger el código administrado.
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], about CLR integration
- extended stored procedures [SQL Server], vs. managed code
- objects [CLR integration]
- Transact-SQL vs. managed code
- managed code [SQL Server], vs. Transact-SQL
- managed code [SQL Server], vs. extended stored procedures
- execution at client vs. execution at server [CLR integration]
ms.assetid: 5aa176da-3652-4afa-a742-4c40c77ce5c3
author: rothja
ms.author: jroth
ms.openlocfilehash: 667ded44c653f71902893f160b6231ad8b751789
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341450"
---
# <a name="clr-integration---overview"></a>Integración CLR: información general
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Common Language Runtime (CLR) es el núcleo de Microsoft .NET Framework y proporciona el entorno de ejecución de todo el código de .NET Framework. Se hace referencia al código que se ejecuta en CLR como código administrado. CLR proporciona varias funciones y servicios necesarios para la ejecución de programas, que incluyen la compilación Just-In-Time (JIT), la asignación y administración de memoria, el forzado de la seguridad de tipos, el control de excepciones, la administración de subprocesos y la seguridad.  Para obtener información, vea .NET Framework SDK.  
  
 Con CLR hospedado en Microsoft SQL Server (denominado integración CLR), puede crear procedimientos almacenados, desencadenadores, funciones definidas por el usuario, tipos definidos por el usuario y agregados definidos por el usuario en código administrado. Dado que el código administrado se compila a código nativo antes de la ejecución, puede lograr un aumento importante del rendimiento en algunas situaciones.  
  
 El código administrado utiliza Seguridad de acceso del código (CAS) para evitar que los ensamblados realicen ciertas operaciones. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa CAS para ayudar a proteger el código administrado y evitar poner en peligro el sistema operativo o el servidor de bases de datos.  
  
## <a name="advantages-of-clr-integration"></a>Ventajas de la integración CLR  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] está diseñado específicamente para el acceso directo a los datos y la manipulación de la base de datos. Aunque [!INCLUDE[tsql](../../includes/tsql-md.md)] destaca en el acceso y administración de datos, no es un lenguaje de programación completo. Por ejemplo, [!INCLUDE[tsql](../../includes/tsql-md.md)] no admite matrices, colecciones, bucles for-each, desplazamiento bit a bit o clases. Aunque algunas de estas construcciones se pueden simular en [!INCLUDE[tsql](../../includes/tsql-md.md)], el código administrado tiene compatibilidad integrada para estas construcciones. Dependiendo de la situación, estas características pueden proporcionar una razón de peso para implementar cierta funcionalidad de base de datos en el código administrado.  
  
 Microsoft Visual Basic .NET y Microsoft Visual C# proporcionan capacidades orientadas a objetos como la encapsulación, la herencia y el polimorfismo. El código relacionado se puede organizar ahora con facilidad en las clases y espacios de nombres. Cuando trabaje con grandes cantidades de código del servidor, esto le permitirá organizar y mantener más fácilmente el código.  
  
 El código administrado es más adecuado que [!INCLUDE[tsql](../../includes/tsql-md.md)] para los cálculos y la lógica de ejecución complicada y presenta una amplia compatibilidad para muchas tareas complejas, como son el tratamiento de cadenas y las expresiones regulares. Con la funcionalidad incluida en la biblioteca de .NET Framework, tiene acceso a miles de clases y rutinas previamente integradas. Se puede tener acceso a éstas con facilidad desde cualquier procedimiento almacenado, desencadenador o función definida por el usuario. La Biblioteca de clases base (BCL) incluye las clases que proporcionan la funcionalidad para la manipulación de cadenas de caracteres, operaciones matemáticas avanzadas, el acceso a archivos, criptografía, etc.  
  
> [!NOTE]  
>  Mientras muchas de estas clases están disponibles para su uso desde el código CLR en SQL Server, las que no son adecuadas para su uso en el servidor (por ejemplo, las clases de selección de ventana), no están disponibles. Para obtener más información, consulte [supported .NET Framework Libraries](../../relational-databases/clr-integration/database-objects/supported-net-framework-libraries.md).  
  
 Uno de las ventajas del código administrado es la seguridad de tipos o la garantía de que el código tiene acceso a los tipos solo de maneras bien definidas y permitidas. Antes de que se ejecute el código administrado, CLR comprueba que el código es seguro. Por ejemplo, se comprueba el código para asegurarse de que no se lea ninguna memoria que no se haya escrito previamente. CLR puede ayudar también a asegurarse de que el código no manipula la memoria no administrada.  
  
 La integración CLR proporciona el potencial para el rendimiento mejorado. Para obtener más información, vea [rendimiento de la integración CLR](../../relational-databases/clr-integration/clr-integration-architecture-performance.md).  
 
> [!WARNING]
>  CLR usa la seguridad de acceso del código (CAS) de .NET Framework, que ya no se admite como un límite de seguridad. Un ensamblado CLR creado con la opción `PERMISSION_SET = SAFE` puede tener acceso a los recursos externos del sistema, llamar a código no administrado y adquirir privilegios sysadmin. A partir de [!INCLUDE[sssql17](../../includes/sssql17-md.md)], se incluye una opción de `sp_configure` denominada `clr strict security` para mejorar la seguridad de los ensamblados CLR. La opción `clr strict security` está habilitada de forma predeterminada y trata los ensamblados `SAFE` y `EXTERNAL_ACCESS` como si estuvieran marcados con `UNSAFE`. La opción `clr strict security` se puede deshabilitar para permitir la compatibilidad con versiones anteriores, pero no se recomienda hacerlo. Microsoft recomienda que todos los ensamblados estén firmados con un certificado o clave asimétrica con el correspondiente inicio de sesión que tenga concedido el permiso `UNSAFE ASSEMBLY` en la base de datos maestra. Para obtener más información, vea [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md). 
  
## <a name="choosing-between-transact-sql-and-managed-code"></a>Elegir entre Transact-SQL y el código administrado  
 Al escribir procedimientos almacenados, desencadenadores y funciones definidas por el usuario, una decisión que debe tomar es si va a utilizar el lenguaje [!INCLUDE[tsql](../../includes/tsql-md.md)] tradicional o un lenguaje .NET Framework como Visual Basic .NET o Visual C#. Use [!INCLUDE[tsql](../../includes/tsql-md.md)] si el código va a realizar principalmente un acceso a los datos con poca o ninguna lógica de procedimientos. Use el código administrado para las funciones con uso importante de CPU y procedimientos que presentan una lógica compleja o si desea utilizar la BCL de .NET Framework.  
  
### <a name="choosing-between-execution-in-the-server-and-execution-in-the-client"></a>Elegir entre ejecución en el servidor y ejecución en el cliente  
 Otro factor a la hora de decidirse sobre si se debe utilizar [!INCLUDE[tsql](../../includes/tsql-md.md)] o el código administrado es la ubicación donde desea que resida el código, el equipo servidor o el equipo cliente. [!INCLUDE[tsql](../../includes/tsql-md.md)] y el código administrado se pueden ejecutar en el servidor. Esto sitúa el código y los datos juntos y le permite aprovechar la capacidad de procesamiento del servidor. Por otro lado, quizá prefiera evitar situar las tareas con un consumo de procesador elevado en el servidor de bases de datos. La mayoría de los equipos cliente actuales son muy eficaces y quizá prefiera aprovechar esta capacidad de procesamiento situando cuanto más código sea posible en el cliente. El código administrado se puede ejecutar en un equipo cliente, mientras que [!INCLUDE[tsql](../../includes/tsql-md.md)] no se puede.  
  
## <a name="choosing-between-extended-stored-procedures-and-managed-code"></a>Elegir entre los procedimientos almacenados extendidos y el código administrado  
 Se pueden crear procedimientos almacenados extendidos para llevar a cabo funcionalidades que no son posibles con los procedimientos almacenados de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Los procedimientos almacenados extendidos pueden, sin embargo, poner en peligro la integridad del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mientras que el código administrado, cuya seguridad de tipos se comprueba, no puede. Además, la administración de memoria, la programación de subprocesos y fibras y los servicios de sincronización se integran más plenamente entre el código administrado de CLR y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Con la integración CLR, tiene una manera más segura que los procedimientos almacenados extendidos de escribir los procedimientos almacenados necesarios para realizar las tareas que no son posibles en [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para obtener más información acerca de la integración CLR y los procedimientos almacenados extendidos, vea [rendimiento de la integración CLR](../../relational-databases/clr-integration/clr-integration-architecture-performance.md).  
  
## <a name="see-also"></a>Consulte también  
 [Instalación del .NET Framework](https://technet.microsoft.com/library/ms166014\(v=SQL.105\).aspx)   
 [Arquitectura de la integración CLR](./clr-integration-architecture-clr-hosted-environment.md)   
 [Acceso a datos desde objetos de base de datos CLR](../../relational-databases/clr-integration/data-access/data-access-from-clr-database-objects.md)   
 [Introducción a la integración CLR](../../relational-databases/clr-integration/database-objects/getting-started-with-clr-integration.md)  
  

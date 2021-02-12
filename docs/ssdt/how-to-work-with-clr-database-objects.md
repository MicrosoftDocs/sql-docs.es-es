---
title: Trabajo con objetos de base de datos CLR
description: Aprenda a usar lenguajes de .NET Framework para recuperar y actualizar datos. Vea cómo crear y usar un objeto de base de datos Common Language Runtime (CLR) en un proyecto.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.allowsqlclrdebugging
ms.assetid: 4a28d43d-eb5e-444d-aace-5df691f38709
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 02f5aa47948ee5cdbf2cc7f95d749a5f56e47328
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017955"
---
# <a name="how-to-work-with-clr-database-objects"></a>Procedimientos: Trabajo con objetos de base de datos CLR

Además del lenguaje de programación Transact\-SQL, puede usar los lenguajes .NET Framework para crear objetos de base de datos que recuperen y actualicen los datos. Los objetos de base de datos que se escriben en código administrado se denominan objetos de base de datos de Common Language Runtime (CLR) de SQL Server. Para ver una explicación de las ventajas de usar objetos de base de datos CLR hospedados en SQL Server y de cómo elegir entre Transact\-SQL y CLR, consulte [Ventajas de la integración CLR](../relational-databases/clr-integration/clr-integration-overview.md) y [Ventajas de utilizar código administrado para crear objetos de base de datos](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100)).  
  
Para crear un objeto de base de datos CLR con SQL Server Data Tools, cree un proyecto de base de datos y, después, agréguele un objeto de base de datos CLR. A diferencia de lo que ocurre en versiones anteriores de Visual Studio, no es necesario crear un proyecto CLR diferente y agregarle una referencia desde el proyecto de base de datos. Al compilar y publicar el proyecto de base de datos, se publican automáticamente los objetos CLR del proyecto al mismo tiempo. Después de publicar estos objetos CLR, se les puede llamar y se pueden ejecutar como cualquier otro objeto de base de datos.  
  
Las páginas de propiedades CLR y Compilación de CLR contienen muchas configuraciones para usar objetos de base de datos CLR en el proyecto. En concreto, la página de propiedades CLR tiene una configuración de nivel de permisos para establecer permisos en el ensamblado CLR. También tiene la configuración “Generar DDL” para controlar si se genera el archivo DDL para los objetos de base de datos CLR agregados al proyecto. La página de propiedades Compilación de CLR contiene todas las opciones de compilador que puede establecer para configurar la compilación de código CLR en el proyecto. Se puede tener acceso a estas páginas de propiedades si se hace clic con el botón derecho en el proyecto en el **Explorador de soluciones** y se selecciona **Propiedades**.  
  
Para habilitar la depuración de objetos de base de datos CLR, abra el **Explorador de objetos de SQL Server**. Haga clic con el botón derecho en el servidor que contiene los artefactos de base de datos CLR que desea depurar y elija **Permitir depuración SQL/CLR**. Aparecerá un cuadro de mensaje con la advertencia: "Tenga en cuenta que durante la depuración se detendrán todos los subprocesos administrados en el servidor. ¿Desea habilitar la depuración CLR de SQL en este servidor?". Cuando depure objetos de base de datos CLR, si interrumpe la ejecución, se detendrán todos los subprocesos del servidor, lo cual afectará a otros usuarios. Por esta razón, no debería depurar aplicaciones para objetos de base de datos CLR en un servidor de producción. Asimismo, debe tener en cuenta que una vez iniciada la depuración, será demasiado tarde para cambiar la configuración en el **Explorador de objetos de SQL Server**. Los cambios realizados en el **Explorador de objetos de SQL Server** no surtirán efecto hasta el inicio de la sesión de depuración siguiente.  
  
Para más información sobre los requisitos para compilar objetos de base de datos CLR, consulte los temas correspondientes en [Compilar objetos de base de datos con la integración de Common Language Runtime (CLR)](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-add-a-clr-database-object-to-your-project"></a>Para agregar un objeto de base de datos CLR a un proyecto  
  
1.  Haga clic con el botón derecho en el proyecto de base de datos **TradeDev** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, haga clic en **Nuevo elemento**.  
  
2.  Seleccione la plantilla **C# SQL CLR** y, a continuación, haga clic en **Función definida por el usuario de CLR de SQL**. Acepte el nombre predeterminado y haga clic en **Agregar**.  
  
3.  Agregue el código siguiente al cuerpo de la clase. Esta función valida un número de teléfono de EE. UU. Debe constar de 3 caracteres numéricos, opcionalmente entre paréntesis, seguidos de un conjunto de 3 caracteres numéricos y después un conjunto de 4 caracteres numéricos. He aquí algunos ejemplos de formatos admitidos: (425) 555-0123, 425-555-0123, 425 555 0123 y 1-425-555-0123.  
  
    ```  
  
    [SqlFunction(IsDeterministic = true, IsPrecise = true)]  
    public static SqlBoolean validatePhone(SqlString phone)  
    {  
        string aNorthAmericanPhoneNumberPattern = @"^[01]?[- .]?(\([2-9]\d{2}\)|[2-9]\d{2})[- .]?\d{3}[- .]?\d{4}$";  
        if (!phone.IsNull)  
        {  
           Regex regex = new Regex(aNorthAmericanPhoneNumberPattern);  
           return regex.IsMatch(phone.Value);  
        }  
        return true;  
     }  
    ```  
  
4.  Observe que `Regex` está subrayado en rojo. Haga clic con el botón derecho en `Regex` y seleccione **Resolver** y, después, **using System.Text.RegularExpressions**.  
  
5.  Si está desarrollando en una instancia de servidor de Microsoft SQL Server 2012, puede omitir este paso. De lo contrario, SQL Server 2005 y SQL Server 2008 solo admiten proyectos de base de datos compilados con la versión 2.0, 3.0 o 3.5 de .NET Framework. Para asegurarse de que la plataforma .NET de destino está establecida correctamente, haga clic con el botón derecho en el proyecto de base de datos **TradeDev** en el **Explorador de soluciones** y, a continuación, seleccione **Propiedades**. En la página de propiedades **SQLCLR**, cambie la **Plataforma de destino** a **.NET Framework 3.5** u otra inferior. Haga clic en **Sí** en la última pantalla para cerrar y volver a abrir el proyecto.  
  
6.  Haga clic con el botón derecho en el proyecto **TradeDev** y seleccione **Compilar** para compilar el proyecto.  
  
7.  Haga doble clic en Suppliers.sql y seleccione **Diseñador de vistas** para abrir la tabla Suppliers en el Diseñador de tablas.  
  
8.  Haga clic en la fila vacía en la cuadrícula de columnas para agregar una nueva columna a la tabla. Escriba **phone** en el campo **Nombre**, **nvarchar (128)** en **Tipo de datos** y deje activado el campo **Permitir valores NULL**.  
  
9. Haga clic con el botón derecho en el nodo **Restricciones CHECK** en el panel Contexto y seleccione **Agregar nueva restricción CHECK**.  
  
10. Reemplace la definición predeterminada de la restricción en el panel de scripts con lo siguiente.  
  
    ```  
    CONSTRAINT [CK_Suppliers_CheckPhone] CHECK (dbo.validatePhone(phone)=1),  
    ```  
  
    De esta forma, cualquier dato que se escriba en el nuevo campo de teléfono se comprobará usando la función definida por el usuario de CLR que agregamos previamente.  
  
11. Presione F5 para compilar e implementar el proyecto en la base de datos local.  
  
### <a name="to-use-clr-database-objects"></a>Para usar objetos de base de datos CLR  
  
1.  En el **Explorador de objetos de SQL Server**, vaya a la base de datos local en la que esté implementando el proyecto.  
  
2.  De manera predeterminada, la integración con CLR está desactivada en SQL Server. Para usar objetos de base de datos CLR, se debe habilitar la integración con CLR. Para ello, use la opción "clr enabled" del procedimiento almacenado sp_configure. Para más información, consulte el tema sobre [la opción clr habilitada](../relational-databases/clr-integration/clr-integration-enabling.md).  
  
    Haga clic con el botón derecho en la base de datos y seleccione **Nueva consulta**. En el panel de consulta, pegue el código siguiente y haga clic en el botón **Ejecutar consulta**.  
  
    ```  
  
    sp_configure 'clr enabled', 1;  
    GO  
    RECONFIGURE;  
    GO  
    ```  
  
3.  Haga clic con el botón derecho en la tabla Suppliers y seleccione **Ver datos**.  
  
4.  Escriba **5** en **id**, **Contoso** en **name**, deje vacío el campo **Address** y escriba **425 3122 1222** en **phone**. Al salir del campo **phone**, aparecerá un mensaje que indica que la instrucción `INSERT` entra en conflicto con la restricción CHECK existente, que comprueba la entrada del campo **phone** usando modelo de teléfono predefinido.  
  
5.  Cambie la entrada a **425 312 1222** y salga del campo. Observe que ahora se acepta la entrada.  
  
## <a name="see-also"></a>Consulte también  
[Ventajas de la integración CLR](../relational-databases/clr-integration/clr-integration-overview.md)  
[Ventajas de utilizar código administrado para crear objetos de base de datos](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100))  
[Crear objetos de base de datos con la integración de Common Language Runtime (CLR)](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md)  

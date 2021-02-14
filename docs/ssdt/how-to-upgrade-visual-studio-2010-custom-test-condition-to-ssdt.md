---
title: Actualización de una condición de prueba personalizada de Visual Studio 2010 desde una versión anterior
description: Obtenga información sobre cómo actualizar una condición de prueba personalizada de Visual Studio 2010 para usarla en SQL Server Data Tools. Vea qué cambios realizar y cómo instalar la condición.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 44c895a3-dee0-4032-a60f-812f5fe3c713
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2e0f9813db19e06729f7164f0ca419801364a365
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018055"
---
# <a name="how-to-upgrade-a-visual-studio-2010-custom-test-condition-from-a-previous-release-to-sql-server-data-tools"></a>Procedimientos: Actualización de una condición de prueba personalizada de Visual Studio 2010 desde una versión anterior a SQL Server Data Tools

Para usar una condición de prueba unitaria creada en una versión anterior a SQL Server Data Tools, debe actualizarla:  
  
-   [Actualizar referencias](#UpdateReferences)  
  
-   [Actualizar atributos de clase y referencias de tipo](#UpdateClassAttributesandTypeReference)  
  
-   [Instalar la condición de prueba actualizada](#ApplytheNewRegistrationProcess)  
  
## <a name="update-references"></a><a name="UpdateReferences"></a>Actualizar referencias  
Para actualizar las referencias del proyecto:  
  
1.  Visual Basic solo: en el **Explorador de soluciones**, haga clic en **Mostrar todos los archivos**.  
  
2.  En el **Explorador de soluciones**, expanda el nodo **Referencias**.  
  
3.  Haga clic con el botón derecho en las referencias de ensamblado siguientes y, a continuación, haga clic en **Quitar**:  
  
    1.  Microsoft.Data.Schema.UnitTesting  
  
    2.  Microsoft.Data.Schema  
  
4.  En el menú **Proyecto** o haciendo clic con el botón derecho en la carpeta de proyecto en el **Explorador de soluciones**, haga clic en **Agregar referencia**.  
  
5.  Haga clic en la pestaña **.NET**.  
  
6.  En la lista **Nombre de componente**, seleccione **System.ComponentModel.Composition** y haga clic en **Aceptar**.  
  
7.  Agregue las referencias de ensamblado necesarias. Haga clic con el botón derecho en el nodo del proyecto y, a continuación, haga clic en **Agregar referencia**. Haga clic en **Examinar** y navegue a la carpeta C:\Archivos de programa (x86)\\MicrosoftSQL Server\110\DAC\Bin. Seleccione Microsoft.Data.Tools.Schema.Sql.dll, haga clic en Agregar y, a continuación, haga clic en Aceptar.  
  
8.  En el menú **Proyecto**, haga clic en **Descargar el proyecto**.  
  
9. Haga clic con el botón derecho en el **Proyecto** en el **Explorador de soluciones** y elija **Editar**`project_name` **.csproj**.  
  
10. Agregue la siguiente instrucción Import después de la importación de `Microsoft.CSharp.targets`:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
11. Guarde el archivo y ciérrelo. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y elija **Volver a cargar el proyecto**.  
  
12. Abra la clase de condición de prueba y quite todas las instrucciones using que empiezan por **Microsoft.Data.Schema**. La forma más sencilla de hacerlo consiste en hacer clic con el botón derecho en el archivo y elegir **Organizar instrucciones Using** y, a continuación, **Quitar y ordenar**. Se deben quitar las siguientes instrucciones using:  
  
    ```  
    using Microsoft.Data.Schema.UnitTesting;  
    using Microsoft.Data.Schema.UnitTesting.Conditions;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema;  
    ```  
  
13. Agregue las siguientes instrucciones using al archivo si no están presentes:  
  
    ```  
    using System.ComponentModel;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
    ```  
  
La condición de prueba emplea ahora referencias de ensamblado de pruebas unitarias de SQL Server.  
  
## <a name="update-class-attributes-and-type-references"></a><a name="UpdateClassAttributesandTypeReference"></a>Actualizar atributos de clase y referencias de tipo  
Reemplazar los antiguos atributos de clase de pruebas unitarias con un nuevo atributo. La extensibilidad de pruebas unitarias de SQL Server se basa ahora en Managed Extensibility Framework (MEF). También debe actualizar algunas referencias de tipo.  
  
### <a name="update-class-attributes"></a>Actualizar atributos de clase  
Actualice el código de la manera siguiente:  
  
1.  Quite el atributo o los atributos `DatabaseSchemaProviderCompatibility`. La característica de extensibilidad de la versión anterior necesitaba estos atributos, que no se admiten en las pruebas unitarias de SQL Server.  
  
2.  Quite el atributo `DisplayName`. El nombre para mostrar se incluye en el nuevo atributo.  
  
3.  Agregue el nuevo atributo `ExportTestCondition`. Este atributo debe estar presente para que la condición de prueba se detecte y utilice en las pruebas unitarias de SQL Server. `ExportTestCondition` y reemplaza los atributos `DatabaseSchemaProviderCompatibility`. `ExportTestCondition` requiere dos parámetros:  
  
    -   `DisplayName` es el primer parámetro. Reemplaza al atributo `DisplayName` y se emplea para describir todas las condiciones de prueba de este tipo.  
  
    -   El segundo parámetro se usa únicamente para identificar la extensión. Puede pasarlo simplemente en el tipo mediante `typeof(NewTestCondition)`, ya que debe ser único. Sin embargo, también se puede pasar un id. de cadena si lo prefiere.  
  
4.  La definición de clase se debe cambiar de la manera siguiente:  
  
    Antes:  
  
    ```  
    [DatabaseSchemaProviderCompatibility(typeof(DatabaseSchemaProvider))]  
    [DatabaseSchemaProviderCompatibility(null)]  
    [DisplayName("NewTestCondition")]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
    Después:  
  
    ```  
    [ExportTestCondition("NewTestCondition ", typeof(NewTestCondition))]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
### <a name="update-type-references"></a>Actualizar referencias de tipo  
Algunos nombres de tipo han cambiado en el marco de pruebas unitarias de SQL Server. Para actualizar el código de manera que use los nuevos nombres de tipo, use **Buscar y reemplazar** en el menú **Edición**. Los nombres de tipo empiezan ahora con **Sql**. Los nombres de clase se deben actualizar de la manera siguiente:  
  
|Nombre de tipo anterior|Nuevo nombre de tipo|  
|-----------------|-----------------|  
|`ExecutionResult`|`SqlExecutionResult`|  
  
## <a name="install-the-upgraded-test-condition"></a><a name="ApplytheNewRegistrationProcess"></a>Instalar la condición de prueba actualizada  
En las versiones anteriores de las pruebas unitarias de base de datos, tenía que instalar la condición de prueba en la caché global de ensamblados o crear un archivo XML que contuviera la información de ensamblado. Con las pruebas unitarias de SQL Server, ya no es necesario este proceso adicional. (Para más información, consulte [Compilar el proyecto e instalar la condición de prueba](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md#xxx).  
  
Después de actualizar las referencias, compruebe que el ensamblado está firmado y compilado.  
  
Después, copie el archivo de ensamblado del directorio de salida, que de manera predeterminada es Mis documentos\Visual Studio 2010\Projects\\<yoursolutionname>\\<yourprojectname>\bin\Debug\\), al directorio %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions. Cuando Visual Studio se inicie, identificará las extensiones del directorio TestConditions y hará que estén disponibles para usarlas en la sesión:  
  
## <a name="upgrade-existing-tests-that-need-to-use-the-new-test-condition"></a>Actualizar pruebas existentes que necesitan usar la nueva condición de prueba  
Busque todos los proyectos de prueba que usen la condición de prueba anterior y que necesiten usar la nueva condición. Asegúrese de que estos proyectos de prueba se hayan actualizado. Para más información, consulte [Actualizar un proyecto de prueba anterior que contiene pruebas unitarias de base de datos](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md).  
  
Quite la referencia de ensamblado a la condición de prueba anterior.  
  
Agregue una nueva prueba unitaria de SQL Server al proyecto para crear una referencia de ensamblado a la condición de prueba actualizada del proyecto. Se debe agregar una clase de prueba para crear la referencia. Puede eliminar la clase de prueba una vez agregada la referencia.  
  
## <a name="see-also"></a>Consulte también  
[Condiciones de prueba personalizadas para pruebas unitarias de SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
  

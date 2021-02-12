---
title: Crear un componente de tiempo de diseño de elemento de informe personalizado | Microsoft Docs
description: Aprenda a crear un componente de tiempo de diseño de elemento de informe personalizado. Este componente proporciona una superficie de diseño activada que puede aceptar operaciones de arrastrar y colocar.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, creating
ms.assetid: 323fd58a-a462-4c48-b188-77ebc0b4212e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1b5ccf62185045dc8d3efaccfe00e26a01f01a9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015665"
---
# <a name="creating-a-custom-report-item-design-time-component"></a>Crear un componente de tiempo de diseño de elemento de informe personalizado
  Un componente de tiempo de diseño de elemento de informe personalizado es un control que se puede utilizar en el entorno de Visual Studio Report Designer. El componente de tiempo de diseño de elemento de informe personalizado proporciona una superficie de diseño activada que puede aceptar las operaciones de arrastrar y colocar, la integración con el explorador de propiedades de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], y la capacidad de proporcionar los editores de propiedades personalizados.  
  
 Con un componente de tiempo de diseño de elemento de informe personalizado, el usuario puede colocar un elemento de informe personalizado en un informe en el entorno de diseño, establecer las propiedades de datos personalizadas en el elemento de informe personalizado y, a continuación, guardar el elemento de informe personalizado como parte del proyecto de informe.  
  
 Las propiedades que se establecen utilizando el componente de tiempo de diseño en el entorno de desarrollo se serializan y deserializan por el entorno de diseño del host y, a continuación, se guardan como elementos en el archivo de lenguaje RDL (Report Definition Language). Cuando el procesador de informes ejecuta el informe, pasa las propiedades que están establecidas utilizando el componente de tiempo de diseño a un componente de tiempo de ejecución del elemento de informe personalizado, que representa el elemento de informe personalizado y lo devuelve al procesador de informes.  
  
> [!NOTE]
>  El componente de tiempo de diseño de elemento de informe personalizado se implementa como un componente de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. En este documento se describirán los detalles de la implementación específicos al componente de tiempo de diseño del elemento de informe personalizado. Para más información sobre cómo desarrollar componentes mediante [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vea [Componentes en Visual Studio](https://go.microsoft.com/fwlink/?LinkId=116576) en MSDN Library.  
  
 Para obtener un ejemplo de un elemento de informe personalizado totalmente implementado, vea [Ejemplos del producto SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="implementing-a-design-time-component"></a>Implementar un componente de tiempo de diseño  
 La clase principal de un componente de tiempo de diseño de elemento de informe personalizado se hereda de la clase **Microsoft.ReportDesigner.CustomReportItemDesigner**. Además de los atributos estándar usados para un control [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], la clase de componente debería definir un atributo **CustomReportItem**. Este atributo debe corresponder al nombre del elemento de informe personalizado como se define en el archivo reportserver.config. Para obtener una lista de atributos [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vea Atributos en la documentación de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK.  
  
 El ejemplo de código siguiente muestra atributos que se aplican a un control de tiempo de diseño del elemento de informe personalizado:  
  
```csharp  
namespace PolygonsCRI  
{  
    [LocalizedName("Polygons")]  
    [Editor(typeof(CustomEditor), typeof(ComponentEditor))]  
        [ToolboxBitmap(typeof(PolygonsDesigner),"Polygons.ico")]  
        [CustomReportItem("Polygons")]  
  
    public class PolygonsDesigner : CustomReportItemDesigner  
    {  
...  
```  
  
### <a name="initializing-the-component"></a>Inicializar el componente  
 Las propiedades especificadas por el usuario para un elemento de informe personalizado se pasan utilizando una clase <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData>. La implementación de la clase **CustomReportItemDesigner** debería reemplazar el método **InitializeNewComponent** para crear una nueva instancia de la clase <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> del componente y establecerla en los valores predeterminados.  
  
 El ejemplo de código siguiente muestra un ejemplo de una clase de componente de tiempo de diseño de elemento de informe personalizado que reemplaza al método **CustomReportItemDesigner.InitializeNewComponent** para inicializar la clase <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> del componente:  
  
```csharp  
public override void InitializeNewComponent()  
        {  
            CustomData = new CustomData();  
            CustomData.DataRowHierarchy = new DataHierarchy();  
  
            // Shape grouping  
            CustomData.DataRowHierarchy.DataMembers.Add(new DataMember());  
            CustomData.DataRowHierarchy.DataMembers[0].Group = new Group();  
            CustomData.DataRowHierarchy.DataMembers[0].Group.Name = Name + "_Shape";  
            CustomData.DataRowHierarchy.DataMembers[0].Group.GroupExpressions.Add(new ReportExpression());  
  
            // Point grouping  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers.Add(new DataMember());  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group = new Group();  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group.Name = Name + "_Point";  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group.GroupExpressions.Add(new ReportExpression());  
  
            // Static column  
            CustomData.DataColumnHierarchy = new DataHierarchy();  
            CustomData.DataColumnHierarchy.DataMembers.Add(new DataMember());  
  
            // Points  
            IList<IList<DataValue>> dataValues = new List<IList<DataValue>>();  
            CustomData.DataRows.Add(dataValues);  
            CustomData.DataRows[0].Add(new List<DataValue>());  
            CustomData.DataRows[0][0].Add(NewDataValue("X", ""));  
            CustomData.DataRows[0][0].Add(NewDataValue("Y", ""));  
        }  
```  
  
### <a name="modifying-component-properties"></a>Modificar propiedades de componentes  
 Puede modificar las propiedades **CustomData** en el entorno de diseño de varias maneras. Puede modificar cualquier propiedad que expuesta por el componente de tiempo de diseño que esté marcada con el atributo <xref:System.ComponentModel.BrowsableAttribute> utilizando el explorador de propiedades de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Además, puede modificar las propiedades si arrastra los elementos a la superficie de diseño del elemento de informe personalizado o si hace clic con el botón derecho en el control en el entorno de diseño y selecciona **Propiedades** en el menú contextual para mostrar una ventana de propiedades personalizada.  
  
 El siguiente ejemplo de código muestra una propiedad **Microsoft.ReportDesigner.CustomReportItemDesigner.CustomData** que tiene aplicado el atributo <xref:System.ComponentModel.BrowsableAttribute>:  
  
```csharp  
[Browsable(true), Category("Data")]  
public string DataSetName  
{  
      get  
      {  
         return CustomData.DataSetName;  
      }  
      set  
      {  
         CustomData.DataSetName = value;  
      }  
   }  
  
```  
  
 Puede proporcionar un cuadro de diálogo de editor de propiedades personalizado al componente de tiempo de diseño. La implementación personalizada del editor de propiedades debería heredar de la clase <xref:System.ComponentModel.ComponentEditor> y debería crear una instancia de un cuadro de diálogo que se puede utilizar para la edición de propiedad.  
  
 En el ejemplo siguiente se muestra una implementación de una clase que hereda de <xref:System.ComponentModel.ComponentEditor> y muestra un cuadro de diálogo de editor de propiedades personalizado:  
  
```csharp  
internal sealed class CustomEditor : ComponentEditor  
{  
   public override bool EditComponent(  
      ITypeDescriptorContext context, object component)  
    {  
     PolygonsDesigner designer = (PolygonsDesigner)component;  
     PolygonProperties dialog = new PolygonProperties();  
     dialog.m_designerComponent = designer;  
     DialogResult result = dialog.ShowDialog();  
     if (result == DialogResult.OK)  
     {  
        designer.Invalidate();  
        designer.ChangeService().OnComponentChanged(designer, null, null, null);  
        return true;  
     }  
     else  
        return false;  
    }  
}  
```  
  
 El cuadro de diálogo de editor de propiedades personalizado puede invocar al editor de expresiones del diseñador de informes. En el ejemplo siguiente, el editor de expresiones se invoca cuando el usuario selecciona el primer elemento del cuadro combinado:  
  
```csharp  
private void EditableCombo_SelectedIndexChanged(object sender,   
    EventArgs e)  
{  
   ComboBox combo = (ComboBox)sender;  
   if (combo.SelectedIndex == 0 && m_launchEditor)  
   {  
      m_launchEditor = false;  
      ExpressionEditor editor = new ExpressionEditor();  
      string newValue;  
      newValue = (string)editor.EditValue(null, m_designerComponent.Site, m_oldComboValue);  
      combo.Items[0] = newValue;  
   }  
}  
  
```  
  
### <a name="using-designer-verbs"></a>Utilizar los verbos de diseñador  
 Un verbo de diseñador es un comando de menú vinculado a un controlador de eventos. Puede agregar verbos de diseñador que aparecerán en el menú contextual de un componente cuando el control de tiempo de ejecución del elemento de informe personalizado se utiliza en el entorno de diseño. Puede devolver la lista de verbos de diseñador disponibles desde el componente de tiempo de ejecución con la propiedad **Verbs**.  
  
 El ejemplo de código siguiente muestra un verbo de diseñador y un controlador de eventos agregados a <xref:System.ComponentModel.Design.DesignerVerbCollection>, así como el código de controlador de eventos:  
  
```csharp  
public override DesignerVerbCollection Verbs  
{  
    get  
    {  
        if (m_verbs == null)  
        {  
            m_verbs = new DesignerVerbCollection();  
            m_verbs.Add(new DesignerVerb("Proportional Scaling", new EventHandler(OnProportionalScaling)));  
         m_verbs[0].Checked = (GetCustomProperty("poly:Proportional") == bool.TrueString);  
        }  
  
        return m_verbs;  
    }  
}  
  
private void OnProportionalScaling(object sender, EventArgs e)  
{  
   bool proportional = !  
        (GetCustomProperty("poly:Proportional") == bool.TrueString);  
   m_verbs[0].Checked = proportional;  
   SetCustomProperty("poly:Proportional", proportional.ToString());  
   ChangeService().OnComponentChanged(this, null, null, null);  
   Invalidate();  
}  
```  
  
### <a name="using-adornments"></a>Utilizar las opciones gráficas  
 Las clases de elemento de informe personalizado también pueden implementar una clase **Microsoft.ReportDesigner.Design.Adornment**. Una opción gráfica permite al control de elemento de informe personalizado proporcionar las áreas fuera del rectángulo principal de la superficie de diseño. Estas áreas pueden administrar los eventos de interfaz de usuario, como los clics del mouse y las operaciones de arrastrar y colocar. La clase **Adornment** que se define en el espacio de nombres **Microsoft.ReportDesigner** de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] es una implementación de paso a través de la clase <xref:System.Windows.Forms.Design.Behavior.Adorner> de Windows Forms. Para obtener la documentación completa de la clase **Adorner**, vea [Información general sobre servicios de comportamiento](/previous-versions/ms171826(v=vs.140)) en MSDN Library. Para obtener el código de ejemplo que implementa una clase **Microsoft.ReportDesigner.Design.Adornment**, vea [Ejemplos del producto SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
 Para obtener más información acerca de cómo programar y usar Windows Forms en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], vea estos temas en MSDN Library:  
  
-   Atributos de tiempo de diseño para componentes  
  
-   Componentes de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]  
  
-   Tutorial: Creación de un control de Windows Forms que aproveche las características de tiempo de diseño de Visual Studio  
  
## <a name="see-also"></a>Consulte también  
 [Arquitectura de elementos de informe personalizados](../../reporting-services/custom-report-items/custom-report-item-architecture.md)   
 [Creación de un componente de tiempo de ejecución de elemento de informe personalizado](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [Bibliotecas de clases de elemento de informe personalizado](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)   
 [Cómo: Implementar un elemento de informe personalizado](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  

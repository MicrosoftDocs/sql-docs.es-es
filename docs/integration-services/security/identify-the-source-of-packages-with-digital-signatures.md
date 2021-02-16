---
description: Identificar el origen de paquetes con firmas digitales
title: Identificar el origen de paquetes con firmas digitales | Microsoft Docs
ms.custom: security
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.digitalsigning.f1
helpviewer_keywords:
- signing packages [Integration Services]
- certificates [Integration Services]
- packages [Integration Services], security
- security [Integration Services], certificates
- signing policies [Integration Services]
ms.assetid: a433fbef-1853-4740-9d5e-8a32bc4ffbb2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f4607ffe77d7fe93aff86d8dc707bb86e515d67d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340797"
---
# <a name="identify-the-source-of-packages-with-digital-signatures"></a>Identificar el origen de paquetes con firmas digitales

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Es posible firmar un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con un certificado digital para identificar su origen. Una vez que el paquete está firmado con un certificado digital, se puede configurar [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para que compruebe la firma digital antes de cargarlo. Para hacer que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] compruebe la firma, puede establecer una opción en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o en la utilidad **dtexec** (dtexec.exe), o bien establecer un valor opcional del Registro.  
  
## <a name="sign-a-package-with-a-digital-certificate"></a>Firmar un paquete con un certificado digital  
 Antes de poder firmar un paquete con un certificado digital, debe obtener o crear el certificado. Cuando lo consiga, puede utilizar el certificado para firmar el paquete. Para obtener más información sobre cómo obtener un certificado y firmar un paquete con él, vea [Firmar un paquete mediante un certificado digital](#cert).  
  
## <a name="set-an-option-to-check-the-package-signature"></a>Establecer una opción para comprobar la firma del paquete  
 Tanto [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] como la utilidad **dtexec** tienen una opción que configura [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para que compruebe la firma digital de un paquete firmado. El uso de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o de la utilidad **dtexec** depende de si desea comprobar todos los paquetes o simplemente algunos de ellos:  
  
-   Para comprobar la firma digital de todos los paquetes antes de cargarlos en tiempo de diseño, establezca la opción **Comprobar la firma digital al cargar un paquete** en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Esta opción es un valor de configuración global para todos los paquetes en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].
  
-   Para comprobar la firma digital de un paquete concreto, especifique la opción **/VerifyS[igned]** al usar la utilidad **dtexec** para ejecutar el paquete. Para obtener más información, consulte [utilidad dtexec](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="set-a-registry-value-to-check-package-signature"></a>Establecer un valor del Registro para comprobar la firma del paquete  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] también admite un valor opcional del Registro, **BlockedSignatureStates**, que puede utilizarse para administrar la directiva de una organización para cargar los paquetes firmados y sin firmar. El valor del Registro puede evitar que los paquetes se carguen si están sin firmar o si tienen firmas que no sean válidas o de confianza. Para obtener más información sobre cómo establecer este valor del Registro, vea [Implementar una directiva de firma estableciendo un valor del Registro](#registry).  
  
> **NOTA:** El valor del Registro **BlockedSignatureStates** opcional puede especificar un parámetro de configuración más restrictivo que la opción de firma digital establecida en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o en la línea de comandos de **dtexec** . En esta situación, el valor del Registro más restrictivo invalida los demás valores.  

## <a name="implement-a-signing-policy-by-setting-a-registry-value"></a><a name="registry"></a> Implementar una directiva de firma estableciendo un valor del Registro
  Se puede usar un valor opcional del Registro para administrar la directiva de una organización para la carga de paquetes firmados o sin firmar. Si utiliza este valor del Registro, debe crearlo en cada equipo en el que se ejecutarán los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y en el que desea exigir el cumplimiento de la directiva. Una vez establecido el valor del Registro, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprobará las firmas antes de cargar los paquetes.  
  
 En este procedimiento de este tema se describe cómo agregar el valor DWORD opcional **BlockedSignatureStates** a la clave del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS. El valor de los datos de **BlockedSignatureStates** determina si se debe bloquear un paquete que tenga una firma que no sea de confianza o una firma no válida, o esté sin firmar. Con respecto al estado de las firmas que se utilizan para firmar paquetes, el valor del Registro **BlockedSignatureStates** usa las siguientes definiciones:  
  
-   Una *firma válida* es una firma que puede leerse correctamente.  
  
-   Una *firma no válida* es una firma para la que la suma de comprobación descifrada (algoritmo hash unidireccional del código de paquete cifrado con una clave privada) no coincide con la suma de comprobación descifrada que se calcula como parte del proceso de carga de paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Una *firma de confianza* es una firma creada mediante un certificado digital firmado por una entidad de certificación raíz de confianza. Este valor de configuración no exige que el firmante se encuentre en la lista de publicadores de confianza del usuario.  
  
-   Una *firma que no es de confianza* es una firma que no se puede comprobar que haya sido emitida por una entidad de certificación raíz de confianza o una firma que no es actual.  
  
 En la tabla siguiente se enumeran los valores válidos de los datos DWORD y su directiva asociada.  
  
|Value|Descripción|  
|-----------|-----------------|  
|0|Sin restricción administrativa.|  
|1|Bloquear firmas no válidas.<br /><br /> Este valor no bloquea los paquetes sin firmar.|  
|2|Bloquear las firmas no válidas y las que no sean de confianza.<br /><br /> Este valor no bloquea los paquetes sin firmar, pero bloquea las firmas generadas automáticamente.|  
|3|Bloquear las firmas no válidas, las que no sean de confianza y los paquetes sin firmar.<br /><br /> Este valor también bloquea las firmas generadas automáticamente.|  
  
> [!NOTE]  
>  El valor recomendado para **BlockedSignatureStates** es 3. Este valor proporciona la protección óptima frente a paquetes sin firmar o firmas que no son válidas o no son de confianza. No obstante, es posible que el valor recomendado no sea adecuado para todas las circunstancias. Para obtener más información sobre la firma de activos digitales, vea el tema de[introducción a la firma de código](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85))en MSDN Library.  
  
### <a name="to-implement-a-signing-policy-for-packages"></a>Para implementar una directiva de firma para paquetes  
  
1.  En el menú **Inicio** , haga clic en **Ejecutar**.  
  
2.  En el cuadro de diálogo Ejecutar, escriba **Regedit** y haga clic en **Aceptar**.  
  
3.  Busque la clave del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS.  
  
4.  Haga clic con el botón derecho en **MSDTS**, seleccione **Nuevo** y, después, haga clic en **Valor DWORD**.  
  
5.  Actualice el nombre del nuevo valor a **BlockedSignatureStates**.  
  
6.  Haga clic con el botón derecho en **BlockedSignatureStates** y haga clic en **Modificar**.  
  
7.  En el cuadro de diálogo **Editar valor DWORD** , escriba el valor 0, 1, 2 o 3.  
  
8.  Haga clic en **OK**.  
  
9. En el menú **Archivo** , haga clic en **Salir**.    

## <a name="sign-a-package-by-using-a-digital-certificate"></a><a name="cert"></a> Firmar un paquete mediante un certificado digital
  En este tema se describe cómo firmar un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con un certificado digital. Puede utilizar una firma digital, junto con otros valores de configuración, para evitar que se cargue y se ejecute un paquete que no es válido.  
  
 Antes de poder firmar un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] debe realizar las tareas siguientes:  
  
-   Crear u obtener una clave privada para asociarla al certificado y almacenarla en el equipo local.  
  
-   Obtener un certificado para firmar código de una entidad de certificación de confianza. Puede utilizar uno de los métodos siguientes para obtener o crear un certificado:  
  
    -   Obtener un certificado de una entidad de certificación pública comercial que se dedique a emitirlos.  
  
    -   Obtener un certificado de un servidor de certificados que permita a una organización emitir certificados internamente. Tiene que agregar el certificado raíz utilizado para firmar dicho certificado al almacén **Entidades de certificación raíz de confianza** . Para ello, puede utilizar el complemento Certificados de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC). Para obtener más información, vea el tema "[Certificate Services](/windows/win32/seccrypto/certificate-services)" en MSDN Library.  
  
    -   Crear su propio certificado solo con fines de pruebas. La herramienta Creación de certificados (Makecert.exe) genera certificados X.509 solo a efectos de pruebas. Para obtener más información, vea el tema "[Herramienta Creación de certificados (Makecert.exe)](/previous-versions/dotnet/netframework-2.0/bfsktky3(v=vs.80))" en MSDN Library.  
  
     Para obtener más información sobre los certificados, vea la Ayuda en pantalla para el complemento Certificados. Para obtener más información sobre cómo firmar activos digitales, vea el tema "[Signing and Checking Code with Authenticode](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537364(v=vs.85))" en MSDN Library.  
  
-   Asegurarse de que el certificado se ha habilitado para la firma de código. Para determinar si un certificado está habilitado para la firma de código, revise las propiedades del certificado en el complemento Certificados.  
  
-   Almacenar el certificado en el almacén personal.  
  
 Una vez completadas las tareas anteriores, puede utilizar el procedimiento siguiente para firmar un paquete.  
  
### <a name="to-sign-a-package"></a>Para firmar un paquete  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que debe firmarse.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  En el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] , en el menú **SSIS** , haga clic en **Firma digital**.  
  
4.  En el cuadro de diálogo **Firma digital** , haga clic en **Firmar**.  
  
5.  En el cuadro de diálogo **Seleccionar el certificado** , seleccione un certificado.  
  
6.  (Opcional) Haga clic en **Ver certificado** para ver la información del certificado.  
  
7.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Seleccionar un certificado** .  
  
8.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Firma digital** .  
  
9. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  
  
     Aunque se haya firmado el paquete, también debe configurar [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para comprobar la firma digital antes de cargar el paquete.  

## <a name="digital-signing-dialog-box-ui-reference"></a><a name="signing_dialog"></a> Referencia de la interfaz de usuario del cuadro de diálogo Firma digital
  Utilice el cuadro de diálogo **Firma digital** para firmar un paquete con una firma digital o para quitar la firma. El cuadro de diálogo **Firma digital** está disponible en la opción de **Firma digital** del menú **SSIS** en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Para más información, consulte [Firmar un paquete mediante un certificado digital](#cert).  
  
### <a name="options"></a>Opciones  
 **Firmar**  
 Haga clic para abrir el cuadro de diálogo **Seleccionar certificado** y seleccione el certificado que quiere usar.  
  
 **Remove**  
 Haga clic para quitar la firma digital.  

## <a name="see-also"></a>Vea también  
 [Paquetes de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Información general sobre seguridad &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
  

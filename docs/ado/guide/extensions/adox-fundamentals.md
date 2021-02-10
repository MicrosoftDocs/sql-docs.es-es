---
description: Aspectos básicos de ADOX
title: Aspectos básicos de ADOX | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADOX, fundamentals
ms.assetid: 954476fc-5f72-4ada-ace5-d9acb27d18f8
author: rothja
ms.author: jroth
ms.openlocfilehash: 6b03c71180a3d32ca3a592c063d5a267f187964c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032177"
---
# <a name="adox-fundamentals"></a>Aspectos básicos de ADOX
Microsoft® ActiveX® Data Objects Extensions for Data Definition Language and Security (ADOX) es una extensión de los objetos de ADO y del modelo de programación. ADOX incluye objetos para la creación y modificación de esquemas, así como para la seguridad. Dado que se trata de un enfoque basado en objetos para la manipulación de esquemas, puede escribir código que funcione con distintos orígenes de datos, independientemente de las diferencias en sus sintaxis nativas.  
  
 ADOX es una biblioteca complementaria para los objetos principales de ADO. Expone objetos adicionales para crear, modificar y eliminar objetos de esquema, como tablas y procedimientos. También incluye los objetos de seguridad para mantener a los usuarios y grupos, y para conceder y revocar los permisos en los objetos.  
  
 Para usar ADOX con la herramienta de desarrollo, debe establecer una referencia a la biblioteca de tipos ADOX. La descripción de la biblioteca ADOX es "Microsoft ADO ext. for DDL and Security". El nombre de archivo de la biblioteca ADOX es Msadox.dll y el identificador de programa (ProgID) es "ADOX". Para obtener más información acerca de cómo establecer referencias a bibliotecas, consulte la documentación de la herramienta de desarrollo.  
  
 El proveedor de Microsoft OLE DB para Microsoft Jet Motor de base de datos es totalmente compatible con ADOX. Es posible que algunas características de ADOX no se admitan, en función del proveedor de datos.  
  
 En este documento se asume un conocimiento práctico del lenguaje de programación de Microsoft® Visual Basic® y un conocimiento general de ADO. Para obtener más información acerca de ADO, vea la [Guía del programador de ADO](../ado-programmer-s-guide.md). Para obtener información general sobre ADOX, vea los temas siguientes:  
  
-   [Modelo de objetos ADOX](../../reference/adox-api/adox-object-model.md)  
  
-   [Objetos ADOX](../../reference/adox-api/adox-objects.md)  
  
-   [Colecciones de ADOX](../../reference/adox-api/adox-collections.md)  
  
-   [Propiedades ADOX](../../reference/adox-api/adox-properties.md)  
  
-   [Métodos ADOX](../../reference/adox-api/adox-methods.md)  
  
-   [Ejemplos de ADOX](../../reference/adox-api/adox-code-examples.md)  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ADOX](../../reference/adox-api/adox-object-model.md)   
 [Ejemplos de código ADOX](../../reference/adox-api/adox-code-examples.md)   
 [Colecciones de ADOX](../../reference/adox-api/adox-collections.md)   
 [Constantes enumeradas de ADOX](../../reference/adox-api/adox-enumerated-constants.md)   
 [Métodos ADOX](../../reference/adox-api/adox-methods.md)   
 [Modelo de objetos ADOX](../../reference/adox-api/adox-object-model.md)   
 [Objetos ADOX](../../reference/adox-api/adox-objects.md)   
 [Propiedades de ADOX](../../reference/adox-api/adox-properties.md)   
 [ADO (multidimensional) (ADO MD)](../multidimensional/ado-multidimensional-ado-md.md)   
 [Programador de ADO ' s guía para el uso de objetos ADO](../ado-programmer-s-guide.md)

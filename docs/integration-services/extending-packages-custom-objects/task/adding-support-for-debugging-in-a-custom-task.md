---
description: Agregar compatibilidad con la depuración de una tarea personalizada
title: Agregar compatibilidad con la depuración de una tarea personalizada | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- BreakpointManager class
- breakpoints [Integration Services]
- custom tasks [Integration Services], debugging
- IDTSSuspend interface
- IDTSBreakpointSite interface
- SSIS custom tasks, debugging
- debugging [Integration Services], custom tasks
ms.assetid: 7f06e49b-0b60-4e81-97da-d32dc248264a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 28ba35dc2aecd6c25da114493b8c6e424b77dccf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346536"
---
# <a name="adding-support-for-debugging-in-a-custom-task"></a>Agregar compatibilidad con la depuración de una tarea personalizada

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  El motor de ejecución de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] permite suspender paquetes, tareas y otros tipos de contenedores durante la ejecución mediante puntos de interrupción. El uso de puntos de interrupción le permite revisar y corregir los errores que impiden que la aplicación o tareas se ejecuten correctamente. La arquitectura de punto de interrupción permite al cliente evaluar el valor en tiempo de ejecución de los objetos del paquete en los puntos definidos de ejecución mientras se suspende el procesamiento de la tarea.  
  
 Los programadores de tareas personalizadas pueden utilizar esta arquitectura para crear destinos de punto de interrupción personalizados mediante la interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> y su interfaz primaria, <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>. La interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> define la interacción entre el motor de ejecución y la tarea para crear y administrar sitios o destinos de puntos de interrupción personalizados. La interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> proporciona los métodos y propiedades a los que llama el motor de ejecución para notificar a la tarea que suspenda o reanude la ejecución.  
  
 Un sitio o destino de punto de interrupción es un punto en la ejecución de la tarea donde se puede suspender el procesamiento. Los usuarios seleccionan entre los sitios de punto de interrupción disponibles en el cuadro de diálogo **Establecer puntos de interrupción**. Por ejemplo, además de las opciones de punto de interrupción predeterminadas, el contenedor de bucles Foreach proporciona la opción "Interrumpir al principio de cada iteración del bucle".  
  
 Cuando una tarea alcanza un destino de punto de interrupción durante la ejecución, evalúa dicho destino para determinar si está habilitado un punto de interrupción. Esto indica que el usuario desea que la ejecución se detenga en ese punto de interrupción. Si el punto de interrupción está habilitado, la tarea provoca el evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A> en el motor de ejecución. El motor de ejecución responde al evento llamando al método **Suspend** de cada tarea que se está ejecutando actualmente en el paquete. La ejecución de la tarea se reanuda cuando el módulo ejecutable llama al método **ResumeExecution** de la tarea suspendida.  
  
 Las tareas que no utilizan los puntos de interrupción deberían implementar todavía las interfaces <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> y <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>. Esto asegura que la tarea se suspende correctamente cuando otros objetos del paquete provocan eventos <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>.  
  
## <a name="idtsbreakpointsite-interface-and-breakpointmanager"></a>Interfaz IDTSBreakpointSite y BreakpointManager  
 Las tareas crean los destinos de punto de interrupción llamando al método <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.CreateBreakpointTarget%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>, proporcionando un identificador entero y una descripción de cadena como parámetros. Cuando la tarea alcanza el punto en el código que contiene un destino de punto de interrupción, evalúa dicho destino mediante el método <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.IsBreakpointTargetEnabled%2A> para determinar si ese punto de interrupción está habilitado. Si es **true**, la tarea lo notifica al motor de ejecución provocando el evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>.  
  
 La interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> define un método único, <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite.AcceptBreakpointManager%2A>, al que el motor de ejecución llama durante la creación de la tarea. Este método proporciona el objeto <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager> como un parámetro, que a continuación lo utiliza la tarea para crear y administrar los puntos de interrupción. Las tareas deben almacenar <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager> localmente para su uso durante los métodos **Validate** y **Execute**.  
  
 En el código de ejemplo siguiente se muestra cómo crear un destino de punto de interrupción mediante <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>. El ejemplo llama al método <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A> para provocar el evento.  
  
```csharp  
public void AcceptBreakpointManager( BreakpointManager breakPointManager )  
{  
   //   Store the breakpoint manager locally.  
   this.bpm  = breakPointManager;  
}  
public override DTSExecResult Execute( Connections connections,  
  Variables variables, IDTSComponentEvents events,  
  IDTSLogging log, DtsTransaction txn)  
{  
   //   Create a breakpoint.  
   this.bpm.CreateBreakPointTarget( 1 , "A sample breakpoint target." );  
...  
   if( this.bpm.IsBreakpointTargetEnabled( 1 ) == true )  
      events.OnBreakpointHit( this.bpm.GetBreakpointTarget( 1 ) );  
}  
```  
  
```vb  
Public Sub AcceptBreakpointManager(ByVal breakPointManager As BreakpointManager)  
  
   ' Store the breakpoint manager locally.  
   Me.bpm  = breakPointManager  
  
End Sub  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
  ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
  ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' Create a breakpoint.  
   Me.bpm.CreateBreakPointTarget(1 , "A sample breakpoint target.")  
  
   If Me.bpm.IsBreakpointTargetEnabled(1) = True Then  
      events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(1))  
   End If  
  
End Function  
```  
  
## <a name="idtssuspend-interface"></a>Interfaz IDTSSuspend  
 La interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> define los métodos a los que llama el motor de ejecución cuando pone en pausa o reanuda la ejecución de una tarea. La interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> implementa la interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>, y la tarea personalizada invalida normalmente los métodos **Suspend** y **ResumeExecution**. Cuando el motor en tiempo de ejecución recibe un evento **OnBreakpointHit** de una tarea, llama al método **Suspend** de cada tarea en ejecución, notificando a las tareas para que se pongan en pausa. Cuando el cliente reanuda la ejecución, el motor en tiempo de ejecución llama al método **ResumeExecution** de las tareas que están suspendidas.  
  
 La suspensión y reanudación de la ejecución de la tarea implica poner en pausa y reanudar el subproceso de ejecución de la tarea. En código administrado, para hacer esto se utiliza la clase **ManualResetEvent** en el espacio de nombres **System.Threading** de .NET Framework.  
  
 En el ejemplo de código siguiente se muestra la suspensión y reanudación de la ejecución de una tarea. Observe que el método **Execute** ha cambiado respecto al ejemplo de código anterior, y el subproceso de ejecución se pone en pausa al activar el punto de interrupción.  
  
```csharp  
private ManualResetEvent m_suspended = new ManualResetEvent( true );  
private ManualResetEvent m_canExecute = new ManualResetEvent( true );  
private int   m_suspendRequired = 0;  
private int   m_debugMode = 0;  
  
public override DTSExecResult Execute( Connections connections, Variables variables, IDTSComponentEvents events, IDTSLogging log, DtsTransaction txn)  
{  
   // While a task is not executing, it is suspended.    
   // Now that we are executing,  
   // change to not suspended.  
   ChangeEvent(m_suspended, false);  
  
   // Check for a suspend before doing any work,   
   // in case the suspend and execute calls  
   // were initiated at virtually the same time.  
   CheckAndSuspend();  
   CheckAndFireBreakpoint( componentEvents, 1);  
}  
private void CheckAndSuspend()  
{  
   // Loop until we can execute.    
   // The loop is required rather than a simple If  
   // because there is a time between the return from WaitOne and the  
   // reset that we might receive another Suspend call.    
   // Suspend() will see that we are suspended  
   // and return.  So we need to rewait.  
   while (!m_canExecute.WaitOne(0, false))  
   {  
      ChangeEvent(m_suspended, true);  
      m_canExecute.WaitOne();  
      ChangeEvent(m_suspended, false);  
   }  
}  
private void CheckAndFireBreakpoint(IDTSComponentEvents events, int breakpointID)  
{  
   // If the breakpoint is enabled, fire it.  
   if (m_debugMode != 0 &&    this.bpm.IsBreakpointTargetEnabled(breakpointID))  
   {  
      //   Enter a suspend mode before firing the breakpoint.    
      //   Firing the breakpoint will cause the runtime   
      //   to call Suspend on this task.    
      //   Because we are blocked on the breakpoint,   
      //   we are suspended.  
      ChangeEvent(m_suspended, true);  
      events.OnBreakpointHit(this.bpm.GetBreakpointTarget(breakpointID));  
      ChangeEvent(m_suspended, false);  
   }  
   // Check for a suspension for two reasons:   
   //   1. If we are at a point where we could fire a breakpoint,   
   //      we are at a valid suspend point.  Even if we didn't hit a  
   //      breakpoint, the runtime may have called suspend,   
   //      so check for it.       
   //   2. Between the return from OnBreakpointHit   
   //      and the reset of the event, it is possible to have  
   //      received a suspend call from which we returned because   
   //      we were already suspended.  We need to be sure it is okay  
   //      to continue executing now.  
   CheckAndSuspend();  
}  
static void ChangeEvent(ManualResetEvent e, bool shouldSet)  
{  
   bool succeeded;  
   if (shouldSet)  
      succeeded = e.Set();  
   else  
      succeeded = e.Reset();  
  
   if (!succeeded)  
      throw new Exception("Synchronization object failed.");  
  
}  
public bool SuspendRequired  
{  
   get   {return m_suspendRequired != 0;}  
   set  
   {  
      // This lock is also taken by Suspend().    
      // Because it is possible for the package to be  
      // suspended and resumed in quick succession,   
      // this property might be set before  
      // the actual Suspend() call.    
      // Without the lock, the Suspend() might reset the canExecute  
      // event after we set it to abort the suspension.  
      lock (this)  
      {  
         Interlocked.Exchange(ref m_suspendRequired, value ? 1 : 0);  
         if (!value)  
            ResumeExecution();  
      }  
   }  
}  
public void ResumeExecution()  
{  
   ChangeEvent( m_canExecute,true );  
}  
public void Suspend()  
{  
   // This lock is also taken by the set SuspendRequired method.    
   // It prevents this call from overriding an   
   // aborted suspension.  See comments in set SuspendRequired.  
   lock (this)  
   {  
      // If a Suspend is required, do it.  
      if (m_suspendRequired != 0)  
         ChangeEvent(m_canExecute, false);  
   }  
   // We can't return from Suspend until the task is "suspended".  
   // This can happen one of two ways:   
   // the m_suspended event occurs, indicating that the execute thread  
   // has suspended, or the canExecute flag is set,   
   // indicating that a suspend is no longer required.  
   WaitHandle [] suspendOperationComplete = {m_suspended, m_canExecute};  
   WaitHandle.WaitAny(suspendOperationComplete);  
}  
```  
  
```vb  
Private m_suspended As ManualResetEvent = New ManualResetEvent(True)  
Private m_canExecute As ManualResetEvent = New ManualResetEvent(True)  
Private m_suspendRequired As Integer = 0  
Private m_debugMode As Integer = 0  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' While a task is not executing it is suspended.    
   ' Now that we are executing,  
   ' change to not suspended.  
   ChangeEvent(m_suspended, False)  
  
   ' Check for a suspend before doing any work,   
   ' in case the suspend and execute calls  
   ' were initiated at virtually the same time.  
   CheckAndSuspend()  
   CheckAndFireBreakpoint(componentEvents, 1)  
  
End Function  
  
Private Sub CheckAndSuspend()  
  
   ' Loop until we can execute.    
   ' The loop is required rather than a simple if  
   ' because there is a time between the return from WaitOne and the  
   ' reset that we might receive another Suspend call.    
   ' Suspend() will see that we are suspended  
   ' and return.  So we need to rewait.  
   Do While Not m_canExecute.WaitOne(0, False)  
              ChangeEvent(m_suspended, True)  
              m_canExecute.WaitOne()  
              ChangeEvent(m_suspended, False)  
   Loop  
  
End Sub  
  
Private Sub CheckAndFireBreakpoint(ByVal events As IDTSComponentEvents, _  
ByVal breakpointID As Integer)  
  
   ' If the breakpoint is enabled, fire it.  
   If m_debugMode <> 0 AndAlso Me.bpm.IsBreakpointTargetEnabled(breakpointID) Then  
              '   Enter a suspend mode before firing the breakpoint.    
              '   Firing the breakpoint will cause the runtime   
              '   to call Suspend on this task.    
              '   Because we are blocked on the breakpoint,   
              '   we are suspended.  
              ChangeEvent(m_suspended, True)  
              events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(breakpointID))  
              ChangeEvent(m_suspended, False)  
   End If  
  
   ' Check for a suspension for two reasons:   
   '   1. If we are at a point where we could fire a breakpoint,   
   '         we are at a valid suspend point.  Even if we didn't hit a  
   '         breakpoint, the runtime may have called suspend,   
   '         so check for it.     
   '   2. Between the return from OnBreakpointHit   
   '         and the reset of the event, it is possible to have  
   '         received a suspend call from which we returned because   
   '         we were already suspended.  We need to be sure it is okay  
   '         to continue executing now.  
   CheckAndSuspend()  
  
End Sub  
  
Shared Sub ChangeEvent(ByVal e As ManualResetEvent, ByVal shouldSet As Boolean)  
  
   Dim succeeded As Boolean  
   If shouldSet Then  
              succeeded = e.Set()  
   Else  
              succeeded = e.Reset()  
   End If  
  
   If (Not succeeded) Then  
              Throw New Exception("Synchronization object failed.")  
   End If  
  
End Sub  
  
Public Property SuspendRequired() As Boolean  
   Get  
               Return m_suspendRequired <> 0  
  End Get  
  Set  
    ' This lock is also taken by Suspend().    
     '   Because it is possible for the package to be  
     '   suspended and resumed in quick succession,   
     '   this property might be set before  
     '   the actual Suspend() call.    
     '   Without the lock, the Suspend() might reset the canExecute  
     '   event after we set it to abort the suspension.  
              SyncLock Me  
                         Interlocked.Exchange(m_suspendRequired,IIf(Value, 1, 0))  
                         If (Not Value) Then  
                                    ResumeExecution()  
                         End If  
              End SyncLock  
   End Set  
End Property  
  
Public Sub ResumeExecution()  
   ChangeEvent(m_canExecute,True)  
End Sub  
  
Public Sub Suspend()  
  
   ' This lock is also taken by the set SuspendRequired method.    
   ' It prevents this call from overriding an   
   ' aborted suspension.  See comments in set SuspendRequired.  
   SyncLock Me  
   ' If a Suspend is required, do it.  
              If m_suspendRequired <> 0 Then  
                         ChangeEvent(m_canExecute, False)  
              End If  
   End SyncLock  
   ' We can't return from Suspend until the task is "suspended".  
   ' This can happen one of two ways:   
   ' the m_suspended event occurs, indicating that the execute thread  
   ' has suspended, or the canExecute flag is set,   
   ' indicating that a suspend is no longer required.  
   Dim suspendOperationComplete As WaitHandle() = {m_suspended, m_canExecute}  
   WaitHandle.WaitAny(suspendOperationComplete)  
  
End Sub  
```  
  
## <a name="see-also"></a>Consulte también  
 [Depurar el flujo de control](../../../integration-services/troubleshooting/debugging-control-flow.md)  
  
  

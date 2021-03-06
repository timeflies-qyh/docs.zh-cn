---
title: 使用跟踪对应用程序进行故障排除
ms.date: 03/30/2017
ms.assetid: 8851adde-c3c2-4391-9523-d8eb831490af
ms.openlocfilehash: b07e850810734082568ddca9776a72575c986094
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837553"
---
# <a name="using-tracking-to-troubleshoot-applications"></a>使用跟踪对应用程序进行故障排除
Windows Workflow Foundation （WF）可以跟踪与工作流相关的信息，以提供有关执行 Windows Workflow Foundation 应用程序或服务的详细信息。 Windows Workflow Foundation 主机能够在工作流实例执行期间捕获工作流事件。 如果工作流生成错误或异常，则可以使用 Windows Workflow Foundation 跟踪详细信息对其处理进行故障排除。  
  
## <a name="troubleshooting-a-wf-using-wf-tracking"></a>使用 WF 跟踪对 WF 进行故障诊断  
 若要检测 Windows Workflow Foundation 活动的处理中的故障，可以使用跟踪配置文件启用跟踪，该跟踪配置文件查询状态为 "出错" 的 <xref:System.Activities.Tracking.ActivityStateRecord>。 在以下代码中指定了相应的查询。  
  
```xml  
<activityStateQueries>  
              <activityStateQuery activityName="*">  
                <states>  
                  <state name="Faulted" />  
                </states>  
              </activityStateQuery>  
 </activityStateQueries>  
```  
  
 如果某个错误发生传播并且在错误处理程序（如 <xref:System.Activities.Statements.TryCatch> 活动）中得到了处理，则可以使用 <xref:System.Activities.Tracking.FaultPropagationRecord> 检测到此错误。 <xref:System.Activities.Tracking.FaultPropagationRecord> 可指示错误的源活动以及错误处理程序的名称。 <xref:System.Activities.Tracking.FaultPropagationRecord> 包含错误的异常堆栈形式的错误详细信息。 下面的示例演示了用于订阅 <xref:System.Activities.Tracking.FaultPropagationRecord> 的查询。  
  
```xml  
<faultPropagationQueries>  
              <faultPropagationQuery faultSourceActivityName ="*" faultHandlerActivityName="*"/>  
 </faultPropagationQueries>  
```  
  
 如果某个错误未在工作流中处理，则它会导致工作流实例出现未经处理的异常并且工作流实例被中止。 若要了解未经处理的异常的详细信息，跟踪配置文件必须查询具有以下示例中指定的 `state name="UnhandledException"` 的工作流实例记录。  
  
```xml  
<workflowInstanceQueries>  
              <workflowInstanceQuery>  
                <states>  
                  <state name="UnhandledException" />  
                </states>  
              </workflowInstanceQuery>  
</workflowInstanceQueries>  
```  
  
 当工作流实例遇到未经处理的异常时，如果已启用 Windows Workflow Foundation 跟踪，则会发出 <xref:System.Activities.Tracking.WorkflowInstanceUnhandledExceptionRecord> 的对象。  
  
 此跟踪记录包含异常堆栈形式的错误详细信息。 它包含出错并导致未经处理的异常的错误（例如活动）源的详细信息。若要从 Windows Workflow Foundation 订阅错误事件，请通过添加跟踪参与者来启用跟踪。 通过查询 `ActivityStateQuery (state="Faulted")`、<xref:System.Activities.Tracking.FaultPropagationRecord> 和 `WorkflowInstanceQuery (state="UnhandledException")` 的跟踪配置文件配置此参与者。  
  
 如果使用 ETW 跟踪参与者启用跟踪，则会向 ETW 会话发出错误事件。 可以使用事件查看器查看这些事件。 这可以在分析通道中的节点**事件查看器 > 应用程序和服务日志-> Microsoft > Windows > 应用程序服务器-应用程序**"下找到。  
  
## <a name="see-also"></a>另请参阅

- [Windows Server App Fabric 监视](https://docs.microsoft.com/previous-versions/appfabric/ee677251(v=azure.10))
- [用 App Fabric 监视应用程序](https://docs.microsoft.com/previous-versions/appfabric/ee677276(v=azure.10))

---
title: IActionOnCLREvent::OnEvent 方法
ms.date: 03/30/2017
api_name:
- IActionOnCLREvent.OnEvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IActionOnCLREvent::OnEvent
helpviewer_keywords:
- OnEvent method [.NET Framework hosting]
- IActionOnCLREvent::OnEvent method [.NET Framework hosting]
ms.assetid: 0970f10c-4304-4c12-91c0-83e51455afb4
topic_type:
- apiref
ms.openlocfilehash: 98807717fc913052dae15f9f3cbd462d4f537ad4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126918"
---
# <a name="iactiononclreventonevent-method"></a>IActionOnCLREvent::OnEvent 方法
对已使用[ICLROnEventManager：： RegisterActionOnEvent](../../../../docs/framework/unmanaged-api/hosting/iclroneventmanager-registeractiononevent-method.md)方法的调用注册的事件执行回调。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OnEvent (  
    [in] EClrEvent event,  
    [in] PVOID     data  
);  
```  
  
## <a name="parameters"></a>参数  
 `event`  
 中[EClrEvent](../../../../docs/framework/unmanaged-api/hosting/eclrevent-enumeration.md)值之一，指示事件的类型。  
  
 `data`  
 中指向对象的指针，该对象包含有关 `event`的详细信息。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|描述|  
|-------------|-----------------|  
|S_OK|`OnEvent` 成功返回。|  
|HOST_E_CLRNOTAVAILABLE|公共语言运行时（CLR）未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 如果某个方法返回 E_FAIL，则 CLR 在该进程内将不再可用。 对任何宿主方法的后续调用都将返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>备注  
 `data` 参数是指向未指定类型的对象的指针。 如果 `event` 参数 `Event_DomainUnload`，则 `data` 是已卸载的 <xref:System.AppDomain> 的数值标识符。 主机可以使用此标识符作为键来执行适当的操作。  
  
 如果 `Event_MDAFired``event`，`data` 是指向[MDAInfo](../../../../docs/framework/unmanaged-api/hosting/mdainfo-structure.md)实例的指针，其中包含来自托管调试助手（MDA）的消息输出。 Mda 是 CLR 的一项功能，可帮助开发人员进行调试，方法是生成有关其他难以捕获的事件的 XML 消息。 在调试托管代码和非托管代码之间的转换时，此类消息特别有用。 有关详细信息，请参阅[诊断托管调试助手的错误](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)。  
  
## <a name="requirements"></a>要求  
 **平台：** 请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为资源包括在 Mscoree.dll 中  
  
 **.NET Framework 版本：** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [使用托管调试助手诊断错误](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
- [EClrEvent 枚举](../../../../docs/framework/unmanaged-api/hosting/eclrevent-enumeration.md)
- [IActionOnCLREvent 接口](../../../../docs/framework/unmanaged-api/hosting/iactiononclrevent-interface.md)
- [ICLRControl 接口](../../../../docs/framework/unmanaged-api/hosting/iclrcontrol-interface.md)
- [ICLROnEventManager 接口](../../../../docs/framework/unmanaged-api/hosting/iclroneventmanager-interface.md)
- [MDAInfo 结构](../../../../docs/framework/unmanaged-api/hosting/mdainfo-structure.md)

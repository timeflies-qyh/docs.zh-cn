---
title: ICorDebugFunction3 接口
ms.date: 03/30/2017
api_name:
- ICorDebugFunction3
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: b22717b9-ead5-4eea-887e-789b52d613dc
topic_type:
- apiref
ms.openlocfilehash: b74008e0a183d46d82c5262209d582537fd155c7
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75938075"
---
# <a name="icordebugfunction3-interface"></a>ICorDebugFunction3 接口
[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 对 ICorDebugFunction 接口进行逻辑扩展，以提供对 ReJIT 请求中的代码的访问。  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|[GetActiveReJitRequestILCode 方法](../../../../docs/framework/unmanaged-api/debugging/icordebugfunction3-getactiverejitrequestilcode-method.md)|获取一个接口指针，该指针指向包含活动 ReJIT 请求中的 IL 的[ICorDebugILCode](../../../../docs/framework/unmanaged-api/debugging/icordebugilcode-interface.md) 。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>需求  
 **平台：** 请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
- [调试](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [ReJIT：操作方法指南](https://docs.microsoft.com/archive/blogs/davbr/rejit-a-how-to-guide)

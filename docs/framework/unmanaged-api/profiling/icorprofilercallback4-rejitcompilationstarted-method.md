---
title: ICorProfilerCallback4::ReJITCompilationStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.ReJITCompilationStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::ReJITCompilationStarted
helpviewer_keywords:
- ReJITCompilationStarted method, ICorProfilerCallback4 interface [.NET Framework profiling]
- ICorProfilerCallback4::ReJITCompilationStarted method [.NET Framework profiling]
ms.assetid: 512fdd00-262a-4456-a075-365ef4133c4d
topic_type:
- apiref
ms.openlocfilehash: 074b0b11a822d2b8bcb9588484557e3e5eba69dd
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2019
ms.locfileid: "74430190"
---
# <a name="icorprofilercallback4rejitcompilationstarted-method"></a>ICorProfilerCallback4::ReJITCompilationStarted 方法
通知探查器实时（JIT）编译器已开始重新编译某个函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ReJITCompilationStarted(   
    [in] FunctionID functionId,  
    [in] ReJITID    rejitId,  
    [in] BOOL       fIsSafeToBlock);  
```  
  
## <a name="parameters"></a>参数  
 `functionId`  
 中JIT 编译器已开始重新编译的函数的 ID。  
  
 `rejitId`  
 中新版本的函数的重新编译 ID。  
  
 `fIsSafeToBlock`  
 [in] `true` 指示阻止可能导致运行时等待调用线程从此回调返回;`false`，指示阻止操作不会影响运行时的操作。 值 `true` 不会损害运行时，但会影响分析结果。  
  
## <a name="remarks"></a>备注  
 由于运行时处理类构造函数的方式，每个函数可以接收多对 `ReJITCompilationStarted` 和[ReJITCompilationFinished](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-rejitcompilationfinished-method.md)方法调用。 例如，运行时开始重新编译方法 A，但类 B 的类构造函数需要运行。 因此，运行时将为类 B 重新编译构造函数并运行它。 当构造函数正在运行时，它会调用方法 A，这将导致重新编译方法 A。 在此方案中，方法 A 的第一次重新编译将暂停。 但是，将使用 JIT 重新编译事件来报告对方法 A 的重新编译。  
  
 当两个线程同时进行回调时，探查器必须支持 JIT 重新编译回调的顺序。 例如，线程 A 调用 `ReJITCompilationStarted`;但是，在线程 A 调用[ReJITCompilationFinished](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-rejitcompilationfinished-method.md)之前，线程 B 会调用[ICorProfilerCallback：： ExceptionSearchFunctionEnter](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-exceptionsearchfunctionenter-method.md) ，其中包含来自线程 A 的 `ReJITCompilationStarted` 回调的函数 ID。由于探查器尚未收到对[ReJITCompilationFinished](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-rejitcompilationfinished-method.md)的调用，因此函数 ID 似乎不应有效。 但在这种情况下，函数 ID 有效。  
  
## <a name="requirements"></a>要求  
 **平台：** 请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-interface.md)
- [ICorProfilerCallback4 接口](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-interface.md)
- [JITCompilationFinished 方法](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-jitcompilationfinished-method.md)
- [ReJITCompilationFinished 方法](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-rejitcompilationfinished-method.md)

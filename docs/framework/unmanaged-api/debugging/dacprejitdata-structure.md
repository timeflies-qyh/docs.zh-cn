---
title: DacpReJitData 结构
ms.date: 02/01/2019
api.name:
- DacpReJitData Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- DacpReJitData Structure
helpviewer.keywords:
- DacpReJitData Structure [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: 77ef2c65157df4a033700bb8d0295875ede46554
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67739107"
---
# <a name="dacprejitdata-structure"></a>DacpReJitData 结构

定义给定探查器检测方法有关的基本信息。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
struct MSLAYOUT DacpReJitData
{
    enum Flags
    {
        kUnknown,
        kRequested,
        kActive,
        kReverted,
    };

    CLRDATA_ADDRESS                 rejitID;
    Flags                           flags;
    CLRDATA_ADDRESS                 NativeCodeAddr;
};
```

## <a name="members"></a>成员

| 成员           | 描述                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| `rejitID`        | 一种方法 ReJit 修订号。                                                          |
| `flags`          | 一个标志，指示该方法的给定版本的 ReJit 检测的当前状态。 |
| `NativeCodeAddr` | 方法的 rejitted 实现基址。                                         |

## <a name="remarks"></a>备注

此结构存在于运行时内，不通过任何标头或库文件公开。 若要使用它，如上所示定义的结构。 此外必须使用定义结构`ms_struct`打包，如果不是使用 Microsoft 编译器。

## <a name="requirements"></a>要求
**平台：** 请参阅[系统需求](../../../../docs/framework/get-started/system-requirements.md)。  
**标头：** 无  
**库：** None  
**.NET Framework 版本：** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>请参阅

- [调试](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [调试结构](../../../../docs/framework/unmanaged-api/debugging/debugging-structures.md)

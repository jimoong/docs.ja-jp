---
title: CorSetENC 列挙型
ms.date: 03/30/2017
api_name:
- CorSetENC
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorSetENC
helpviewer_keywords:
- CorSetENC enumeration [.NET Framework metadata]
ms.assetid: fe4150e8-071d-43fb-8e06-c3c616dbeed2
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: bf1be8d5c709f3d6e5991e4d33dde2e923291a95
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54569415"
---
# <a name="corsetenc-enumeration"></a>CorSetENC 列挙型
メタデータの生成中の動作を決定する値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```  
typedef enum CorSetENC {  
  
    MDSetENCOn                  = 0x00000001,  
    MDSetENCOff                 = 0x00000002,  
  
    MDUpdateENC                 = 0x00000001,  
    MDUpdateFull                = 0x00000002,  
    MDUpdateExtension           = 0x00000003,  
    MDUpdateIncremental         = 0x00000004,  
    MDUpdateDelta               = 0x00000005,  
    MDUpdateMask                = 0x00000007  
  
} CorSetENC;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`MDSetENCOn`|互換性のために残されています。|  
|`MDSetENCOff`|互換性のために残されています。|  
|`MDUpdateENC`|一方、メタデータを更新するには、トークン移動できないことを示します。|  
|`MDUpdateFull`|トークンを更新中に移動できることを示します。|  
|`MDUpdateExtension`|追加機能のみを更新できますで構成されることを示します。 トークンを移動することはできません。|  
|`MDUpdateIncremental`|コンパイルが増分であることを示します。|  
|`MDUpdateDelta`|その唯一の変更されたメタデータを保存するかを示します。|  
|`MDUpdateMask`|含まれています`MDUpdateENC`、`MDUpdateFull`と`MDUpdateIncremental`します。|  
  
## <a name="requirements"></a>必要条件  
 **プラットフォーム:**[システム要件](../../../../docs/framework/get-started/system-requirements.md)に関するページを参照してください。  
  
 **ヘッダー:** CorHdr.h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目
- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)

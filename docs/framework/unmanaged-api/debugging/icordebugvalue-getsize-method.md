---
title: ICorDebugValue::GetSize メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugValue.GetSize
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugValue::GetSize
helpviewer_keywords:
- GetSize method, ICorDebugValue interface [.NET Framework debugging]
- ICorDebugValue::GetSize method [.NET Framework debugging]
ms.assetid: 445a9ee3-e050-4f3a-931a-96b0efb00110
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: fb0030a25dd48ab4236ec2b51891e0ac41aac902
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54612620"
---
# <a name="icordebugvaluegetsize-method"></a>ICorDebugValue::GetSize メソッド
この"ICorDebugValue"オブジェクトのバイト単位のサイズを取得します。  
  
## <a name="syntax"></a>構文  
  
```  
HRESULT GetSize (  
    [out] ULONG32   *pSize  
);  
```  
  
#### <a name="parameters"></a>パラメーター  
 `pSize`  
 [out]この値のオブジェクトのバイト単位のサイズ。  
  
## <a name="remarks"></a>Remarks  
 値の型が参照型の場合は、このメソッドは、オブジェクトのサイズではなく、ポインターのサイズを返します。  
  
 `ICorDebugValue::GetSize`メソッドを返します。`COR_E_OVERFLOW`オブジェクトには、64 ビット プラットフォーム上で 4 GB より大きい。 使用して、 [icordebugvalue 3::getsize64](../../../../docs/framework/unmanaged-api/debugging/icordebugvalue3-getsize64-method.md)メソッド代わりにオブジェクトは 4 GB より大きい。  
  
## <a name="requirements"></a>必要条件  
 **プラットフォーム:**[システム要件](../../../../docs/framework/get-started/system-requirements.md)に関するページを参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [GetSize64 メソッド](../../../../docs/framework/unmanaged-api/debugging/icordebugvalue3-getsize64-method.md)

---
title: '方法: 変更セットを表示します。'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 126e7245-c5a0-4ebf-800d-cc1fcf9cd0ab
ms.openlocfilehash: e6030a48a773dcf985eee5c4c113b02386780707
ms.sourcegitcommit: d9a0071d0fd490ae006c816f78a563b9946e269a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2019
ms.locfileid: "55065818"
---
# <a name="how-to-display-a-changeset"></a>方法: 変更セットを表示します。
<xref:System.Data.Linq.DataContext> によって追跡される変更を <xref:System.Data.Linq.DataContext.GetChangeSet%2A> を使って表示することができます。  
  
## <a name="example"></a>例  
 以下の例では、住所がロンドンの顧客を取得し、これをパリに変更してから変更内容をデータベースに送信しています。  
  
 [!code-csharp[DLinqDebuggingSupport#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqDebuggingSupport/cs/Program.cs#2)]
 [!code-vb[DLinqDebuggingSupport#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqDebuggingSupport/vb/Module1.vb#2)]  
  
 このコードからの出力は次のようになります。 最後に概要として、8 か所の変更が示されることに注意してください。  

 ```console
CustomerID: AROUT
   Original value: London
   Updated value: Paris
CustomerID: BSBEV
   Original value: London
   Updated value: Paris
CustomerID: CONSH
   Original value: London
   Updated value: Paris
CustomerID: EASTC
   Original value: London
   Updated value: Paris
CustomerID: NORTS
    Original value: London
    Updated value: Paris
CustomerID: PARIS
    Original value: London
    Updated value: Paris
CustomerID: SEVES
    Original value: London
    Updated value: Paris
CustomerID: SPECD
    Original value: London
    Updated value: Paris
Total changes: {Added: 0, Removed: 0, Modified: 8}
```
  
## <a name="see-also"></a>関連項目
- [デバッグのサポート](../../../../../../docs/framework/data/adonet/sql/linq/debugging-support.md)

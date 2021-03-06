---
title: プロパティ変更イベント
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom controls [Windows Forms], property changes (using code)
- properties [Windows Forms], changes
ms.assetid: 268039ec-5aaa-4d76-b902-acccb036c850
ms.openlocfilehash: d02c6f6f3b5a3add7d78f6c3813a36d08b2b9cb3
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54584858"
---
# <a name="property-changed-events"></a>プロパティ変更イベント
という名前のプロパティのときに通知を送信する、制御したいかどうか*PropertyName*という名前のイベントの定義の変更、 *PropertyName* `Changed`という名前のメソッドと`On` *PropertyName* `Changed`イベントを発生させます。 Windows フォームでの名前付け規則では、単語を追加*Changed*プロパティの名前にします。 プロパティ変更イベントに関連付けられているイベントのデリゲート型は<xref:System.EventHandler>、イベントのデータ型は、<xref:System.EventArgs>します。 基本クラス<xref:System.Windows.Forms.Control>など多くのプロパティ変更イベントを定義<xref:System.Windows.Forms.Control.BackColorChanged>、 <xref:System.Windows.Forms.Control.BackgroundImageChanged>、 <xref:System.Windows.Forms.Control.FontChanged>、 <xref:System.Windows.Forms.Control.LocationChanged>、およびその他。 イベントに関する背景情報は、次を参照してください。[イベント](../../../../docs/standard/events/index.md)と[Windows フォーム コントロールのイベント](../../../../docs/framework/winforms/controls/events-in-windows-forms-controls.md)します。  
  
 プロパティ変更イベントは、変更に応答するイベント ハンドラーをアタッチするコントロールのコンシューマーができるため便利です。 コントロールを生成するプロパティ変更イベントに応答する場合は、オーバーライド、対応する`On` *PropertyName* `Changed`メソッド、イベントにデリゲートをアタッチする代わりにします。 その他のプロパティを更新するか、その描画サーフェイスの一部またはすべてを再描画、コントロールは通常、プロパティ変更イベントに応答します。  
  
 次の例は、どのように`FlashTrackBar`カスタム コントロールの応答から継承したプロパティ変更イベントの一部<xref:System.Windows.Forms.Control>します。 完全なサンプルでは、「[方法。進行状況を示す Windows フォーム コントロールを作成する](../../../../docs/framework/winforms/controls/how-to-create-a-windows-forms-control-that-shows-progress.md)します。  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#2](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#2)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#2](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#2)]  
  
## <a name="see-also"></a>関連項目
- [イベント](../../../../docs/standard/events/index.md)
- [Windows フォーム コントロールのイベント](../../../../docs/framework/winforms/controls/events-in-windows-forms-controls.md)
- [Windows フォーム コントロールのプロパティ](../../../../docs/framework/winforms/controls/properties-in-windows-forms-controls.md)

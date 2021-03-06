---
title: '方法: セットとして機能する Windows フォーム RadioButton コントロールをグループ化'
ms.date: 03/30/2017
helpviewer_keywords:
- radio buttons [Windows Forms], grouping
- controls [Windows Forms], grouping
- Windows Forms controls, grouping
- RadioButton control [Windows Forms], grouping
ms.assetid: 58f8fe34-50b7-49d8-a2be-c271be3c6b32
ms.openlocfilehash: 2758ff2380431668b2c908dbddd5dbe2094ccd0c
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54569337"
---
# <a name="how-to-group-windows-forms-radiobutton-controls-to-function-as-a-set"></a>方法: セットとして機能する Windows フォーム RadioButton コントロールをグループ化
Windows フォーム<xref:System.Windows.Forms.RadioButton>間うち 1 つだけをプロシージャまたはオブジェクトに割り当てることが、2 つ以上の設定の選択をユーザーに付与するコントロールが設計されています。 たとえばのグループ<xref:System.Windows.Forms.RadioButton>コントロールは、注文の配送業者をパッケージの選択を表示可能性がありますが、通信事業者の 1 つだけ使用されます。 そのため 1 つだけ<xref:System.Windows.Forms.RadioButton>一度に選択できる場合でも、機能グループの一部であります。  
  
 ラジオ ボタンをグループ化など、コンテナー内でそれらを描画することで、<xref:System.Windows.Forms.Panel>コントロール、<xref:System.Windows.Forms.GroupBox>コントロール、またはフォーム。 フォームになる 1 つのグループに直接追加されるすべてのラジオ ボタン。 個別のグループを追加するにはパネルまたはグループ ボックス内を配置する必要があります。 パネルまたはグループ ボックスの詳細については、次を参照してください。 [Panel コントロールの概要](../../../../docs/framework/winforms/controls/panel-control-overview-windows-forms.md)または[GroupBox コントロールの概要](../../../../docs/framework/winforms/controls/groupbox-control-overview-windows-forms.md)します。  
  
### <a name="to-group-radiobutton-controls-as-a-set-to-function-independently-of-other-sets"></a>その他のセットとは無関係に関数をセットとして RadioButton コントロールをグループ化  
  
1.  ドラッグ、<xref:System.Windows.Forms.GroupBox>または<xref:System.Windows.Forms.Panel>コントロールから、 **Windows フォーム** タブで、**ツールボックス**フォーム上にします。  
  
2.  描画<xref:System.Windows.Forms.RadioButton>のコントロールに対して、<xref:System.Windows.Forms.GroupBox>または<xref:System.Windows.Forms.Panel>コントロール。  
  
## <a name="see-also"></a>関連項目
- <xref:System.Windows.Forms.RadioButton>
- [RadioButton コントロールの概要](../../../../docs/framework/winforms/controls/radiobutton-control-overview-windows-forms.md)
- [Panel コントロールの概要](../../../../docs/framework/winforms/controls/panel-control-overview-windows-forms.md)
- [GroupBox コントロールの概要](../../../../docs/framework/winforms/controls/groupbox-control-overview-windows-forms.md)
- [CheckBox コントロールの概要](../../../../docs/framework/winforms/controls/checkbox-control-overview-windows-forms.md)
- [RadioButton コントロール](../../../../docs/framework/winforms/controls/radiobutton-control-windows-forms.md)

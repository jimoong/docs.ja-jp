---
title: '方法: コントロールの描画クラスを使用して、'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- professional appearance [Windows Forms], rendering Windows Forms controls
- visual themes [Windows Forms], applying to Windows Forms controls
- visual styles [Windows Forms], rendering Windows Forms controls
ms.assetid: c0125e34-cd74-4c35-818c-3e40f462b0a3
ms.openlocfilehash: 4453e04f6fe36ad2b0de7696da68d55264cd47b0
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54534671"
---
# <a name="how-to-use-a-control-rendering-class"></a>方法: コントロールの描画クラスを使用して、
この例では、使用、<xref:System.Windows.Forms.ComboBoxRenderer>ドロップダウン矢印のコンボ ボックス コントロールを描画するクラス。 例から成る、<xref:System.Windows.Forms.Control.OnPaint%2A>単純なカスタム コントロールのメソッド。 <xref:System.Windows.Forms.ComboBoxRenderer.IsSupported%2A?displayProperty=nameWithType>プロパティを使用して、アプリケーション ウィンドウのクライアント領域の視覚スタイルが有効かどうかを確認します。 Visual スタイルが、アクティブな場合、<xref:System.Windows.Forms.ComboBoxRenderer.DrawDropDownButton%2A?displayProperty=nameWithType>メソッドは、visual スタイルは; を使用して、下矢印を表示する場合は、<xref:System.Windows.Forms.ControlPaint.DrawComboButton%2A?displayProperty=nameWithType>メソッドは、従来の Windows スタイルで下矢印を表示します。  
  
## <a name="example"></a>例  
 [!code-cpp[System.Windows.Forms_ControlRenderer#10](../../../../samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms_ControlRenderer/cpp/form1.cpp#10)]
 [!code-csharp[System.Windows.Forms_ControlRenderer#10](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms_ControlRenderer/CS/form1.cs#10)]
 [!code-vb[System.Windows.Forms_ControlRenderer#10](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms_ControlRenderer/VB/form1.vb#10)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
-   派生したカスタム コントロール、<xref:System.Windows.Forms.Control>クラス。  
  
-   A<xref:System.Windows.Forms.Form>カスタム コントロールをホストします。  
  
-   参照、 <xref:System?displayProperty=nameWithType>、 <xref:System.Drawing?displayProperty=nameWithType>、 <xref:System.Windows.Forms?displayProperty=nameWithType>、および<xref:System.Windows.Forms.VisualStyles?displayProperty=nameWithType>名前空間。  
  
## <a name="see-also"></a>関連項目
- [visual スタイルが使用されているコントロールのレンダリング](../../../../docs/framework/winforms/controls/rendering-controls-with-visual-styles.md)

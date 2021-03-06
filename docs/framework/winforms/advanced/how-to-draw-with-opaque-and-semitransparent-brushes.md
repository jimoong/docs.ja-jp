---
title: '方法: 不透明な直線および半透明ブラシを使用して描画します。'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- semi-transparent shapes [Windows Forms], drawing
- transparency [Windows Forms], semi-transparent shapes
- alpha blending [Windows Forms], brush
- brushes [Windows Forms], using semi-transparent
ms.assetid: a4f6f6b8-3bc8-440a-84af-d62ef0f8ff40
ms.openlocfilehash: 922cf6f9fd506e6095d87c3e0ee9eff85f920eba
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54586966"
---
# <a name="how-to-draw-with-opaque-and-semitransparent-brushes"></a>方法: 不透明な直線および半透明ブラシを使用して描画します。
図形を塗りつぶすときに<xref:System.Drawing.Brush> オブジェクトを <xref:System.Drawing.Graphics> クラスの fill メソッドの 1 つに渡す必要があります。 <xref:System.Drawing.SolidBrush.%23ctor%2A> コンストラクターのパラメーターの 1 つは、<xref:System.Drawing.Color> オブジェクトです。 不透明な図形を塗りつぶすには、色のアルファ コンポーネントを 255 に設定します。 半透明な図形を塗りつぶすには、アルファ コンポーネントを 1 ～ 254 の値に設定します。  
  
 半透明な図形を塗りつぶすするとき、図形の色は、背景の色とブレンドされます。 アルファ コンポーネントは、図形と背景色が混在する方法を指定します。0 に近いのアルファ値は、背景色の比重が高く、255 に近いアルファ値は、図形の色の比重が高くなります。  
  
## <a name="example"></a>例  
 次の例では、ビットマップを描画し、ビットマップの重複する 3 つの楕円を塗りつぶします。 最初の楕円はアルファ コンポーネントに 255 を使用するので、不透明です。 2 番目と 3 番目の楕円は、アルファ コンポーネントに 128 を使用するので、楕円から背景画像を確認できます。 <xref:System.Drawing.Graphics.CompositingQuality%2A> プロパティを設定する呼び出しにより、3 番目の楕円がのブレンドがガンマ補正と組み合わせて実行されます。  
  
 以下のコードの出力を次の図に示します。  
  
 ![不透明な直線および半透明な](../../../../docs/framework/winforms/advanced/media/compqualellipse.png "compqualellipse")  
  
 [!code-csharp[System.Drawing.AlphaBlending#31](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.AlphaBlending/CS/Class1.cs#31)]
 [!code-vb[System.Drawing.AlphaBlending#31](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.AlphaBlending/VB/Class1.vb#31)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 前の例は、Windows フォームで使用するために設計されていて、<xref:System.Windows.Forms.PaintEventArgs> のパラメーターである `e`<xref:System.Windows.Forms.PaintEventHandler> を必要とします。  
  
## <a name="see-also"></a>関連項目
- [Windows フォームにおけるグラフィックスと描画](../../../../docs/framework/winforms/advanced/graphics-and-drawing-in-windows-forms.md)
- [アルファ ブレンドの直線と塗りつぶし](../../../../docs/framework/winforms/advanced/alpha-blending-lines-and-fills.md)
- [方法: コントロールに透明な背景を提供します。](../../../../docs/framework/winforms/controls/how-to-give-your-control-a-transparent-background.md)
- [方法: 不透明な直線および半透明な直線を描画します。](../../../../docs/framework/winforms/advanced/how-to-draw-opaque-and-semitransparent-lines.md)

---
title: '方法: データ コレクションの既定のビューを取得する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data collections [WPF], creating views of
- data binding [WPF], creating views of data collections
ms.assetid: b641e96c-c2f6-42ea-9c5d-bac81176ad65
ms.openlocfilehash: 386c25998c85de2f674200fe7d269ae2fdabd72d
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54588403"
---
# <a name="how-to-get-the-default-view-of-a-data-collection"></a>方法: データ コレクションの既定のビューを取得する
ビューでは、並べ替え、フィルター処理、または条件をグループ化に応じて、さまざまな方法で表示する同じデータ収集を許可します。 すべてのコレクションには、バインディング ソースとしてコレクションを指定するときに、実際のバインディング ソースとして使用される 1 つの共有の既定ビューがあります。 この例では、コレクションの既定のビューを取得する方法を示します。  
  
## <a name="example"></a>例  
 ビューを作成するには、コレクションへのオブジェクト参照が必要です。 このデータ オブジェクトは、データ ソースのプロパティを取得することによって、またはバインディングのプロパティを取得することによって、データ コンテキストを取得することによって、独自の分離コード オブジェクトを参照することによって取得できます。 この例は、取得する方法を示します、<xref:System.Windows.FrameworkElement.DataContext%2A>データ オブジェクトと使用のこのコレクションの表示を直接、既定のコレクションを取得します。  
  
 [!code-csharp[CollectionView#2](../../../../samples/snippets/csharp/VS_Snippets_Wpf/CollectionView/CSharp/Page1.xaml.cs#2)]
 [!code-vb[CollectionView#2](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/CollectionView/VisualBasic/Page1.xaml.vb#2)]  
  
 この例では、ルート要素は、<xref:System.Windows.Controls.StackPanel>します。 <xref:System.Windows.FrameworkElement.DataContext%2A>に設定されている*myDataSource*を参照するデータ プロバイダーである、<xref:System.Collections.ObjectModel.ObservableCollection%601>の*順序*オブジェクト。  
  
 [!code-xaml[CollectionView#CollectionViewDataContext](../../../../samples/snippets/csharp/VS_Snippets_Wpf/CollectionView/CSharp/Page1.xaml#collectionviewdatacontext)]  
  
 または、インスタンス化し、独自のビューを使用してコレクションにバインド、<xref:System.Windows.Data.CollectionViewSource>クラス。 このコレクション ビューが直接バインドするコントロールのみ共有します。 例については、ビューのセクションで作成する方法を参照してください、[データ バインディングの概要](../../../../docs/framework/wpf/data/data-binding-overview.md)します。  
  
 コレクション ビューで提供される機能の例については、次を参照してください[ビューのデータを並べ替える](../../../../docs/framework/wpf/data/how-to-sort-data-in-a-view.md)、[ビュー内のフィルター データ](../../../../docs/framework/wpf/data/how-to-filter-data-in-a-view.md)、および[移動のオブジェクト データ CollectionView](../../../../docs/framework/wpf/data/how-to-navigate-through-the-objects-in-a-data-collectionview.md).  
  
## <a name="see-also"></a>関連項目
- [XAML でビューを使用してデータの並べ替えおよびグループ化を行う](../../../../docs/framework/wpf/data/how-to-sort-and-group-data-using-a-view-in-xaml.md)
- [方法トピック](../../../../docs/framework/wpf/data/data-binding-how-to-topics.md)

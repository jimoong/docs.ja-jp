---
title: IsTrue 演算子 (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.istrue
helpviewer_keywords:
- IsTrue operator [Visual Basic]
- OrElse operator [Visual Basic]
ms.assetid: b6cec0f2-61b1-4331-a7f0-4d07ee3179d6
ms.openlocfilehash: 780e67cf54c0bec230d5b052b877cf97a76d3f6d
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54641123"
---
# <a name="istrue-operator-visual-basic"></a>IsTrue 演算子 (Visual Basic)
式は、かどうかを判断します`True`します。  
  
 呼び出すことはできません`IsTrue`明示的に、コードが、Visual Basic のコンパイラが使用できることからコードを生成する`OrElse`句。 クラスまたは構造体を定義しでその型の変数を使用している場合、`OrElse`句が定義する必要があります`IsTrue`でそのクラスまたは構造体。  
  
 コンパイラは、`IsTrue`と`IsFalse`演算子として、*ペア*します。 つまり、それらのいずれかを定義する場合をする必要がありますも定義、もう 1 つ。  
  
## <a name="compiler-use-of-istrue"></a>IsTrue のコンパイラの使用  
 クラスまたは構造体を定義した場合でその型の変数を使用することができます、 `For`、 `If`、 `Else If`、または`While`ステートメント、または、`When`句。 コンパイラが演算子の種類に変換する必要がありますこれを行う場合、`Boolean`条件をテストするための値します。 適切な演算子は、次の順序で検索します。  
  
1.  クラスまたは構造体から拡大変換演算子`Boolean`します。  
  
2.  クラスまたは構造体から拡大変換演算子`Boolean?`します。  
  
3.  `IsTrue`演算子に対して、クラスまたは構造体。  
  
4.  縮小変換`Boolean?`からの変換を伴わない`Boolean`に`Boolean?`します。  
  
5.  クラスまたは構造体から縮小変換演算子`Boolean`します。  
  
 変換を定義していない場合`Boolean`または`IsTrue`演算子、コンパイラには、エラーが通知されます。  
  
> [!NOTE]
>  `IsTrue`演算子は、*オーバー ロードされた*、いるクラスまたは構造体を再定義できますの動作のオペランドがそのクラスまたは構造体の型を持つときにすることを意味します。 コードは、このようなクラスまたは構造体に、この演算子を使用する場合は、再定義された動作を確認ください。 詳細については、「 [Operator Procedures](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次のコード例の定義を含む構造体のアウトラインを定義する、`IsFalse`と`IsTrue`演算子。  
  
 [!code-vb[VbVbalrOperators#28](../../../visual-basic/language-reference/operators/codesnippet/VisualBasic/istrue-operator_1.vb)]  
  
## <a name="see-also"></a>関連項目
- [IsFalse 演算子](../../../visual-basic/language-reference/operators/isfalse-operator.md)
- [方法: 演算子を定義します。](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [OrElse 演算子](../../../visual-basic/language-reference/operators/orelse-operator.md)

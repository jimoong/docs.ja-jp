---
title: ByVal (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.ByVal
- ByVal
helpviewer_keywords:
- ByVal keyword [Visual Basic], contexts
- ByVal keyword [Visual Basic]
ms.assetid: 1eaf4e58-b305-4785-9e3d-e416b9c75598
ms.openlocfilehash: 6fa87db4fbab961dd1aa526e2ac8ff15b031005b
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54650081"
---
# <a name="byval-visual-basic"></a>ByVal (Visual Basic)
呼び出されたプロシージャまたはプロパティが呼び出し元のコードで引数を基になる変数の値を変更できないように、引数が渡されることを指定します。  
  
## <a name="remarks"></a>Remarks  
 `ByVal` 修飾子は、次のコンテキストで使用できます。  
  
 [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)  
  
 [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)  
  
 [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)  
  
 [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
 [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
## <a name="example"></a>例  
 使用例を次に示します、`ByVal`パラメーターを渡す参照型の引数で機能します。 引数は、例では、 `c1`、クラスのインスタンス`Class1`します。 `ByVal` 手順でコードの参照の引数の基になる値の変更を防止`c1`、アクセス可能なフィールドとのプロパティは保護されませんが、`c1`します。  
  
 [!code-vb[VbVbalrKeywords#10](../../../visual-basic/language-reference/codesnippet/VisualBasic/byval_1.vb)]  
  
## <a name="see-also"></a>関連項目
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
- [引数の値渡しと参照渡し](../../../visual-basic/programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)

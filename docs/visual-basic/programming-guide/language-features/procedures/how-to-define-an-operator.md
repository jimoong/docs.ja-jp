---
title: '方法: 定義の演算子 (Visual Basic)'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- operators [Visual Basic], defining
- procedures [Visual Basic], operator
- Visual Basic code, operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- operator procedures [Visual Basic], about operator procedures
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: d4b0e253-092a-4e6e-9fe2-01f562140a29
ms.openlocfilehash: 6ced9e2ab71ccb00c9ce3495e38d895a7104fdde
ms.sourcegitcommit: facefcacd7ae2e5645e463bc841df213c505ffd4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2019
ms.locfileid: "55738657"
---
# <a name="how-to-define-an-operator-visual-basic"></a>方法: 定義の演算子 (Visual Basic)
標準の演算子の動作を定義するクラスまたは構造体を定義している場合 (など`*`、 `<>`、または`And`) 1 つまたは両方のオペランドがクラスまたは構造体の型であるとき。  
  
 演算子プロシージャ内でクラスまたは構造体として標準の演算子を定義します。 演算子のすべてのプロシージャである必要があります`Public``Shared`します。  
  
 クラスまたは構造体で演算子を定義が呼び出されますも*オーバー ロード*演算子。  
  
## <a name="example"></a>例  
 次の例では、定義、`+`構造体の演算子と呼ばれる`height`します。 構造体は、フィートやインチ単位の高さを使用します。 1 つ*インチ*2.54 センチメートル、もう 1 つは、 *foot* 12 インチです。 コンス トラクターを実行する正規化された値 (インチ < 12.0) を確認するには、*剰余*12 の演算です。 `+`演算子では、コンス トラクターを使用して、正規化された値を生成します。  
  
 [!code-vb[VbVbcnProcedures#25](./codesnippet/VisualBasic/how-to-define-an-operator_1.vb)]  
  
 構造体をテストする`height`を次のコード。  
  
 [!code-vb[VbVbcnProcedures#26](./codesnippet/VisualBasic/how-to-define-an-operator_2.vb)]  
  
  
## <a name="see-also"></a>関連項目
- [演算子プロシージャ](./operator-procedures.md)
- [方法: 変換演算子を定義します。](./how-to-define-a-conversion-operator.md)
- [方法: 演算子プロシージャを呼び出す](./how-to-call-an-operator-procedure.md)
- [方法: 演算子を定義するクラスを使用して、](./how-to-use-a-class-that-defines-operators.md)
- [Operator ステートメント](../../../../visual-basic/language-reference/statements/operator-statement.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
- [方法: 構造体を宣言する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [Mod 演算子](../../../../visual-basic/language-reference/operators/mod-operator.md)

---
title: Windows システムの大きなオブジェクト ヒープ
ms.date: 05/02/2018
helpviewer_keywords:
- large object heap (LOH)"
- LOH
- garbage collection, large object heap
- GC [.NET ], large object heap
author: rpetrusha
ms.author: ronpet
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: 68513d2535ea9e19a42f9e58b9d423e17008f9de
ms.sourcegitcommit: ff1d40507b3eb6e2185478e37c66c66be6de46f1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/11/2018
---
# <a name="the-large-object-heap-on-windows-systems"></a>Windows システムの大きなオブジェクト ヒープ

.NET のガベージ コレクター (GC) は、オブジェクトを小さなオブジェクトと大きなオブジェクトに分けます。 オブジェクトが大きい場合、その属性のいくつかは、オブジェクトが小さい場合に比べて、より重要な意味を持つようになります。 たとえば、最適化 (つまり、メモリをヒープ上の他の場所にコピーする処理) のコストが高くなる場合があります。 そのため、.NET ガベージ コレクターは、大きなオブジェクトを大きなオブジェクト ヒープ (LOH) に配置します。 このトピックでは、大きなオブジェクト ヒープについて詳しく説明します。 大きなオブジェクトの定義、その収集方法、大きなオブジェクトがパフォーマンスに与える影響などについて説明します。

> [!IMPORTANT]
> このトピックでは、Windows システムのみで実行されている .NET Framework と .NET Core の大きなオブジェクト ヒープについて説明します。 その他のプラットフォームの .NET 実装で実行されている LOH については取り上げません。

## <a name="how-an-object-ends-up-on-the-large-object-heap-and-how-gc-handles-them"></a>大きなオブジェクト ヒープのオブジェクトが最終的にどのようになり、GC でどのように処理されるかについて

オブジェクトのサイズが 85,000 バイト以上の場合、大きなオブジェクトと見なされます。 この数値は、パフォーマンス チューニングによって決定されたものです。 85,000 バイト以上のオブジェクトの割り当て要求の場合、ランタイムはそれを大きなオブジェクト ヒープに割り当てます。

.NET GC に関する基本事項をいくつか確認することで、これが何を意味するのかを理解しやすくなります。

.NET ガベージ コレクターは世代別コレクターです。 世代 0、世代 1、世代 2 という 3 つの世代があります。 世代が 3 つあるのは、適切にチューニングされたアプリでは、ほとんどのオブジェクトが世代 0 で終了するためです。 たとえば、サーバー アプリでは、各要求に関連付けられた割り当ては、その要求の完了後に終了する必要があります。 処理中の割り当て要求は、世代 1 に進み、その世代で終了します。 本質的に、世代 1 は、短期間存在するオブジェクトの領域と長期間存在するオブジェクトの領域との間でバッファーとして機能します。

小さなオブジェクトは常に世代 0 に割り当てられ、その有効期間に応じて、世代 1 または世代 2 に昇格される場合があります。 大きなオブジェクトは常に世代 2 に割り当てられます。

大きなオブジェクトは世代 2 に属します。これは、大きなオブジェクトが世代 2 の収集中にのみ収集されるためです。 ある世代の収集時には、それより存在期間の短い世代もすべて収集されます。 たとえば、世代 1 の GC が行われるときには、世代 1 と世代 0 の両方が収集されます。 また、世代 2 の GC が行われるときには、ヒープ全体が収集されます。 そのため、世代 2 の GC は*フル GC* とも呼ばれます。 この記事では、フル GC ではなく、世代 2 の GC と呼びますが、どちらの用語も使用できます。

世代では、GC ヒープの論理ビューが提供されます。 物理的には、オブジェクトはマネージド ヒープ セグメントに存在します。 *マネージド ヒープ セグメント* は、GC がマネージド コードに代わって [VirtualAlloc 関数](https://msdn.microsoft.com/library/windows/desktop/aa366887(v=vs.85).aspx)を呼び出して OS から予約するメモリのチャンクです。 CLR が読み込まれると、GC は 2 つの初期ヒープ セグメントを割り当てます。その 1 つは小さなオブジェクト用 (小さなオブジェクト ヒープ、つまり SOH) で、もう 1 つは大きなオブジェクト用 (大きなオブジェクト ヒープ) です。

割り当て要求は、これらのマネージド ヒープ セグメントにマネージド オブジェクトを配置すると満たされます。 オブジェクトが 85,000 バイト未満の場合、SOH のセグメントに配置されます。それ以外の場合は、LOH セグメントに配置されます。 セグメントに割り当てるオブジェクトが増加していくと、セグメントは (小さいチャンクへと) コミットされます。
SOH の場合、GC で残ったオブジェクトは次の世代に昇格されます。 世代 0 のコレクションで残ったオブジェクトは、世代 1 のオブジェクトと見なされるようになり、以後同様です。 ただし、最も古い世代で終了しなかったオブジェクトは、引き続き最も古い世代と見なされます。 つまり、世代 2 で残ったオブジェクトは世代 2 のオブジェクトであり、LOH で残ったオブジェクトは LOH オブジェクト (世代 2 で収集) となります。 

ユーザー コードは、世代 0 (小さなオブジェクト) または LOH (大きなオブジェクト) にのみ割り当てることができます。 GC だけが、世代 1 (世代 0 で残ったオブジェクトを昇格) と世代 2 (世代 1 と世代 2 で残ったオブジェクトを昇格) のオブジェクトを "割り当てる" ことができます。

ガベージ コレクションがトリガーされると、GC は、ライブ オブジェクトをトレースし、それらを最適化します。 ただし、最適化のコストが高いため、GC は LOH を*一掃* し、終了したオブジェクトから空きリストを作成します。これらのオブジェクトは、大きなオブジェクトの割り当て要求を満たすために、後で再利用できます。 隣接する複数の終了したオブジェクトは、1 つの空きオブジェクトに結合されます。

.NET Core と .NET Framework (.NET Framework 4.5.1 以降) には <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode?displayProperty="fullname"> プロパティが含まれています。これを使用して、ユーザーは LOH を次のフル ブロッキング GC 中に最適化するよう指定できます。 将来的には、.NET で LOH を自動的に最適化できるようになります。 つまり、大きなオブジェクトを割り当てたときに、そのオブジェクトが移動されないようにするには、オブジェクトを固定する必要があります。

図 1 に示したシナリオでは、GC は世代 0 の GC で `Obj1` と `Obj3` が終了した後に世代 1 を形成し、世代 1 の GC で `Obj2` と `Obj5` が終了した後に世代 2 を形成しています。 この図と次の図は単に例を示すためのものであることに注意してください。ヒープで何が起こっているかをわかりやすくするために、図にはごくわずかなオブジェクトしか含まれていません。 実際には、通常、GC にはより多くのオブジェクトが含まれます。

![図 1: 世代 0 の GC と世代 1 の GC](media/loh/loh-figure-1.jpg)   
図 1: 世代 0 および世代 1 の GC。

図 2 では、`Obj1` と `Obj2` を終了させた世代 2 の GC の後、GC は、`Obj1` と `Obj2` が占有していたメモリから隣接する空き領域を形成し、`Obj4` に対する割り当て要求を満たすためにその空き領域を使用しています。 最後のオブジェクト `Obj3` からセグメントの末尾までの領域は、割り当て要求を満たすために使用することもできます。
 
![図 2: 世代 2 の GC の後](media/loh/loh-figure-2.jpg)  
図 2: 世代 2 の GC の後

大きなオブジェクトの割り当て要求に応じるだけの十分な空き領域がない場合、GC はまず、OS からさらにセグメントを取得することを試みます。 それが失敗した場合、多少の領域を解放できることを期待して、世代 2 の GC をトリガーします。

世代 1 または世代 2 の GC 中に、ガベージ コレクターは、ライブ オブジェクトが存在しないセグメントを、[VirtualFree 関数](https://msdn.microsoft.com/library/windows/desktop/aa366892(v=vs.85).aspx)を呼び出して解放し、OS に戻します。 最後の有効なオブジェクトからセグメントの末尾までの領域はデコミットされます (ただし、世代 0/世代 1 が有効な短期セグメントの場合を除きます。この場合、アプリケーションですぐに割り当てられるため、ガベージ コレクターは一部をコミットしたままにします)。 また、空き領域は引き続きコミットされていますが、リセットされます。つまり、OS はその領域のデータをディスクに書き戻す必要がありません。

LOH は世代 2 の GC 中にのみ収集されるため、LOH セグメントを解放できるのはその GC 中のみとなります。 図 3 に示したシナリオでは、ガベージ コレクターは 1 つのセグメント (セグメント 2) を解放して OS に戻し、残りのセグメントでより多くの領域をデコミットしています。 大きなオブジェクトの割り当て要求を満たすために、セグメント末尾のデコミットされた領域を使用する必要がある場合は、メモリを再度コミットします  (コミット/デコミットの詳細については、[VirtualAlloc](https://msdn.microsoft.com/library/windows/desktop/aa366887(v=vs.85).aspx) に関するドキュメントを参照してください)。
 
![図 3: 世代 2 の GC 後の LOH](media/loh/loh-figure-3.jpg)  
図 3: 世代 2 の GC 後の LOH

## <a name="when-is-a-large-object-collected"></a>大きなオブジェクトが収集されるタイミング

一般に、GC は次の 3 つの状態のいずれかになった場合に発生します。

- 割り当てが世代 0 または大きなオブジェクトのしきい値を超えている場合。

   しきい値は世代のプロパティの 1 つです。 世代のしきい値は、ガベージ コレクターがオブジェクトをそれに割り当てるときに設定されます。 しきい値を超えたときに、その世代に対して GC がトリガーされます。 小さなオブジェクトまたは大きなオブジェクトを割り当てると、世代 0 と LOH のしきい値がそれぞれ消費されます。 ガベージ コレクターがオブジェクトを世代 1 と世代 2 に割り当てると、それらのしきい値が使用されます。 これらのしきい値は、プログラムの実行時に動的に調整されます。

   これは一般的なケースです。マネージド ヒープでの割り当てにより、ほとんどの GC が発生します。

- <xref:System.GC.Collect%2A?displayProperty=nameWithType> メソッドが呼び出された場合。

   パラメーターなしの <xref:System.GC.Collect?displayProperty=nameWithType> メソッドが呼び出されたか、別のオーバーロードに引数として <xref:System.GC.MaxGeneration?displayProperty=nameWithType> が渡された場合、マネージド ヒープの残りの部分と共に LOH が収集されます。

- システムのメモリが不足している場合。

   これは、ガベージ コレクターが、OS からメモリ使用率が高いという通知を受け取ったときに起こります。 ガベージ コレクターは、世代 2 の GC を行うことが有効であると判断した場合は、それをトリガーします。

## <a name="loh-performance-implications"></a>LOH のパフォーマンスへの影響

大きなオブジェクト ヒープへの割り当ては、次のようにパフォーマンスに影響します。

- 割り当てコスト。

   CLR では、すべての新しいオブジェクトに対して解放時にメモリがクリアされることが保証されています。 これは、大きなオブジェクトの割り当てコストが完全に、メモリのクリアによって占められることを意味します (GC をトリガーしない限り)。 1 バイトをクリアするのに 2 サイクルかかるとすると、大きなオブジェクトの中で最小のものであっても、クリアするのに 170,000 サイクルかかります。 2 GHz のマシンで 16 MB のメモリをクリアする場合、約 16 ミリ秒かかります。 これはかなり大きなコストです。

- コレクション コスト。

   LOH と世代 2 は一緒に収集されるため、いずれかのしきい値を超えると、世代 2 のコレクションがトリガーされます。 LOH のしきい値によって世代 2 の収集がトリガーされた場合、世代 2 が GC 後に非常に小さくなるとは限りません。 世代 2 にあまり多くのデータがない場合、影響は最小限になります。 ただし、世代 2 が大きい場合は、世代 2 の多くの GC がトリガーされると、パフォーマンス上の問題が生じることがあります。 多くの大きなオブジェクトがごく一時的に割り当てられており、大きな SOH がある場合は、GC の実行に過度な時間を費やす可能性があります。 さらに、非常に大きなオブジェクトの割り当てと解放を引き続き繰り返すと、割り当てコストが相当に増加します。

- 参照型の配列要素。

   LOH 上の非常に大きなオブジェクトは、通常は配列です (非常に大きなインスタンス オブジェクトを持つということは、ごくまれです)。 配列の要素に参照が多く含まれる場合、要素に参照が多く含まれない場合には存在しないコストが発生します。 要素に参照が含まれない場合、ガベージ コレクターは配列をまったく確認する必要がありません。 たとえば、配列を使用してバイナリ ツリーのノードを格納する場合、これを実装する方法の 1 つは、ノードの右および左のノードを実際のノードで参照することです。

   ```csharp
   class Node
   {
      Data d;
      Node left;
      Node right;
   };

   Node[] binary_tr = new Node [num_nodes];
   ```

   `num_nodes` が大きい場合、ガベージ コレクターは、要素ごとに少なくとも 2 回の参照を行う必要があります。 他に、左右のノードのインデックスを格納するという方法もあります。

   ```csharp
   class Node
   {
      Data d;
      uint left_index;
      uint right_index;
   } ;
   ```

   左のノードのデータを `left.d` として参照する代わりに、それを `binary_tr[left_index].d` として参照します。 ガベージ コレクターは、左右のノードに対して参照を確認する必要がありません。

3 つの要因のうち、通常は最初の 2 つが 3 番目より重要です。 そのため、一時オブジェクトを割り当てる代わりに再利用する大きなオブジェクトのプールを割り当てることをお勧めします。 

## <a name="collecting-performance-data-for-the-loh"></a>LOH のパフォーマンス データの収集

特定の領域のパフォーマンス データを収集するには、次のような状況である必要があります。

1. この領域に着目する理由がわかっている。

1. 既知の他の領域を調べつくしたが、確認されたパフォーマンス上の問題を説明できる原因が見つからなかった。

メモリと CPU の基礎の詳細については、ブログの「[Understand the problem before you try to find a solution](https://blogs.msdn.microsoft.com/maoni/2006/09/01/understand-the-problem-before-you-try-to-find-a-solution/)」 (解決策を見つける前に問題を理解する) を参照してください。

次のツールを使用して、LOH のパフォーマンスに関するデータを収集することができます。

- [.NET CLR メモリ パフォーマンス カウンター](#net-clr-memory-performance-counters)

- [ETW イベント](#etw-events)

- [デバッガー](#a-debugger)

### <a name="net-clr-memory-performance-counters"></a>.NET CLR メモリ パフォーマンス カウンター

これらのパフォーマンス カウンターは、通常、パフォーマンスの問題について調べる最初の手順として有効です (ただし、[ETW イベント](#etw)を使用することをお勧めします)。 パフォーマンス モニターを構成するには、図 4 に示すように、必要なカウンターを追加します。 LOH に関連するものは次のとおりです。

- **\# Gen 2 Collections**

   プロセスが開始されてから世代 2 の GC が発生した回数を示します。 このカウンターは、世代 2 のコレクション (フル ガベージ コレクションとも呼ばれる) の最後にインクリメントされます。 このカウンターは、最後に計測された値を表示します。

- **Large Object Heap size**

   LOH の現在のサイズ (空き領域を含む) をバイト単位で表示します。 このカウンターは、割り当てがなされるたびに更新されるのではなく、ガベージ コレクションが 1 回終了するごとに更新されます。

パフォーマンス カウンターを見る一般的な方法は、パフォーマンス モニター (perfmon.exe) で表示することです。 対象のプロセスに対して表示するカウンターを追加するには、[カウンターの追加] を使用します。 図 4 に示すように、パフォーマンス カウンターのデータは、ログ ファイルに保存できます。

![図 4: パフォーマンス カウンターの追加。](media/loh/perfcounter.png)    
図 4: 世代 2 の GC 後の LOH

また、パフォーマンス カウンターに対してプログラムでクエリを実行することもできます。 多くのユーザーは、日常的なテスト プロセスの一部として、そのような方法でカウンターのデータを収集しています。 カウンターに通常範囲外の値を見つけたときは、他の手段を使用して、調査に役立つ詳細なデータを取得します。

> [!NOTE]
> ETW ではより豊富な情報が提供されるため、パフォーマンス カウンターではなく、ETW イベントを使用することをお勧めます。

### <a name="etw"></a>ETW

ガベージ コレクターは、ヒープで行われる内容とその理由を理解するのに役立つ、豊富な ETW イベントのセットを提供します。 次のブログ記事には、ETW を使用して GC イベントを収集および理解する方法が示されています。

- [GC ETW イベント - 1 ](http://blogs.msdn.com/b/maoni/archive/2014/12/22/gc-etw-events.aspx)

- [GC ETW イベント - 2](http://blogs.msdn.com/b/maoni/archive/2014/12/25/gc-etw-events-2.aspx)

- [GC ETW イベント - 3](http://blogs.msdn.com/b/maoni/archive/2014/12/25/gc-etw-events-3.aspx) 

- [GC ETW イベント - 4](http://blogs.msdn.com/b/maoni/archive/2014/12/30/gc-etw-events-4.aspx)

一時 LOH の割り当てによる過度の世代 2 の GC を特定するには、GC のトリガー理由の列を確認します。 大きな一時オブジェクトのみを割り当てる簡単なテストの場合は、次の [PerfView](https://www.microsoft.com/download/details.aspx?id=28567) コマンド ラインを使用して、ETW イベントに関する情報を収集できます。

```console
perfview /GCCollectOnly /AcceptEULA /nogui collect
```

結果は次のようになります。
 
![図 5: PerfView を使用する ETW イベントの確認](media/loh/perfview.png)  
図 5: PerfView を使用した場合に表示される ETW イベント

ご覧のとおり、すべての GC は世代 2 の GC であり、AllocLarge によってすべてトリガーされます。これは、大きなオブジェクトの割り当てにより、この GC がトリガーされたことを意味します。 **LOH Survival Rate %** 列に 1% と示されているため、これらの割り当ては一時的なものであることがわかります。

これらの大きなオブジェクトを割り当てたユーザーを示す追加の ETW イベントを収集することができます。 たとえば、以下のコマンド ラインを使用します。

```console
perfview /GCOnly /AcceptEULA /nogui collect
```

この場合、AllocationTick イベントが収集されます。このイベントは、約 100 K 相当の割り当てごとに発生します。 つまり、イベントは、大きなオブジェクトが割り当てられるたびに発生します。 次に、大きなオブジェクトを割り当てた呼び出し履歴を示す、GC ヒープ割り当てビューのいずれかを確認できます。
 
![図 6: GC ヒープ割り当てビュー](media/loh/perfview2.png)  
図 6: GC ヒープ割り当てビュー
 
ご覧のとおり、これは、`Main` メソッドから大きなオブジェクトを割り当てるだけの非常に簡単なテストです。

### <a name="a-debugger"></a>デバッガー

メモリ ダンプしかない状態で、LOH に実際にどのオブジェクトが存在するかを確認する必要がある場合は、.NET で提供される [SoS デバッガー拡張](http://msdn2.microsoft.com/ms404370.aspx)を使用できます。 

> [!NOTE]
> このセクションに示されているデバッグ コマンドは、[Windows デバッガー](http://www.microsoft.com/whdc/devtools/debugging/default.mspx)に適用できます。

LOH の分析からのサンプル出力を次に示します。

```
0:003> .loadby sos mscorwks
0:003> !eeheap -gc
Number of GC Heaps: 1
generation 0 starts at 0x013e35ec
sdgeneration 1 starts at 0x013e1b6c
generation 2 starts at 0x013e1000
ephemeral segment allocation context: none
segment   begin allocated     size
0018f2d0 790d5588 790f4b38 0x0001f5b0(128432)
013e0000 013e1000 013e35f8 0x000025f8(9720)
Large object heap starts at 0x023e1000
segment   begin allocated     size
023e0000 023e1000 033db630 0x00ffa630(16754224)
033e0000 033e1000 043cdf98 0x00fecf98(16699288)
043e0000 043e1000 05368b58 0x00f87b58(16284504)
Total Size 0x2f90cc8(49876168)
------------------------------
GC Heap Size 0x2f90cc8(49876168)
0:003> !dumpheap -stat 023e1000 033db630
total 133 objects
Statistics:
MT   Count   TotalSize Class Name
001521d0       66     2081792     Free
7912273c       63     6663696 System.Byte[]
7912254c       4     8008736 System.Object[]
Total 133 objects
```

LOH のヒープ サイズは (16,754,224 + 16,699,288 + 16,284,504) = 49,738,016 バイトです。 アドレス 023e1000 と 033db630 の間で、8,008,736 バイトが <xref:System.Object?displayProperty=fullName> オブジェクトの配列によって占有され、6,663,696 バイトが <xref:System.Byte?displayProperty=nameWithType> オブジェクトの配列によって占有され、2,081,792 バイトが空き領域によって占有されています。

デバッガーによって、LOH の合計サイズが 85,000 バイトより少ないことが示される場合があります。 その理由は、ランタイム自体が LOH を使用して、大きなオブジェクトよりも小さいいくつかのオブジェクトを割り当てるためです。

LOH は最適化されないため、LOH が断片化の元であると考えられる場合もあります。 断片化の意味を以下に示します。

- マネージド ヒープの断片化: マネージド オブジェクト間の空き領域の量で示されます。 SoS では、`!dumpheap –type Free` コマンドはマネージド オブジェクト間の空き領域の量を示します。

- 仮想メモリ (VM) アドレス空間の断片化: `MEM_FREE` としてマークされるメモリです。 windbg でさまざまなデバッガー コマンドを使用して取得することができます。

   次の例は、VM 領域内の断片化を示しています。

   ```
   0:000> !address
   00000000 : 00000000 - 00010000
   Type     00000000
   Protect 00000001 PAGE_NOACCESS
   State   00010000 MEM_FREE
   Usage   RegionUsageFree
   00010000 : 00010000 - 00002000
   Type     00020000 MEM_PRIVATE
   Protect 00000004 PAGE_READWRITE
   State   00001000 MEM_COMMIT
   Usage   RegionUsageEnvironmentBlock
   00012000 : 00012000 - 0000e000
   Type     00000000
   Protect 00000001 PAGE_NOACCESS
   State   00010000 MEM_FREE
   Usage   RegionUsageFree
   … [omitted]
   -------------------- Usage SUMMARY --------------------------
   TotSize (     KB)   Pct(Tots) Pct(Busy)   Usage
   701000 (   7172) : 00.34%   20.69%   : RegionUsageIsVAD
   7de15000 ( 2062420) : 98.35%   00.00%   : RegionUsageFree
   1452000 (   20808) : 00.99%   60.02%   : RegionUsageImage
   300000 (   3072) : 00.15%   08.86%   : RegionUsageStack
   3000 (     12) : 00.00%   00.03%   : RegionUsageTeb
   381000 (   3588) : 00.17%   10.35%   : RegionUsageHeap
   0 (       0) : 00.00%   00.00%   : RegionUsagePageHeap
   1000 (       4) : 00.00%   00.01%   : RegionUsagePeb
   1000 (       4) : 00.00%   00.01%   : RegionUsageProcessParametrs
   2000 (       8) : 00.00%   00.02%   : RegionUsageEnvironmentBlock
   Tot: 7fff0000 (2097088 KB) Busy: 021db000 (34668 KB)

   -------------------- Type SUMMARY --------------------------
   TotSize (     KB)   Pct(Tots) Usage
   7de15000 ( 2062420) : 98.35%   : <free>
   1452000 (   20808) : 00.99%   : MEM_IMAGE
   69f000 (   6780) : 00.32%   : MEM_MAPPED
   6ea000 (   7080) : 00.34%   : MEM_PRIVATE

   -------------------- State SUMMARY --------------------------
   TotSize (     KB)   Pct(Tots) Usage
   1a58000 (   26976) : 01.29%   : MEM_COMMIT
   7de15000 ( 2062420) : 98.35%   : MEM_FREE
   783000 (   7692) : 00.37%   : MEM_RESERVE

   Largest free region: Base 01432000 - Size 707ee000 (1843128 KB)
   ```

より一般的に VM の断片化が見られるケースは、大きな一時オブジェクトが、OS から新しいマネージド ヒープ セグメントを頻繁に取得して空のセグメントを開放して OS に戻すためにガベージ コレクターを必要とするような場合です。

LOH が VM の断片化を生じさせているかどうかを確認するには、[VirtualAlloc](https://msdn.microsoft.com/library/windows/desktop/aa366887(v=vs.85).aspx) と [VirtualFree](https://msdn.microsoft.com/library/windows/desktop/aa366892(v=vs.85).aspx) にブレークポイントを設定して、その呼び出し元を確認します。 たとえば、OS から 8 MB を超える仮想メモリ チャンクを割り当てようとしているオブジェクトを調べる場合、次のようにブレークポイントを設定できます。

```console
bp kernel32!virtualalloc "j (dwo(@esp+8)>800000) 'kb';'g'"
```

このコマンドはデバッガーを中断し、[VirtualAlloc](https://msdn.microsoft.com/library/windows/desktop/aa366887(v=vs.85).aspx) が 8 MB (0x800000) より大きい割り当てサイズで呼び出された場合にのみ、呼び出し履歴を表示します。

CLR 2.0 では *VM Hoarding* という機能が追加されました。これは、セグメント (大きなオブジェクト ヒープと小さなオブジェクト ヒープを含む) が頻繁に取得され、解放されるシナリオで役立つ場合があります。 VM Hoarding を指定するには、ホスティング API で、`STARTUP_HOARD_GC_VM` というスタートアップ フラグを指定します。 空のセグメントを解放して OS に戻す代わりに、CLR はこれらのセグメント上のメモリをデコミットして、スタンバイ リストに含めます  (大きすぎるセグメントの場合、CLR はこれを行わないことに注意してください)。CLR は、新しいセグメント要求を満たすために、後でこれらのセグメントを使用します。 次にアプリで新しいセグメントが必要になったときに、CLR は、このスタンバイ リストに十分に大きなセグメントがあれば、それを使用します。

VM Hoarding は、メモリ不足の例外を回避するために、システム上で実行されている優先度の高いアプリである一部のサーバー アプリなど、既に取得されているセグメントに保持する必要があるアプリケーションでも役立ちます。

この機能を使用する際には、アプリケーションを慎重にテストして、アプリケーションのメモリ使用量が十分に安定しているのを確認することを強くお勧めします。
bp kernel32!virtualalloc "j (dwo(@esp+8)>800000) 'kb';'g'"
```

This command breaks into the debugger and shows the callstack only if [VirtualAlloc](https://msdn.microsoft.com/library/windows/desktop/aa366887(v=vs.85).aspx) is called with an allocation size greater than 8MB (0x800000).

CLR 2.0 added a feature called *VM Hoarding* that can be useful for scenarious where segments (including on the large and small object heaps) are frequently acquired and released. To specify VM Hoarding, you specify a startup flag called `STARTUP_HOARD_GC_VM` via the hosting API. Instead of releasing empty segments back to the OS, the CLR decommits the memory on these segments and puts them on a standby list. (Note that the CLR doesn't do this for segments that are too large.) The CLR later uses those segments to satisfy new segment requests. The next time that your app needs a new segment, the CLR uses one from this standby list if it can find one that’s big enough.

VM hoarding is also useful for applications that want to hold onto the segments that they already acquired, such as some server apps that are the dominant apps running on the system, to avoid out of memory exceptions.

We strongly recommend that you carefully test your application when you use this feature to ensure your application has fairly stable memory usage.
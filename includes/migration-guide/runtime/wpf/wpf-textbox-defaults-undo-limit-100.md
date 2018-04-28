### <a name="wpf-textbox-defaults-to-undo-limit-of-100"></a><span data-ttu-id="6a1fb-101">WPF TextBox は既定で元に戻す上限が 100 に設定される</span><span class="sxs-lookup"><span data-stu-id="6a1fb-101">WPF TextBox defaults to undo limit of 100</span></span>

|   |   |
|---|---|
|<span data-ttu-id="6a1fb-102">説明</span><span class="sxs-lookup"><span data-stu-id="6a1fb-102">Details</span></span>|<span data-ttu-id="6a1fb-103">.NET 4.5 では、WPF テキスト ボックスの既定の元に戻す上限は 100 です (.NET 4.0 では無制限でした)。</span><span class="sxs-lookup"><span data-stu-id="6a1fb-103">In .NET 4.5, the default undo limit for a WPF textbox is 100 (as opposed to being unlimited in .NET 4.0)</span></span>|
|<span data-ttu-id="6a1fb-104">提案される解決策</span><span class="sxs-lookup"><span data-stu-id="6a1fb-104">Suggestion</span></span>|<span data-ttu-id="6a1fb-105">元に戻す上限が 100 では低すぎる場合、<xref:System.Windows.Controls.Primitives.TextBoxBase.UndoLimit> を使用して上限を明示的に設定できます。</span><span class="sxs-lookup"><span data-stu-id="6a1fb-105">If an undo limit of 100 is too low, the limit can be set explicitly with <xref:System.Windows.Controls.Primitives.TextBoxBase.UndoLimit></span></span>|
|<span data-ttu-id="6a1fb-106">スコープ</span><span class="sxs-lookup"><span data-stu-id="6a1fb-106">Scope</span></span>|<span data-ttu-id="6a1fb-107">エッジ</span><span class="sxs-lookup"><span data-stu-id="6a1fb-107">Edge</span></span>|
|<span data-ttu-id="6a1fb-108">Version</span><span class="sxs-lookup"><span data-stu-id="6a1fb-108">Version</span></span>|<span data-ttu-id="6a1fb-109">4.5</span><span class="sxs-lookup"><span data-stu-id="6a1fb-109">4.5</span></span>|
|<span data-ttu-id="6a1fb-110">型</span><span class="sxs-lookup"><span data-stu-id="6a1fb-110">Type</span></span>|<span data-ttu-id="6a1fb-111">ランタイム</span><span class="sxs-lookup"><span data-stu-id="6a1fb-111">Runtime</span></span>|
|<span data-ttu-id="6a1fb-112">影響を受ける API</span><span class="sxs-lookup"><span data-stu-id="6a1fb-112">Affected APIs</span></span>|<ul><li><xref:System.Windows.Controls.TextBox?displayProperty=nameWithType></li></ul>|

### <a name="winforms-domain-upbutton-and-downbutton-actions-are-in-sync-now"></a><span data-ttu-id="f916b-101">WinForm の Domain upbutton アクションと downbutton アクションが同期するようになった</span><span class="sxs-lookup"><span data-stu-id="f916b-101">WinForm's Domain upbutton and downbutton actions are in sync now</span></span>

|   |   |
|---|---|
|<span data-ttu-id="f916b-102">説明</span><span class="sxs-lookup"><span data-stu-id="f916b-102">Details</span></span>|<span data-ttu-id="f916b-103">.NET Framework 4.7.1 およびそれより前のバージョンでは、コントロールにテキストが存在すると <xref:System.Windows.Forms.DomainUpDown> コントロールの <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> アクションが無視され、開発者は <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> アクションを使用する前にコントロールで <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> アクションを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f916b-103">In the .NET Framework 4.7.1 and previous versions the <xref:System.Windows.Forms.DomainUpDown> control's <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> action is ignored when control text is present, and the developer is required to use <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> action on the control before using <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> action.</span></span> <span data-ttu-id="f916b-104">.NET Framework 4.7.2 以降では、このシナリオで <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> アクションと <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> アクションの両方が独立して動作し、同期を維持します。</span><span class="sxs-lookup"><span data-stu-id="f916b-104">Starting with the .NET Framework 4.7.2 both the <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> and <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> actions work independently in this scenario and remain in sync.</span></span>|
|<span data-ttu-id="f916b-105">提案される解決策</span><span class="sxs-lookup"><span data-stu-id="f916b-105">Suggestion</span></span>|<span data-ttu-id="f916b-106">アプリケーションでこれらの変更を利用するには、.NET Framework 4.7.2 以降で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f916b-106">In order for an application to benefit from these changes, it must run on the .NET Framework 4.7.2 or later.</span></span> <span data-ttu-id="f916b-107">アプリケーションは、次のいずれかの方法でこれらの変更を利用できます。</span><span class="sxs-lookup"><span data-stu-id="f916b-107">The application can benefit from these changes in either of the following ways:</span></span><ul><li><span data-ttu-id="f916b-108">.NET Framework 4.7.2 を対象にして再コンパイルします。</span><span class="sxs-lookup"><span data-stu-id="f916b-108">It is recompiled to target the .NET Framework 4.7.2.</span></span> <span data-ttu-id="f916b-109">.NET Framework 4.7.2 以降を対象とする Windows フォーム アプリケーションでは、この変更が既定で有効になります。</span><span class="sxs-lookup"><span data-stu-id="f916b-109">This change is enabled by default on Windows Forms applications that target the .NET Framework 4.7.2 or later.</span></span></li><li><span data-ttu-id="f916b-110">下の例のように、app.config ファイルの <code>&lt;runtime&gt;</code> セクションに次の [AppContext スイッチ](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element)を追加し、それを <code>false</code> に設定することで、以前のスクロール動作を無効にします。</span><span class="sxs-lookup"><span data-stu-id="f916b-110">It opts out of the legacy scrolling behavior by adding the following [AppContext Switch](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element) to the <code>&lt;runtime&gt;</code> section of the app config file and setting it to <code>false</code>, as the following example shows.</span></span></li></ul><pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>|
|<span data-ttu-id="f916b-111">スコープ</span><span class="sxs-lookup"><span data-stu-id="f916b-111">Scope</span></span>|<span data-ttu-id="f916b-112">エッジ</span><span class="sxs-lookup"><span data-stu-id="f916b-112">Edge</span></span>|
|<span data-ttu-id="f916b-113">Version</span><span class="sxs-lookup"><span data-stu-id="f916b-113">Version</span></span>|<span data-ttu-id="f916b-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="f916b-114">4.7.2</span></span>|
|<span data-ttu-id="f916b-115">型</span><span class="sxs-lookup"><span data-stu-id="f916b-115">Type</span></span>|<span data-ttu-id="f916b-116">再ターゲット中</span><span class="sxs-lookup"><span data-stu-id="f916b-116">Retargeting</span></span>|
|<span data-ttu-id="f916b-117">影響を受ける API</span><span class="sxs-lookup"><span data-stu-id="f916b-117">Affected APIs</span></span>|<ul><li><xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType></li><li><xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType></li></ul>|

### <a name="sslstream-supports-tls-alerts"></a><span data-ttu-id="e7113-101">SslStream で TLS アラートに対応</span><span class="sxs-lookup"><span data-stu-id="e7113-101">SslStream supports TLS Alerts</span></span>

|   |   |
|---|---|
|<span data-ttu-id="e7113-102">説明</span><span class="sxs-lookup"><span data-stu-id="e7113-102">Details</span></span>|<span data-ttu-id="e7113-103">TLS ハンドシェイクに失敗すると、最初の I/O 読み取り/書き込み操作によって <xref:System.IO.IOException?displayProperty=name> と内部例外 <xref:System.ComponentModel.Win32Exception?displayProperty=name> がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e7113-103">After a failed TLS handshake, an <xref:System.IO.IOException?displayProperty=name> with an inner <xref:System.ComponentModel.Win32Exception?displayProperty=name> exception will be thrown by the first I/O Read/Write operation.</span></span> <span data-ttu-id="e7113-104"><xref:System.ComponentModel.Win32Exception?displayProperty=name> の <xref:System.ComponentModel.Win32Exception.NativeErrorCode?displayProperty=name> コードは、この [Schannel ドキュメント](https://msdn.microsoft.com/library/windows/desktop/dd721886%28v=vs.85%29.aspx)を使用してリモート パーティからの TLS アラートにマップすることができます。詳細については、「[RFC 2246: Section 7.2.2 Error alerts](https://tools.ietf.org/html/rfc2246#section-7.2.2)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e7113-104">The <xref:System.ComponentModel.Win32Exception.NativeErrorCode?displayProperty=name> code for the <xref:System.ComponentModel.Win32Exception?displayProperty=name> can be mapped to the TLS Alert from the remote party using this [Schannel documentation](https://msdn.microsoft.com/library/windows/desktop/dd721886%28v=vs.85%29.aspx).For more information, see [RFC 2246: Section 7.2.2 Error alerts](https://tools.ietf.org/html/rfc2246#section-7.2.2).</span></span> <br/><span data-ttu-id="e7113-105">.NET Framework 4.6.2 およびそれより前のバージョンでの動作では、他のパーティがハンドシェイクに失敗してそのすぐ後に接続を拒否した場合、トランスポート チャネル (通常は TCP 接続) は書き込みまたは読み取り中にタイムアウトします。</span><span class="sxs-lookup"><span data-stu-id="e7113-105">The behavior in .NET Framework 4.6.2 and earlier is that the transport channel (usually TCP connection) will timeout during either Write or Read if the other party failed the handshake and immediately afterwards rejected the connection.</span></span>|
|<span data-ttu-id="e7113-106">提案される解決策</span><span class="sxs-lookup"><span data-stu-id="e7113-106">Suggestion</span></span>|<span data-ttu-id="e7113-107"><xref:System.IO.Stream.Read(System.Byte[],System.Int32,System.Int32)>/<xref:System.IO.Stream.Write(System.Byte[],System.Int32,System.Int32)> などのネットワーク I/O API を呼び出すアプリケーションは、<xref:System.IO.IOException> または <xref:System.TimeoutException?displayProperty=name> を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e7113-107">Applications calling network I/O APIs such as <xref:System.IO.Stream.Read(System.Byte[],System.Int32,System.Int32)>/<xref:System.IO.Stream.Write(System.Byte[],System.Int32,System.Int32)> should handle <xref:System.IO.IOException> or <xref:System.TimeoutException?displayProperty=name>.</span></span><br/><span data-ttu-id="e7113-108">TLS アラート機能は、.NET Framework 4.7 以降では既定で有効になります。</span><span class="sxs-lookup"><span data-stu-id="e7113-108">The TLS Alerts feature is enabled by default starting with .NET Framework 4.7.</span></span> <span data-ttu-id="e7113-109">.NET Framework 4.7 またはそれより後のシステムで動作する 4.0 から 4.6.2 のバージョンの .NET Framework を対象とするアプリケーションは、互換性を維持するためにこの機能を無効にします。</span><span class="sxs-lookup"><span data-stu-id="e7113-109">Applications targeting versions of the .NET Framework from 4.0 through 4.6.2 running on a .NET Framework 4.7 or higher system will have the feature disabled to preserve compatibility.</span></span> <br/><span data-ttu-id="e7113-110">.NET Framework 4.7 以降で動作する .NET Framework 4.6 以降のアプリケーションでこの機能を有効または無効にするには、次の構成 API を使います。</span><span class="sxs-lookup"><span data-stu-id="e7113-110">The following configuration API is available to enable or disable the feature for .NET Framework 4.6 and later applications running on .NET Framework 4.7 or later.</span></span><ul><li><span data-ttu-id="e7113-111">プログラムによる:</span><span class="sxs-lookup"><span data-stu-id="e7113-111">Programmatically:</span></span></li></ul><span data-ttu-id="e7113-112">ServicePointManager は 1 回のみ初期化するため、アプリケーションで行われる一番最初の動作にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e7113-112">Must be the very first thing the application does since ServicePointManager will initialize only once:</span></span><pre><code class="lang-csharp">AppContext.SetSwitch(&quot;TestSwitch.LocalAppContext.DisableCaching&quot;, true);&#13;&#10;AppContext.SetSwitch(&quot;Switch.System.Net.DontEnableTlsAlerts&quot;, true); // Set to &#39;false&#39; to enable the feature in .NET Framework 4.6 - 4.6.2.&#13;&#10;</code></pre><ul><li><span data-ttu-id="e7113-113">AppConfig:</span><span class="sxs-lookup"><span data-stu-id="e7113-113">AppConfig:</span></span></li></ul><pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Net.DontEnableTlsAlerts=true&quot;/&gt;&#13;&#10;&lt;!-- Set to &#39;false&#39; to enable the feature in .NET Framework 4.6 - 4.6.2. --&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre><ul><li><span data-ttu-id="e7113-114">レジストリ キー (マシン グローバル):</span><span class="sxs-lookup"><span data-stu-id="e7113-114">Registry key (machine global):</span></span></li></ul><span data-ttu-id="e7113-115">.NET Framework 4.6 - 4.6.2 でこの機能を有効にするには、Value を <code>false</code> に設定します。</span><span class="sxs-lookup"><span data-stu-id="e7113-115">Set the Value to <code>false</code> to enable the feature in .NET Framework 4.6 - 4.6.2.</span></span><pre><code>Key = HKLM\SOFTWARE\[Wow6432Node\]Microsoft\.NETFramework\AppContext\Switch.System.Net.DontEnableTlsAlerts&#13;&#10;Type = String&#13;&#10;Value = &quot;true&quot;&#13;&#10;</code></pre>|
|<span data-ttu-id="e7113-116">スコープ</span><span class="sxs-lookup"><span data-stu-id="e7113-116">Scope</span></span>|<span data-ttu-id="e7113-117">エッジ</span><span class="sxs-lookup"><span data-stu-id="e7113-117">Edge</span></span>|
|<span data-ttu-id="e7113-118">Version</span><span class="sxs-lookup"><span data-stu-id="e7113-118">Version</span></span>|<span data-ttu-id="e7113-119">4.7</span><span class="sxs-lookup"><span data-stu-id="e7113-119">4.7</span></span>|
|<span data-ttu-id="e7113-120">型</span><span class="sxs-lookup"><span data-stu-id="e7113-120">Type</span></span>|<span data-ttu-id="e7113-121">再ターゲット中</span><span class="sxs-lookup"><span data-stu-id="e7113-121">Retargeting</span></span>|
|<span data-ttu-id="e7113-122">影響を受ける API</span><span class="sxs-lookup"><span data-stu-id="e7113-122">Affected APIs</span></span>|<ul><li><xref:System.Net.Security.SslStream?displayProperty=nameWithType></li><li><xref:System.Net.WebRequest?displayProperty=nameWithType></li><li><xref:System.Net.HttpWebRequest?displayProperty=nameWithType></li><li><xref:System.Net.FtpWebRequest?displayProperty=nameWithType></li><li><xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType></li><li><xref:System.Net.Http?displayProperty=nameWithType></li></ul>|

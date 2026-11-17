<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>Free VPN Test</title>
</head>
<body>
    <h1>IP & Timezone Test</h1>
    <div id="result"></div>

    <script>
        async function testProxy() {
            const resultDiv = document.getElementById('result');
            
            try {
                // ipinfo.io にアクセス（GitHub/Cloudflare経由になる）
                const response = await fetch('https://ipinfo.io/json');
                const data = await response.json();
                
                const now = new Date();
                const timezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
                
                resultDiv.innerHTML = `
                    <p><strong>IP:</strong> ${data.ip}</p>
                    <p><strong>Location:</strong> ${data.city}, ${data.region}, ${data.country}</p>
                    <p><strong>ISP/Org:</strong> ${data.org}</p>
                    <p><strong>Timezone:</strong> ${timezone}</p>
                    <p><strong>Local Time:</strong> ${now.toLocaleString()}</p>
                    
                    ${data.org.includes('GitHub') || data.org.includes('Cloudflare') 
                        ? '<p style="color:green;">✓ GitHub/Cloudflare IP detected.</p>' 
                        : '<p style="color:red;">✗ Unexpected IP type.</p>'}
                `;
                
                // ここで重要：IPの国とブラウザのタイムゾーンが一致しているか確認
                if (data.country !== 'US' && timezone !== 'UTC') {
                     // 例: IPがUSなのにタイムゾーンがAsia/Tokyoなら不自然
                     console.warn("IP and Timezone mismatch detected!");
                }

            } catch (error) {
                resultDiv.innerHTML = `<p>Error: ${error.message}</p>`;
            }
        }

        testProxy();
    </script>
</body>
</html>

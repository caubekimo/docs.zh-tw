---
title: 風險降低︰WCF 服務和憑證驗證
ms.date: 03/30/2017
ms.assetid: ef19c91a-b9df-4bf0-a28e-eb1e99c4bc95
ms.openlocfilehash: 8c8493efa2c3223809ad87e01e3faddaea859ca8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "73457790"
---
# <a name="mitigation-wcf-services-and-certificate-authentication"></a>風險降低︰WCF 服務和憑證驗證

.NET Framework 4.6 新增了 TLS 1.1 和 TLS 1.2 至 WCF SSL 的通訊協定預設清單。 當用戶端和伺服器電腦安裝 .NET Framework 4.6 或更新版本時，會使用 TLS 1.2 進行交涉。

## <a name="impact"></a>影響

TLS 1.2 不支援 MD5 憑證驗證。 因此，如果客戶使用的 SSL 憑證使用 MD5 執行雜湊演算法，WCF 用戶端就無法連線到 WCF 服務。 如需詳細資訊，請參閱[風險降低︰WCF 服務和憑證驗證](mitigation-wcf-services-and-certificate-authentication.md)。

## <a name="mitigation"></a>降低

您可以執行下列其中一項動作來解決這個問題，使 WCF 用戶端可以連線到 WCF 伺服器︰

- 更新憑證為不使用 MD5 演算法。 這是建議的解決方案。

- 如果繫結在原始程式碼中不是以動態方式設定，請更新應用程式的組態檔，以使用 TLS 1.1 或舊版的通訊協定。 這可讓您繼續使用執行 MD5 雜湊演算法的憑證。

  > [!CAUTION]
  > 我們不建議採取這項因應措施，因為使用 MD5 雜湊演算法的憑證並不安全。

  下列組態檔示範如何進行：

  ```xml
  <configuration>
      <system.serviceModel>
          <bindings>
              <netTcpBinding>
                  <binding>
                      <security mode= "None|Transport|Message|TransportWithMessageCredential" >
                          <transport clientCredentialType="None|Windows|Certificate"
                                              protectionLevel="None|Sign|EncryptAndSign"
                                              sslProtocols="Ssl3|Tls1|Tls11">
                          </transport>
                      </security>
                  </binding>
              </netTcpBinding>
          </bindings>
      </system.serviceModel>
  </configuration>
  ```

- 如果繫結在原始程式碼中是以動態方式設定，請更新 <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A?displayProperty=nameWithType> 屬性，以在原始程式碼中使用 TLS 1.1 (<xref:System.Security.Authentication.SslProtocols.Tls11?displayProperty=nameWithType>) 或舊版的通訊協定。

  > [!CAUTION]
  > 我們不建議採取這項因應措施，因為使用 MD5 雜湊演算法的憑證並不安全。

## <a name="see-also"></a>另請參閱

- [應用程式相容性](application-compatibility.md)

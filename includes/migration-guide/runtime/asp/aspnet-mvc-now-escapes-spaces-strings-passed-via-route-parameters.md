---
ms.openlocfilehash: 7444ddbdd4a7c5f731fba8528ee2334374fc254e
ms.sourcegitcommit: 7980a91f90ae5eca859db7e6bfa03e23e76a1a50
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81275054"
---
### <a name="aspnet-mvc-now-escapes-spaces-in-strings-passed-in-via-route-parameters"></a>ASP.NET MVC 現在會將透過路由參數傳入之字串中的空格逸出

|   |   |
|---|---|
|詳細資料|為了遵守 RFC 2396，從路由填入動作參數時，現在會將路由路徑中的空格逸出。 因此，雖然 <code>/controller/action/some data</code> 先前會比對路由 <code>/controller/action/{data}</code> 並提供 <code>some data</code> 作為資料參數，但現在會改為提供 <code>some%20data</code>。|
|建議|您應該更新程式碼，以將路由中的字串參數設為未逸出。 如果需要原始 URI，您可以使用 <xref:System.Net.HttpWebRequest.RequestUri>.OriginalString API 來存取。|
|影響範圍|Minor|
|版本|4.5.2|
|類型|執行階段|
|受影響的 API|<ul><li><xref:System.Web.Mvc.RouteAttribute.%23ctor(System.String)></li></ul>|

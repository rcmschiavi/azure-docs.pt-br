---
title: Upgrade Bing News Search API v5 to v7 | Microsoft Docs
description: Identifies the parts of your application that you need to update to use version 7.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 5334C475-4841-4736-A66E-DC1E07CBCEC9
ms.service: cognitive-services
ms.technology: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e55f18e219d79fd8b34d5281a0b4061e7dd0dc60
ms.contentlocale: pt-br
ms.lasthandoff: 07/06/2017

---

# <a name="news-search-api-upgrade-guide"></a>News Search API upgrade guide

> [!NOTE]
> V7 is a Preview release. All aspects of the API and documentation are subject to change. Use only for testing in a non-production environment.

This upgrade guide identifies the changes between version 5 and version 7 of the Bing News Search API. Use this guide to help you identify the parts of your application that you need to update to use version 7.

## <a name="breaking-changes"></a>Breaking changes

### <a name="endpoints"></a>Endpoints

- The endpoint's version number changed from v5 to v7. For example, https://api.cognitive.microsoft.com/bing/\*\*v7.0**/news/search.

### <a name="error-response-objects-and-error-codes"></a>Error response objects and error codes

- All failed requests should now include an `ErrorResponse` object in the response body.

- Added the following fields to the `Error` object.  
  - `subCode`&mdash;Partitions the error code into discrete buckets, if possible
  - `moreDetails`&mdash;Additional information about the error described in the `message` field
   

- Replaced the v5 error codes with the following possible `code` and `subCode` values.

|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing returns ServerError whenever any of the sub-code conditions occur. The response includes these errors if the HTTP status code is 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Blocked|Bing returns InvalidRequest whenever any part of the request is not valid. For example, a required parameter is missing or a parameter value is not valid.<br/><br/>If the error is ParameterMissing or ParameterInvalidValue, the HTTP status code is 400.<br/><br/>If the error is HttpNotAllowed, the HTTP status code 410.
|RateLimitExceeded||Bing returns RateLimitExceeded whenever you exceed your queries per second (QPS) or queries per month (QPM) quota.<br/><br/>Bing returns HTTP status code 429 if you exceeded QPS and 403 if you exceeded QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing returns InvalidAuthorization when Bing cannot authenticate the caller. For example, the `Ocp-Apim-Subscription-Key` header is missing or the subscription key is not valid.<br/><br/>Redundancy occurs if you specify more than one authentication method.<br/><br/>If the error is InvalidAuthorization, the HTTP status code is 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing returns InsufficientAuthorization when the caller does not have permissions to access the resource. This can occur if the subscription key has been disabled or has expired. <br/><br/>If the error is InsufficientAuthorization, the HTTP status code is 403.

- The following maps the previous error codes to the new codes. If you've taken a dependency on v5 error codes, update your code accordingly.

|Version 5 code|Version 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Disabled|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Blocked|InvalidRequest.Blocked

### <a name="object-changes"></a>Object changes

- Added the `contractualRules` field to the [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. The `contractualRules` field contains a list of rules that you must follow (for example, article attribution). You must apply the attribution provided in `contractualRules` instead of using `provider`. The article includes `contractualRules` only when the [Web Search API](../bing-web-search/search-the-web.md) response contains a News answer.

## <a name="non-breaking-changes"></a>Non-breaking Changes

### <a name="query-parameters"></a>Query parameters

- Added Products as a possible value that you may set the [category](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#category) query parameter to. See [Categories By Markets](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#categories-by-market).  
    
- Added the [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortby) query parameter, which returns trending topics sorted by date with the most recent first.  
  
- Added the [Since](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#since) query parameter, which returns trending topics that were discovered by Bing on or after the specified Unix epoch timestamp.

### <a name="object-changes"></a>Object changes

- Added the `mentions` field to the [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. The `mentions` field contains a list of entities (persons or places) that were found in the article.  
  
- Added the `video` field to the [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. The `video` field contains a video that's related to the news article. The video is either an \<iframe\> that you can embed or a motion thumbnail.   
  
- Added the `sort` field to the [News](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) object. The `sort` field shows the sort order of the articles. For example, the articles are sorted by relevance (default) or date.  
  
- Added the [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortvalue) object, which defines a sort order. The `isSelected` field indicates whether the response used the sort order. If **true**, the response used the sort order. If `isSelected` is **false**, you can use the URL in the `url` field to request a different sort order.


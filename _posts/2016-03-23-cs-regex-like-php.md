---
layout: post
title: CSharp 정규식 함수를 PHP 처럼 사용
date: 2016-03-23T15:47:00+09:00
categories: [CSharp]
description: CSharp Regex 함수
comments: false
tags: [csharp]
---
```csharp
//HTML 태그 제거
public string RemoveHTMLTag(String text)
{
    return System.Text.RegularExpressions.Regex.Replace(text, @"<(.|\n)*?>", String.Empty);
}

//PHP에 있는 preg_match와 동일
private string preg_match(string RegexSource, string BaseSource)
{
    Regex regex = new Regex(RegexSource);
    Match mc = regex.Match(BaseSource);

    return RemoveHTMLTag(mc.Value);
}

//PHP에 있는 preg_match_all와 동일
private List<string> preg_match_all<T>(string RegexSource, string BaseSource)
{
    Regex regex = new Regex(RegexSource);
    MatchCollection mc = regex.Matches(BaseSource);

    string Downgrade;

    List<string> list = new List<string>();

    foreach (Match m in mc)
    {
        Downgrade = RemoveHTMLTag(m.Value);
        list.Add(Downgrade);
    }

    return list;
}
```
얼마전까지 사용하던 코드. PHP 의 preg 를 생각하면서(..) 짠거라서 함수명도 동일하게 작성했다. 다만, 얼마전에 사용하려고 보니 살짝 개조가 필요했다. 참고로 HTML 태그가 자동으로 strip 하게끔 짜여있는데 되도록 제거 후 사용해야 한다. 좋은 방법은 아님.

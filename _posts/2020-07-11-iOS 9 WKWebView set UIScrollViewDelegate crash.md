---
title: "iOS 9 WKWebView set UIScrollViewDelegate crash"
categories: 
  - iOS
last_modified_at: 2020-07-11T13:00:00+09:00

toc: true
---
2020.07.11

&nbsp; iOS 9 버전에서 WKWebView를 사용할 때, scrollView의 delegate를 설정했을 때 발생하는 크래쉬 이슈이다.

```
webView.scrollView.delegate = self;
```

```
- [UIScrollView setDelegate:] + 40
- [WKScrollView _updateDelegate] + 228
- [WKWebView dealloc] + 216
```

&nbsp; 해당 이슈는 iOS 9 이하의 버전에서만 발생하는 이슈로, webView가 dealloc 된 후 (webView = nil), nil에 scrollView의 delegate가 할당되어서 발생하는 이슈로 판단된다.

### 해결방법

&nbsp; webView가 dealloc 되기 전, delegate 설정을 초기화해주면 된다.

```
webView.scrollView.delgate = nil;
```

### 참고링크
https://www.programmersought.com/article/42711586898/
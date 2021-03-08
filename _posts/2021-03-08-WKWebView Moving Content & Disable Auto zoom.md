---
title: "WKWebView Moving Content & Disable Auto zoom"
categories: 
  - iOS
  - Objective-C
  - WKWebView
last_modified_at: 2021-03-08T13:00:00+09:00

toc: true
---
2021.03.08

### 키보드 높이만큼 웹뷰 올리기

&nbsp; WKWebView에서 키보드가 올라왔을 때 키보드 때문에 내용이 가려지게 된다. 아래 코드를 viewController에 구현을 해주면, 키보드가 올라올 때 그 만큼 웹뷰도 함께 올라가게 된다.

```
// Call this method somewhere in your view controller setup code.
- (void)registerForKeyboardNotifications
{
    [[NSNotificationCenter defaultCenter] addObserver:self
            selector:@selector(keyboardWasShown:)
            name:UIKeyboardDidShowNotification object:nil];
 
   [[NSNotificationCenter defaultCenter] addObserver:self
             selector:@selector(keyboardWillBeHidden:)
             name:UIKeyboardWillHideNotification object:nil];
 
}
 
// Called when the UIKeyboardDidShowNotification is sent.
- (void)keyboardWasShown:(NSNotification*)aNotification
{
    NSDictionary* info = [aNotification userInfo];
    CGSize kbSize = [[info objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue].size;
 
    UIEdgeInsets contentInsets = UIEdgeInsetsMake(0.0, 0.0, kbSize.height, 0.0);
    scrollView.contentInset = contentInsets;
    scrollView.scrollIndicatorInsets = contentInsets;
 
    // If active text field is hidden by keyboard, scroll it so it's visible
    // Your app might not need or want this behavior.
    CGRect aRect = self.view.frame;
    aRect.size.height -= kbSize.height;
    if (!CGRectContainsPoint(aRect, activeField.frame.origin) ) {
        [self.scrollView scrollRectToVisible:activeField.frame animated:YES];
    }
}
 
// Called when the UIKeyboardWillHideNotification is sent
- (void)keyboardWillBeHidden:(NSNotification*)aNotification
{
    UIEdgeInsets contentInsets = UIEdgeInsetsZero;
    scrollView.contentInset = contentInsets;
    scrollView.scrollIndicatorInsets = contentInsets;
}

```


### 웹뷰 input Field 선택 시, 자동으로 확대되는 것 막기

&nbsp; 웹뷰에서 input Field를 선택하게 될 때, 자동으로 확대가 되버린다. 이를 막기 위해선 html head에 아래를 추가해주면 해결된다.

```
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1,user-scalable=0"/>
```

### 참고링크
* [Moving Content That Is Located Under the Keyboard
](https://developer.apple.com/library/archive/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html)
* [Disable WKWebView zoom](https://rick38yip.medium.com/disable-wkwebview-zoom-when-clicking-on-text-input-html-fields-by-injecting-css-5372adc1ae97)

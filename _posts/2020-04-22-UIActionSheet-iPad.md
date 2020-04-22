---
title: "UIActionSheet iPhone/iPad 적용"
categories: 
  - iOS
last_modified_at: 2020-04-22T13:00:00+09:00

toc: true
---
2020.04.22

## UIActionSheet 이란
쉽게 말하면 화면에 버튼 목록을 말한다. 아이폰 사용자라면 밑의 이미지가 익숙할 것이다. <br>
<img src="https://user-images.githubusercontent.com/58776221/80002806-16a82f00-84fb-11ea-8272-06ba83054042.png"/> <br>
출처 : https://www.ioscreator.com/tutorials/swiftui-action-sheet-tutorial <br>

```
Important: UIActionSheet is deprecated in iOS 8. (Note that UIAction
SheetDelegate is also deprecated.)To create and manage action sheets in 
iOS 8 and later, instead use UIAlertController with a preferred
Style of UIAlertControllerStyleActionSheet.
```

UIActionSheet이 iOS 8.3 이후부터는 사용할 수 없다...<br>
대신에 `UIAlertController`를 사용하고, `preferredStyle`에 `UIAlertControllerStyleActionSheet`을 설정해야 한다.

## iPhone에 UIActionSheet 적용

```objectivec
UIAlertController *actionSheet = [UIAlertController alertControllerWithTitle:@"Title" message:@"Message" preferredStyle:UIAlertControllerStyleActionSheet];

UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:^(UIAlertAction *action){
    NSLog(@"OK");
}];
[actionSheet addAction:okAction];
[actionSheet show];
```

## iPad에 UIActionSheet 적용

iPad에 적용할 경우, popoverPresentationController를 사용해야 한다.<br>
화면 가운데에 출력학 위해서 CGRect x,y 좌표를 설정해야 한다.

```objectivec
UIAlertController *actionSheet = [UIAlertController alertControllerWithTitle:@"Title" message:@"Message" preferredStyle:UIAlertControllerStyleActionSheet];

UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:^(UIAlertAction *action){
    NSLog(@"OK");
}];
[actionSheet addAction:okAction];
[actionSheet setModalPresentationStyle:UIModalPresentationPopover];
[actionSheet.popoverPresentationController setPermittedArrorDirections:0];

CGRect sourceRect = CGRectZero;
sourceRect.origin.x = CGRectGetMidX(actionSheet.view.bounds) - actionSheet.view.frame.origin.x/2.0;
sourceRect.origin.y = CGRectGetMidY(actionSheet.view.bounds) - actionSheet.view.frame.origin.y/2.0;

popoverPresentationController.sourceRect = sourceRect;
popoverPresentationController.sourceView = actionSheet.view;

[actionSheet show];
```

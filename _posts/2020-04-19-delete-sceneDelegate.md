---
title: "[Xcode] SceneDelegate를 없애고 iOS App 실행하기"
categories: 
  - iOS
last_modified_at: 2020-04-19T13:00:00+09:00

toc: true
---
2020.04.19

Xcode11부터 iOS App 프로젝트를 생성하면 기본적으로 SceneDelegate가 적용된 템플릿이 생성됩니다. 이번 글에서는 SceneDelegate를 사용하지 않고 iOS App을 빌드하는 방법을 적겠습니다. 첨부하는 이미지는 Objective-C 기반의 프로젝트 바탕입니다.

### 1. 프로젝트 생성
iOS Single View App으로 기본 프로젝트를 생성합니다.<br>
<img width="663" alt="스크린샷 2020-04-20 오전 12 47 26" src="https://user-images.githubusercontent.com/58776221/79693517-16fbbc80-82a6-11ea-85ef-d56b4a4419b9.png"> <br>
SceneDelegate가 생성된 것을 볼 수 있습니다.<br>
정상실행을 확인하기 위해서 Main.storyboard에 라벨을 추가했습니다. 밑의 그림은 앱의 정상작동 화면입니다.<br>
<img width="397" alt="스크린샷 2020-04-20 오전 12 52 11" src="https://user-images.githubusercontent.com/58776221/79693534-2f6bd700-82a6-11ea-9d06-1c607582c954.png">

### 2. info.plist에서 Application Scene Manifest 항목을 삭제합니다.
<img width="1022" alt="스크린샷 2020-04-20 오전 12 53 58" src="https://user-images.githubusercontent.com/58776221/79693545-401c4d00-82a6-11ea-9e9e-0f339cbaf679.png">

### 3. SceneDelegate 파일을 삭제합니다.
objC 기반의 프로젝트인 경우 SceneDelegate.h, SceneDelegate.m 파일을 삭제합니다.<br>
Swift 기반의 프로젝트인 경우 SceneDelegate.swift 파일을 삭제합니다.<br>

### 4. 메서드 삭제
AppDelegate 클래스에서 `configurationForConnectingSceneSession`, `didDiscardSceneSessions` 메서드를 삭제합니다.<br>
<img width="1277" alt="스크린샷 2020-04-20 오전 12 58 37" src="https://user-images.githubusercontent.com/58776221/79693552-52968680-82a6-11ea-87e4-e9221d90c72d.png"><br>
그대로 프로젝트를 실행하면 화면이 출력되지 않고 동시에 다음과 같은 로그를 확인할 수 있습니다. <br>
<img width="405" alt="스크린샷 2020-04-20 오전 1 01 01" src="https://user-images.githubusercontent.com/58776221/79693573-617d3900-82a6-11ea-8089-decc0d63488f.png"> <br>

```
2020-04-20 01:04:27.108826+0900 sampleApp[3738:123971] [Application] The app delegate must implement the window property if it wants to use a main storyboard file.
```

<br>
AppDelegate에 window property를 구현하라는 의미인 것 같습니다. `UIApplicationDelegate` 내부를 살펴보면 window property가 있습니다.<br>
<img width="992" alt="스크린샷 2020-04-20 오전 1 20 48" src="https://user-images.githubusercontent.com/58776221/79693584-6fcb5500-82a6-11ea-9af4-1b40bab56500.png"> <br>
AppDelegate.h에 프로퍼티를 선언합니다.<br>

```
// AppDelegate.h
#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>

@property (nonatomic, strong) UIWindow *window;

@end
```

<br>
실행해보면 제대로 동작하는 것을 확인할 수 있습니다.<br>
<img width="992" alt="스크린샷 2020-04-20 오전 1 20 48" src="https://user-images.githubusercontent.com/58776221/79693584-6fcb5500-82a6-11ea-9af4-1b40bab56500.png">

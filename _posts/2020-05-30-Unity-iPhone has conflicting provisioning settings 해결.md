---
title: "Unity-iPhone has conflicting provisioning settings 해결방법"
categories: 
  - iOS
  - Unity
last_modified_at: 2020-05-30T13:00:00+09:00

toc: true
---
2020.05.30


## 이슈
* Unity 빌드 시 Signing&Capabilities에서 Automatically managing signing 체크박스 선택 시 에러 발생

<img src="https://user-images.githubusercontent.com/58776221/83326855-22350580-a2b2-11ea-8d16-264c96630949.png"/>

## 에러 메시지

```
Unity-iPhone has conflicting provisioning settings.
Unity-iPhone is automatically signed for development, but a conflicting code signing identity iPhone Distribution has been manually specified. Set the code signing identity value to "iPhone Developer" in the build settings editor, or switch to manual signing in the Signing & Capabilities editor.
```

## 해결방법
* Build Settings -> Code Signing Identify (All, Levels) 이동
* Any iOS SDK에서 iOS Developer 선택 (Debug, Release, ReleaseForProfiling, ReleaseForRunning 모두)

<img src="https://user-images.githubusercontent.com/58776221/83326861-3416a880-a2b2-11ea-8f9e-8c7c31fa66dc.png"/>

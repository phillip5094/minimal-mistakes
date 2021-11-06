---
title: "Unity iOS Resolver에서 xcworkspace 생성되지 않는 이슈"
categories: 
  - iOS
  - Unity
last_modified_at: 2021-11-07T13:00:00+09:00

toc: true
---
2021.11.07


# Unity iOS Resolver에서 xcworkspace 생성되지 않는 이슈

## 1. 현상

* Cocoapods을 1.11.2로 업데이트를 하니, iOS Resolver 빌드할 때 에러와 함께 xcworkspace가 생성되지 않는 이슈를 발견했다.
* 에러 로그

```
iOS framework addition failed due to a CocoaPods installation failure. This will will likely result in an non-functional Xcode project.

After the failure, "pod repo update" was executed and succeeded. "pod install" was then attempted again, and still failed. This may be due to a broken CocoaPods installation. See: https://guides.cocoapods.org/using/troubleshooting.html for potential solutions.

pod install output:



    [33mWARNING: CocoaPods requires your terminal to be using UTF-8 encoding.
    Consider adding the following to ~/.profile:

    export LANG=en_US.UTF-8
    [0m
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/unicode_normalize/normalize.rb:141:in `normalize': Unicode Normalization not appropriate for ASCII-8BIT (Encoding::CompatibilityError)
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/config.rb:166:in `unicode_normalize'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/config.rb:166:in `installation_root'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/config.rb:226:in `podfile_path'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/user_interface/error_report.rb:105:in `markdown_podfile'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/user_interface/error_report.rb:30:in `report'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/command.rb:66:in `report_error'
	from /Library/Ruby/Gems/2.6.0/gems/claide-1.0.3/lib/claide/command.rb:396:in `handle_exception'
	from /Library/Ruby/Gems/2.6.0/gems/claide-1.0.3/lib/claide/command.rb:337:in `rescue in run'
	from /Library/Ruby/Gems/2.6.0/gems/claide-1.0.3/lib/claide/command.rb:324:in `run'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/command.rb:52:in `run'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/bin/pod:55:in `<top (required)>'
	from /usr/local/bin/pod:23:in `load'
	from /usr/local/bin/pod:23:in `<main>'
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/unicode_normalize/normalize.rb:141:in `normalize': Unicode Normalization not appropriate for ASCII-8BIT (Encoding::CompatibilityError)
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/config.rb:166:in `unicode_normalize'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/config.rb:166:in `installation_root'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/config.rb:226:in `podfile_path'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/config.rb:205:in `podfile'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/command.rb:160:in `verify_podfile_exists!'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/command/install.rb:46:in `run'
	from /Library/Ruby/Gems/2.6.0/gems/claide-1.0.3/lib/claide/command.rb:334:in `run'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/lib/cocoapods/command.rb:52:in `run'
	from /Library/Ruby/Gems/2.6.0/gems/cocoapods-1.11.2/bin/pod:55:in `<top (required)>'
	from /usr/local/bin/pod:23:in `load'
	from /usr/local/bin/pod:23:in `<main>'
```

* 이에 대한 해결방법을 3가지 찾았다.

## 2. 해결방법

### 1) 그냥 `pod install` 직접 호출

* 생성된 Xcode 프로젝트(xcworkspace가 없는)로 가서 pod install을 직접 호출해준다.
* Podfile에는 문제가 없으니 정상적으로 설치된다.
* 다만, Unity 빌드할 때마다 매번 pod install을 직접 호출해야하는 번거로움이 있다.

### 2) `Use Shell to Execute Cocoapod Tool` 체크박스 해제

* `Assets > External Dependency Manager > iOS Resolver > Settings`로 가서 `Use Shell to Execute Cocoapod Tool` 체크박스를 해제한다.

<img src="https://raw.githubusercontent.com/phillip5094/phillip5094.github.io/master/imgs/20211107/UseSheelToExecuteCocoapodTool.png"/>

* 이 설정은 Cocoapods 명령어 실행 전에 shell 환경설정이 필요할 때 사용하는 것으로 굳이 사용하지 않는다면 체크를 해제해도 무방한 것 같다.
* 다만 누군가 저 설정을 써야한다면 해결방법이 없는듯..


### 3) Cocoapods을 1.10.x 버전으로 다운그레이드

* 현재 이슈가 Cocoapods 1.11.x 버전의 버그이니 1.11.x 버전을 삭제하고 1.10.x 버전으로 다시 설치한다.

---
layout: post
title : "[Swift] App SandBox의 구조와 역할"
categories : Swift
tags : 
    - Swift
    - App Bundle
    - SandBox
    - Bundle Container
    - Data Container
    - iCloud Container
---     
## iOS 앱 샌드박스(App SandBox)

- `샌드박스` 란 어린아이를 보호하기 위해 모래통에서만 놀도록 하는데서 유래한 접근 보안 모델
- iOS 에서는 하나의 앱마다 sandbox를 두고 공유되지 않도록 하여 접근에 대해 보호함
- sandbox 내의 파일만 정보를 주고 받을 수 있도록 함
- 전략
  - 앱이 시스템과 상호 작용하는 방식을 설명할 수 있습니다. 시스템에서 작업을 완료하는 데 필요한 접근권한을 앱에 "부여"하는 것이죠.
  - 열기 및 저장, 드래그 앤 드롭 및 친숙한 사용자 상호 작용을 통해 앱에 투명하게 추가 접근 권한을 부여할 수 있습니다.
        
![Untitled](https://user-images.githubusercontent.com/110437548/229504418-d3456290-034f-40ac-9258-4e76b7c2afbf.png)   

- 샌드박스가 없을 경우 → 앱에서 사용자의 모든 데이터에 접근 가능 & 모든 시스템 자원이 앱에 접근 가능
- 이러한 보안 문제로 인해 iOS 정책은 사용자 앱에 모든 리소스들이나 사용자 데이터에 접근 불가능하도록 함

    (샌드박스 내의 파일만 접근 가능)   
![image](https://user-images.githubusercontent.com/110437548/229504448-62e99476-410d-421d-9206-d66ca266756c.png)   
![image](https://user-images.githubusercontent.com/110437548/229504483-f663c870-0b25-4516-b413-f76a6c721372.png)   



위와 같은 구조로 하나의 앱에 샌드박스가 구성되어 있다.    

샌드박스 내에는 파일, 환경설정, 네트워크 리소스, 하드웨어 등에 대한 앱의 접근을 제한하는 세분화된 제어 집합이다.   

데이터를 분리하고 고립시키고 보안 침해의 가능성을 낮춘다.   

샌드박스 내부에는 다양한 역할을 하는 Container들이 있다.    

하나 하나 살펴보자.   

## 1. Bundle

- Bundle의 경로 : Bundle.main.bundlePath
- Bundle의 특징
    - Bundle Container은 파일 시스템 내의 하나의 디렉토리
    - 실행 가능(Executable 파일), info.plist 파일, Resources(이미지, 사운드, strings 등)을 함께 그룹화
    - 읽기 전용이므로 수정이 필요한 경우 데이터 컨테이너로 옮겨서 작업
    - iTunes, iCloud에 백업이 되지 않음
    - 컴파일이 실행되는 동안 파일이 생성
- 커스텀 파일을 어디에 배치해야 할지 결정할 때 앱 번들 구조에 대한 이해가 필요하다!

### Bundle Container의 구조
   
```xml
MyApp.app
   MyApp
   MyAppIcon.png
   MySearchIcon.png
   Info.plist
   Default.png
   MainWindow.nib
   Settings.bundle
   MySettingsIcon.png
   iTunesArtwork
   en.lproj
      MyImage.png
   fr.lproj
      MyImage.png
```   


1. MyApp (필수)   

  - 앱의 코드를 포함하고 있는 실행가능한 파일 
  - .app 확장자를 뗀 것이 실제 앱 프로젝트의 이름과 같음      
    
* * *         


2. Application icons((MyAppIcon.png, MySearchIcon.png, and MySettingsIcon.png)    

  - 앱 아이콘은 앱을 표시하는데 사용   
  - 예를 들어 홈 스크린, 검색 결과 그리고 설정에서 앱이 앱의 아이콘으로 표시   
  - 대부분의 경우 앱 아이콘을 꼭 포함해야 한다.         
     
* * *   


3. Info.plist (필수)    
  - bundle ID, 버젼 번호 등 앱에 대한 구성(configuration) 정보를 포함하고 있는 파일     
         
* * *  


4. Launch images(Default.png)   
  - 앱의 시작 인터페이스를 보여주는 이미지이고 시스템은 제공된 런치 이미지 중 하나를 앱이 윈도우와 유저 인터페이스를 로드할 동안 임시로 사용한다.   
  - 만약 임시 런처 이미지가 없다면 검은 화면이 보여진다.        
    
 * * *   

5. MainWindow.nib    
  - 앱의 main nib file은 앱 런치 시간에 앱을 로드하기 위한 기본 인터페이스 객체를 포함한다.   
  - 보통 앱의 메인 윈도우 객체와 앱 델리게이트 객체를 갖고 있다.          
   
* * *     


6. Settings.bundle   
  - 앱의 application-specific preferences를 포함하는 특별한 타입의 플러그인이다.   
  - 이 번들은 property list와 구성하기 윈한 다른 리소스 파일이 포함되어 있고 preference를 보여준다.    
          
* * *   


7. Custom resource files
  - non-localized 리소스들은 최상위 디렉토리에 위치하고 localized 리소스는 language-specific 하위 디렉토리에 위치한다.   
    
* * *   



## 2. Data

![image](https://user-images.githubusercontent.com/110437548/229504536-639d5b6a-328b-458b-ad77-390395d9ff7c.png)   

1. Documents

- 유저가 앱을 통해 생성한 파일, 다운로드한 파일같은 것들을 저장해주는 디렉터리 (음악, pdf 등)
- 설정에 따라 유저가 직접 파일 추가 및 삭제 가능 ( info.plist에서 UIFileSharingEnabled = YES로 설정)
- 하드디스크와 같은 저장소 역할
- 유저에게 노출되는 파일만 저장해야한다. 내부의 파일들은 ITuns와 iCloud에 백업된다

ex) 그림 파일이나 텍스트 파일 / 메모 / 동영상   

---   

1-1. Documents / inbox

- Documents의 내부 inbox
- **외부 앱에서 요청하여 가져온 데이터를 inbox에 저장**한다
- iCloud/iTunes에 백업된다.
- 타 앱을 통해 전송받은 파일이 저장되는 디렉토리 (메일 앱 첨부파일 등)
    - 읽거나 삭제 가능
    - 새 파일 추가나 기존 파일 수정을 불가능

ex) 메일의 첨부파일 / 사진 편집을 위해 사진첩에서 가져온 사진

---

2-0. Library

- 유저 데이터 파일 및 임시 파일을 제외한 모든 파일들을 관리
- 어플을 구동하기 위해 필요한 데이터 (유저 정보, 앱 설정 데이터) → 앱 구동을 이해 반드시 필요하나, 유저에게 공개되고 싶지 않은 민감한 데이터 저장시 사용
- 앱의 기능이나 관리에 필요한 파일 저장
- 주로 서브디렉토리인 Application Support와 Caches를 이용하지만 커스텀 디렉토리 사용이 가능
- Preference, Cookies, Saved Application State, WebKit등 필요할 때 시스템에서 자동 생성
- 하위에 Application Support / Cache / Preferences 가 존재
- iCloud/iTunes에 백업된다.

---

2-1. Library / Application Supprot

- 앱의 기능 또는 관리를 위해 지속적으로 관리해야되는 파일 저장
- Documents와 거의 동일한 속성을 가지지만, 유저에 대한 노출 여부에 따라 위치가 결정됩니다.
- 주로 BundleID나 회사명 등의 서브디렉토리로 만들어 관리
- CoreData 기본 저장 경로
- Realm은 Documents경로를 사용하는데 Documents는 노출이 되기 때문에 중요한 정보들이 저장되어있으면 Application Support폴더로 변경해서 사용하는 것이 좋다.
- iCloud/iTunes에 백업된다.

ex) 메모장 앱의 코어 데이터 저장소로 사용 / 앱 생성 데이터 / 채팅앱의 대화내역

카카오톡의 경우 서버에 메시지가 보존되는 기간이 3일이다. 메시지가 기기를 통해서 들어오면 기기의 로컬 데이터베이스에 저장된다. 지속적으로 관리되고 유저의 접근이 닿지 않는곳, 백업이 되는 곳은 applicaion support이다.

---

2-2. Library / Caches

- 앱의 동작 속도/데이터 절약 등을 위해 사용되는 공간
- 예를 들어 Background에서 Foreground로 넘어올 때 스냅샷 이미지로 사용됨.
- 앱이 쉽게 재생성할 수 있는 파일, 쉽게 다운로드 받을 수 있는 파일들이 저장됨.
- 앱이 실행중에 삭제되지 않는 것이 보장되며 백업되지 않는다.

ex) Background에서 Foreground로 넘어올 때 스냅샷 이미지   
ex) 웹 서버에서 받아온 임시 데이터

---

2-3. Library / Preferences

- 앱의 중요 설정들이 담김.
- **NSUserDefaults** 를 사용해 파일을 만들어 저장

---


3. temp
- 임시 데이터 저장
- 잠깐 쓰고 버릴 데이터 저장
- OS나 유저에 의해 종종 비워지는 공간
- 현재 앱 실행중에는 사용하지만 유지할 필요 없는 임시 파일 저장
- 사용 후 필요없어진 파일은 직접 삭제해주는 것을 권장
- iCloud/iTunes에 백업되지 않는다.

ex) 내 메모를 외부로 내보내기 위한 백업파일

## 참고

[https://jinnify.tistory.com/26](https://jinnify.tistory.com/26)

[https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1)

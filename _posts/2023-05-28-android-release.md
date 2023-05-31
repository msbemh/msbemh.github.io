---
sidebar:
  nav: "docs"
title : Android App Release
---
안드로이드 앱 배포를 해보자

***

**준비사항**    


+ **로깅 비활성화**
Groovy의 경우 `debugger false`   
Kotilin의 경우 `isDebuggable = false` 로 설정 합니다.   
(저는 Groovy로 했습니다 😊)

```groovy
android {
    ...
    buildTypes {
      release {
        debuggable false
        ...
      }
      debug {
        debuggable true
        ...
      }
    }
    ...
  }
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**Gradle** : 빌드 자동화 시스템   
Groovy 언어 or Kotlin 언어를 사용하여 스크립트를 작성할 수 있습니다.   
**Groovy** : 자바 플랫폼에서 실행되는 스크립트 언어입니다.   
**Kotilin** : JetBrains에서 개발한 프로그래밍 언어입니다.   
주로 JVM(Java Virtual Machine) 상에서 실행되며, Java와 호환성이 높습니다.   
{: .notice--info}

+ **앱 버전정보 세팅**   
Gradle build 파일에서 버전을 세팅할 수 있습니다.   
`build.gradle(Module :app)`   

```groovy
android {
    namespace 'com.example.calendarmanage'
    compileSdk 33

    defaultConfig {
        applicationId "com.example.calendarmanage"
        minSdk 24
        targetSdk 33
        versionCode 1
        versionName "1.0"
        // Required ONLY if your minSdkVersion is below 21
        multiDexEnabled true

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }
    productFlavors {
            demo {
                ...
                versionName "1.1-demo"
            }
            full {
                ...
            }
        }
    ...
}

```

> `applicationId`   
고유한 애플리케이션 ID   
이 ID로 기기와 Google Play 스토어에서 앱을 고유하게 식별할 수 있습니다.

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
앱을 게시한 후에는 애플리케이션 ID를 변경하지 마세요.    
변경하면 Google Play 스토어에서 후속 업로드를 새 앱으로 간주합니다.   
applicationId 를 com.example.* 이런식으로 지으면 PlayStore에 배포하지 못합니다.
{: .notice--info}

> `namespace`   
`R` 클래스의 네임스페이스로 사용합니다.   
예를 들어 위의 빌드 파일에서 `R` 클래스는 `com.example.calendarmanage.R`에 생성됩니다.


> `versionCode`  
내부 버전 번호로 사용되는 양의 정수입니다.   
숫자가 클수록 최신 버전임을 나타냅니다.     
안드로이드 시스템은 버전코드 값을 사용하여 사용자가 현재 기기에 설치된 버전보다 낮은 버전코드를 가진 APK를 설치하지 못하도록 함으로써 다운그레이드를 방지합니다.   
앱이 연속적으로 릴리스될 때마다 더 큰 값을 사용해야 합니다.   
이전 버전에 이미 사용한 버전코드로는 Play 스토어에 APK를 업로드할 수 없습니다.

> `versionName`   
사용자에게 표시되는 버전 번호입니다.   
예를들어 `<mager\>.<minor>.<point>` 이런식으로 표시할 수 있습니다.

> `productFlavors`   
안드로이드 앱에서 다양한 버전을 만들기 위한 기능 중 하나입니다.   
위 예에서는, demo 버전은 `versionName`을 재정의 했으므로 해당 `versionName`을 가지게 됩니다.   
full 버전은 재정의가 없으므로 default `versionName`을 가지게 됩니다.   

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
`manifest`요소에 직접 앱 버전을 정의하는 경우, Gradle Build 파일의 버전 값이 manifest의 설정을 재정의 합니다.   
헷갈리지 않게 manifest 보다는 Gradle Build에 버전을 명시하는게 더 좋습니다. 😊(아님말고... ㅎ)   
{: .notice--info}

> `compileSdk`   
앱이 해당 SDK 버전에서 빌드됩니다.   
compileSdk를 변경하면 앱의 동작이 변경될 수 있으므로, 새로운 SDK 버전을 사용하기 전에 충분한 테스트를 수행해야 합니다.   

> `minSdkVersion`   
애플리케이션이 실행하는 데 필요한 최소 API 수준을 지정하는 정수입니다.    
Android 시스템은 시스템의 API 수준이 이 속성에 지정된 값보다 낮은 경우 사용자가 애플리케이션을 설치하지 못하도록 합니다.    
항상 이 속성을 선언해야 합니다.

> `targetSdkVersion`   
애플리케이션의 target API 수준을 지정하는 정수입니다. 설정하지 않을 경우 기본값은 `minSdkVersion`에 주어진 값과 동일합니다.   
플랫폼의 API 수준이 앱의 `targetSdkVersion`이 선언한 버전보다 높은 경우 시스템은 앱이 예상대로 계속 작동하도록 호환성 동작을 사용 설정할 수 있습니다.

> Android에서는 `PackageManager.getPackageInfo(java.lang.String, int)` 메소드를 사용하여, 버전 정보를 얻을 수 있습니다.

```java
public PackageInfo getPackageInfo (String packageName, PackageManager.PackageInfoFlags flags)
```

---

**자료 및 리소스 수집**    
앱을 출시하기 위해 준비하려면 몇 가지 지원 항목을 수집해야 합니다.    
최소한 앱 서명을 위한 암호화 키와 앱 아이콘이 포함되어야 합니다.   

+ **암호화키**   
Android에서는 모든 APK를 기기에 설치하거나 업데이트하기 전에 인증서를 사용하여 디지털 서명해야 합니다.  
Google Play 스토어의 경우 2021년 8월 이후에 생성된 모든 앱은 Play 앱 서명을 사용해야 합니다.

**<i class="fa fa-info-circle" aria-hidden="true"></i> 중요**   
앱은 유효 기간이 2033년 10월 22일 이후에 끝나는 암호화 키로 서명해야 합니다.
{: .notice--info}


+ **앱아이콘**   
Google 스토어 등록정보를 게시하려면 앱 아이콘을 제공해야 합니다.    
앱 아이콘이 앱의 런처 아이콘을 대체하지는 않지만, 화질과 해상도가 높아야 하며 Google Play 아이콘 디자인 사양을 따라야 합니다.

**<i class="fa fa-info-circle" aria-hidden="true"></i> 요구사항**   
32비트 PNG(알파 포함)   
크기: 512x512픽셀   
최대 파일 크기: 1,024KB   
Google Play 아이콘 디자인 사양을 충족합니다.   
순위, 가격, Google Play 카테고리를 암시하거나 사용자에게 오해의 소지를 줄 수 있는 배지 또는 텍스트를 포함하지 않습니다.    
<a href="https://developer.android.com/studio/publish/preparing">자세한 내용은 메타데이터 정책을 참고하세요. </a>   
저는 그림판으로 그리고 GIMP디자인 툴을 이용해서 만들었습니다.
{: .notice--info}

---
**암호화키 생성** 

+ **Upload upload key 와 keystore 생성**   
Android Studio를 이용해서 Upload key와 keystore를 생성하겠습니다.    
<a href="https://developer.android.com/studio/publish/app-signing#generate-key">안드로이드 공식 홈페이지 (Sign your app)</a>

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**Keystore** : 안드로이드 앱에서 사용되는 **암호화된 키**와 **인증서**를 안전하게 저장하는 데 사용되는 시스템입니다. 
{: .notice--info}



> 메뉴바에서 Build > Generate Signed Bundle/APK 클릭   
<img src="{{ site.baseurl }}/assets/images/generate keystore menu.png">

> Android App Bundle 또는 APK 중 아무거나 선택합니다.   
<img src="{{ site.baseurl }}/assets/images/generate keystore menu2.png">

> keystore가 없다면, `Create new...` 버튼클릭   
<img src="{{ site.baseurl }}/assets/images/generate keystore menu3.png">

> 작성을 완료해서 keystore 생성   
<img src="{{ site.baseurl }}/assets/images/generate keystore menu4.png">

+ **upload key를 이용하여 나의 app에 서명하기**   

> `Key store path` 란에 내가 생성한 key store파일 경로를 입력합니다.    
그리고 password, alias를 입력하고 `next`버튼을 클릭합니다.    
password는 keysotre의 password와 일치해야 합니다.   
<img src="{{ site.baseurl }}/assets/images/generate keystore menu5.png">

> 앱 배포가 목적이기 때문에 release만 선택해도 됩니다.   
<img src="{{ site.baseurl }}/assets/images/generate keystore menu6.png">

> {안드로이드 앱 경로}\release 에 aab파일이 생성됩니다.   
이제 Play 앱 서명에 앱을 등록하고 출시할 앱을 업로드할 준비가 되었습니다.

---

**PlayConsole에서 앱 배포**   

 + <a href="https://play.google.com/console/signup">Google Play Store</a>로 이동   

 + Play Console에 로그인

 + 개인용 개발자 계정 만들기 (25$ 지불)

 + 모든 앱 > 앱만들기

 + 대시보드의 앱설정 모든 항목에 응답해야함   

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**개인정보처리방침 화면 필요**
<a href="https://www.privacy.go.kr/front/main/main.do">개인정보 포털</a>로 이동하면 쉽게 만들 수 있습니다.   
"기업 공공 서비스 > 개인정보처리방침 만들기" 에서 만드시면 됩니다.   
`   
**삭제 안내 가이드 화면 필요**   
`   
**앱이콘 및 그래픽이미지 필요**    
{: .notice--info}

 + App Bundle 추가
Upload 키로 Sign한 App Bundle 파일(.abb)을 드래그앤 드랍으로 올리시면 됩니다.   

이제 **Google에서 이 앱을 승인**하면 Playstore에 출시되게 됩니다.





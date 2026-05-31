## 2.2 설정 파일 작성

USB 에 앱 인스톨러가 준비되었다면 하기 과정을 진행합니다.

1. 설치할 앱을 `apps` 폴더 내에 배치
2. 앱 인스톨러 설정 파일(`hrc_installer.cfg`) 파일을 수정.


### 2.2.1. 설치할 앱 배치

COM(제어기 본체)나 TP(티칭펜던트)에 설치하고자 하는 타겟 App 폴더나 파일을 `apps` 폴더 아래에 복사해 넣습니다.

<div style="max-width:fit-content;">

```text
📁 USB_ROOT (USB 최상단)
 ┗ 📁 hi6
    ┗ 📁 apps
       ┗ 📁 Install_Apps
          ┣ 📄 hrc_installer
          ┣ 📄 hrc_installer.cfg
          ┣ 📄 hrc_installer.png
          ┣ 📄 info.json
          ┗ 📁 mastering           <-- (설치할 앱)
```

</div>


### 2.2.2. 앱 인스톨러 설정 파일(`hrc_installer.cfg`) 작성 

설정 파일(`hrc_installer.cfg`)은 일종의 작업 명세서입니다.  
정상 동작을 위해 텍스트 에디터로 하기 양식에 맞게 작성합니다.  


#### 앱 종류

설치하려는 앱의 종류에 따라 앱 인스톨러 설정 파일의 명령어가 달라집니다.  
하기 표를 확인합니다.  

<div style="max-width:fit-content;">


| Category | <a href="https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/en/1-intro/README?cont_model=${cont_model}" style="color:#222222">TP 전용 앱(App)</a> | <a href="https://hrbook-hrc.web.app/#/view/doc-hi6-sdk/en/1-intro/2-plugin-app-concept?cont_model=${cont_model}" style="color:#222222">Plug-in 앱(Apps)</a> |
| :--- | :--- | :--- |
| **설명** | TP 화면에서 직접 구동되는 앱<br>(예: [Cimon Xpanel](https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/ko/2-installation/2-install?cont_model=${cont_model})) | COM 내부 서버를 통해 구동되는 앱<br>(예: [pickit](https://hrbook-hrc.web.app/#/view/doc-hi6-pickit/ko/README)) |
| **설치 위치** | TP  | COM |
| **인스톨러 명령어** | **`xcopy`**  | **`rcopy`**  |



#### 명령어 종류

a. `xcopy`
- **용도**: `TP` 내부에 `TP 전용 앱`을 설치할 때 사용합니다.
- **형식**: `xcopy` \[원본 경로\] \[타겟 경로\]
- 동작 방식
  - 타겟 경로 끝부분의 슬래시(/) 유무에 따라 복사 방식이 달라집니다.
  - 끝에 /가 없을 때 (이름 지정 복사)
    - 원본 폴더/파일이 타겟 경로의 이름으로 덮어쓰기 형태로 복사됩니다.  
    - 예) xcopy $(AppDir)/Xpanel /usr/share/hyundai/hi6/apps/Xpanel2  
          ➔ Xpanel2라는 이름으로 복사됩니다.
  - 끝에 /가 있을 때 (하위 폴더로 복사)
    - 원본 폴더/파일이 타겟 경로의 하위에 원래 이름 그대로 복사됩니다.  
    - 예) xcopy $(AppDir)/Xpanel /usr/share/hyundai/hi6/apps/ 
           ➔ apps 폴더 안에 Xpanel 폴더가 통째로 복사합니다.

b. `rcopy` 
- **용도:** `COM` 내부에 `플러그인 앱`을 설치할 때 사용합니다.
- **형식:** `rcopy` \[원본 경로\] \[타겟 경로\]
- 동작 방식
  - 타겟 경로에 지정된 폴더 내부에 원본 폴더명 그대로 업로드됩니다.
  - 타겟 경로가 서버에 존재하지 않으면 자동으로 계층별 폴더를 생성(mkdir)합니다.
  - 예) rcopy $(AppDir)/pickit $(RemoteAppsDir)/temp  
        ➔ `COM` 에 $(RemoteAppsDir) 에 temp 폴더가 없으면 생성 후에 그 아래에 pickit 폴더를 업로드합니다.


#### 경로 매크로


| 매크로 | 설명 |
| :--- | :--- |
| **`$(AppDir)`** | 현재 USB에 꽂힌 `hrc_installer` 실행 파일이 있는 위치<br>(2.2.1의 Install_Apps 에 해당)|
| **`$(RemoteAppsDir)`** | COM 의 기본 앱 설치 경로 |
| **`$(RemoteReleaseDir)`** | COM 의 빌트인(built-in) 플러그인 앱 설치 경로 |


{% hint style="warning" %}
일반 사용자는 `rcopy` 명령어 사용시, `$(RemoteAppsDir)` 을 타겟 경로로 사용합니다.
{% endhint %}


#### 작성 양식

앱 인스톨러의 양식은 다음과 같습니다.

```bash
# 앱 설명

# 주석
명령어 소스경로 타겟경로
```

- `앱 설명` 란은 첫번째 줄에 주석으로 달아야합니다.  
  해당 란의 내용은 `hrc_installer` 프로그램 실행 시 `타이틀 바`에 표시가 됩니다.  

- `주석` 은 해당 설정 명령어들에 대한 부연을 적는 용도이며, 인스톨러의 로그 화면에서는 확인되지않습니다.  

- `명령줄` 은 상기 `명령어 종류` 섹션의 `형식` 대로 작성합니다.  

예) xcopy

```bash
# XPanel 

#  Xpanel 과 XpanelFiles 를 TP apps 아래에 설치
xcopy Xpanel /usr/share/hyundai/hi6/apps/
xcopy XpanelFiles /usr/share/hyundai/hi6/apps/Xpanel/
```

예) rcopy

```bash
# App_Description 

rcopy $(AppDir)/App_Name $(RemoteAppsDir)
```

</div>


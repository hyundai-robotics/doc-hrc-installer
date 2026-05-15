
[__SOURCE](./README.md)
# ${cont_model} 제어기 기능설명서 - 앱(App) 인스톨러

[__SOURCE](0-about-this-manual/precautions.md)
# 사전 주의사항

{% include file="ko/precautions.md" %}

[__SOURCE](1-intro/README.md)
# 1. 개요

본 설명서를 잘 이해하기 위해서는 아래의 지식을 갖추고 있어야 합니다.

- [${cont_model} 제어기 조작 설명서 - TP630](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/ko-tp630/README?cont_model=${cont_model})
- [${cont_model} 제어기 기능 설명서 - Teach Pendant 앱(App)](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/ko-tp630/README?cont_model=${cont_model})
- [${cont_model} 제어기 기능 설명서 - SDK](https://hrbook-hrc.web.app/#/view/doc-hi6-sdk/ko/README?cont_model=${cont_model})
- [${cont_model} 제어기 기능 설명서 - Open API](https://hrbook-hrc.web.app/#/view/doc-hi6-open-api/ko/README?cont_model=${cont_model})

이 설명서는 `hrc_installer` 기반으로 사용자가 개발한 앱을 설치하는 방법을 안내합니다.


[__SOURCE](2-preparation/README.md)
# 2. 설치 준비

이 장에서는 설치 준비 단계를 설명합니다.

2.1. [`앱 인스톨러` 다운로드](./1-download.md)  
2.2. [`앱 인스톨러` 설정 파일 작성](./2-config.md) 

[__SOURCE](2-preparation/1-download.md)
## 2.1 앱 인스톨러 다운로드

### 2.1.1. 앱 인스톨러 설치 과정

1. [HD현대로보틱스 공식 홈페이지의 다운로드 센터](https://hd-hyundairobotics.com/download-center/list)에서 `hrc_installer.zip` 파일을 다운받습니다.
2. USB 메모리(FAT32 포맷 권장)의 최상단(Root) 경로에 압축을 해제합니다.
3. 하기 디렉토리 구조를 확인합니다.

### 2.1.2. 디렉토리 구조 확인

- 하기 디렉토리 구조와 동일한지 확인합니다.
- 하기 4가지 파일 모두 존재하는지 확인합니다.

<div style="max-width:fit-content;">

{% hint style="warning" %}
하기 구조가 반드시 지켜져야 앱 인스톨러가 TP 에서 정상적으로 확인됩니다.
{% endhint %}

```text
📁 USB_ROOT (USB 최상단)
 ┗ 📁 hi6
    ┗ 📁 apps
       ┗ 📁 Install_Apps
          ┣ 📄 hrc_installer       (인스톨러 실행 파일)
          ┣ 📄 hrc_installer.cfg   (설치 설정 파일)
          ┣ 📄 hrc_installer.png   (인스톨러 아이콘 이미지)
          ┗ 📄 info.json           (앱 정보 파일)
```

</div>

[__SOURCE](2-preparation/2-config.md)
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


| 구분 | [TP 전용 앱](https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/ko/1-intro/README?cont_model=${cont_model}) | [플러그인(plug-in) 앱](https://hrbook-hrc.web.app/#/view/doc-hi6-sdk/ko/1-intro/2-plugin-app-concept?cont_model=${cont_model}) |
| :--- | :--- | :--- |
| **설명** | TP 화면에서 직접 구동되는 앱<br>(예: [Cimon Xpanel](https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/ko/2-installation/2-install?cont_model=Hi6)) | COM 내부 서버를 통해 구동되는 앱<br>(예: [pickit](https://hrbook-hrc.web.app/#/view/doc-hi6-pickit/ko/README)) |
| **설치 위치** | TP  | COM |
| **인스톨러 명령어** | **`xcopy`**  | **`rcopy`**  |


</div>

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

[__SOURCE](3-execution/README.md)
# 3. 실행

앱 인스톨러 준비가 완료된 USB를 이용하여 실제 앱 설치를 진행하고, 그 결과를 확인합니다.

[__SOURCE](3-execution/1-run.md)
## 3.1 인스톨러 실행

USB 준비가 완료되었다면 다음 순서에 따라 인스톨러를 실행합니다.

#### 1단계 

앱 인스톨러 준비가 완료된 USB 메모리를 TP 의 USB 포트에 삽입합니다.

<figure>
  <img src="../_assets/0_usb_ready.png" style="max-height:200px;">
  <figcaption>Fig1. 준비 완료된 USB 폴더 구조와 설정 파일 내용</figcaption>
</figure>

TP 홈에서 USB 연결 상태를 확인합니다.

<figure>
  <img src="../_assets/0_usb_ready_tp_home.png" style="max-height:350px;">
  <figcaption>Fig2. 작업표시줄에서 USB 연결 상태 확인</figcaption>
</figure>



#### 2단계

앱 인스톨러를 확인합니다.  
- TP 홈 > 서비스 > 10: 앱(App) > 위치 클릭 > 설치하려는 앱 인스톨러 클릭

<figure>
  <img src="../_assets/1_app_installer_list.png" style="max-height:350px;">
  <figcaption>Fig3. 인스톨러 확인</figcaption>
</figure>


#### 3단계 

앱 화면 하단의 \[F4: 가동\] 버튼을 클릭하여 인스톨러를 실행합니다.

<figure>
  <img src="../_assets/2_app_installer_executed.png" style="max-height:350px;">
  <figcaption>Fig4. 인스톨러 실행 화면</figcaption>
</figure>

#### 4단계

\[START\] 를 클릭하여 설치를 진행합니다.

<figure>
  <img src="../_assets/3_app_installer_start.png" style="max-height:350px;">
  <figcaption>Fig 5-1. 인스톨러 시작 화면</figcaption>
</figure>

<figure>
  <img src="../_assets/4_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 5-2. 인스톨러 종료 화면</figcaption>
</figure>



#### 5단계

설치가 모두 완료되면 [Exit] 버튼을 눌러 인스톨러를 종료 후, `제어기를 재부팅합니다.`

{% hint style="warning" %}
제어기가 재부팅돼야 설치한 APP 이 정상적으로 동작합니다.
{% endhint %}


[__SOURCE](3-execution/2-result.md)
## 3.2 설치 로그(결과) 확인

설치 진행 상황과 최종 결과는 인스톨러 화면 중앙의 로그 창을 통해 실시간으로 확인할 수 있습니다.  
글자 색상을 통해 상태를 직관적으로 파악할 수 있습니다.

### 3.2.1 로그 색상 안내

- 🔵 파란색: 현재 실행 중인 명령어 (예: xcopy, rcopy 등)

- ⚫ 검은색: 복사 진행 상태 및 일반 세부 정보

- 🟢 초록색: 개별 명령 정상 처리 완료 (Pass)

- 🔴 빨간색: 개별 명령 작업 실패 및 에러 발생 사유 (Fail)


### 3.2.2 최종 설치 결과 판별

- 모든 작업이 끝나면 로그 창의 가장 마지막 줄에 최종 요약 결과가 출력됩니다.

- TotalLines 란, `hrc_installer.cfg` 에서 입력한 명령어 줄 수를 의미합니다.

- 설치 성공: 🟢 PASS: TotalLines=[전체 명령 수], NG=0  
  -> 에러 없이 모든 앱 설치가 정상적으로 완료되었음을 의미합니다.

<figure>
  <img src="../_assets/4_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 6. 인스톨러 정상 종료 화면</figcaption>
</figure>

- 설치 실패: 🔴 FAIL: TotalLines=[전체 명령 수], NG=[실패 건수]  
  -> 일부 또는 전체 명령에서 에러가 발생했음을 의미합니다. 위쪽의 빨간색 에러 메시지를 확인하고 조치해야 합니다.

<figure>
  <img src="../_assets/5_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 7. 인스톨러 실행 실패 화면</figcaption>
</figure>


[__SOURCE](4-troubleshooting/README.md)
# 4. 문제 해결 및 에러 코드

설치 중 `FAIL`이 발생한 경우, 로그 창에 출력된 빨간색 에러 메시지를 확인하고 아래 표를 참고하여 원인을 파악 및 조치하십시오.


| 에러 메시지 (로그 출력) | 발생 원인 및 조치 방법 |
| :--- | :--- |
| **Unsupported command** | 지원하지 않는 명령어입니다. `hrc_installer.cfg` 파일의 명령어(`xcopy`, `rcopy` 등)에 오타가 없는지 확인하십시오. |
| **Failed in deleting previous folder.** | 타겟 경로에 이미 존재하는 기존 폴더를 삭제하지 못했습니다. 제어기 내부 권한 문제이거나 파일이 사용 중일 수 있습니다. |
| **Failed in creating destination folder.** | 대상 폴더를 생성하지 못했습니다. 제어기의 저장 공간 용량이나 권한 설정을 확인하십시오. |
| **Failed in copying file.** | 실제 파일을 복사하는 과정에서 실패했습니다. 원본 파일의 손상 여부나 USB 연결 상태를 확인하십시오. |
| **Fail: Invalid destination. Only '$(RemoteAppsDir)' or '$(RemoteReleaseDir)' are allowed.** | `rcopy` 사용 시 대상 경로 지정이 잘못되었습니다. 타겟 경로에 허용된 매크로만 정확히 사용했는지 확인하십시오. |
| **Fail: Local source path does not exist.** | 명령어에 지정된 로컬 원본 경로(USB 내부)를 찾을 수 없습니다. 경로 오타 또는 실제 파일이 USB에 존재하는지 확인하십시오. |
| **Fail: Network connection failed.** | `rcopy` 실행 시 원격 통신에 실패했습니다. TP와 제어기(COM) 간의 네트워크 연결 상태를 점검하십시오. |
| **Fail: API 'isExist' / 'mkdir' / 'upload' / 'rdelete' call failed.** | 원격 서버의 파일 제어 API(조회/생성/업로드/삭제) 호출에 실패했습니다. 제어기 시스템 상태를 확인하십시오. |
| **Fail: Source disappeared during scan. (Check USB connection)** | 파일 스캔 중 원본 대상이 사라졌습니다. 설치 도중 USB 연결이 끊어졌는지 확인하십시오. |
| **Fail: Cannot open local file. (USB connection / File Permission issue)** | 로컬 파일 읽기에 실패했습니다. 파일의 읽기 권한이나 USB 접속 불량 문제를 확인하십시오. |
| **Fail: Process line crashed. / cannot be started. / failed.** | 사용자가 지정한 외부 시스템 명령어(`>`) 프로세스가 정상적으로 실행되지 않았거나 도중 비정상 종료되었습니다. |

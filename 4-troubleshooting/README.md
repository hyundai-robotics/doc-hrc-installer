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

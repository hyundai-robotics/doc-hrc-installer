<style>
  .blue-box {
    color: dodgerblue; /* 또는 blue */
    font-size: 10%; /* 크기 조절도 가능 */
  }
</style>

## 3.2 설치 로그(결과) 확인

설치 진행 상황과 최종 결과는 인스톨러 화면 중앙의 로그 창을 통해 실시간으로 확인할 수 있습니다.  
글자 색상을 통해 상태를 직관적으로 파악할 수 있습니다.

### 3.2.1 로그 색상 안내

- 🟦 파란색: 현재 실행 중인 명령어 (예: xcopy, rcopy 등)

- ⬛ 검은색: 복사 진행 상태 및 일반 세부 정보

- 🟩 초록색: 개별 명령 정상 처리 완료 (Pass)

- 🟥 빨간색: 개별 명령 작업 실패 및 에러 발생 사유 (Fail)


### 3.2.2 최종 설치 결과 판별

- 모든 작업이 끝나면 로그 창의 가장 마지막 줄에 최종 요약 결과가 출력됩니다.

- TotalLines 란, `hrc_installer.cfg` 에서 입력한 명령어 줄 수를 의미합니다.

- 설치 성공: 🟩 PASS: TotalLines=[전체 명령 수], NG=0  
  -> 에러 없이 모든 앱 설치가 정상적으로 완료되었음을 의미합니다.

<figure>
  <img src="../_assets/4_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 6. 인스톨러 정상 종료 화면</figcaption>
</figure>

- 설치 실패: 🟥 FAIL: TotalLines=[전체 명령 수], NG=[실패 건수]  
  -> 일부 또는 전체 명령에서 에러가 발생했음을 의미합니다. 위쪽의 빨간색 에러 메시지를 확인하고 조치해야 합니다.

<figure>
  <img src="../_assets/5_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 7. 인스톨러 실행 실패 화면</figcaption>
</figure>


# Drill-13: 아이템 모드 구현

게임프레임워크 실습에 타이틀, 일시정지, 아이템 선택 모드를 연결한 과제입니다.

## 저장소

- Public GitHub: [Jun1188/Drill-13](https://github.com/Jun1188/Drill-13)
- 요구사항과 변경 전후 기록: [PRD.md](PRD.md)
- [개발 커밋 기록](https://github.com/Jun1188/Drill-13/commits/main/)
- 실행에 필요한 Python 코드와 PNG 이미지는 이 저장소 루트에 포함되어 있습니다.

## 설치 및 실행

Python과 pico2d가 동작하는 환경에서 다음 명령을 실행합니다.

```sh
git clone https://github.com/Jun1188/Drill-13.git
cd Drill-13
python -m pip install pico2d PySDL2 transitions
python main.py
```

이미지는 현재 작업 폴더에서 불러옵니다. 반드시 `main.py`와 이미지가 있는 저장소 루트에서 실행합니다.
시작하면 로고를 약 2초 표시한 후 타이틀 화면으로 전환합니다.

## 조작법

| 화면 | 키 | 동작 |
|---|---|---|
| 타이틀 | Space | 새 게임 시작 |
| 플레이 | ← / → | 좌우 이동 |
| 플레이 | Space | 선택한 공 발사. 수면 중에는 먼저 기상 |
| 플레이 | M | 아이템 모드 진입 |
| 아이템 | 0 | 공 발사 비활성화 |
| 아이템 | 1 | 작은 공 선택: 21×21 |
| 아이템 | 2 | 큰 공 선택: 41×41 |
| 아이템 | M | 플레이로 복귀 |
| 플레이 | P | 일시정지 |
| 일시정지 | P | 플레이 재개 |
| 플레이·아이템·일시정지 | Esc | 타이틀 복귀 |
| 로고·타이틀 | Esc | 종료 |
| 모든 화면 | 창 닫기 | 종료 |

- 아이템 모드에서는 플레이어 이동과 Space 입력이 차단됩니다. 이미 발사한 공과 게임 시간은 계속 진행합니다.
- 숫자 키는 공 종류만 선택합니다. M으로 복귀한 후 Space를 눌러 발사합니다.
- 선택 변경은 새로 발사하는 공에 적용하며, 이미 발사한 공의 크기는 유지합니다.
- 시작 시 작은 공이 선택되어 있습니다. 타이틀에서 새 게임을 시작하면 기본 선택으로 초기화합니다.
- 캐릭터는 약 5초간 대기하면 잠듭니다. 아이템 모드에서도 수면 시간은 진행됩니다.
- P 일시정지는 캐릭터·공·애니메이션·수면 타이머를 멈춥니다.

## 포함 파일

- 실행 및 모드: main.py, game_framework.py, logo_mode.py, title_mode.py, play_mode.py, pause_mode.py, item_mode.py
- 월드와 객체: game_world.py, boy.py, ball.py, grass.py
- 이미지: animation_sheet.png, grass.png, ball21x21.png, ball41x41.png, item_select.png, title.png, tuk_credit.png, run_animation.png

## 개발 기록과 확인 상태

기존 LEC15 관련 이력과 실제 단계별 변경 이력을 독립 저장소로 이전했습니다. 파일 경로를 저장소 루트로 바꾸면서 커밋 해시는 변경되었습니다. 이전 대응표는 PRD.md에 있습니다.
과제별 최소 20개 실제 개발 커밋 조건은 아직 충족하지 않았습니다. 기존 수업 이력을 새 과제의 개발 기록으로 대체 계산하지 않습니다.
빌드와 실행 테스트는 에이전트가 수행하지 않았습니다. 사용자가 PRD.md의 확인 절차에 따라 실행 결과를 확인해야 합니다.
e-Class 제출은 별도이며 완료되지 않았습니다.

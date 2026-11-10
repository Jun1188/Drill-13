# Drill-13: 아이템 모드 구현

## 근거와 작업 위치
- 사용자 제공 Drill #13 슬라이드: 아이템 모드에서 0·1·2로 발사 없음·작은 공·큰 공을 선택한다.
- 사용자 추가 요구: M으로 진입·복귀, 플레이어 조작 차단, 게임 진행 유지, 단계별 커밋.
- 작업 위치: 다른 Drill과 같은 상위 폴더의 독립 `Drill-13` 저장소.
- 예시 영상은 사용자 지시에 따라 제외했다.

## 요구사항 및 구현 범위
- 기존 게임 프레임워크로 logo_mode, title_mode, play_mode를 연결한다.
- 시작 시 로고를 2초 표시한 후 타이틀로 전환한다.
- 타이틀에서 Space를 누르면 기존 캐릭터 플레이를 시작한다.
- 플레이에서 Esc를 누르면 타이틀로 돌아간다.
- M으로 item_mode에 진입하고 다시 M을 누르면 플레이로 복귀한다. 길게 눌러도 반복 전환하지 않는다.
- 진입 시 이동을 멈추고 방향키 입력을 비운다. 아이템 모드에서는 방향키·Space로 플레이어를 조작할 수 없다.
- 아이템 모드에서도 기존 공 이동, 애니메이션, 수면 대기 시간과 게임 월드 갱신은 계속 진행한다.
- 0: 발사 없음, 1: 기존 21×21 공, 2: 41×41 공. 숫자 키 자체는 공을 발사하지 않는다.
- 선택은 플레이로 복귀한 후 새로 발사하는 공에 적용한다. 이미 발사한 공은 변하지 않는다.
- 기본 선택은 작은 공이다. 타이틀에서 새 게임을 시작하면 기본 선택으로 초기화한다.
- 기존 item_select.png를 중앙에 표시한다. 아이템 모드의 Esc는 타이틀로 복귀한다.
- 플레이에서 P를 누르면 pause_mode를 쌓고 다시 P를 누르면 기존 플레이로 복귀한다.
- 일시정지 중 캐릭터, 공, 애니메이션과 수면 타이머를 정지하고 중앙에 일시정지 기호를 표시한다.
- 일시정지 중 Esc를 누르면 타이틀로 돌아간다. P를 길게 눌러도 반복 전환하지 않는다.
- 로고와 타이틀에서 Esc를 누르거나 어느 화면에서든 창을 닫으면 종료한다.
- 플레이 종료 시 게임 월드를 비워 재시작할 때 객체가 중복되지 않게 한다.

## 실행 방법
- Python과 pico2d, PySDL2(sdl2), transitions가 설치된 환경을 사용한다.
- 작업 디렉터리를 이 폴더로 설정하고 `python main.py`를 실행한다.
- M: 아이템 모드 진입·복귀. 아이템 모드의 0/1/2: 발사 없음/작은 공/큰 공 선택.
- P: 일시정지 및 재개. Esc: 플레이 또는 일시정지에서 타이틀 복귀.
- 방향키 좌우: 이동. Space: 공 발사. 5초 대기: 수면. 수면 중 Space: 기상.

## 필요한 파일
- 코드: main.py, game_framework.py, logo_mode.py, title_mode.py, play_mode.py,
  pause_mode.py, item_mode.py, game_world.py, boy.py, grass.py, ball.py
- 이미지: tuk_credit.png, title.png, animation_sheet.png, grass.png,
  ball21x21.png, ball41x41.png, item_select.png
- 기존 이미지와 의존성을 사용하며 새 의존성을 추가하지 않는다.

## 사용자 테스트 항목
빌드와 테스트는 에이전트가 실행하지 않았다. 사용자가 다음 항목을 확인한다.
1. main.py 실행 후 로고가 약 2초 표시되고 타이틀로 전환되는지 확인한다.
2. 타이틀에서 Space를 누르면 캐릭터와 잔디가 표시되는지 확인한다.
3. 이동, 공 발사, 수면과 기상이 동작하는지 확인한다.
4. 플레이에서 Esc로 복귀한 뒤 다시 시작하면 이전 캐릭터와 공이 남지 않는지 확인한다.
5. 로고와 타이틀의 Esc 및 각 화면의 창 닫기로 정상 종료되는지 확인한다.

6. 공 발사 후 P를 눌러 캐릭터와 공이 멈추고, 다시 P를 누르면 이어지는지 확인한다.
7. P를 길게 눌러도 화면이 반복 전환되지 않는지 확인한다.
8. 이동 중 정지한 뒤 방향키를 놓고 재개하면 이동이 멈추는지 확인한다.
9. 대기 중 5초 이상 일시정지해도 재개 직후 수면으로 전환되지 않는지 확인한다.
10. 일시정지에서 Esc로 타이틀 복귀 및 창 닫기로 종료되는지 확인한다.

11. 공을 발사한 뒤 M을 눌러 공 이동은 계속되고 플레이어 이동·Space 발사는 차단되는지 확인한다.
12. 이동 중 M으로 진입한 뒤 복귀해도 방향키를 새로 누르기 전까지 멈춰 있는지 확인한다.
13. M → 0 → M → Space: 새 공이 생기지 않는지 확인한다. 수면 중 첫 Space는 기존대로 기상만 수행한다.
14. M → 1 → M → Space: 작은 공, M → 2 → M → Space: 큰 공이 표시되는지 확인한다.
15. 공 발사 후 다른 크기로 바꿔도 기존 공의 크기는 유지되는지 확인한다.
16. M을 길게 눌러도 반복 전환되지 않는지 확인한다. M을 놓고 다시 눌러 복귀한다.
17. 아이템 모드에서 5초 기다리면 수면으로 전환되는지 확인한다. P 일시정지는 수면 시간을 멈춰야 한다.
18. 아이템 모드의 Esc, 창 닫기, 타이틀에서 재시작을 확인한다.

## 제출 상태
- 로컬 구현 단계이다. 실행 결과는 사용자 확인이 필요하다.
- 독립 로컬 Git 저장소를 생성했다. Public GitHub 저장소 생성과 원격 업로드는 진행하지 않았다.
- 이전한 8개 커밋 중 2개는 기존 LEC15 이력, 6개는 이번 단계별 개발 기록이다. 문서 정리는 별도 실제 커밋으로 남긴다.
- 과제별 최소 20개 실제 개발 커밋 조건은 미충족이다. 기존 수업 이력을 Drill-13 개발 커밋으로 대체 계산하지 않는다.
- 실행 확인 전에는 제출 준비 완료로 판단하지 않는다.
- e-Class 제출은 미완료이며 과제 안내와 마감 시각은 확인하지 않았다.

## 단계별 변경과 이력 이전
1. 기존 모드 연결·일시정지 작업을 현재 상태의 기준 커밋으로 기록.
2. Ball 생성자에 size=21을 추가하고 크기별 이미지 캐시를 분리. 기존 호출은 작은 공을 유지.
3. Boy.ball_size와 발사 차단 조건 추가. 선택한 크기를 Ball에 전달.
4. item_mode 추가. 진입 시 이동 정지, 숫자 선택, 월드 갱신과 안내 이미지 표시.
5. play_mode에 M 전환 연결. P 정지 시간만 수면 타이머에서 제외.
6. pause_mode에서도 M 키 해제를 반영하여 이후 입력이 막히는 경우 처리.
7. 독립 Drill-13 저장소로 해당 경로의 실제 Git 이력을 이전. 기존 저장소에는 복원 커밋을 기록.
8. 현재 요구사항·사용자 테스트·변경 대조와 제출 상태 문서화.

이전은 git fast-export / fast-import로 수행했다. 저장소 루트로 파일 경로를 변경했으므로 해시는 달라진다.
기존 LEC15 폴더는 아이템 모드 작업 전 기능으로 복원했다. 기존 저장소 이력을 삭제하거나 재작성하지 않았다.

| 단계 | 원본 커밋 | Drill-13 커밋 |
|---|---|---|
| 기존 LEC15 업로드 | d499776 | f4b6cc6 |
| 기존 LEC15 갱신 | 42c7483 | cba7ef3 |
| 1 | f532cc4 | b922460 |
| 2 | e20352f | 74e7853 |
| 3 | 235b105 | a3b5c16 |
| 4 | e8185aa | 30367b5 |
| 5 | a3a9d61 | f05c91b |
| 6 | fa80e46 | 5978dbb |

## 아이템 모드 코드 변경 전후
기준은 이번 아이템 작업 시작 시점이다. 기존 모드 연결 내용은 기준 커밋 b922460에 보존되어 있다.
아래 diff의 `-`는 변경 전, `+`는 변경 후다. 새 파일은 변경 전 없음이다. 모든 변경 블록을 포함한다.

### ball.py

```diff
--- before/ball.py
+++ after/ball.py
@@ -4,11 +4,12 @@
 
 
 class Ball:
-    image = None
+    images = {}
 
-    def __init__(self, x, y, velocity):
-        if Ball.image is None:
-            Ball.image = load_image('ball21x21.png')
+    def __init__(self, x, y, velocity, size=21):
+        if size not in Ball.images:
+            Ball.images[size] = load_image(f'ball{size}x{size}.png')
+        self.image = Ball.images[size]
         self.x, self.y = x, y
         self.velocity = velocity
 
```

### boy.py

```diff
--- before/boy.py
+++ after/boy.py
@@ -18,6 +18,7 @@
         self.dir = 0
         self.face_dir = 1
         self.pressed_keys = set()
+        self.ball_size = 21
 
         self.machine = Machine(
             model=self,
@@ -47,7 +48,9 @@
         self.frame = 0
 
     def fire_ball(self):
-        ball = Ball(self.x, self.y, self.face_dir * 10)
+        if self.ball_size == 0:
+            return
+        ball = Ball(self.x, self.y, self.face_dir * 10, self.ball_size)
         game_world.add_oject(ball)
 
     def handle_event(self, event):
```

### item_mode.py

```diff
--- before/item_mode.py
+++ after/item_mode.py
@@ -0,0 +1,61 @@
+from pico2d import *
+import game_framework
+import game_world
+import play_mode
+import title_mode
+
+image = None
+
+
+def init():
+    global image
+    image = load_image('item_select.png')
+    boy = play_mode.boy
+    boy.pressed_keys.clear()
+    boy.dir = 0
+    if boy.state == 'RUN':
+        boy.stop_run()
+
+
+def finish():
+    global image
+    image = None
+
+
+def handle_events():
+    resume_requested = False
+    for event in get_events():
+        if event.type == SDL_QUIT:
+            game_framework.quit()
+            return
+        elif event.type == SDL_KEYDOWN:
+            if event.key == SDLK_ESCAPE:
+                game_framework.pop_mode()
+                game_framework.change_mode(title_mode)
+                return
+            elif event.key == SDLK_m:
+                if not play_mode.item_key_down:
+                    play_mode.item_key_down = True
+                    resume_requested = True
+            elif event.key in (SDLK_0, SDLK_1, SDLK_2):
+                play_mode.boy.ball_size = {
+                    SDLK_0: 0, SDLK_1: 21, SDLK_2: 41
+                }[event.key]
+        elif event.type == SDL_KEYUP:
+            if event.key == SDLK_m:
+                play_mode.item_key_down = False
+            elif event.key == SDLK_p:
+                play_mode.pause_key_down = False
+    if resume_requested:
+        game_framework.pop_mode()
+
+
+def update():
+    play_mode.update()
+
+
+def render():
+    clear_canvas()
+    game_world.render()
+    image.draw(400, 300)
+    update_canvas()
```

### play_mode.py

```diff
--- before/play_mode.py
+++ after/play_mode.py
@@ -3,17 +3,20 @@
 import game_world
 import title_mode
 import pause_mode
+import item_mode
 from boy import Boy
 from grass import Grass
 
 boy = None
 pause_started_at = 0.0
 pause_key_down = False
+item_key_down = False
+world_paused = False
 
 
 def handle_events():
-    global pause_key_down
-    pause_requested = False
+    global pause_key_down, item_key_down, world_paused
+    requested_mode = None
     for event in get_events():
         if event.type == SDL_QUIT:
             game_framework.quit()
@@ -24,18 +27,27 @@
         elif event.type == SDL_KEYDOWN and event.key == SDLK_p:
             if not pause_key_down:
                 pause_key_down = True
-                pause_requested = True
+                requested_mode = pause_mode
         elif event.type == SDL_KEYUP and event.key == SDLK_p:
             pause_key_down = False
+        elif event.type == SDL_KEYDOWN and event.key == SDLK_m:
+            if not item_key_down:
+                item_key_down = True
+                requested_mode = item_mode
+        elif event.type == SDL_KEYUP and event.key == SDLK_m:
+            item_key_down = False
         else:
             boy.handle_event(event)
-    if pause_requested:
-        game_framework.push_mode(pause_mode)
+    if requested_mode is not None:
+        world_paused = requested_mode is pause_mode
+        game_framework.push_mode(requested_mode)
 
 
 def init():
-    global boy, pause_key_down
+    global boy, pause_key_down, item_key_down, world_paused
     pause_key_down = False
+    item_key_down = False
+    world_paused = False
     boy = Boy()
     game_world.add_oject(boy)
     game_world.add_oject(Grass(), 0)
@@ -49,12 +61,14 @@
 
 def pause():
     global pause_started_at
-    pause_started_at = get_time()
+    if world_paused:
+        pause_started_at = get_time()
 
 
 def resume():
     # 일시정지 시간은 수면 대기 시간에서 제외한다.
-    boy.idle_started_at += get_time() - pause_started_at
+    if world_paused:
+        boy.idle_started_at += get_time() - pause_started_at
     direction = (int(SDLK_RIGHT in boy.pressed_keys) -
                  int(SDLK_LEFT in boy.pressed_keys))
     if boy.state == 'RUN':
```

### pause_mode.py

```diff
--- before/pause_mode.py
+++ after/pause_mode.py
@@ -30,6 +30,8 @@
         elif event.type == SDL_KEYUP:
             if event.key == SDLK_p:
                 play_mode.pause_key_down = False
+            elif event.key == SDLK_m:
+                play_mode.item_key_down = False
             elif event.key in (SDLK_LEFT, SDLK_RIGHT):
                 play_mode.boy.pressed_keys.discard(event.key)
     if resume_requested:
```

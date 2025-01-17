아래의 문제를 해결하기 위해서 참고할 자료는 WWDC 세션에서만 찾습니다.   
그 외에는 [LLDB 공식 문서](https://lldb.llvm.org/)까지만 허용합니다.  
다른 곳에서 자료를 찾는 것은 반칙입니다. 여러분의 양심에 맡깁니다.

### 참고할 WWDC 세션 힌트

* Debugging Tips and Tricks
* Debugging with Xcode 9
* Visual Debugging with Xcode
* LLDB: Beyond "po"



### 문제

- ViewController.swift 파일의 23번째 줄에 브레이크 포인트를 설정하려면 입력해야 하는 LLDB 명령어는? 
``` 
breakpoint set --line 23 
```
- `changeTextColor`라는 심볼에 브레이크 포인트를 설정하기 위해 입력해야 하는 LLDB 명령어는? 
``` 
breakpoint set --name changeTextColor 
```
- Breakpoint Navigator를 통해 `titleLabel`의 `text`가 `"두 번째 뷰 컨트롤러!"`인 경우에만 작동을 일시정지하고 `titleLabel`의 `text`를 출력하는 액션을 실행하도록 설정해보세요
```
```
- 오류(Error) 혹은 익셉션(Exception)이 발생한 경우 프로세스의 동작을 멈추도록 하는 방법에 대해 알아봅시다
```
```
- View Controller의 뷰 위에는 사용자 눈에 보이지 않는 뷰가 있습니다. 이 뷰의 오토레이아웃 제약을 확인해서 알려주세요
```
(lldb) expression -l objc -O -- [`self.view` recursiveDescription]
<UIView: 0x7ff6eb0104e0; frame = (0 0; 320 568); autoresize = W+H; layer = <CALayer: 0x600002c01940>>
   |👉 <UIView: 0x7ff6eb010650; frame = (103.5 482.5; 207 207); autoresize = RM+BM; tag = 100; layer = <CALayer: 0x600002c01960>> 👈
   | <UILabel: 0x7ff6eb00fc60; frame = (87 133; 240 21); text = 'LLDB 세상에 오신 것을 환영합니다!'; opaque = NO; autoresize = RM+BM; userInteractionEnabled = NO; layer = <_UILabelLayer: 0x600000f777f0>>
   | <UIButton: 0x7ff6eb010b90; frame = (187.5 170; 39 30); opaque = NO; autoresize = RM+BM; layer = <CALayer: 0x600002c01260>>
```
- 디버그 모드로 실행중인 상태에서 사용자 눈에 보이지 않는 뷰의 색상을 분홍색으로 변겅해보세요
```
(lldb) po unsafeBitCast(0x7ff6eb010650, to: UIView.self)
<UIView: 0x7ff6eb010650; frame = (103.5 482.5; 207 207); autoresize = RM+BM; tag = 100; layer = <CALayer: 0x600002c01960>>

(lldb) po unsafeBitCast(0x7ff6eb010650, to: UIView.self).backgroundColor
▿ Optional<UIColor>
  - some : <UIDynamicSystemColor: 0x600003908f00; name = systemBackgroundColor>

(lldb) po unsafeBitCast(0x7ff6eb010650, to: UIView.self).backgroundColor = .systemPink
0 elements

(lldb) po unsafeBitCast(0x7ff6eb010650, to: UIView.self).backgroundColor
▿ Optional<UIColor>
  - some : <UIDynamicModifiedColor: 0x60000220b690; contrast = normal, baseColor = <UIDynamicSystemColor: 0x60000390c280; name = systemPinkColor>>
```
<img width="506" alt="스크린샷 2022-01-18 오후 3 32 11" src="https://user-images.githubusercontent.com/47771646/149883175-9a1f8c21-b527-4785-a4f0-320305d16aba.png">

- 두 번째 뷰 컨트롤러의 뷰가 화면에 표시된 상태에서, 두 번째 뷰 컨트롤러 까지의 메모리 그래프를 캡쳐해보세요
```
```
- LLDB의 특정 명령어의 별칭을 설정해줄 수 있는 명령어는 무엇일까요?
```
command alias poc expression -l objc -O --
```
- LLDB의 `v`, `po`, `p` 명령어의 차이에 대해 알아봅시다
```
v
po <expr> = expression --object-description -- <expr> : debug description
p <expr> = expression -- <expr> : outputs LLDB-formmatted description
```

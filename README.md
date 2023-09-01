### Tmux
#### Session 확인
```
tmux ls
```

#### 세션 생성
	tmux
	tmux new -s session_name

#### 세션 종료
	exit
    #or
	tmux kill-session -t session_name

#### Terminal List 보기
	tmux ls
#### Terminal 들어가기
	tmux attach -t 0
	tmux attach -t 1
	tmux attach -t 2

#### 나오기
	ctl+b,d

#### 명령 전달
tmux send-keys  -t session_name:0.0 'whomai' Enter

#### Window
* 윈도우 생성
    ctl+b , c
* 윈도우 이름 변경
    ctl+b , ,(진짜 컴마)
* 다음 윈도우
    ctl+b , n
* 이전 윈도우
    ctl+b , p
* 모든 윈도우 리스트 보기
    ctl+b , w

#### Pane
* 세로로 윈도우 분할
    ctl+b , %
* 가로로 윈도우 분할
    ctl+b, "
* 팬 이동
    ctl+b, q + 숫자
    ctl+b, q + 방향키
* 줌 :  특정 팬을 전체화면으로 전환, 한번 더 누르면 원상태 복구
    ctl+b, z
* 기타
    C+b,alt+화살표 : 해당 방향으로 pane 크기변경
    C+b,q : 해당 숫자 pane으로 이동
    C+b,o : 순서대로 이동
    C+b,z : 현재 pane 최대화 토글
    C+b,space : pane배치 변경
    C+b,x : eXit pane (Pane 강제 종료)
#### 기타
* C+b,? : list-keys
* Scroll Mode 진입 : C+b,[     
    - 아래화살표·위화살표,PageUp,PageDown키로 화면 이동
    - esc키로 화Scroll Mode 끝내기

#### alias example
```
alias t='tmux '
alias ta='tmux attach -t '
alias tl='tmux ls'
alias tn='tmux new -s '
```

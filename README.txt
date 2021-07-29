# androidDeviceConvertToNoteBook

androidDeviceConvertToNoteBook - Setting up the development environment on your Android device


############################################################

  Setting up the development environment on your Android device

############################################################

1.Install Termux (google player)

2.Setting Termux
  2-1. 안드로이드 앱 설정 - termux 앱 권한 저장공간 허용 
        https://wiki.termux.com/wiki/Termux-setup-storage
        Android 11
	You may get "Permission denied" error when trying to access shared storage, even though the permission has been granted.
	Workaround:
	Go to Android Settings --> Applications --> Termux --> Permissions
	Revoke Storage permission
	Grant Storage permission again
		
  2-2. $ termux-setup-storage
  
  2-3. $ pkg update && pkg upgrade
  
    위 명령어 실행 시  아래와 같은 에러 문구 출력되며 설치 되지 않는 경우 (android+7)
    https://github.com/termux/termux-packages/issues/6726 내용 참고 하여 termux-change-repo 명령어로 repo 설정 
    하거나 아래 명령어 실행하여 문제되는 repo 제거 - repo 설정 변경 추천 
    pkg remove game-repo
    pkg remove science-repo
    pkg update 
    
    Err:5 https://dl.bintray.com/grimler/game-packages-24 games Release
	  403  Forbidden
    Err:6 https://dl.bintray.com/grimler/science-packages-24 science Release
	  403  Forbidden
    Reading package lists... Done
    
    참고 :pkg 명령어 사용법 -> https://admion.net/port-and-pkg/
    
  2-4. $ termux-change-repo  
  
  2-5. $ pkg install vim
  
  2-6. $ pkg install openssh
  
  2-7. 기타 필요한 프로그램 설치 - 우분투를 설치 하여 사용할 것이기 때문에 위 정 설정만 해주면 됨
  
  2-8. Termux에서 Backspace 키를 누를때 진동 없애기 
       .inputrc 파일 생성하고 set bell-style none 구문을 입력 하고 저장 재 실행 후 테스트
       vi ~/.inputrc
       set bell-style none
      	
3.Install Andronix (google player) 

4.Install bVNC: Secure VNC Viewer (google player)

5.Install ubuntu
  5-1. Termux execute
  5-2. $ mkdir ubuntu  
  5-3. $ cd ubuntu
  5-4. $ pkg update -y && pkg install curl proot tar -y && curl https://raw.githubusercontent.com/AndronixApp/AndronixOrigin/master/Installer/Ubuntu20/ubuntu20-xfce.sh | bash
    (Andronix execute -> Ubuntu -> Install -> Ubuntu 20.04 -> XFCE : command copy)
        
6.Install Openbox 
  6-1. Termux execute
  6-2. $ cd ubuntu
  6-3. $ pkg update -y && pkg install curl proot tar -y && curl https://raw.githubusercontent.com/AndronixApp/AndronixOrigin/master/Installer/Ubuntu20/ubuntu20-openbox.sh | bash
    (Andronix execute -> Ubuntu -> Install -> Ubuntu 20.04 -> Window Manager -> Openbox : command copy)
  6-4. $ ./start-ubuntu20.sh
  6-5. # openbox.sh (chmod 755)
  6-7. # ./openbox.sh
   	cf) 설치 중 오류 발생 시 아래의 dns 정보 수정
	    - dns setting
	    - vi /etc/resolv.conf
	    nameserver 1.1.1.1
	    nameserver 8.8.8.8
	    nameserver 8.8.4.4
  - 설치 종료 시점 vncserver 패스워드 설정
  
7. ubuntu vi 설치 및 .vnc 접속 환경 설정 
   7-1. $ cd ubuntu
   7-2. $ ./start-ubuntu20.sh
   7-3. # apt update
   7.4. # apt install vim
   7-5. $ vi vnc.sh 파일 생성하여 아래 명령어 넣어 실행 권한 부여
          vncserver-stop && vncserver-start
          chmod 755 vnc.sh
   7-6. $ ./vnc.sh (vnc 서비스 시작)   
   	위와 같이 수동 실행 하거나 
   	vi ~/.profile 파일 하단에 아래 구문 추가 하여 자동 실행 처리 
   	vncserver-stop & vncserver-start 
   7-7. vnc 접속 시 cairo-dock 자동 실행 오류 해결법
	vnc 접속 시 접속은 되나 검정 바탕화면에 cairo-dock 자동실행이 안되는 경우 
	
	vi ~/.profile 파일 맨 마직막 부분에 아래 두 구문 추가
	/root/.vnc/xstartup 파일 내용에서 dbus-launch cairo-dock 구문 #으로 주석 처리
	
	nohup cairo-dock 1>/dev/null 2>&1 &
	/root/.vnc/xstartup
	
   7-8. vncserver 해상도 설정 (접속 환경에 맞는 해상도로 조정)
   	vi .profile 하단에 아래 구문 추가 
	$ xrandr -s 1920x1080
	
	=======================================
		
   	ex) .profile 파일 최종 수정 내용 (맨 하단에 아래 4줄 추가 처리)        
	vncserver-stop && vncserver-start
	nohup cairo-dock 1>/dev/null 2>&1 &
	/root/.vnc/xstartup
	xrandr -s 1920x1080
	
	ex) .vnc/xstartup 파일 최종 수정 내용 
	root@localhost:~/.vnc# more xstartup
	#!/bin/bash
	[ -r ~/.Xresources ] && xrdb ~/.Xresources
	export PULSE_SERVER=127.0.0.1
	export DISPLAY=:1
	XAUTHORITY=~/.Xauthority
	export XAUTHORITY
	dbus-launch openbox
	#dbus-launch cairo-dock  (주석처리)
	feh --bg-fill /usr/share/wallpaper.jpg
	
8.connect bVNC
   localhost  5901
   user_id  : root 계정 (우분투 계정)
   user_pw  : openbox 설치 중 설정 한 vncserver 패스워드 값
   
   * 접속 후 cairo-dock 테마 변경 및 메뉴 구성 취향에 맞게 설정 
    
9. Ubuntu 기본 환경 설정 및 개발 프로그램 설치 (arm 버젼으로 설치해야 함)
   9-0. .bashrc 파일에 아래 내용 추가 
        $ vi ~/.bashrc
	alias cls='clear'
	alias python='python3'
	alias pip=pip3
	TZ='Asia/Seoul'; export TZ
	
	(선택사항)
	우분투 20에서 zsh 설정하고 oh my zsh 설치하기
	https://jkim83.tistory.com/136
	
	sudo passwd 명령어로 패스워드 우선 설정 
	$ sudo apt install zsh -y && chsh -s `which zsh`	
	chsh:PAM authentication failed 오류 발생시 해결방법
	$ sudo vim /etc/pam.d/chsh
	auth required pam_shells.so 주석처리
	sudo chsh $USER -s $(which zsh)
	로그아웃, 로그인
	oh-my-zsh 설치
	$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
	
	agnoster 테마변경
	테마를 robbyrussell -> agnoster로 수정
	$ sudo vim ~/.zshrc
	 as-is
	 ZSH_THEME="robbyrussell" 
	 to-be
	 ZSH_THEME="agnoster"

	 터미널 글자가 깨지는 경우는  powserline 설치 
	 $ git clone https://github.com/powerline/fonts.git
	$ cd fonts
	$ ./install.sh
	
	zsh 사용 할 경우 아래 내용 추가 처리 
	$ sudo sudo vim ~/.zshrc
	alias cls='clear'
	alias python='python3'
	alias pip=pip3
	TZ='Asia/Seoul'; export TZ
	
   
   9-1. 한글 지원 폰트 설치 - 글꼴 설치 한다고 한글입력 처리등이 되지는 않음 (한글 적용 시도 했지만 적용이 안됨 - 문제점)
        * 나눔글꼴 설치
        sudo apt-get install fonts-nanum   (해당 내용 설치 하면 chromium 한글깨짐 해결됨)
	
	* 네이버 코딩 글꼴 설치
	apt-get install unzip
	wget https://github.com/naver/d2codingfont/releases/download/VER1.3.2/D2Coding-Ver1.3.2-20180524.zip
	unzip D2Coding-Ver1.3.2-20180524.zip
	mkdir /usr/share/fonts/truetype/D2Coding
	cp ./D2Coding/*.ttf /usr/share/fonts/truetype/D2Coding/
	fc-cache -v
	
	* OpenBox Configuration Manager 창 실행 후 
	Appearance 탭 선택 후 글꼴 모두 D2Coding 으로 설정 	
	
	
	* 한글 설정 (아쉽게도 100%로 지원은 안되는 듯 함)
	Application - Settings - ISus Preferences 실행	
	General 탭에서 
	Keyboard Shortcuts 에서 <Ctrl><Shift>space 설정 
	Use custom fonts 선택 하고 글꼴을 D2Coding Regular 선택
		혹시 해당 폰트 설정 후 안된다면 아래의 명령어로 폰트 설치 후 
		$ sudo apt install fonts-nanum fonts-nanum-extra fonts-nanum-coding ibus-hangul
		NanumBarunGothic Regular 폰트로 설정 합니다
		
	Input Method 탭을 누르고 Add 버튼을 누르면 언어 리스트가 뜨는데 여기서 Korean 을 선택하고 다시
	Hangul을 선택 한 후 Add 버튼을 누르면 Input Method 에 Korean - Hangul 이 추가 됩니다.
	기존에 있던 English - English (US) 는 삭제 합니다.
	
	terminal 에서 아래 명령어 실행 
	$ ibus-setup-hangul
	Hangul toggle key 항목에 Add버튼을 누른 후 Control + Alt + space 키를 눌러 한영 전환키를 추가 합니다.
	(중요 - 실제 전환은 Control+Alt+spce 로 동작이됨 위에서 설정한 Keyboard Shortcuts로 안됨 동일한 값으로 설정하면 동작 안함 ㅠㅠ)
	
	IBus daemon 자동 실행 설정
	~/.vnc/xstartup 파일에 아래 내용을 추가 해 줍니다.
	ibus-daemon -drx
	
	terminal 글꼴도 D2Coding Regular로 설정 
	
	(위와 같이 설정 시 한글 설정된 firefox , terminal 등에서 정상 동작함)
	chromium ,vscode ,intellj 등 에서 동작 안함 100% 되지는 않는듯 함 
	우선 일부라도 된다는 거에 만족 혹시 완벽한 방법 찾으면 내용 수정 하겠음
			
   9-2. * apt install <program> 설치
        apt install ssh
	
        apt install firefox
	apt-get install libreoffice
	apt install openjdk-11-jdk
	sudo apt install python3-pip  (python3는 기본 설치 되어 있으나 pip3는 설치 안되어 있음으로 설치)
	apt install nodejs
	apt install npm
	apt install git
	
	git 사용자 등록 및 인증정보 자동 저장
	$ git config --global user.name "devsunset"
	$ git config --global user.email devsunset@gmail.com
	$ git config --global credential.helper store
	$ git config --list
		
   9-3. * 다운로드 설치
  	- vscode 
	https://code.visualstudio.com/
	Other platform > ARM64 버젼 deb 파일 다운로드 후 dpkg 명령어로 설치 
	ex) dpkg -i code_1.58.1-1626157156_arm64.deb (해당 버젼 실행 문제 - --no-sandbox 관련 에러 발생 )
		
	최신버젼 설치시 실행 실행이 안되는 문제 발생 되는 경우 아래 링크로 이전 버젼 다운 받아 설치 진행 
	https://update.code.visualstudio.com/{version}/linux-deb-arm64/stable
	ex)https://update.code.visualstudio.com/1.50.1/linux-deb-arm64/stable
	
	설치 도중 libxss1 관련 에러 발생시 아래 명령어로 관련 모듈 설치 후 재 진행
	apt install libxss1 
	
	- intellij
	download (linux tar.gz)
	https://www.jetbrains.com/ko-kr/idea/download/other.html 
	설치 폴더 생성
	mkdir -p /workspace/program
	다운로드 파일 /workspace/program 으로 이동 후 
	tar -zxvf 설치파일   명령어로 압축 해제 후
	해제 된 폴더/bin 으로 이동하여 idea.sh 실행
	
	- go
	arm 버젼 다운로드 다운르도
	https://golang.org/dl/
	설치방법		
	https://golang.org/doc/install
	rm -rf /usr/local/go && tar -C /usr/local -xzf 다운받은파일명
		You can do this by adding the following line to your $HOME/.profile or /etc/profile (for a system-wide installation):
		/etc/profile 파일에 설정 하도록 함
	export PATH=$PATH:/usr/local/go/bin
	go version	
	
	-dbeaver
	https://dbeaver.io/download/ arm 버젼 다운로드
	/workspace/program 경로에 
	tar -zxvf 다운로드파일명 명령어로 압축 해제
	/workspace/program/dbeaver/dbeaver  실행

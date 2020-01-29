---
layout: post
title: Ubuntu 초기 세팅
category: Linux
tag: [Linux]
---

<br>

## Ubuntu 설치 후 초기 세팅을 정리한 글입니다.

<br>

# 소프트웨어 다운로드 설정

소프트웨어 다운르도를 최신으로 업데이트 해준다.
<pre class="prettyprint">
sudo apt-get update
sudo apt-get upgrade
</pre>

<br>
정상적으로 안 될 때에는 /etc/apt/source.list 파일 내 ubuntu 서버를 국내 서버로 바꿔준다.(kakao 등..)
<pre class="prettyprint">
sudo gedit /etc/apt/source.list
</pre>
<br>
<br>

# 한글 키보드 설정

### fcitx-hangul 설치
<pre class="prettyprint">
sudo apt-get install fcitx-hangul
</pre>
<br>

### 한글팩 설치
>1. Settings - Region & Language - Manage Installed Languages 클릭
>2. 언어팩 설치 팝업 뜨면 install 클릭
>3. 하단의 ‘Keyboard input method system'에서 fcitx 선택
>4. 재부팅
<br>

<br>

### fcitx 설정
>1. 전체 앱에서 fcitx-configure 실행
>2. 하단의 +버튼을 클릭 후 Only Show Current Language 체크 해제
>3. Hangul 항목 추가
>4. Global Config 탭에서 Trigger Input Method 항목을 한/영 키로 설정(Ralt)
>5. Extra key for trigger input method는 체크 해제

<br>

# 네트워크 설정 (고정 IP 사용시)

>1. Settings - Network - Wired 톱니바퀴 클릭
>2. IPv4에서 Automatic을 Manual로 변경
>3. 하단에 IP주소, 넷마스크, 게이트웨이, DNS(,로 여러개 추가 가능) 작성
>4. identity 탭에서 이름 설정 가능

<br>

# Git SSH 설정

<pre class="prettyprint">
git config --global user.email "email@email.com"
cd ~/.ssh
ssh-keygen -t rsa -b 4096 -C "email@email.com"
cat id_rsa.pub
</pre>

출력된 암호키를 전부 복사한다.
github 계정 settings - SSH and GPG keys에 등록 

<br>

# terminator, zsh 설정

우선 https://github.com/naver/d2codingfont 에서 최신버전 D2Coding 폰트를 받는다.
(압축해제 후 D2Coding 폴더 내 TTF 더블클릭, install)
<br>
<br>

### terminator와 zsh 설치
<pre class="prettyprint">
sudo apt-get install terminator
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
</pre>
<br>

### zsh 플러그인 설치 
<pre class="prettyprint">
cd ${ZSH_CUSTOM1:-$ZSH/custom}/plugins
git clone https://github.com/djui/alias-tips.git
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
</pre>
<br>

### .zshrc 설정
<details>
  <summary>.zshrc</summary>

{%raw%}
# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH="/home/down/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/robbyrussell/oh-my-zsh/wiki/Themes
ZSH_THEME="agnoster"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in ~/.oh-my-zsh/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment the following line to disable bi-weekly auto-update checks.
# DISABLE_AUTO_UPDATE="true"

# Uncomment the following line to automatically update without prompting.
# DISABLE_UPDATE_PROMPT="true"

# Uncomment the following line to change how often to auto-update (in days).
# export UPDATE_ZSH_DAYS=13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS=true

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in ~/.oh-my-zsh/plugins/*
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(
 git
 alias-tips
 zsh-autosuggestions
 zsh-syntax-highlighting
)

source $ZSH/oh-my-zsh.sh

# User configuration

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# ssh
# export SSH_KEY_PATH="~/.ssh/rsa_id"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.

# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"

prompt_context() { 
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then 
    prompt_segment black default "%(!.%{%F{yellow}%}.)$USER" 
  fi 
}

# for TERM setting
export TERM=xterm

# for ZSH AutoSuggestion
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=cyan'

# for Java
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# for Android
ANDROID_TOOLS=/home/down/Android/Sdk/platform-tools
export PATH=$PATH:$ANDROID_TOOLS
{%endraw%}

</details>
<br>

### terminator 설정

>1. terminator 우클릭 - preferences - global - Hide size from title 체크
>2. Profiles - general - User the system fixed width font 체크 해제, D2Coding Regular로 변경 / Show titlebar 체크 해제
>3. colors탭에서 Use colors from system theme 해제 / Built in schemes - White on black / Palette - Built in schemes - Solarized
>4. Background탭에서 Transparent background 체크 / 0.85로 변경
>5. Scrolling탭에서 infinite scrollback 체크 
>6. 창 크기 조절은 ~/.config/termintor/config파일 수정 ([[[window0]]] 하단에 size = 740, 540 등으로 추가)

<br>

# Top bar 숨기기

>1. ubuntu software에서 Hide Top Bar 다운로드
>2. Settings - extensions - hide top bar 설정에서 only hide panel when a window takes the space 체크 해제
>3. only when the active window takes the space 체크

<br>

# Settings 설정

>1. Dock - auto-hide the dock 체크 해제 / icon size - 28 / position - bottom
>2. Power - Blank screen - never / Automatic suspend - when on battery power

<br>

# tweaks, dconf editor 설정

ubuntu software에서 tweaks, dconf editor 다운로드
<br>

### tweaks 설정 

>1. Desktop - Show Icons, Home, trash 체크 / Network Servers 체크 해제 (19.04는 extensions에 있는 듯)
>2. Fonts - Interface - Ubuntu Regular / Document - Sans Regular / Monospace - Ubuntu Mono Regular / Legacy Window Titles - Ubuntu Bold
>3. Fonts - Scaling Factor로 확대 및 축소 가능(top bar에 access 아이콘 뜨면 software에서 Remove Accessibility로 제거 가능)
>4. Top bar - battery percentage, weekday, date 체크, seconds 체크 해제

<br>

### dconf editor 설정

>1. /org/gnome/shell/extensions/dash-to-dock
>2. background-opacity : 0.7
>3. dock-position : BOTTOM / show-apps-at-top : 체크

<br>

# 18.04에서 19.04 테마 쓰기

>1. communitheme 설치 <pre class="prettyprint">
snap install communitheme
</pre>
>2. 재부팅 후 로그인 창에서 기어 아이콘 클릭 - "Ubuntu with communitheme snap" 클릭
>3. tweaks - appearance에서 icons, applications를 communitheme으로 변경 / cursor는 DMZ-white

<br>

# chrome 설치

ubuntu software에서 Google Chrome 설치

<br>

# Jdk 설치

>1. 설치 (jre도 같이 설치됨)<pre class="prettyprint">
sudo apt-get install openjdk-11-jdk
java --version (체크용)
</pre>
>2. 다음을 .zshrc에 추가 (path 설정)
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
(버전에 맞게 수정 필요)

<br>

# 안드로이드 스튜디오 설치

>1. https://developer.android.com/studio/install?hl=ko 참조하여 설치
>2. setting - sdk manager에서 sdk 설치
>3. material theme plugin 설치 및 기본 세팅 파일 불러오기
>4. Tools - Create desktop entry로 바로가기 생성


<br>

# pip 설치

python2, 3은 기본으로 설치되어 있음.
<pre class="prettyprint">
sudo apt-get install python3-pip
pip3 --version (체크용)
</pre>

<br>

# Mattermost 설치

<pre class="prettyprint">
sudo snap install mattermost-desktop --beta
</pre>

설치 후 서버 이름 및 url 설정

<br>

# VSCode, Filezilla, Postman 설치

ubuntu software에서 모두 설치 가능

<br>

# Barrier 설치

https://github.com/debauchee/barrier/wiki/Building-on-Linux 참고해서 설치
(git timeout 에러시 git 사이트에서 직접 주소 복사하여 clone)

<br>

# IntelliJ IDEA, PyCharm, CLion설치

IntelliJ IDEA, PyCharm은 snap으로 간단히 설치 가능하다.

<pre class="prettyprint">
sudo snap install intellij-idea-community --classic
sudo snap install pycharm-community --classic
</pre>

CLion은 https://www.jetbrains.com/ko-kr/clion/download/#section=linux 에서 다운로드 받을 수 있지만 30일 평가판이다.

<br>

# ROS2 설치

ROS2 dashing 버전은 https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Install-Debians/#id7 을 참고해서 설치한다.(18.04)

Setup Locale과 Install Additional*은 생략 가능

<br>

---
참고한 사이트 및 서적 : https://snowdeer.github.io/
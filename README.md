# dotfiles (otumarastudio)

`kkami` 맥(M-series)의 셸/터미널/멀티플렉서 환경 설정.
**bare git repo** 방식으로 `$HOME` 에서 직접 관리. 새 맥에서도 한 번에 복원 가능.

---

## 왜 만들었나

- 새 맥 셋업하면서 환경 portable하게 만들고 싶었음
- 터미널 이리저리 옮겨다닐 때마다 alias/테마 다시 깔기 귀찮음
- "기반은 portable하게, 편의성은 선택적으로" 원칙
- AI 보조 도구(Warp 등)는 보조로만 쓰고, 메인은 어디서나 동일한 zsh 환경

## 결론 (선택한 스택)

| 레이어        | 선택                                  | 이유                                       |
|---------------|---------------------------------------|--------------------------------------------|
| Terminal      | iTerm2                                | macOS 표준, 어디서나 안정적, 폰트/색상 자유 |
| Shell         | zsh (macOS 기본)                      | 그대로                                     |
| Framework     | oh-my-zsh                             | 플러그인/테마 생태계 표준                  |
| Prompt        | powerlevel10k (Classic)               | 빠르고 풍부한 정보, 폰트 의존 작음         |
| Plugins       | git (만)                              | 최소주의, 필요할 때 추가                   |
| Multiplexer   | tmux                                  | SSH/장시간 작업/세션 분할                  |
| Font          | MesloLGS Nerd Font                    | p10k 추천, 아이콘 풀지원                   |
| Dotfile mgmt  | bare git repo (`~/.dotfiles`)         | 심볼릭링크 없음, 도구 무의존, 표준 관행    |

서브: Warp (AI 필요할 때만)

---

## 추적되는 파일

```
~/.zshrc        # oh-my-zsh + p10k 테마 + dotfiles alias + PATH
~/.p10k.zsh     # powerlevel10k 설정 (Classic 스타일, p10k configure 결과)
~/.tmux.conf    # mouse on / true color / vi copy mode / 1-indexed windows
~/README.md     # 이 파일
```

---

## 새 맥에서 복원하는 법

```bash
# 1. 사전 도구
xcode-select --install                       # git
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install gh tmux
brew install --cask iterm2 font-meslo-lg-nerd-font
gh auth login                                # https + browser

# 2. oh-my-zsh + powerlevel10k
git clone --depth=1 https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
  ~/.oh-my-zsh/custom/themes/powerlevel10k

# 3. dotfiles bare repo 클론 (기존 ~/.zshrc 등은 미리 백업할 것)
git clone --bare https://github.com/otumarastudio/dotfiles.git $HOME/.dotfiles
alias dotfiles='git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
dotfiles checkout                            # 충돌 시 mv 로 백업 후 재시도
dotfiles config --local status.showUntrackedFiles no

# 4. iTerm2: Settings → Profiles → Text → Font → MesloLGS NF
# 5. 새 셸 열면 끝
```

---

## 자주 쓰는 명령어

```bash
# dotfiles 관리
dotfiles status
dotfiles add ~/.zshrc
dotfiles commit -m "..."
dotfiles push

# tmux
tmux                       # 새 세션
tmux ls                    # 세션 목록
tmux attach -t 0           # 붙기
# 세션 안에서 prefix = Ctrl+b
#   c     새 창
#   "     가로 분할 / %  세로 분할
#   d     detach (백그라운드 유지)
#   화살표 pane 이동
#   r     conf reload (우리가 추가한 binding)

# powerlevel10k 재설정
p10k configure
```

---

## 컴퓨터 이름 메모

- 이전: `HEEUNGS-PRO의 MacBook Pro`
- 현재: `kkami`
- 시스템 단(`scutil`/`/etc/hosts`/preferences) 모두 정리됨
- iCloud 디바이스 표시 이름은 별개로 갱신됨 (재부팅 또는 iCloud 재로그인 시)

---

## 작업 이력

- 컴퓨터 이름 변경 후 잔재 정리 (시스템 클린 확인)
- dotfiles bare repo 셋업 + GitHub 연결 (`otumarastudio/dotfiles`)
- iTerm2 + Meslo Nerd Font 설치
- oh-my-zsh + powerlevel10k (Classic) 설치 및 설정
- tmux 설치 + minimal config
- iTerm2 색상 테마 후보 다운로드 → `~/Downloads/iterm-themes/`

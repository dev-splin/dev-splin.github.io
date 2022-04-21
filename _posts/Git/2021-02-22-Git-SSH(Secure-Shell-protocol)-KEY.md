---
title: "Git : SSH(Secure Shell protocol) Key"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git : SSH Key"
  - "Git : SSH Key Tip"
toc: true
toc_sticky: true
toc_label: 목차
published: false
---

## SSH(Secure Shell protocol) Key

- push를 할 때마다 아이디와 비밀번호를 입력하는 것은 번거로울 수 있기 때문에 간편하고 자동적으로 할 수 있으며 안전한 방법이 SSH Key를 이용하는 것입니다.
- 사용하고 있는 terminal과 server간에 안전하게 ID와 Password를 유지해 주는 방법입니다.

![SSH key](https://user-images.githubusercontent.com/79291114/108801837-204a6980-75da-11eb-9bf1-418864c54967.jpg)

- server에는 public key를 제공하고 나의 컴퓨터에는 private key를 생성해 사용함으로써 push를 할 때 번거롭게 ID와 Password를 입력하지 않아도 됩니다.



### SSH Key Setting 방법

1. github 페이지에서 아바타를 선택해 Settings에 들어갑니다.

   ![SSH key setting1](https://user-images.githubusercontent.com/79291114/108801831-1e80a600-75da-11eb-8566-943d145ab67f.jpg)

2. 왼쪽에 `SSH and GPG keys`에 들어가 `generating SSH keys` 를 눌러 가이드로 갑니다.

   ![SSH key setting2](https://user-images.githubusercontent.com/79291114/108801832-1f193c80-75da-11eb-9837-fb37660eff36.jpg)

3. `Adding a new SSH key to your GitHub account` 를 눌러 들어갑니다.

   ![SSH key setting3](https://user-images.githubusercontent.com/79291114/108801834-1fb1d300-75da-11eb-8339-f9b3d10a4fdd.jpg)

4. 운영체제를 선택한 후 `Generated a new SSH key and added it to the ssh-agnet` 로 들어가 운영체제를 선택한 후 그대로 따라합니다.

   ![SSH key setting4](https://user-images.githubusercontent.com/79291114/108801836-1fb1d300-75da-11eb-818c-09fc6828c572.jpg)

5. SSH key를 생성했으면 `Adding a new SSH key to your GitHub account` 가 있는 페이지로 다시 가서 그대로 따라하시면 됩니다. `Switching remote URLs from HTTPS to SSH` 을 하셔야 SSH를 사용할 수 있습니다.

6. 다 설정을 하면 앞으로는 아이디와 패스워드를 입력하지 않고도 검증된 유저로써 commit을 할 수 있습니다.



### SSH Key Setting 후 오류

```
ssh: connect to host github.com 
port 22: connection timed out 
fatal: could not read from remote repository. 
please make sure you have the correct access rights and the repository exists.
```

- 이런 오류가 발생한다면 [Using SSH over the HTTPS port](https://docs.github.com/en/github/authenticating-to-github/using-ssh-over-the-https-port) GitHub 사이트를 보고 해결할 수 있습니다.
- 오류의 원인은 방화벽이 SSH 연결을 허용하지 않을 때가 있다는 것 때문입니다.

**해결 방법**

1. `ssh -T -p 443 git@ssh.github.com ` 명령어를 통해 HTTPS 포트를 통한 SSH가 가능한지 테스트 합니다.

   ```
   $ ssh -T -p 443 git@ssh.github.com
   > Hi username! You've successfully authenticated, but GitHub does not
   > provide shell access.
   # 이런 message가 나온다면 HTTPS 포트(443)를 통한 SSH가 가능하다는 것입니다.
   ```

2. `~/.ssh/config` 명령어를 통해 편집기를 열고 다음 텍스트를 작성해줍니다.

   ```
   Host github.com
     Hostname ssh.github.com
     Port 443
   # SSH 설정을 재정의하여 해당 서버 및 포트를 통해 GitHub에 대한 연결을 강제로 실행할 수 있습니다.
   ```

3. `ssh -T git@github.com` 다시 한번 더 연결하여 이 기능이 작동하는지 테스트해 봅니다.



### cmder에서 SSH Agent 사용하기

- SSH를 사용하다 보면 서버에 push 나 pull 등등을 할 때마다 SSH 암호를 필요로 합니다다.
- 암호를 매번입력하기 번거롭기 때문에 Terminal시작할 때 SSH Agent를 실행시켜 시작할 때 한 번만 SSH 암호를 입력 받은 후 프로세스가 종료되기 전 까진 push 나 pull 등을 암호 입력없이 사용할 수 있는 아주 간편한 방법입니다.

1. cmder를 설치한 경로로 이동한 후 config 폴더 내에 user_profile.cmd를 텍스트 편집기로 열어줍니다.

2. ```
   :: uncomment this to have the ssh agent load when cmder starts
   :: call "%GIT_INSTALL_ROOT%/cmd/start-ssh-agent.cmd" /k exit  << 여기 이 부분 ::(주석)을 제거해주면 끝입니다.
   ```

3. cmder을 다시 시작하면 처음에 암호를 입력하게 되고 그 이후로는 입력하지 않아도 됩니다.

---

출처 : https://academy.dream-coding.com/

https://docs.github.com/en/github/authenticating-to-github/using-ssh-over-the-https-port

https://docs.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account

https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
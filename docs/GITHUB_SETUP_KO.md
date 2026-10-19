# MLTuno GitHub 저장소 생성 가이드

**GitHub 웹사이트 + Git 방식 · GitHub CLI(`gh`) 설치 불필요**

현재 확인한 `git version 2.28.0`으로 아래 Git 명령을 사용할 수 있습니다.
이 절차에서는 Homebrew나 CLT 재설치를 진행하지 않습니다.
명령은 단계별로 실행하고, 오류가 나오면 다음 단계로 넘어가지 마세요.
이미 완료한 초기화 단계는 반복하지 않습니다.

프로젝트 표기는 **MLTuno**, 저장소 이름은 **mltuno**, 부제는
**Machine Learning–Based Autotuning**으로 사용합니다.

MLTuno는 ytopt를 기반으로 기본 튜닝 인터페이스를 유지하는 연구 프레임워크입니다.
GC_TLA의 Gaussian copula 기반 전이학습 튜닝 방법은 MLTuno의 한 모듈로 들어가며,
현재 신진연구 과제에서는 심층학습 기반 전이학습 방법으로 확장합니다.
프로젝트 전체의 범위는 ML 기반 자동 튜닝으로 유지합니다.

## 1. 초기 구조

ytopt 저장소를 복제하여 개발 이력과 코드를 유지하고, 자신의 GitHub 계정에
별도의 `mltuno` 저장소로 올리는 방식을 권합니다.
GitHub의 Fork 표시가 붙는 방식은 아니지만 원본 커밋 이력은 이어집니다.

| 구성 | 역할 |
| --- | --- |
| `ytopt/` | 기존 Python 패키지와 기본 튜닝 인터페이스 |
| `ytopt-libe/` | ytopt에서 제공하는 libEnsemble 기반 예제 |
| `external/GC_TLA/` | 원본 GC_TLA 저장소를 참조하는 Git submodule |
| `ytopt/mltla/` | 빈 Python 패키지로 생성하며, GC_TLA 연결 코드와 후속 전이학습 방법을 추가할 위치 |
| `docs/upstream/` | 시작 시점의 ytopt README 및 두 프로젝트의 커밋 기록 |
| `README.md` | MLTuno 영문 소개 및 국문 버전 링크 |
| `README_KO.md` | MLTuno 국문 소개 및 영문 버전 링크 |

프로젝트 이름을 바꾸더라도 초기에는 `ytopt/`, 기존 import 경로, 튜닝 명령을 유지합니다.
`setup.py`의 패키지명만 `mltuno`로 바꾸면 버전 파일 경로 등도 영향을 받으므로,
패키지 배포명 변경은 실제 코드 정리 단계에서 진행합니다.

GC_TLA를 submodule로 추가하면 특정 커밋의 원본을 연결해 둘 수 있습니다.
실제로 ytopt 인터페이스를 통해 전이학습 튜닝을 실행하려면 별도의 연결 코드가 필요합니다.
이 가이드는 저장소 초기화와 소개 문서 작성 단계입니다.

## 2. 준비

아래 명령은 macOS/Linux의 Bash 또는 Zsh 기준이며, Git과 GitHub 계정이 필요합니다.
`gh --version`, `gh auth login`, `gh auth setup-git` 단계는 사용하지 않습니다.

```bash
git --version
```

함께 제공된 `README.md`, `README_KO.md`, `MLTuno_GITHUB_SETUP_KO.md`를 다운로드합니다.
아래 예시는 세 파일이 `~/Downloads`에 있다고 가정합니다.
파일이 다른 폴더에 있으면 `MLTUNO_DOCS`만 실제 경로로 수정하세요.
다운로드 과정에서 파일명에 `(1)` 등이 붙었다면 실제 이름을 확인해 맞춰주세요.

```bash
MLTUNO_DOCS="$HOME/Downloads"
ls "$MLTUNO_DOCS/README.md" \
   "$MLTUNO_DOCS/README_KO.md" \
   "$MLTUNO_DOCS/MLTuno_GITHUB_SETUP_KO.md"
```

세 파일이 모두 표시되면 준비가 끝났습니다.

## 3. GitHub 웹사이트에서 빈 저장소 생성

1. [GitHub 새 저장소 만들기](https://github.com/new)에 접속합니다.
2. Owner는 본인 계정, Repository name은 `mltuno`로 지정합니다.
3. Description에는 `Machine Learning-Based Autotuning built on ytopt`를 입력합니다.
4. 공개 범위를 **Public**으로 선택합니다.
5. README 추가는 끄고, `.gitignore`와 License는 **None**으로 둡니다.
6. **Create repository**를 누릅니다.

이미 비어 있는 `mltuno` 저장소를 만들었다면 이 단계는 생략합니다.
README 등을 추가해 커밋이 있는 저장소라면 아래 첫 업로드 명령을 바로 적용하지 말고
기존 이력을 먼저 확인합니다. 강제 push는 사용하지 않습니다.

## 4. ytopt 기반 로컬 저장소 생성

아래 예시는 `~/Projects/mltuno`에 프로젝트를 만듭니다.
기존 `mltuno` 폴더가 있다면 새로 복제하거나 초기화 명령을 반복하지 말고,
해당 폴더에서 `git status`, `git remote -v`, `git submodule status`로 진행 상태를 확인합니다.

### 4-1. ytopt 복제 및 원본 원격 저장소 이름 변경

```bash
mkdir -p "$HOME/Projects"
cd "$HOME/Projects"
git clone --branch main https://github.com/ytopt-team/ytopt.git mltuno
cd mltuno
```

이후 명령은 계속 이 `mltuno` 폴더에서 실행합니다.

```bash
git remote rename origin upstream
git branch --unset-upstream
git remote -v
```

원본 ytopt는 `upstream`으로 보관합니다. 본인 저장소를 연결할 `origin`은 나중에 추가합니다.

### 4-2. 커밋 작성자 확인

```bash
git config user.name
git config user.email
```

둘 다 본인의 정보로 표시되면 다음으로 넘어갑니다.
비어 있거나 다른 정보이면 아래 값을 실제 이름과 GitHub에 등록한 이메일로 바꿔 실행합니다.
이 설정은 현재 저장소에만 적용됩니다.

```bash
git config user.name "YOUR_NAME"
git config user.email "YOUR_EMAIL"
```

### 4-3. 원본 자료 보존 및 GC_TLA 추가

```bash
mkdir -p docs/upstream
cp README.md docs/upstream/ytopt-README.md
git rev-parse HEAD > docs/upstream/ytopt-commit.txt

git submodule add https://github.com/tlranda/GC_TLA.git external/GC_TLA
git -C external/GC_TLA rev-parse HEAD > docs/upstream/gc_tla-commit.txt
```

`external/GC_TLA/`는 원본 GC_TLA를 참조하는 submodule입니다.
MLTuno에서 새로 작성할 연결 코드는 다음의 `ytopt/mltla/`에 둡니다.

### 4-4. mltla 패키지 틀 생성 및 소개 문서 적용

```bash
mkdir -p ytopt/mltla
touch ytopt/mltla/__init__.py

MLTUNO_DOCS="$HOME/Downloads"
cp "$MLTUNO_DOCS/README.md" README.md
cp "$MLTUNO_DOCS/README_KO.md" README_KO.md
cp "$MLTUNO_DOCS/MLTuno_GITHUB_SETUP_KO.md" docs/GITHUB_SETUP_KO.md
```

`ytopt/mltla/__init__.py`는 빈 패키지 틀입니다. 전이학습 튜닝 기능은 아직 구현되지 않습니다.

### 4-5. 변경 내용 확인 및 커밋

```bash
git add README.md README_KO.md docs .gitmodules external/GC_TLA ytopt/mltla/__init__.py
git diff --cached --stat
git status
```

추가한 문서, submodule, `ytopt/mltla/`가 표시되는지 확인한 다음 커밋합니다.

```bash
git commit -m "Initialize MLTuno on ytopt with GC_TLA research module"
```

`docs/upstream/ytopt-README.md`는 원본 기록용입니다.
옮겨진 문서 안의 상대경로 링크는 달라질 수 있으므로,
설치 설명은 [공식 ytopt README](https://github.com/ytopt-team/ytopt)를 참고합니다.

## 5. GitHub 인증 준비

기존에 HTTPS 방식의 GitHub 인증을 설정했다면 6단계로 넘어갑니다.
인증을 처음 설정한다면 GitHub 웹사이트에서 Personal Access Token을 만듭니다.
브라우저 로그인과 터미널의 Git 인증은 별개입니다.

1. [Fine-grained token 만들기](https://github.com/settings/personal-access-tokens/new)를 엽니다.
2. Token name은 `mltuno-git`, Expiration은 사용할 기간에 맞게 정합니다.
3. Resource owner는 본인 계정으로 선택합니다.
4. Repository access에서 **Only select repositories → mltuno**를 선택합니다.
5. Repository permissions에 **Contents: Read and write**를 추가합니다.
6. ytopt에 포함된 `.github/workflows/ci.yml`도 처음 업로드하므로,
   **Workflows: Read and write** 권한도 추가합니다.
7. **Generate token**을 누르고 생성된 토큰을 보관합니다.

토큰은 다음 단계의 `git push`가 Password를 물을 때 입력합니다.
명령어 URL이나 README에 넣지 않습니다.

## 6. 본인 저장소 연결 및 업로드

아래 `YOUR_GITHUB_ID`를 실제 GitHub 아이디로 바꿉니다.
4단계에서 원본 원격 저장소를 `upstream`으로 바꿔 둔 상태를 기준으로 합니다.

```bash
git remote add origin https://github.com/YOUR_GITHUB_ID/mltuno.git
git remote -v
```

다음과 같이 연결되어야 합니다.

| 이름 | 대상 |
| --- | --- |
| `origin` | `https://github.com/YOUR_GITHUB_ID/mltuno.git` |
| `upstream` | `https://github.com/ytopt-team/ytopt.git` |

`origin already exists`가 나오면 `git remote -v`로 주소를 확인합니다.
이미 본인의 `mltuno` 주소이면 `remote add`를 반복하지 않고 아래 push로 진행합니다.
다른 주소이면 먼저 어느 저장소인지 확인합니다.

```bash
git push -u origin main
```

인증을 요청하면 Username에는 GitHub 아이디, Password에는 5단계에서 만든 토큰을 입력합니다.
Password 입력 중 화면에 글자가 보이지 않는 것은 정상입니다.
기존 인증 정보가 사용되면 질문 없이 업로드될 수 있습니다.

완료 후 브라우저에서 `https://github.com/YOUR_GITHUB_ID/mltuno`를 엽니다.
루트의 `README.md`가 저장소 첫 화면에 표시되고, 언어 링크로 국문 버전을 볼 수 있습니다.
이 소개 화면에는 별도의 GitHub Pages 설정이 필요하지 않습니다.

## 7. 다른 컴퓨터에서 받기

```bash
git clone --recurse-submodules https://github.com/YOUR_GITHUB_ID/mltuno.git
cd mltuno
git submodule status
```

이미 일반 `git clone`으로 받았다면 저장소 루트에서 다음을 실행합니다.

```bash
git submodule update --init --recursive
```

GC_TLA 원본까지 필요한 경우 Git clone 방식을 사용합니다.
GitHub의 기본 Download ZIP에는 submodule의 실제 파일이 포함되지 않습니다.

## 8. 개발 방향과 유지 관리

기본 튜닝은 [ytopt 설치·사용법](https://github.com/ytopt-team/ytopt#installation-instructions)을 따릅니다.
초기 Python 패키지 이름은 `ytopt`이며, 이 저장소 생성만으로 `pip install mltuno`가 제공되지는 않습니다.

GC_TLA는 원 논문의 실험 재현용 코드입니다.
원본 `setup.py`에는 실행에 필요한 전체 의존성이 선언되어 있지 않으므로,
단순 설치만으로 최신 ytopt 환경과의 통합이 완료되었다고 가정하지 않습니다.
연결 모듈을 개발하면서 모델·데이터 형식과 의존성 버전을 함께 검증합니다.

초기 개발 범위는 다음 순서가 적절합니다.

1. MLTuno 저장소에서 기존 ytopt 기본 튜닝 예제를 실행하여 기준 동작을 확보합니다.
2. 원본 GC_TLA의 전이학습 실험을 재현합니다.
3. `ytopt/mltla/`에 소스 튜닝 데이터, GC 기반 후보 생성, 타깃 평가를 연결합니다.
4. 같은 인터페이스 아래에 후속 심층학습 기반 전이학습 방법을 추가합니다.

MLTuno에서 새로 작성하는 연결 코드는 상위 저장소에 둡니다.
`external/GC_TLA/` 내부를 직접 수정하면 그 변경은 별도 저장소에 속하므로,
상위 저장소 커밋만으로 수정된 소스가 배포되지 않습니다.
원본 자체의 변경이 필요해지면 해당 저장소의 fork를 만들고 submodule URL을 변경합니다.

ytopt의 변경 사항은 아래처럼 먼저 확인하고 선택적으로 반영합니다.

```bash
git fetch upstream
git log --oneline main..upstream/main
git diff main...upstream/main -- ytopt setup.py
```

통합 시 README나 설정 파일에 충돌이 생길 수 있으므로,
원본 변경 내용을 확인한 뒤 별도 작업 브랜치에서 merge하고 기본 튜닝을 검증합니다.
GC_TLA의 커밋은 상위 저장소에 고정되므로 일반 clone/update에서 자동으로 최신 버전으로 바뀌지 않습니다.

## 9. 라이선스와 연구 출처

ytopt와 GC_TLA의 현재 라이선스는 모두 BSD-2-Clause입니다.
복제한 ytopt의 `LICENSE.md` 및 원본 저작권 표기를 유지하고,
GC_TLA의 라이선스와 저작권 표기도 그대로 보존합니다.
새 코드에도 적용할 라이선스와 기여자 표기는 후속 개발 시 명확하게 관리합니다.
소개에는 ytopt와 GC_TLA의 역할을 설명하고,
원본 저장소 링크, 논문 인용, 라이선스 및 기여자 표기로 출처를 명시합니다.

- [ytopt](https://github.com/ytopt-team/ytopt)
- [ytopt 라이선스](https://github.com/ytopt-team/ytopt/blob/main/LICENSE.md)
- [GC_TLA](https://github.com/tlranda/GC_TLA)
- [GC_TLA 라이선스](https://github.com/tlranda/GC_TLA/blob/master/LICENSE.md)
- Randall et al. (2023), [Transfer-Learning-Based Autotuning Using Gaussian Copula](https://doi.org/10.1145/3577193.3593712), ICS 2023.
- [Git 명령으로 로컬 저장소를 GitHub에 올리기](https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github)
- [GitHub Personal Access Token 안내](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [Git submodule 문서](https://git-scm.com/docs/git-submodule)
- [GitHub 소스 아카이브 안내](https://docs.github.com/en/repositories/working-with-files/using-files/downloading-source-code-archives)

2026-10-19 확인 시점의 원본 HEAD:

- ytopt: `bd0d926d72994b034ca6664ecf67624624ce74fc`
- GC_TLA: `dc3b0290ed69be44b8978313101306703e83d7e5`

위 생성 명령은 실행 시점의 기본 브랜치를 가져오며, 실제 시작 커밋은
`docs/upstream/`의 기록 파일 및 Git 커밋 이력에서 확인할 수 있습니다.

검증 범위: 원본 README·패키지 설정·라이선스와 Git/GitHub 공식 문서를 확인하고,
안내문의 Bash 명령 구문을 점검했습니다. 사용자의 macOS에서 직접 실행한 것은 아닙니다.
이 문서를 준비하면서 사용자 GitHub 저장소를 만들거나 push하지 않았고,
ytopt·GC_TLA의 설치 및 통합 실행도 수행하지 않았습니다.

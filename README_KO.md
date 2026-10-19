<p align="center">
  <img src="docs/assets/mltuno-logo.png" alt="MLTuno" width="480">
</p>

[English](README.md) | **한국어**

**머신러닝 기반 자동 튜닝**

MLTuno는 애플리케이션과 컴퓨팅 시스템의 머신러닝 기반 자동 튜닝을 위한 연구 프레임워크로,
현재 개발 중입니다. [ytopt](https://github.com/ytopt-team/ytopt)를 기반으로 하며,
기존 튜닝 인터페이스를 유지하면서 새로운 학습·탐색 방법을 추가합니다.

## 구성 및 연구 방향

- **ytopt 기반:** ytopt의 문제 정의, 탐색 방법, 평가 절차를 공통 튜닝 인터페이스로 사용합니다.
- **전이학습 모듈:** Gaussian copula 기반 전이학습 자동 튜닝 방법인
  [GC_TLA](https://github.com/tlranda/GC_TLA)(ICS 2023)를 MLTuno의 한 모듈로 통합합니다.
- **진행 중인 연구:** ML 기반 자동 튜닝이라는 범위 안에서,
  HPC 애플리케이션을 위한 심층학습 기반 전이학습 방법으로 확장합니다.

## 개발 현황

초기 개발 단계입니다. 저장소는 ytopt를 기반으로 구성하며,
GC_TLA 원본은 Git submodule로 포함합니다.
GC_TLA와 새로운 전이학습 방법을 공통 ytopt 인터페이스에 연결하는 기능은 개발 예정입니다.

## 시작하기

[저장소 생성 가이드](docs/GITHUB_SETUP_KO.md)를 참고하세요.
기본 설치와 튜닝은 [ytopt 문서](https://github.com/ytopt-team/ytopt#installation-instructions)를 따릅니다.
초기 Python 패키지와 튜닝 명령어는 `ytopt` 이름을 유지합니다.

## 연구 지원

| 항목 | 내용 |
| --- | --- |
| 연구기간 | 2026–2029 |
| 과제명 | 심층학습 모델을 이용한 전이학습 기반 HPC 애플리케이션 자동 튜닝 프레임워크 개발 |
| 지원사업 | 기초연구사업 신진연구 |
| 지원기관 | 한국연구재단 (National Research Foundation of Korea, NRF) |

## 참고문헌 및 출처

- [ytopt](https://github.com/ytopt-team/ytopt)
- Randall, Koo, et al. (2023).
  [Transfer-Learning-Based Autotuning Using Gaussian Copula](https://doi.org/10.1145/3577193.3593712).
  ICS 2023. [코드](https://github.com/tlranda/GC_TLA).

## 라이선스

원본 프로젝트의 BSD-2-Clause 라이선스와 저작권 표기를 유지합니다:
[ytopt](LICENSE.md),
[GC_TLA](https://github.com/tlranda/GC_TLA/blob/master/LICENSE.md).

<p>
  <a href="docs/assets/mltuno-icon.png"><img src="docs/assets/mltuno-icon.png" alt="MLTuno icon" width="28" align="absmiddle"></a>
  <a href="docs/assets/mltuno-icon.png">아이콘</a> · <a href="docs/assets/mltuno-logo.png">아이콘·글자 조합형</a>
</p>

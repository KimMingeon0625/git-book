# A/B Test

## A/B 테스트

* A/B 테스트 (버킷 테스트 or 분할-실행 테스트)는 두개의 변형 A와B를 사용하는 종합 대조 실험(controlled experiment)이다.&#x20;
* 목표&#x20;
  * 관심 분야에 대한 결과를 늘리거나 극대화하는 웹 페이지 변경 사항이 무엇인지를 규명.&#x20;
  * 변수 A에 비해 변수 B에 보이는 응답을 테스트하고, 어떤 것이 더 효과적인지 판단한다.

## A/B 테스트 단점

* 테스트하는 기간동안 일정 비율만큼 더 좋지않은 서비스를 실 서비스에 지속적으로 노출 \
  (최악의 결과 -> 고객 이탈)&#x20;
* 서비스 노출 시간이 길어질수록 잔존율이 높아지는 경우 짧은 테스트 기간으로는 올바른 선택을 하지 못하게 된다.

## A/B 테스트 보완가능 알고리즘

#### MAB 알고리즘( Multi armed Bandit)

정확한 승률을 알지 못하는 여러 대의 슬롯머신을 가지고 도박을 할 때 가장 많은 돈을 따기 위해서 어떤 전략(알고리즘)을 가지고 게임을 해야 가장 많은 돈을 딸 수 있을 것인가 하는 문제를 빗대어 MAB이라고 부르게 된 것이다. ( 슬롯머신을 부르는 명칭 : One-Armed bandit(외팔이 도둑놈) ) \
\-> 통계에 대한 정확한 이해를 가지고 실행해야 한다.

## A/B 테스트 프로세스

* 기존에 존재하는 데이터를 분석한다.
* 목표를 구체화한다.&#x20;
* 지표를 선정한다.&#x20;
* 가설을 수립한다.&#x20;
* 실험을 설계 및 실행한다.&#x20;
* 결과를 분석한다.

## A/B 테스트 샘플링

특정 사용자가 A/B 테스트가 진행중인 웹사이트를 처음 접속해서, B 테스트가 설정되었다면, 다음에 접속했을 때도 B 테스트로 수행되어야 한다. 일반적으로 사용자 ID 기반으로 구현한다.

![](https://lh5.googleusercontent.com/MkZqsHtQ1sxXKIFi7a1fqX44q5KEirkfpFzVRclUXuDj22NHKOaguHMja3l6Hi5lmhVvxuqI6EnUeXG6-QmplK1ZdNOy5L6hKjeSMqUK-BMvScUlaCE9l1bbfAzk5NwBdXlSx2uH)

**Hash 함수를 사용하여 A/B 테스트 샘플링**

![](https://lh4.googleusercontent.com/IdzgmbM0lFWlC4pELG9lJqDeFGoO4VftQXXJnD6uTjeF0chW-RjdPP-P4YnlkurXapNeetU6czDP0\_7TcTJusoLrh\_3D5nZfmwyn9xM4Skf1qXZxltdB6gUcBFQ\_bG4dSVprJoj\_)

**사용자 마다 Bucket Number를 설정해서 해당 하는 범위에 속하는 사이트로 선택이 된다. 사용자 ID가 바뀌지 않는 이상, 일관되게 선택된다.**

****

\[참고 URL]&#x20;

1. [개발자를 위한 A/B 테스트 해시 샘플링](https://brunch.co.kr/@springboot/283)
2. [Websites (JavaScript): Integrate - Matomo Analytics (formerly Piwik Analytics) - Developer Docs - v4](https://developer.matomo.org/guides/ab-tests/browser)

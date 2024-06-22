# Reactor 스터디
## Flux
- 0-N개의 아이템을 처리
- onNext(~N), onComplete, onError

## Mono
- 0개 또는 1개의 아이템을 처리
- OnNext(0~1), OnComplete, OnError

## Testing
- 자동화된 테스트 작성
- 시나리오 검증과 관련된 StepVerifier 제공
- https://projectreactor.io/docs/core/release/reference/#_testing_a_scenario_with_stepverifier

## Operator
- Flux, Mono 데이터를 가공하거나, 변환하거나, 필터링할 수 있는 함수형 연산자
- 스트림 데이터를 어떻게 처리할 것이냐?의 관점

### map, filter, take, flatMap
#### map
- 스트림 내의 각 요소를 1:1로 변환하기

#### filter
- 조건을 만족하는 요소들만 걸러내기

#### take
- 주어진 개수 또는 시간만큼의 요소만을 스트림에서 가져오기

#### flatMap
- 각 요소를 다른 Flux나 Mono로 변환하고, 이를 하나의 스트림으로 병합하기

### concatMap, flatMapMany, defaultIfEmpty/switchIfEmpty, merge/zip
#### concatMap
- flatMap과 비슷하나, 다음 요소를 처리하기전에 이전 요소의 작업이 완료될 때까지 기다림
- 순서가 중요한 작업에는 flatMap 대신 concatMap을 사용하는 것이 좋음

#### flatMapMany
- Mono -> Flux 변환에 사용

#### defaultIfEmpty/switchIfEmpty
- defaultIfEmpty: 원래 스트림이 비어있을 경우, 기본값을 제공
- switchIfEmpty: 원래 스트리이 비어있을 경우, 대체 스트림을 사용

#### merge/zip
- 여러 개의 스트림을 하나로 모아주는 역할

### count, distinct, reduce, groupBy
#### count
- Flux 또는 Mono의 요소를 계산
- 주로 스트림의 크기를 알고 싶을 때 사용

#### distinct
- 스트림 내의 중복된 요소를 제거하고, 각 요소의 고유성을 보장
- 요소의 equals()와 hashCode() 메서드를 사용하여 중복 여부를 판단

#### reduce
- 스트림의 모든 요소를 하나의 값으로 축약
- 초기값과 누적 함수를 사용하여 스트림을 처리
- 초기값을 제공하지 않는 버전의 경우 첫 번째 요소를 초기값으로 사용

#### groupBy
- 스트림의 요소들을 그룹화
- 주어진 키 함수를 사용하여, 각 요소를 그룹별로 묶고 그룹별로 스트림을 생성

### delaySequence/limitRate, sample
#### delaySequence/limitRate
- delaySequence: 스트림의 시작을 지정된 시간만큼 지연
- limitRate: 요소 방출을 조절하여, 한 번에 방출되는 요소의 수를 제한, 주로 스트림의 소비 속도를 제한하여 백프레셔를 관리하는데 사용

#### sample
- 주기적으로 스트림을 샘플링하여, 주어진 시간 간격 동안 스트림의 마지막 요소를 가져옴

## Schedulers
- 비동기 작업을 처리하는 스레드 관리를 위한 인터페이스
- Schedulers를 통해 어떤 스레드에서 작업을 실행할지 제어할 수 있음
- Reactor에서는 다양한 Schedulers를 제공하여, 개발자가 요구하는 비동기 처리와 스레드 관리를 쉽게 설정할 수 있음

### immediate()
- 현재 호출된 스레드에서 즉시 작업을 실행
- 추가적인 스레드 생성 없이 현재 스레드를 그대로 사용

### single()
- 단일 백그라운드 스레드에서 작업을 수행

### parallel()
- 고정된 개수의 스레드를 사용하여, 작업을 병렬로 처리
- 기본적으로 CPU 코어 수에 맞게 스레드를 생성하여, CPU bound 작업에 적합

### boundElastic()
- 동적 스레드 풀을 사용하여, 필요에 따라 스레드를 생성하고 일정 시간 동안 사용되지 않는 스레드를 정리
- 대기 시간이 긴 I/O bound 작업에 적합

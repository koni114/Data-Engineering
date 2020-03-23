## Spark 아파치 스파크 & Hadoop 소개
### 하둡 Hadoop
* 하둡 프레임워크는 파일 시스템인 HDFS(Hadoop Distributred File System)과 데이터를 처리하는 맵리듀스 엔진을 합친 것으로 대규모 클러스터 상의 데이터 처리를 다음과 같은 방식으로 단순화함

  * Job을 잘게 분할하고 클러스터의 모든 노드로 매핑
  * 각 노드는 잡을 처리한 중간결과를 생성
  * 분할된 중간 결과를 집계(reduce)해서 최종 결과를 냄
  * 데이터를 여러 노드로 분산하고 전체 연산을 잘게 나누어 동시에 처리하는 병렬 처리가 가능

* 부분적으로 장애가 발생할 경우 전체 시스템은 동작할 수 있는 장애 내성(fault tolerance)를 갖는 시스템을 만들 수 있음
* 맵리듀스의 핵심은 최소한의 API(map, reduce) 만 노출해 대규모 분산 시스템을 다루는 복잡한 작업을 감추고 병렬 처리를 프로그래밍적 방법으로 지원한다는 점
* 데이터가 크기 때문에 데이터를 가져와서 처리하는 것이 아니라 데이터가 저장된 곳으로 프로그램 전송
* 이 점이 바로 기존 데이터 웨어하우스 및 관계형 DB와 맵리듀스의 가장 큰 차이점

#### 한계
* 맵리듀스 잡의 결과를 다른 잡에서 사용하려면 이 결과를 HDFS에 저장해야하기 때문에, 이전 잡의 결과가 다음 잡의 입력이 되는 반복 알고리즘에는 적합하지 않음
* 나눴다가 다시 합치는 2단계 패러다임을 적용하기 어려운 경우가 있음
* 하둡은 low-level framework 이다 보니, 데이터를 조작하는 high-level framework나 도구가 많아 환경이 복잡해짐

### 스파크 Spark
* 아파치 스파크는 오픈소스 분산 쿼리 및 처리 엔진
* 데이터가 메모리에 저장되어 있을 때는 아파치 하둡보다 100배 빠르며, 디스크에 저장되어 있을 때는 10배 빠름
* spark는 데이터를 읽고, 변형하고, 합계를 낼 수 있으며 복잡한 통계 모델을 쉽게 학습하고 베포할 수 있음
* 스파크 API는 자바, 스칼라, 파이썬, R, SQL을 이용해 접근 가능
* 어플리케이션을 빌드하는데 쓰일 수 있고, 여러 어플리케이션을 라이브러리로 묶어서 클러스터에 베포할 수도 있으며 파이썬 노트북(Spark-notebook, Apache Zepplin 등)을 통해 대화식으로 빠른 분석을 수행 할 수 있음

#### 기능
* 빅데이터 애플리케이션에 필요한 대부분의 기능 지원
* 맵리듀스와 유사한 일괄 처리 기능
* 실시간 데이터 처리 기능(Spark Streaming)
* SQL과 유사한 정형 데이터 처리 기능(Spark SQL)
* 그래프 알고리즘(Spark GraphX)
* 머신 러닝 알고리즘(Spark MLlib)

#### 장점
* 데이터를 메모리에 캐시로 저장하는 인-메모리 실행 모델로 비약적인 성능을 향상시킴
* 사용자가 클러스터를 다루고 있다는 사실을 인지할 필요가 없도록 설계된 기반의 API 제공
* 로컬 프로그램을 작성하는 것과 유사한 방식으로 분산 프로그램을 작성할 수 있음
* 스칼라, R, Python, 자바 지원
* Spark Shell(Read-Eval-Print Loop, REPL)을 이용해 간단한 실험을 하거나 테스트를 할 수 있음
* 다양한 유형의 클러스터 매니저를 사용할 수 있음(Standalone cluster, Hadoop YARN 등)

#### 단점
* 데이터 셋이 적어 단일 노드로 충분한 애플리케이션에서 스파크는 분산 아키텍처로 인해 오히려 성능이 떨어질 수 있음

#### 컴포넌트
#####  Spark Core
* Spark Job과 다른 Spark Component에 필요한 기본 기능 제공
* 분산 데이터 컬렉션을 추상화한 객체인 RDD로 다양한 연산 및 변환 메소드를 제공
* RDD는 노드에 장애가 발생해도 데이터셋을 재구성 할 수 있는 복원성을 가지고 있음
* Spark Core는 HDFS, GlusterFS, S3 등 다양한 파일 시스템에 접근 가능
* 공유 변수(broadcast variable)과 누적 변수(accumulator)를 사용해 컴퓨팅 노드 간 정보 공유
* 네트워킹, 보안, 스케쥴링, 데이터 셔플링등 기본 기능 제공

##### Spark SQL
* 스파크와 하이브 SQL이 지원하는 SQL 을 사용해 대규모 분산 정형 데이터를 다룰 수 있음
* JSON 파일, Parquet 파일, RDB 테이블, 하이브 테이블 등 다양한 정형 데이터를 읽고 쓸 수 있음
* DataFrame 과 Dataset 에 적용된 연산을 일정 시점에 RDD 연산으로 변환해 일반 스파크 잡으로 실행 함

##### Spark Streaming
* 실시간으로 스트리밍 데이터를 처리하는 프레임워크
* HDFS, 아파치 카프카(Kafka), 아파치 플럼(Flume), 트위터, ZeroMQ와 더불어 커스텀 리소스도 사용할 수 있음
* 다른 스파크 컴포넌트와 함께 사용할 수 있어 실시간 데이터 처리를 머신 러닝 작업, SQL 작업, 그래프 연산 등을 통합할 수 있음

##### Spark MLlib
* 머신 러닝 알고리즘 라이브러리
* RDD 또는 DataFrame 의 데이터셋을 변환하는 머신 러닝 모델을 구현

##### Spark GraphX
* 그래프는 정점과 두 정점을 잇는 간선으로 구성된 데이터 구조
* 그래프 RDD(EdgeRDD 및 VertexRDD) 형태의 그래프 구조를 만들 수 있는 기능을 제공
* 데이터 변환 및 조작하는 함수를 제공하는 분석 도구

##### 기타
Apache Mahout : 스케일러블한 머신 러닝 프레임워크
Apache Giraph : 빅데이터 그래프 프로세싱
Apache Pig : 대용량 데이터 분석 플랫폼
Apache Hive : 데이터 웨어하우스
Apache Drill : 대규모 데이터의 SQL 분석 제공
Apache Impala : SQL 병렬 처리 엔진 클러스터 관리 도구
Apache Ambari : 클러스터 관리 도구 (프로비저닝, 관리, 모니터링 등)
하둡과 다른 시스템 간 데이터 전송하는 인터페이스 도구
Apache Sqoop : 하둡과 다른 데이터 저장소 간 대용량 데이터 전송
Apache Flume : 대용량 로그 데이터를 수집
Apache Chukwa : 대용량 로그 데이터 수집 및 분석
Flume comparison to Chukwa
Apache Storm : 실시간 데이터 처리
기본적인 데이터 스토리지, 동기화, 스케줄링 등을 제공하는 인프라도구
Apache Oozie : 워크플로우 스케쥴러
Apache HBase : 비관계형(non-relational) 분산(distributed) 데이터베이스
Apache Zookeeper : 분산된 시스템을 관리하는 코디네이션 서비스(coordination service)
스파크로 대체할 수 있는 기능
그래프 프로세싱 Giraph -> Spark GraphX
머신러닝 Mahout -> Spark MLlib
실시간 데이터 처리 Storm -> Spark Streaming (2.2.1 이하는 완벽 대체 불가)
데이터 분석 Pig, 데이터 전송 Sqoop -> Spark Core 와 Spark SQL 로 대체
SQL 관련 Impala, Drill -> Spark SQL 을 포괄하는 기능으로 함께 사용할 수 있음
인프라 관련 도구는 대체할 수 없음

# Spark Practice

## Spark tip

1. 기능 찾기
2. Reference 참조
3. Library

> row 개념 X
>
> 분산환경에서 data를 다룬다.
>
> 원형객체 -> 자바에서 성능 손실
>
> ! 수동 생성 지양
>
> 데이터의 일부에 문제가 생겨도 스스로 알아서 복구
>
> 다수의 '데이터 요소(element)'가 모인 집합
>
> 하둡의 파일 입출력 API에 의존
>
> 스파크 실행 -> spark job을 실행
>


text file read: ex) spark.sparkContext.textFile("~~~");

중복 data 제거: ex) words.distinct().count();
		    filter(vo -> ~~~);


## Spark Cluster

> 여러 대의 서버가 마치 한 대의 서버처럼 동작
>
> Spark	{ Node				Local
	{ Node1, Node2, Node3, ...	Spark Cluster
	{ Node1, Node2, Node3, ...	Hadoop Cluster

> HDFS(파일 저장) / YARN(Reduce 관리)에서 구동

분산처리: Transformation ex) .map, .flatmap
	  log가 보이지 않음

	  Action ex) .count, .collect
	  모아서 나중에 실행


## Driver

> Spark Context 생성, 그 인스턴스를 포함하는 프로그램
>
> 각 워커 노드들에게 작업을 지시하고, 결과를 취함
>
> driver 실행 서버 != cluster를 구성하는 서버
>
>> RDD operation

다른 타입의 결과를 반환: transformation
결과값을 얻어내는 연산: action

Spark RDD 생성: ex) sc.parallelize(1 to 10);
		    rdd1.map((x) -> (x+1));

JavaPairRDD : groupByKey, reduceByKey, mapValues
OrderedRDD : sortByKey
DoubleRDD : sum, mean ...


## Tuple

ex) scala.Tuple2: 키1, 값1


## Spark Context

> 스파크 애플리케이션과 클러스터를 연결
>
> 모든 스파크 애플리케이션은 반드시 생성되어야 함

ex) SparkConf conf = new SparkConf().setMaster("local[\*]").setAppName("RDDCreateSmaple");

\* : thread의 수, 최대

ex) JavaSparkContext sc = new JavaSparkContext(conf);
    sc.parallelize(a, b(파티션 수));

파일을 읽을 때: ex) sc.textFile("~~~");
		파일의 각 줄은 한 개의 RDD 구성 요소


## Collect

> RDD의 모든 원소를 모아서 배열(RDD X)로 돌려줌 -> Action에 해당
>
> collect 연산을 호출한 서버의 메모리에 수집
> 
> but, 기능 디버깅, 작은 데이터를 처리할 때만 사용, 대용량 -> 성능 문제를 고려해야 함

flatMap: ex) JavaRDD<String> rdd3 = rdd1.flatMap((String t) -> Arrays.asList(t.split(",")).iterator());

ex) JavaPairRDD<String, Integer> rdd4 = rdd1.mapToPair((String t) -> new Tuple2<String, Integer>(t, 1).mapValues((Integer v1) -> v1 + 1);

ex) JavaPairRDD<Integer, String> rdd3 = rdd1.flatMapValues((String v1) -> Arrays.asList(v1.split(",")));







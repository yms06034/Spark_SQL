![image](https://user-images.githubusercontent.com/98085184/230558684-48ec2d3b-07e0-474f-b328-696d00f112ad.png)

# # Spark_SQL Project
E-commerce 데이터를 활용해 DB -> Spark_SQL -> DB 담아주는 flow 작업을 진행하였습니다.

## Description
![image](https://user-images.githubusercontent.com/98085184/230559238-a03f6085-4ab6-463b-b9a8-3636881fcce6.png)
[Database Schema]
**Brazilian E-Commerce Public Dataset** 

- SQL문으로 데이터 분석 및 전처리를 진행하였고, 최종 쿼리문을 통해 Dashborad에 바로 사용할 수 있도록 진행하였고 DB에 담아줬다.

### RETROSPECTIVE
- 이번 Spark_SQL를 사용하면서 Spark Backend에 대해 많은 공부를 진행하였다.
- 또한 본 Dataset을 통해 e-commerce에 대해 많은 이해와 DE 관련하여 더 Deep한 공부를 진행하였다.
    - 왜 DE가 필요한지에 대해 많은 생각을 하게 된 계기가 되었다
-   **Catalyst**
    
    -   사용자가 쓴 코드를 Spark에서 실행 가능하게 끔 변환 해주는 엔진
        
    -   Query Plan Optimization
        
    -   이렇게 최적화된 코드는 Tungsten으로 넘어감
        
    -   **Logical Plan → Physical Plan으로 변환**
        
        1.  분석 : DataFrame 객체의 relation을 계산, column의 타입과 이름을 확인
        2.  Logical Plan 최적화
            1.  상수로 표현된 표현식을 Compile Time에 계산 (x runtime)
            2.  Predicate Pushdown: join & filter → filter & join
                1.  **ex) Scan → filter → join → project → aggregate**
            3.  Projection Pruning: 연산에 필요한 칼럼만 가져오기
        3.  Physical Plan 만들기
            1.  Spark에서 실행 가능한 Plan으로 변환
        4.  코드 제너레이션
            1.  최적화된 Physical Plan을 Java Bytecode로 변환
        
        ```python
        # Parsed Logical Plan -> Analyzed Logical Plan ->
        # -> Optimized Physical Plan -> Physical Plan
        
        spark.sql(query).explain(True)
        
        ```
        
    -   **Logical Plan?**
        
        -   수행해야하는 모든 transformation단계에서 대한 추상화
        -   데이터가 어떻게 변해야 하는지 정의하지만 실제 어디서 어떻게 동작하는지는 정의하지 않음 스키마
    -   **Physical Plan?**
        
        -   Logical Plan이 어떻게 클러스터 위에서 실행될지 정의
        -   실행 전략을 만들고 Cost Model에 따라 최적화
-   **Tungsten**
    
    -   Physical Plan으 선택되고 나면 분산 환경에서 실행될 Bytecode가 만들어짐
        -   Code Generation이라고 부름
    -   스파크 엔진의 성능 향상이 목적
        -   **메모리 관리 최적화**
        -   **캐시 활용 연산**
        -   **코드 생성**

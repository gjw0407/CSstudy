# Spring Batch

# 개념

> 로깅/추적, 트랜잭션 관리, 작업 처리 통계, 작업 재시작, 건너뛰기, 리소스 관리 등 대용량 레코드 처리에 필수적인 기능 제공
> 
- 실패 시 실패한 지점부터 실행

# 용어

- Job : 배치처리 과정 객체
- JobInstance : Job의 실행의 단위
- JobParameter : JobInstance 구별, JobInstance에 전달되는 매개변수 역할
- JobExecution : JobInstance에 대한 실행 시도에 대한 객체 (JobInstance 실행에 대한 상태, 시작 시간, 종료 시간, 생성 시간 등의 정보를 지님)
- Step : Job의 배치처리를 정의하고 순차적인 단계를 캡슐화
- StepExecution : Step 실행 시도에 대한 객체 (JobExecution과 비슷)
- ExecutionContext : Job에서 데이터를 공유 할 수 있는 데이터 저장소
- JobRepository : 모든 배치 처리 정보를 담고있는 매커니즘
- JobLauncher : Job과 JobParameters를 사용하여 Job을 실행하는 객체
- ItemReader : Step에서 Item을 읽어오는 인터페이스
- ItemWriter : 처리 된 Data를 Writer 할 때 사용
- ItemProcessor : Reader에서 읽어온 Item을 데이터를 처리하는 역할

# 출처

[https://khj93.tistory.com/entry/Spring-Batch란-이해하고-사용하기](https://khj93.tistory.com/entry/Spring-Batch%EB%9E%80-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

[https://jojoldu.tistory.com/324](https://jojoldu.tistory.com/324)
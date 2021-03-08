# Memory Management

## Logical address (Virtual address)
- 프로세스마다 독립적으로 가지는 주소 공간으로 0에서부터 시작한다.
- CPU가 물리적 주소로 접근하기 위해 사용하는 참조하는 주소
- MMU(Memory-Management Unit)와 같은 하드웨어 장치를 통해 논리적 주소를 물리적 주소로 변환할 수 있다. (주소 바인딩)

## Pysical address
- 메모리에 실제로 올라가는 물리적 위치를 의미하는 주소

## 주소 바인딩
- Complie time : 컴파일 시 물리적 주소 결정 
- Load time : 프로그램이 시작할 때 물리적 주소 결정
- Runtime(execution): 
  - 프로그램 실행 중에 메모리 위치가 변경될 수 있다.
  - MMU와 같은 별도의 하드웨어 지원이 필요하다.
 
### MMU(Memory-Management Unit)
- 논리적 주소를 물리적 주소로 매핑해주는 하드웨어 장치
  - base(relocation) register: 접근할 수 있는 메모리 주소의 최솟값
  - limit register: 논리적 주소의 범위
![MMU](https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/2974/6522.PNG)

## Swapping
- 프로세스를 일시적으로 메모리에서 back store(하드 디스크 등)으로 쫓아내는 것
![swapping](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F99D56D385C74261A04E7B6)

## Contiguous allocation
메모리의 연속적인 공간에 프로세스를 할당하는 방법

### 고정 분할
미리 분할해 놓은 메모리 영역에 할당
- 프로세스가 할당될 수 있지만 크기가 작아 할당되지 않는 공간(외부 조각), 프로세스가 할당된 메모리 영역에 남는 공간(내부 조각)이 생길 수 있다.
![고정 분할 할당](https://blog.kakaocdn.net/dn/cYaWAO/btqvi5E2VKS/zcShnDe3YlKz3S2OFz1hXK/img.png)

### 가변 분할
- 프로세스의 크기대로 메모리를 분할해서 순서대로 할당한다. 실행 종료 후에 메모리의 남는 공간(hole)이 외부 조각으로 생길 수 있다.
![가변 분할 할당](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBh8v2%2FbtqviGMiHGG%2FKvqOWT37ob899ZgfHIx7p1%2Fimg.png)

### Dynamic Storage Allocation Problem
size n을 만족하는 적절한 hole을 찾는 문제
 - First fit: 가장 먼저 찾는 hole에 할당
 - Best fit: 모든 hole 중에서 가장 작은 hole에 할당 
 - Worst fit: 모든 hole 중에서 가장 큰 hole에 할당 
 
### Compaction
hole을 한 곳에 모아 하나의 큰 block 생성 → 비용이 크다

## Noncontiguous allocation

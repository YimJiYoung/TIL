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

### Paging

![paging](https://miro.medium.com/max/1200/1*-SDSBsaYzc7TaGhZhAf26A.jpeg)
logical memory를 동일한 크기의 page 단위로 쪼개고 page는 noncontiguous하게 저장된다.
주소 변환을 위해서는 page와 frame을 매핑시키는 page table이 필요하다.
- page : logical memory를 일정된 한 크기로 나눈 블록이다.
- frame : physical memory를 일정된 한 크기로 나눈 블록이다.

### Page Table

page table은 메인 메모리에 위치하게 되고 따라서 데이터에 접근하기 위해서는 두 번의 메모리 접근이 필요하다 (page table 접근 -> 실제 데이터가 있는 곳 접근)
따라서 속도 향상을 위해서 translation look-aside buffer(TLB, 메인 메모리와 cpu 사이에 존재하는 캐시)를 사용할 수 있다.
TLB에는 빈번하게 사용되는 page를 저장한다. 
![](https://media.geeksforgeeks.org/wp-content/uploads/paging-2.jpg)

### Two Level Page Table

주소가 32bit로 구성되어 있는 경우 2의 32승개의 주소를 표현할 수 있고, 주소는 1Byte마다 매겨지므로 가능한 최대 메모리의 크기는 4GB가 될 것이다. 그럼 4kB의 크기를 갖는 page는 1M개가 생길 것이며 이를 표현할 수 있는 page table의 크기는 1M * 4B(page entry 크기) = 4MB가 된다. 프로세스마다 4MB의 page table이 필요한데 실제로 사용되는 page는 일부이므로 공간 낭비가 심하다. 이런 공간을 줄이기 위해서 Two Level Page Table을 사용한다. 2번 page table을 접근해야하기 때문에 시간적으로는 손해지만 사용되지 않는 주소 공간(page)에 대해서는 안쪽 page table을 생성하지 않고 outer에서 NULL을 바라보게 하여 공간을 줄일 수 있다.

![](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99CD0B3359C4A82D36)
![](https://lh3.googleusercontent.com/proxy/oBm2UFRPsWywYf28IdkEvslqDIaO58jYPEGpKYS5kTFeqn2_YcXilp6vpa3cG84wr1tF2t5Wp3IMktwik7p0SRwRB9x6ac0rqM5YlfE9-mBlL8J64JjIWDNsyA02ozs0fpjR9Rckw81aRnniBMTrLg_v0UmfuOBKvTrE)

### Inverted Page Table

page frame 하나당 page table에 하나의 entry를 둔 것. (system wide - 프로세스가 하나의 page table 공유)
- 장점: 공간 절약
- 단점: 데이터 접근 시 pid, page number로 테이블 탐색하는 시간 소요.
![](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile28.uf.tistory.com%2Fimage%2F9933253359C4A8301D6602)


### valid/invalid, protection bit
- valid/invalid bit: 해당 주소의 frame이 사용할 수 있는지 의미
   - 사용 불가한 경우: 프로세스가 해당 주소 부분 사용하지 않는 경우, 현재 메모리에 없고 swap area에 위치한 경우
- protection vit: 접근 권한 의미 (read/write/read-only)

## Segmentation

logical address를 동일한 크기의 page가 아닌 code, data, stack과 같이 의미 단위의 segment로 쪼개는 방식.
- 논리 주소: segmentation number, offset
- segment table: limit(segment의 크기), base
- 장점: 의미 단위로 쪼개기 때문에 보안, 공유 설정하기에 유리하다. segment의 개수는 page보다 많지 않으므로 table로 인한 메모리의 낭비가 적다.
- 단점: segment의 크기가 균일하지 않으므로 가변분할 방식과 동일한 문제가 발생한다 (외부 조각 발생)
![](https://www.enterprisestorageforum.com/wp-content/uploads/2021/02/paging-and-segmentation_6019c4f2d369c.png)


### Paged Segmentation
- segment를 page 단위로 쪼개는 방법. segment 당 page table이 존재하게 된다 → segmentation의 외부 단편화 문제 해결
![](https://www.gatevidyalay.com/wp-content/uploads/2018/11/Segmented-Paging-Translating-Logical-Address-into-Physical-Address-Diagram.png)

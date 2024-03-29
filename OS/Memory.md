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

# Virtual Memory
프로그램이 실행되기 위해 그 프로세스의 주소 공간 전체가 메모리에 올라와 있어야 하는 것은 아니다. 메모리의 연장 공간으로 디스크의 스왑 영역이 사용될 수 있기 때문에 프로그램 입장에서는 물리적 메모리 크기에 대한 제약을 생각할 필요가 없어진다. 

## demand paging
페이지가 필요할 때 메모리에 올리는 기법 → 메모리 사용량 감소, 더 많은 프로그램 동시에 실행 가능

## Page Fault
어떤 페이지에 대해 접근하는 경우(address translation) 해당 페이지가 물리적 메모리에 없는 경우 page fault가 발생한다.
page fault는 trap의 한 종류이며 OS는 trap 발생 시 해당 페이지를 Dish I/O 작업을 통해 메모리에 올린다. 

![](https://media.geeksforgeeks.org/wp-content/uploads/121-1.png)

## Page Replacement Algorithm
page fault 발생 시 비용이 큰 disk I/O가 발생하기 때문에 page fault를 방지하는 것이 중요하다.
page replacement algorithm은 page fault가 발생했을때 메모리가 꽉 찬 경우 어떤 페이지을 내쫓을 것인지에 대한 것으로 page fault rate을 줄이도록 설계한다.

### LRU(Least Recently Used)
- 시간 지역성(최근에 참조된 페이지가 가까운 미래에 다시 참조될 가능성이 높은 성질)을 이용한 방법. 
- 가장 오래 전에 참조된 페이지를 쫓아냄

### LFU(Least Frequetly Used)
- 참조 횟수가 가장 적은 페이지를 쫓아냄
- 장기적인 관점에서의 참조 성향을 고려하지만 시간 지역성을 고려하지는 못함
  - ex) 예전에 많이 사용되었던 페이지가 더이상 사용되지 않음에도 교체 대상이 되지 않는 문제 발생할 수 있다

### Clock Algorithm
![clock](http://pages.cs.wisc.edu/~bart/537/lecturenotes/figures/s21.clock.gif)
- LRU나 LFU 알고리즘은 페이지의 참조 순서나 참조 횟수를 (linked list나 heap과 같은 자료구조를 사용하여) 소프트웨어적으로 유지해야하는 오버헤드가 있다. 클락 알고리즘은 하드웨어적인 지원으로 LRU를 근사하여 최근에 참조되지 않은 페이지 중 하나를 교체하는 알고리즘으로 Not Recently Used 알고리즘으로도 불린다.
- 메모리 내의 페이지에 대한 참조 비트를 가지고 있고 이는 참조되었을 때 1로 설정되고 clock의 시곗바늘(iterator, handle)이 돌면서 0으로 설정한다. 시곗바늘이 가리키는 페이지의 참조 비트가 0일 경우 해당 페이지는 시곗바늘이 한 바퀴 도는 동안 참조되지 않은 페이지를 의미하므로 교체 대상이 된다.
- 알고리즘 개선을 위해 modified_bit를 사용할 수 있다. modified_bit은 최근에 수정되었음을 의미하며 따라서 swap out될 때 변경 사항을 저장해야하므로 disk I/O가 발생한다. 따라서 modified_bit가 0인 페이지를 우선적으로 교체 대상으로 설정한다.


## Page Frame의 Allocation


프로그램마다 몇 개의 page frame을 할당할 것인지의 문제. 프로그램이 원활하게 실행되기 위해서는 page fault가 적게 발생해야 하며, 일정 수준 이상의 frame을 할당 받아야 한다.
- ex) code, data, stack 영역 동시에 접근, for문 순회할 때 for문 내의 코드 반복 접근
- **Global replacement** : 다른 프로세스에 할당된 frame 뺏어올 수 있음.
  - LRU, LFU 전체 페이지 대상으로
  - Working set, PFF 알고리즘
- **Local replacement** : 현재 수행중인 프로세스에게 할당된 frame 내에서만 replacement. 프로세스마다 페이지 프레임을 미리 할당하는 것을 전제로 함.
   - 프로세스별로 LRU, LFU 등의 알고리즘 독자적으로 운영 가능


### Thrashing

![thrashing](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2623B436575917D11D)

프로세스가 필요한 최소의 page frame을 할당받지 못한 경우 발생하는 문제
- page fault rate 증가
- cpu utilization 감소
  - page fault로 인한 disk I/O 빈번하게 발생 -> process block, context swtich 발생 -> OS가 프로세스를 실행하는 시간보다 page fault를 처리하는 오버헤드가 커지므로 cpu utilization은 감소한다.

### Working-set

![working-set](https://examradar.com/wp-content/uploads/2016/10/Example-4.6.png)

프로세스는 일정 시간동안 특정 주소 영역을 집중적으로 참조하는 경향이 있고 이렇게 집중적으로 참조되는 페이지들의 집합을 지역성 집합이라고 한다.
워킹셋 알고리즘은 이러한 지역성 집합이 메모리에 동시에 올라갈 수 있도록 하여 thrashing을 방지하는 알고리즘이다.

- 워킹셋 : 현재 시점에서 윈도우만큼의 시간 간격에서 참조된 페이지들의 집합
- 프로세스의 워킹셋을 구성하는 페이지들이 모두 메모리에 올라갈 수 있는 경우에만 할당
- 아닌 경우에는 할당된 페이지 프레임 모두 반환한다. 프로세스의 주소 공간 전체를 swap out(**Suspended**)

### Page Fault Frequency: PFF

![PFF](https://lh3.googleusercontent.com/proxy/DDfSGPyOVgC6Mv3ZJOWX-Ldrpj7ljdqpRzGS1vGp8qLL82gsu1esqXr3r-SyixLXaUDxiEZZJp8H7wZw6Eia2TGCDQxWbVZpQ6NWwY7y2g3aXX3tH6iDn6T4)

프로세스의 page fault frequency를 주기적으로 조사하고 이 값에 근거하여 각 프로세스에 할당할 프레임 수를 조절한다.
- page fault frequency > upper bound: 프로세스에 할당된 프레임 수가 부족하다고 판단 -> 페이지 프레임 할당량 증가
- page fault frequency < lower bound: 프로세스에게 필요 이상으로 많은 프레임이 할당된 것으로 판단 -> 페이지 할당량 감소

# File System

## File
메모리는 주소를 통해 접근했지만 디스크의 파일은 이름을 통해 접근한다. (a named collection of related information)
- OS는 다양한 저장장치를 파일이라는 논리적 단위를 통해 접근할 수 있도록 한다.
- operation
  - create, read, write
  - open : file의 metatdata 메모리에 적재
  - lseek : 읽기 및 쓰기를 위한 파일의 pointer 위치 재지정

### File Metadata
파일을 관리하기 위한 부가적인 정보
- 파일 이름, 유형, 저장정보 위치, 파일 크기
- 접근 권한(read/write/execute), 시간, 소유자

### File System
- OS에서 파일을 관리하는 부분 
- file, file metadata, directory(일종의 file - 해당 directory에 속한 files의 정보 갖고 있음) 관리

### Partition (Logical Dist)
디스크를 파티션으로 구성하고 각 파티션을 File system 또는 swaping area로 사용할 수 있다.

### File Open
![](https://jhi93.github.io/assets/img/os/openfunction3.png)

#### Open File Table
현재 open된 파일들의 메타데이터 저장소(in memory)

#### File Descriptor
각 프로세스마다 open한 파일에 대한 정보 저장 (Open File Table에서의 위치 정보)

### File Protection
- Grouping : 각 user를 owner, group, other로 구분하고 접근 권한을 read, write, execute 3bit로 표시 -> 9bit

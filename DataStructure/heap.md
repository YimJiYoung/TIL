# Heap


최댓값 및 최솟값을 찾아내는 연산을 빠르게 하기 위해 고안된 자료 구조이며 우선순위 큐를 구현하는데 사용된다. heap은 다음과 같은 두가지 특징을 가진다.
- Complete Binary Tree : 마지막 레벨의 노드를 제외하고는 차있고(full), 마지막 레벨에서 가장 오른쪽부터 연속된 몇 개의 노드가 비어있을 수 있음.
- Heap property: 부모는 자식보다 크거나 같다(max heap), 부모는 자식보다 작거나 같다(min heap)
![heap](https://i.imgur.com/1mghTRv.png)

## Array로 구현
heap의 complete binary tree라는 특징을 이용하면 array로 구현할 수 있다.
- `A[i]`의 부모 노드 -> `A[i/2]`
- `A[i]`의 자식 노드 -> `A[2*i]`, `A[2*i+1]`

![](https://www.gatevidyalay.com/wp-content/uploads/2019/11/Array-Representation-of-Heap.png)

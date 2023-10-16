---
{"dg-publish":true,"permalink":"/CODING/C언어/C언어로 링크드 리스트 (노드) 만들기/"}
---


```c
#include <stdio.h>
#include <stdlib.h>
struct Node* InsertNode(struct Node* current, int data);
void DestroyNode(struct Node* destroy, struct Node* head);
struct Node* CreateNode(int data);
void PrintNodeFrom(struct Node* from);
int CountNode(struct Node* head);
int HasNode(struct Node* head, int data);
void PrintNodeFromBehind(struct Node* from);
void ConnectNode(struct Node* head);

struct Node {
	int data;              /* 데이터 */
	struct Node* nextNode; /* 다음 노드를 가리키는 부분 */
	struct Node* prevNode;
};

int main() {
	struct Node* Node1 = CreateNode(100);
	struct Node* Node2 = InsertNode(Node1, 200);
	struct Node* Node3 = InsertNode(Node2, 300);
	struct Node* Node4 = InsertNode(Node1, 400);
	
	ConnectNode(Node1); // head 와 tail 연결
	
// 노드가 전부 연결되어있어 PrintNodeFrom 함수를 실행하면 무한반복됨

	PrintNodeFrom(Node1);
	printf("총 노드는 %d개 입니다 \n", CountNode(Node1));

	if (HasNode(Node1, 400)) {
		printf("400의 데이터를 가진 노드가 존재합니다. \n");
	}
	DestroyNode(Node4, Node1);
	PrintNodeFrom(Node1);
	printf("---------------- \n");
	PrintNodeFromBehind(Node3);
	
	printf("Node1 이전 노드의 data 값: %d \n", (Node1->prevNode)->data);
	printf("마지막 노드의 다음노드 data 값 %d \n", (Node3->nextNode)->data);
	return 0;
}

void PrintNodeFrom(struct Node* from) {
	/* from 이 NULL 일 때 까지,
	 즉 끝 부분에 도달할 때 까지 출력 */
	while (from) {
		printf("노드의 데이터 : %d \n", from->data);
		from = from->nextNode;
	}
}

void PrintNodeFromBehind(struct Node* from) {
	while (from) {
		printf("노드의 데이터 : %d \n", from->data);
		from = from->prevNode;
	}
}

/* current 라는 노드 뒤에 노드를 새로 만들어 넣는 함수 */
struct Node* InsertNode(struct Node* current, int data) {
	/* current 노드가 가리키고 있던 다음 노드가 after 이다 */
	struct Node* after = current->nextNode;
	
	/* 새로운 노드를 생성한다 */
	struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
	
	/* 새 노드에 값을 넣어준다. */
	newNode->data = data;
	newNode->nextNode = after;
	newNode->prevNode = current;
	
	/* current 는 이제 newNode 를 가리키게 된다 */
	current->nextNode = newNode;
	
	/* after 노드가 있다면 after의 prevNode는 이제 newNode를 가리키게 된다*/
	if (after) after->prevNode = newNode;
	
	return newNode;
} /* 선택된 노드를 파괴하는 함수 */
void DestroyNode(struct Node* destroy,
				 struct Node* head) { /* 다음 노드를 가리킬 포인터*/
	struct Node* next = head;           /* head 를 파괴하려 한다면 */
	if (destroy == head) {
		(next->nextNode)->prevNode = NULL; // head 다음 노드의 prevNode를 NULL로 수정
		free(destroy);
		return;
	}              /* 만일 next 가 NULL 이면 종료 */
	while (next) { /* 만일 next 다음 노드가 destroy 라면 next 가 destory 앞 노드*/
		if (next->nextNode == destroy) { /* 따라서 next 의 다음 노드는 destory 가
										  아니라 destroy 의 다음 노드가 된다. */
			next->nextNode = destroy->nextNode;
			(destroy->nextNode)->prevNode = next;
		} /* next 는 다음 노드를 가리킨다. */
		next = next->nextNode;
	}
	free(destroy);
}
/* 새 노드를 만드는 함수 */
struct Node* CreateNode(int data) {
	struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
	
	newNode->data = data;
	newNode->nextNode = NULL;
	newNode->prevNode = NULL;
	
	return newNode;
}

int CountNode(struct Node* head) {
	int count = 0;
	while(head) {
		count++;
		head = head->nextNode;
	}
	return count;
}

int HasNode(struct Node* head, int data) {
	while(head) {
		if (head->data == data) {
			return 1;
			break;
		}
		else {
			head = head->nextNode;
		}
	}
	return 0;
}

void ConnectNode(struct Node* head) {
	struct Node* index = head;
	while (index->nextNode) {
		index = index->nextNode;
	}
	head->prevNode = index;
	index->nextNode = head;
}
```

ㅋ캬캬 구글링 없이 만들었지롱~~
#코딩공부 


typedef 类型定义

```c
struct node {} 
// 定义结构体, 使用要 
struct node n;

typedef struct node{}NODE;	// 也可以省略node
NODE n; //使用

// c++中
struct node{int a;}
node node1;  // 能直接用
struct node{int a;} node1;	// 定义并声明node1
// c++用typedef
typedef struct node{int a} Node;	// Node是结构体类型
Node n1;
n1.a = 10;	// 要先声明在使用

```




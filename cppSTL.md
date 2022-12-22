### vector的使用

```c++
#include <iostream>
#include <vector>

using namespace std;

int main() {
    vector<int> vec;
    for (int i = 0; i < 10; i++) {
        vec.push_back(i);
    }
    //迭代器可以看作是指向元素的一个指针，用法和指针相同
    vector<int>::iterator it = vec.begin();
    //end迭代器指向的是容器最后一个元素的下一个位置
    while(it != vec.end()) {
        cout << *it << endl;
        it++;
    }
    return 0;
}
```



### foreach的使用

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

//想要对容器中每个元素进行的操作
void print(int num) {
    cout << num << endl;
}

int main() {
    vector<int> vec;
    for (int i = 0; i < 10; i++) {
        vec.push_back(i);
    }
    //起始迭代器，结束迭代器，元素操作的函数名
    for_each(vec.begin(), vec.end(), print);
    return 0;
}
```


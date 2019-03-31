**题目描述：**

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**例如：**
- - -
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)

输出：7 -> 0 -> 8

原因：342 + 465 = 807
- - -
**思路：**

这道题相对来说比较简单，只是常规的按位加法而已，只不过是使用链表来存储每一位，我们只要把链表依次遍历即可。时间复杂度为(m+n)//两个链表的长度

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
static const auto asd = []{
    std::ios::sync_with_stdio(false);
    std::cin.tie(NULL);
    return 0;
}();
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ios::sync_with_stdio(false);
        cin.tie(nullptr);
        
        ListNode* sum = new ListNode(0);
        ListNode* p = sum;
        
        int jin = 0;
        
        while(l1 != NULL && l2 != NULL){
            ListNode* temp = new ListNode((l1->val+l2->val+jin)%10);
            jin = (l1->val+l2->val+jin)/10;
            l1 = l1->next;
            l2 = l2->next;
            p->next = temp;
            p = temp;
        }
        
        while(l1 != NULL){
            ListNode* temp = new ListNode((l1->val+jin)%10);
            jin = (l1->val+jin)/10;
            l1 = l1->next;
            p->next = temp;
            p = temp;
        }
        
        while(l2 != NULL){
            ListNode* temp = new ListNode((l2->val+jin)%10);
            jin = (l2->val+jin)/10;
            l2 = l2->next;
            p->next = temp;
            p = temp;
        }
        if(jin != 0){
            ListNode* temp = new ListNode(jin);
            p->next = temp;
            p = temp;
        }
        p->next = NULL;
        
        return sum->next;
    }
};
```
```
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
```
- - -
这两条语句的作用：

首先，C\+\+为了兼容C语言，为了不让cin、cout和printf、scanf混淆，C\+\+设立了一个缓存区用来实现两者的兼容。这就是cin、cout比printf、scanf要慢的原因。而ios::sync_with_stdio(false);这条语句就是让cin、cout解除这一种缓存关系，让cin和cout不在进去缓存区，直接输入输出。当面对大数据量的输入输出的时候便会提速。

tie是将两个stream绑定的函数，空参数的话返回当前的输出流指针。而cin和cout是绑定在一起的。

因为std :: cin默认是与std :: cout绑定的，所以每次操作的时候（也就是调用”<<”或者”>>”）都要刷新（调用flush），这样增加了IO的负担，通过tie(nullptr)来解除std :: cin和std :: cout之间的绑定，来降低IO的负担使效率提升。
- - -
**体会：**
这一道题目，思路想的很快，并且时间复杂度也不高，但是依旧卡了我很久，原因就是链表类的初始化总是忘记，导致系统报错空指针异常。在这个方面很不严谨。同时通过这一个题目，看大佬写的代码，发现了一个c++有趣的事情，就是如何给cin加速，也算是长见识了。


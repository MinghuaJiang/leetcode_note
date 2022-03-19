# LinkedList

* 链表是通过pointer来连接在一起的，无论在哪个节点添加或者删除元素都不需要shift元素，但是访问任意一个节点本身需要O(N), 所以链表适合在两端增加和删除
* 链表分为单向链表和双向链表，双向链表只需要当前节点，就可以做增加节点和删除节点，而单向链表一般需要prev和curr两个node的pointer。
* 单向链表一般有一个head指针，这个head可以是一个dummy，也可以是指向第一个节点的。

{% code title="SinglyLinkedList.java" %}
```java
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode() {}
    public ListNode(int val){
        this.val = val;
    }
    public ListNode(int val, ListNode next){
        this.val = val;
        this.next = next;
    }
}

public class SinglyLinkedList {
    private ListNode head;
    private int size;
    public SinglyLinkedList(){
    }

    public void insertLast(int val){
        ListNode tmp = new ListNode(val);
        if (this.head == null){
            this.head = tmp;
        }else {
            ListNode curr = this.head;
            while (curr.next != null) {
                curr = curr.next;
            }

            curr.next = tmp;
        }

        this.size++;
    }

    public void insertFirst(int val){
        ListNode tmp = new ListNode(val);
        tmp.next = this.head;
        this.head = tmp;

        this.size++;
    }

    public void removeLast(){
        if (this.isEmpty()){
            throw new IllegalStateException();
        }
        
        ListNode curr = this.head;
        ListNode prev = null;
        while(curr.next != null){
            prev = curr;
            curr = curr.next;
        }

        if (prev == null){
            this.head = null;
        }else{
            prev.next = null;
        }

        this.size--;
    }

    public void removeFirst(){
        if (this.isEmpty()){
            throw new IllegalStateException();
        }
        
        this.head = this.head.next;
        this.size--;
    }

    public void insertAtPosition(int index, int val){
        if (index > this.size){
            throw new IllegalArgumentException("invalid index");
        }

        ListNode tmp = new ListNode(val);

        if (this.head == null) {
            this.head = tmp;
        }else {
            ListNode curr = this.head;
            for (int i = 1; i < index; i++) {
                curr = curr.next;
            }

            tmp.next = curr.next;
            curr.next = tmp;
        }

        this.size++;
    }

    public int size(){
        return this.size;
    }

    public boolean isEmpty(){
        return this.head == null;
    }

    public String toString(){
        StringBuilder sb = new StringBuilder();
        ListNode curr = this.head;
        sb.append("[");
        while (curr != null){
            sb.append(curr.val);
            sb.append(",");
            curr = curr.next;
        }

        sb.deleteCharAt(sb.lastIndexOf(","));
        sb.append("]");
        return sb.toString();
    }
}
```
{% endcode %}

{% code title="DoublyLinkedList.java" %}
```java
class DoublyListNode{
    public int val;
    public DoublyListNode prev;
    public DoublyListNode next;
    public DoublyListNode(int val){
        this.val = val;
    }
    public DoublyListNode(int val, DoublyListNode prev, DoublyListNode next){
        this.val = val;
        this.prev = prev;
        this.next = next;
    }
}

public class DoublyLinkedList {
    private DoublyListNode head;
    private DoublyListNode tail;
    private int size;

    public DoublyLinkedList(){
        this.head = new DoublyListNode(-1);
        this.tail = new DoublyListNode(-1);
        this.head.next = tail;
        this.tail.prev = head;
    }

    public void insertLast(int val){
        DoublyListNode tmp = new DoublyListNode(val, this.tail.prev, this.tail);
        this.tail.prev.next = tmp;
        this.tail.prev = tmp;
        this.size++;
    }

    public void insertFirst(int val){
        DoublyListNode tmp = new DoublyListNode(val, this.head, this.head.next);
        this.head.next.prev = tmp;
        this.head.next = tmp;
        this.size++;
    }

    public void removeFirst(){
        if (this.isEmpty()){
            throw new IllegalStateException();
        }

        this.head.next.next.prev = this.head;
        this.head.next = this.head.next.next;
    }

    public void removeLast(){
        if (this.isEmpty()){
            throw new IllegalStateException();
        }

        this.tail.prev.prev.next = this.tail;
        this.tail.prev = this.tail.prev.prev;
    }

    public int getSize(){
        return size;
    }

    public boolean isEmpty(){
        return this.head.next == this.tail;
    }

    public String toString(){
        StringBuilder sb = new StringBuilder();
        DoublyListNode curr = this.head.next;
        sb.append("[");
        while (curr != tail){
            sb.append(curr.val);
            sb.append(",");
            curr = curr.next;
        }

        sb.deleteCharAt(sb.lastIndexOf(","));
        sb.append("]");
        return sb.toString();
    }

    public static void main(String[] args) {
        DoublyLinkedList list = new DoublyLinkedList();
        list.insertLast(1);
        list.insertFirst(0);
        list.insertLast(4);
        list.insertLast(5);
        list.removeLast();
        list.removeFirst();
        System.out.println(list);
        System.out.println(list.getSize());
    }
}
```
{% endcode %}

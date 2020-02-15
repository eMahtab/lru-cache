# LRU Cache
## https://leetcode.com/problems/lru-cache


## Implementation :

```java
/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */

class LRUCache {
    
    Map<Integer, ListNode> hashtable = new HashMap<Integer, ListNode>();
    ListNode head;
    ListNode tail;
    int totalItemsInCache;
    int maxCapacity;
    
    private class ListNode {
      int key;
      int value;
      ListNode prev;
      ListNode next;
    }

    public LRUCache(int capacity) {
      this.totalItemsInCache = 0;
      this.maxCapacity = capacity;
      head = new ListNode();
      tail = new ListNode();
      head.next = tail;
      tail.prev = head;
    }
    
    public int get(int key) {
      ListNode node = hashtable.get(key);
      if (node == null) 
         return -1;

      moveToHead(node);
      return node.value;  
    }
    
    private void moveToHead(ListNode node) {
      removeFromList(node);
      addToFront(node);
    }
    
    private void removeFromList(ListNode node) {
      ListNode savedPrev = node.prev;
      ListNode savedNext = node.next;

      savedPrev.next = savedNext;
      savedNext.prev = savedPrev;
    }
    
    private void addToFront(ListNode node) {
      // Wire up the new node being to be inserted
      node.prev = head;
      node.next = head.next;
      head.next.prev = node;
      head.next = node;
    }
    
    public void put(int key, int value) {
      ListNode node = hashtable.get(key);

      if (node == null) {
        ListNode newNode = new ListNode();
        newNode.key = key;
        newNode.value = value;
          
        hashtable.put(key, newNode);
        addToFront(newNode);
        totalItemsInCache++;

        if (totalItemsInCache > maxCapacity) {
          removeLRUEntry();
        }
      } else {
        node.value = value;
        moveToHead(node);
      }
    }
    
    private void removeLRUEntry() {
      ListNode tail = popTail();

      hashtable.remove(tail.key);
      --totalItemsInCache;
    }

    private ListNode popTail() {
      ListNode tailItem = tail.prev;
      removeFromList(tailItem);

      return tailItem;
    }
}

```

# References :
1. https://www.youtube.com/watch?v=S6IfqDXWa10
2. https://github.com/bephrem1/backtobackswe/blob/master/Linked%20Lists/LRUCache/LRUCache.java

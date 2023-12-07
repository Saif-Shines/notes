# 4 Semesters of CS in 5 hrs

[Four Semesters of Computer Science in Six Hours](http://btholt.github.io/four-semesters-of-cs/)

# Big O & Recursion

All that I already know.

One useful thing is, Big O in this section talks about the number of iterations only. Not Space or Time complexity.

# Bubble sort

```jsx
/*
  Write a bubble sort here
  Name the function bubbleSort
  
  If you want to suspend running the unit tests, change describe to xdescribe
  
  Bubble sort works by comparing two adjacent numbers next to each other and then
  swapping their places if the smaller index's value is larger than the larger
  index's. Continue looping through until all values are in ascending order
  
  Provided is an optional visualization helper. Call snapshot(yourArray) at the
  beginning of each i of your inner loop with the state of the being-sorted
  array and the helper tool will show you how your array looks in the HTML section
  of CodePen. This is optional and only for your help.
  
*/

function bubbleSort (nums) {
  for(i = 0; i < nums.length; i++){
    for(let n = 0; n < nums.length; n++){
     snapshot(nums);
     if(nums[n] > nums[n+1]){
       [nums[n], nums[n+1]] = [nums[n+1], nums[n]];
     } 
   } 
  } 
}

// unit tests
// do not modify the below code
describe('bubble sort', function() {
  it('should sort correctly', () => {
    var nums = [10,5,3,8,2,6,4,7,9,1];
    bubbleSort(nums);
    expect(nums).toEqual([1,2,3,4,5,6,7,8,9,10]);
    done();
  });
});
```

# Insertion Sort

[Know Thy Complexities!](http://www.bigocheatsheet.com)

# Merge Sort

```jsx
var nums = [10, 5, 3, 8, 2, 6, 4, 7, 9, 1];

function mergeSort(nums) {
  if (nums.length < 2) {
    return nums;
  }

  const length = nums.length;
  const middle = Math.floor(length / 2);
  const left = nums.slice(0, middle);
  const right = nums.slice(middle, length);

  const sortedLeft = mergeSort(left);
  const sortedRight = mergeSort(right);

  return stitch(sortedLeft, sortedRight);
}

function stitch(left, right) {
  const results = [];

  while (left.length && right.length) {
    if (left[0] <= right[0]) {
      results.push(left.shift());
    } else {
      results.push(right.shift());
    }
  }

  return [...results, ...left, ...right];
}

mergeSort(nums);
```

<aside>
💡 You are never going to write merge sort or any algorithm in your entire career. What is useful about algorithms is that; you get the thought process wound up and ability to deconstruct and use them.

</aside>

So backing up the above call out,

1. Take an example problem: Find the median of these two arrays
    1. [1,3,8,9]
    2. [2,3,7,10]
2. The naive way of solving it can be, 
    1. Sorting the two arrays.
    2. Then finding middle index and picking up the element on that index.
3. But from the merge sort that we have learnt so far, on *deconstruction* we can absorb that, this problem is similar to the `stitch` method.
    1. You can compare them on the fly with if condition and push them to the new array without having to sortthem individually.
    2. Once you try to push them, you don't have to push all the elements at all, you need to push only 5th element because median simply means that. 
    
    # LinkedList
    
    ```jsx
    /*
      LinkedList
      
      Name your class / constructor (something you can call new on) LinkedList
      
      LinkedList is made by making nodes that have two properties, the value that's being stored and a pointer to
      the next node in the list. The LinkedList then keep track of the head and usually the tail (I would suggest
      keeping track of the tail because it makes pop really easy.) As you may have notice, the unit tests are the
      same as the ArrayList; the interface of the two are exactly the same and should make no difference to the
      consumer of the data structure.
      
      I would suggest making a second class, a Node class. However that's up to you how you implement it. A Node
      has two properties, value and next.
      
      length - integer  - How many elements in the list
      push   - function - accepts a value and adds to the end of the list
      pop    - function - removes the last value in the list and returns it
      get    - function - accepts an index and returns the value at that position
      delete - function - accepts an index, removes value from list, collapses, 
                          and returns removed value
    
      As always, you can change describe to xdescribe to prevent the unit tests from running while
      you work
    */
    
    class Node {
      constructor(v) {
        this.value = v;
        this.next = null;
      }
    }
    
    class LinkedList {
      constructor() {
        this.head = null;
        this.length = 0;
        this.tail = null;
      }
    
      push(value) {
        const node = new Node(value);
        this.length++;
        if(!this.head){
          this.head = node;
        }
        else{
          this.tail.next = node;
        }
        this.tail = node;
      }
    
      pop() {
        return this.delete(this.length-1);  
      }
      
      
      _find(value, test = this._test) {
        let current = this.head;
        let i = 0;
        while(current){
          if(test(value,current.value,i,current)){
            return current;
          }
          current = current.next;
          i++;
        }
        return null;
      }
    
      _test(a, b) {
        return a == b;
      }
      
      _testIndex(search, __, i) {
        return search === i;  
      }
      
      get(index) {
        const node = this._find(index,this._testIndex);
        if(!node) return null;
        return node.value;
      }
      
      delete(index) {
        console.log(index);
        if(index === 0 ){
          const head = this.head;
          if(head){
            this.head = head.next;
          }
          else{
            this.tail = this.head = null;
          }
          this.length--;
          return head.value;
        }
        const node = this._find(index-1, this._testIndex);
        const excise = node.next;
        if(!excise) return null;
        node.next = excise.next;
        if(node.next && !node.next.next) this.tail = node.next;
        this.length--;
        return excise.value;
      
      }
    
    }
    
    // unit tests
    // do not modify the below code
    describe('LinkedList', function() {
      const range = length => Array.apply(null, {length: length}).map(Number.call, Number);
      const abcRange = length => range(length).map( num => String.fromCharCode( 97 + num ) );
      let list;
      
      beforeEach( () => {
        list = new LinkedList();
      })
      
      it('constructor', () => {
        expect(list).toEqual(jasmine.any(LinkedList));
      });
      
      it('push', () => {
        abcRange(26).map( character => list.push(character) );
        expect(list.length).toEqual(26);
      });
      
      it('pop', () => {
        abcRange(13).map( character => list.push(character) );
        expect(list.length).toEqual(13);
        range(10).map( () => list.pop() );
        expect(list.length).toEqual(3);
        expect(list.pop()).toEqual('c');
      });
      
      it('get', () => {
        list.push('first');
        expect(list.get(0)).toEqual('first');
        list.push('second');
        expect(list.get(1)).toEqual('second');
        expect(list.get(0)).toEqual('first');
        abcRange(26).map( character => list.push(character));
        expect(list.get(27)).toEqual('z');
        expect(list.get(0)).toEqual('first');
        expect(list.get(9)).toEqual('h');
        list.pop();
        expect(list.get(list.length-1)).toEqual('y');
      });
      
      it('delete', () => {
        abcRange(26).map( character => list.push(character) );
        list.delete(13);
        expect(list.length).toEqual(25);
        expect(list.get(12)).toEqual('m');
        expect(list.get(13)).toEqual('o');
        list.delete(0);
        expect(list.length).toEqual(24);
        expect(list.get(0)).toEqual('b');
      });
      
    });
    ```
    
    # Binary tree
    
    ```jsx
    /*
    
    Binary Search Tree!
    
    Name your class Tree. 
    
    I'd suggest making another class called Node. You don't have to; you can make them all plain JS objects
    
    Here you'll make a BST. Your Tree class will have keep track of a root which will be the first item added
    to your tree. From there, if the item is less than the value of that node, it will go into its left subtree
    and if greater it will go to the right subtree.
    
    There is a tree visualization helper. It will tell show you how your tree looks as you create it. In order
    for this to work and for the unit tests to pass you, you must implement a Tree .toObject function that returns
    your tree as a series of nested objects. Those nested objects must use the following names for their properties
    
    value - integer     - value being contained in the node
    left  - Node/object - the left node which itself may be another tree
    right - Node/object - the right node which itself may be another tree
    
    As always, you can change describe to xdescribe to prevent the unit tests from running
    
    */
    
    class Node{
      constructor(value, left=null,right=null){
        this.value = value;
        this.left = left;
        this.right = right;
      }
    }
    
    class Tree{
      constructor(){
        this.root = null;
        
      }
      toObject(){
        return this.root;
      }
      add(value){
        if(!this.root){
          this.root = new Node(value);
          return;
        }
        
        let current = this.root;
        while(true){
          if(current.value > value){
            // go left
            if(current.left){
              current = current.left;
            }
            else{
              current.left = new Node(value);
              break;
            }
          } else{
            // go right
            if(current.right){
              current = current.right;
            }
            else{
              current.right = new Node(value);
              break;
            }
          }
        }
      }
      
    }
    
    // unit tests
    // do not modify the below code
    describe('Binary Search Tree', function() {
      it('creates a correct tree', () => {
        const nums = [3,7,4,6,5,1,10,2,9,8];
        const tree = new Tree();
        nums.map( num => tree.add(num));
        const objs = tree.toObject();
        render(objs, nums);
    
        expect(objs.value).toEqual(3);
        
        expect(objs.left.value).toEqual(1);
        expect(objs.left.left).toBeNull();
        
        expect(objs.left.right.value).toEqual(2);
        expect(objs.left.right.left).toBeNull();
        expect(objs.left.right.right).toBeNull();
        
        expect(objs.right.value).toEqual(7);
        
        expect(objs.right.left.value).toEqual(4);
        expect(objs.right.left.left).toBeNull();
        
        expect(objs.right.left.right.value).toEqual(6);
        expect(objs.right.left.right.left.value).toEqual(5);
        expect(objs.right.left.right.left.right).toBeNull();
        expect(objs.right.left.right.left.left).toBeNull();
        
        expect(objs.right.right.value).toEqual(10);
        expect(objs.right.right.right).toBeNull();
        
        expect(objs.right.right.left.value).toEqual(9);
        expect(objs.right.right.left.right).toBeNull();
        
        expect(objs.right.right.left.left.value).toEqual(8);
        expect(objs.right.right.left.left.right).toBeNull();
        expect(objs.right.right.left.left.left).toBeNull();
      });
    });
    ```
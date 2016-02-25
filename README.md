# cs-implementing-a-linkedlist-lab


## Objectives

1.  Write an implementation of a LinkedList.


## Implementing the List interface with a linked list

For this lesson we provide a partial implementation of the List interface that uses a linked list to store the elements.  We left xxxx of the methods incomplete; your job is to fill them in.  We provide JUnit tests you can use to check your work.

If you are not familiar with linked lists, here is a brief introduction.  A data structure is "linked" if it is made up of objects, often called "nodes", that contain references to other nodes.  In a linked *list*, each node contains a reference to the next node in the list.  Other linked structures include trees and graphs, in which nodes can contain references to more than one other node.

A linked list is use to store a sequence of elements, so each node contains a reference to an element, or sometimes to a collection of elements.  The element part of the node is sometimes called "cargo", so you can think of nodes as rail cars, where each car contains cargo and a connection to the next car.

Let's see what this looks like in code.  Here's a class definition for a simple Node:

```java
public class ListNode {

	public Object cargo;
	public ListNode next;

	public ListNode() {
		this.cargo = null;
		this.next = null;
	}
	
	public ListNode(Object cargo) {
		this.cargo = cargo;
		this.next = null;
	}
	
	public ListNode(Object cargo, ListNode next) {
		this.cargo = cargo;
		this.next = next;
	}
	
	public String toString() {
		return "ListNode(" + cargo.toString() + ")";
	}
}
```

Each Node has two instance variables: `cargo` is a reference to some kind of Java Object, and `next` is a reference to the next Node in the list.  In the last Node in the list, by convention, `next` is `null`.

Node provides several constructors, allowing you to provide values for `cargo` and `next`, or initializing them to the default value, `null`.

You can think of each Node as a list with a single element, but more generally, a list can contain any number of Nodes.  There are several ways to make a new list.  A simple way is to create a set of Nodes, like this:

```java
		ListNode node1 = new ListNode(1);
		ListNode node2 = new ListNode(2);
		ListNode node3 = new ListNode(3);
```

And then link them up, like this:

```java
		node1.next = node2;
		node2.next = node3;
		node3.next = null;
```

Alternatively, you can create a Node and link it at the same time.  For example, if you want to add a new Node at the beginning of a list, you can do it like this:

```java
		ListNode node0 = new ListNode(0, node1);
```

After this sequence of instructions, we have four Nodes containing the Integers 0, 1, 2, and 3 as cargo, linked up in increasing order.  In the code that has 3 as `cargo`, the `next` field is `null`.

TODO: possibly a diagram here


## Instructions


*   When you check out the repository for this lab, you should find a file structure similar to what you saw in previous labs.  The top level directory contains `CONTRIBUTING.md`, `LICENSE.md`, `README.md`, and the directory that contains the code for this lab, `javacs-lab03`.

    In the subdirectory `javacs-lab02/src/com/flatironschool/javacs` you'll find the source files you need for this lab:

    *  `MyLinkedList.java` contains a partial implementation of the `List` interface using a linked list to store the elements.

    *  `MyArrayListTest.java` contains JUnit tests for `MyArrayList`.

    In `javacs-lab03`, you'll find the Ant build file `build.xml`.  If you are in this directory, you should be able to run `ant MyArrayList` to run `MyArrayList.java`, which contains a few simple tests.  Or you can run `ant test` to run the JUnit test.
    
    When you run the tests, xxx out of 19 should fail.  If you examine the source code, you'll find four `TODO` comments indicating which methods you will fill in.

*   Before you start filling in the missing methods, let's walk through some of the code.  Here are the instance variables and the constructor. 

```java
public class MyLinkedList<E> implements List<E> {

	private int size;            // keeps track of the number of elements
	private Node head;           // reference to the first node
	
	public MyLinkedListSoln() {
		head = null;
		size = 0;
	}
}
```

As the comments indicate, `size` keeps track of how many elements are in `MyLinkedList`, and `head` is a reference to the first Node in the list, or `null` if the list is empty.

Storing the number of elements is not necessary, and in general it is risky to keep redundant information, because updating it takes time, and if it's not updated correctly, it creates opportunities for error.  But it might be worth it in this case because it makes it faster to get the length of the list.  If we store `size` explicitly, we can implement the `size` method in constant time.  Otherwise, we have to travese the list and count the elements, which requires linear time.  

If we store `size` explicitly, we have to update it each time we add or remove an element, so that slows down those methods a little, but it doesn't change their order of growth, so it's probably worth it.

The constructor sets `head` to `null`, which indicates an empty list, and sets `size` to 0.

This class uses the type parameter `E` for the type of the cargo.  This type parameter appears in the definition of Node, which is nested inside `MyLinkedList`:

```java
	private class Node {
		public E cargo;
		public Node next;
		
		public Node(E cargo, Node next) {
			this.cargo = cargo;
			this.next = next;
		}
	}
```

At this point you should have a good idea how this implementation of linked lists works.
But before you get started on the exercises, we'll look at one more method.  Here's my implementation of `add`:

```java
	public boolean add(T e) {
		if (head == null) {
			head = new Node(e);
		} else {
			Node node = head;
			for ( ; node.next != null; node = node.next) {
				// find the last node
			}
			node.next = new Node(e);
		}
		size++;
		return true;
	}
```
This example demonstrates two patterns you'll need for your solutions:

1.  For many methods, we have to handle the first element of the list as a special case.  In this example, if we are adding the first element of a list, we have to modify `head`.  Otherwise, we traverse the list, find the end, and add the new node.

2.  This method shows how to use a `for` loop to traverse the Nodes in a list.  In your solutions, you will probably write several variations on this loop.  Notice that we had to declare `node` before the loop so we can access it after the loop.


*  Now it's your turn: fill in the body of `indexOf`.  As usual, you should [read the documentation](https://docs.oracle.com/javase/7/docs/api/java/util/List.html#indexOf(java.lang.Object)) so you know what it's supposed to do.  In particular, notice how it is supposed to handle `null`.

To make things a little easier, I've provided a helper method called `equals` that compares an element from the array to a target value and returns `true` if they are equal (and it handles `null` correctly).  This method is private because it is used inside this class but it is not part of the `List` interface.

When you are done, run `ant test` again; `testIndexOf` should pass now, as well as the other tests that depend on it.

*   Only two more methods to go, and you'll be done with this lab.  The next one is the two-parameter version of `add` that takes an index and stores the new value at the given index, shifting the other elements if necessary.

Again, [read the documentation](https://docs.oracle.com/javase/7/docs/api/java/util/List.html#add(int,%20E)), write an implementation, and run the tests for confirmation.


*  Last one: fill in the body of `remove`.  [The documentation is here](https://docs.oracle.com/javase/7/docs/api/java/util/List.html#remove(int)).  When you finish this one, all tests should pass.


*  Once you have your implementation working, compare it to mine, which you can find by checking out the solutions branch of the repo, or [you can read it on GitHub](https://TODO: add_this_later).



# A quick note on garbage collection

In `MyArrayList`, the array grows if necessary, but it never shrinks.  The array never gets garbage collected, and in some cases the elements don't get garbage collected, until the list itself is destroyed.

One advantage of the linked list implementation is that it shrinks when elements are removed, and the unused Nodes get garbage collected. 

Here is my implementation of the `clear` method:

```java
	public void clear() {
		head = null;
		size = 0;
	}
```

When we set `head` to clear, we remove a reference to the first Node.  If there are no other references to that Node (and there shouldn't be), it will get garbage collected.  At that point, the reference to the second Node is removed, so it gets garbage collected, too.  This process continues until all Nodes are collected.

So how should we classify `clear`?  The method itself contains two constant time operations, so it sure looks like it's constant time.  But when you invoke it, you make the garbage collector do work that's proportional to the number of elements.  So there's an argument that we should consider it linear!

This is a subtle example of what is sometimes called a "performance bug"; that is, a program that is correct in the sense that it does the right thing, but it doesn't belong to the order of growth we expected.  In languages like Java that do a lot of work, like garbage collection, behind the scenese, this kind of error can be hard to find.


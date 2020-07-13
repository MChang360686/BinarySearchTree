# BinarySearchTree
/*Create a Binary Search tree capable of storing, searching for, and returning integer values.
Initially created for Case Western Reserve University EECS 233 Intro to Data Structures
during Summer 2020 semester in Java.*/

import java.io.*;

public class BinarySearchTree {
    Node root;

    public class Node{
        int key;                       //Node will have variables for a key(its data), an int count
        int count;                     //for the kth smallest element, and its left and right children
        Node leftChild, rightChild;


        public Node(int item){         //Initialize the node; it receives an item, count = 0,
            count = 0;                 //and in the beginning, the left and right children are null
            key = item;
            leftChild = rightChild = null;
        }

    }



    BinarySearchTree(){         //Root will be set to null when the BST is created
        root = null;
    }



    public void insert(int key){                //When inserting, we receive an integer
        root = insertRecursively(root, key);    //We insert recursively by calling insertRecursively
    }                                           //and giving it a root and key



    public Node insertRecursively(Node root,int key){
        if (root == null) {
            root = new Node(key);                   //If the root is empty, make the key
            return root;                            //the new root and return
        }


        if (key < root.key) {
            root.leftChild = insertRecursively(root.leftChild, key);  //If key is less than the current root
        } else if (key > root.key) {                                  //it becomes leftChild
            root.rightChild = insertRecursively(root.rightChild, key);//Else if greater become rightChild
        }

        return root;                                    //return root to recursively insert
    }



    public Node delete(Node root, int key){         //Receive tree's root and key to delete
        if(root == null) {                          //If root is null, then the tree is empty
            return null;
        }

        if(key < root.key) {                        //if the key we are looking for is less than root
            root.leftChild = delete(root.leftChild, key);//recurse the root's left child
        }

        else if(key > root.key){                    //otherwise, the key is greater than the root
            root.rightChild = delete(root.rightChild, key);//recurse with root.rightChild

        }else{

            if(root.leftChild == null){          //if the left child is null(end of left subtree)
                return root.rightChild;             //check the right subtree for the key to delete

            }else

                if(root.rightChild == null) {      //or if the right child is null, check left subtree
                    return root.leftChild;

            }
            root.key = smallestValue(root.rightChild);//Use this method if the "root" node in question
            //has two children; will return the smallest item in the RIGHT subtree
            root.rightChild = delete(root.rightChild, root.key);

        }

        return root;            //recursively check root for the key to be deleted

    }


    public int smallestValue(Node root){    //This method's purpose is to find the inorder successor
        int minValue = root.key;            //to a root with two children

        while(root.leftChild != null){      //Finds the smallest value on the assumption that
            minValue = root.leftChild.key;  //the smallest value in a tree is the leftmost
            root = root.leftChild;          //value
        }
        return minValue;                    //recursively check the keys until root == null
    }


    public void inOrderRec(Node root){
        if(root != null){                  //and while the root is not null,
            inOrderRec(root.leftChild);    //print the left node
            System.out.print(root.key + " ");
            inOrderRec(root.rightChild);   //print the right node, and then root
        }
    }



    public Node search(Node root, int key){
        if(root == null){
            System.out.println("There is no tree");//Maybe the tree doesn't exist
            return null;
        }

        if(root.key == key) {                       //if the root has the key
            System.out.println(root.key + " is in the tree");
            return root;

        }
        if(root.key > key)                          //if key < root.key, check left subtree recursively
            return search(root.leftChild, key);
        return search(root.rightChild, key);        //otherwise check the right subtree recursively

    }



    public static int sum(Node root){
        if(root == null)                    //If there is no tree, sum = 0
            return 0;

        return(root.key + sum(root.leftChild) + sum(root.rightChild));
    }       //add the keys of the root to the addNodes values of the left and right children



    public static int kthSmallest(Node root, int k) {
        int count = 0;                              //Count will keep track of all the times
        int kSmallest = Integer.MIN_VALUE;          //we iterate through the tree, and current
        Node current = root;                        //stores the node. kSmallest stores the value of k

        while(current != null){                     //While the current node is not null, if there
            if(current.leftChild == null){          //is no left child, increase count
                count++;
                if(count == k){                     //if count reaches k, we have found the kth
                    kSmallest = current.key;        //smallest element, and we should store it
                }
                current = current.rightChild;       //check the right child if count != k
            }
            else{
                Node precursor = current.leftChild; //else find the inOrder successor to current
                while(precursor.rightChild != null && precursor.rightChild != current){
                    precursor = precursor.rightChild;//Check the right subtree again
                }
                if(precursor.rightChild == null){   //If the right subtree does not work
                    precursor.rightChild = current; //check the left subtree of the right subtree
                    current = current.leftChild;
                }else{
                    precursor.rightChild = null;    //if we've reached the end of the line
                    count++;                        //increment count again
                    if(count == k){                 //check if count is equal to k
                        kSmallest = current.key;    //if yes, we have a value that matches the description
                    }
                    //Change the value of current to check right subtree
                    current = current.rightChild;   
                }
            }
        }
        return kSmallest;           
        //return the found value for kSmallest if it exists
    }

    public static void main(String[] args){
        BinarySearchTree treeTwo = new BinarySearchTree();
        treeTwo.insert(5);
        treeTwo.insert(23);
        treeTwo.inOrderRec(treeTwo.root);
        treeTwo.delete(treeTwo.root,5);
        treeTwo.insert(6);
        treeTwo.insert(33);
        treeTwo.search(treeTwo.root, 23);
        System.out.println(sum(treeTwo.root));
        System.out.println(kthSmallest(treeTwo.root,2));
    }
}

//Additional test file to evaluate BinarySearchTree

public class Test extends BinarySearchTree{
    public static void main(String[] args){
        BinarySearchTree tree = new BinarySearchTree();
        tree.search(tree.root,666);
        tree.insert(5);
        tree.insert(1);
        tree.insert(76);
        tree.insert(357);
        tree.insert(4);
        tree.insert(666);
        tree.insert(10);
        tree.insert(12);
        tree.insert(44);
        tree.inOrderRec(tree.root);
        System.out.println();
        tree.delete(tree.root,1);
        tree.delete(tree.root,76);
        tree.delete(tree.root,666);
        tree.inOrderRec(tree.root);
        System.out.println();
        tree.search(tree.root, 5);
        tree.search(tree.root, 44);
        System.out.println(sum(tree.root));
        System.out.println(kthSmallest(tree.root, 3));
    }
}


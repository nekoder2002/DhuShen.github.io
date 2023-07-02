---
title: 二叉树
img: http://rx4hz3911.hd-bkt.clouddn.com/b923e97ab2c0f2bd0053b23a16d92dcf.jpg
date: 2023-07-02 17:30
summary: 使用Java实现二叉树
categories: 算法分析
tags:
  - 二叉树
  - 数据结构
  - Java
---
## 二叉树介绍
二叉树是n(n>=0)个结点的有限集合，该集合或者为空集（称为空二叉树），或者由一个根结点和两棵互不相交的、分别称为根结点的左子树和右子树组成。

下图展示了一棵普通二叉树
![](http://rx4hz3911.hd-bkt.clouddn.com/2-1Q226195949495.gif)

## 代码实现
```
public class BinaryTree<T> {
    private final TreeNode<T> root;

    private Integer HEIGHT;

    private Integer MAXBRANCH;

    public BinaryTree(T[] arr) {
        MAXBRANCH = 0;
        root = buildTree(Arrays.stream(arr).iterator());
        MAXBRANCH = MAXBRANCH % 2 == 0 ? MAXBRANCH : MAXBRANCH + 1;
    }

    public BinaryTree(TreeNode<T> root) {
        this.root = root;
    }

    public TreeNode<T> getRoot() {
        return root;
    }

    public List<T> preTraverse() {
        List<T> list = new ArrayList<>();
        ArrayDeque<TreeNode<T>> stack = new ArrayDeque<>();
        TreeNode<T> ptr = root;
        while (!stack.isEmpty() || ptr != null) {
            if (ptr != null) {
                list.add(ptr.val);
                stack.push(ptr);
                ptr = ptr.left;
            } else {
                ptr = stack.pop();
                ptr = ptr.right;
            }
        }
        return list;
    }

    public List<T> midTraverse() {
        List<T> list = new ArrayList<>();
        ArrayDeque<TreeNode<T>> stack = new ArrayDeque<>();
        TreeNode<T> ptr = root;
        while (!stack.isEmpty() || ptr != null) {
            if (ptr != null) {
                stack.push(ptr);
                ptr = ptr.left;
            } else {
                ptr = stack.pop();
                list.add(ptr.val);
                ptr = ptr.right;
            }
        }
        return list;
    }

    public List<T> postTraverse() {
        List<T> list = new ArrayList<>();
        ArrayDeque<TreeNode<T>> stack = new ArrayDeque<>();
        TreeNode<T> ptr = root;
        TreeNode<T> pre = null;
        while (!stack.isEmpty() || ptr != null) {
            if (ptr != null) {
                if (ptr.right == pre || ptr.left == pre && ptr.right == null) {
                    //完全访问，出栈
                    pre = stack.pop();
                    ptr = stack.peek();
                    list.add(pre.val);
                } else if (!stack.isEmpty() && ptr.left == pre) {
                    //左子树完全访问
                    pre = ptr;
                    ptr = stack.peek().right;
                } else {
                    pre = ptr;
                    ptr = ptr.left;
                    stack.push(pre);
                }
            } else {
                ptr = stack.peek();
                pre = null;
            }
        }
        return list;
    }

    public int getHeight() {
        if (HEIGHT == null) {
            HEIGHT = getHeight(root);
        }
        return HEIGHT;
    }

    @Override
    public String toString() {
        //枝距离
        int width = (int) Math.pow(2, getHeight() - 1) * getMaxBranch();
        //所需空行
        int block = ((int) Math.pow(2, getHeight()) - 1) * getMaxBranch();
        int layer = 0;
        StringBuilder builder = new StringBuilder();
        ArrayDeque<TreeNode<T>> printQueue = new ArrayDeque<>();
        ArrayDeque<TreeNode<T>> branchQueue = new ArrayDeque<>();
        if (root != null) {
            printQueue.offer(root);
        }
        while (layer < getHeight()) {
            builder.append(" ".repeat(block));
            int size = printQueue.size();
            for (int i = 0; i < size; i += 1) {
                TreeNode<T> node = printQueue.poll();
                if (node != null && node.val != null) {
                    builder.append(node.val);
                    branchQueue.offer(Objects.requireNonNullElseGet(node.left, TreeNode::nullNode));
                    branchQueue.offer(Objects.requireNonNullElseGet(node.right, TreeNode::nullNode));
                } else {
                    branchQueue.offer(TreeNode.nullNode());
                    branchQueue.offer(TreeNode.nullNode());
                }
                builder.append(" ".repeat(width * 4 - (node == null || node.val == null ? 0 : node.val.toString().length())));
            }
            if (block >= width) {
                size = branchQueue.size();
                block -= width;
                builder.append('\n');
                builder.append(" ".repeat(block));
                for (int i = 0; i < size; i += 2) {
                    TreeNode<T> leftNode = branchQueue.poll();
                    TreeNode<T> rightNode = branchQueue.poll();
                    boolean flag1 = leftNode != null && leftNode.val != null;
                    boolean flag2 = rightNode != null && rightNode.val != null;
                    if (flag1) {
                        builder.append('+');
                        builder.append("-".repeat(width - 1));
                        printQueue.offer(leftNode);
                    } else {
                        builder.append(" ".repeat(width));
                        printQueue.offer(TreeNode.nullNode());
                    }
                    if (flag1 || flag2) {
                        builder.append('+');
                    } else {
                        builder.append(' ');
                    }
                    if (flag2) {
                        builder.append("-".repeat(width - 1));
                        builder.append('+');
                        printQueue.offer(rightNode);
                    } else {
                        builder.append(" ".repeat(width));
                        printQueue.offer(TreeNode.nullNode());
                    }
                    builder.append(" ".repeat(width * 2 - 1));
                }
                builder.append('\n');
                width = width / 2;
            }
            layer++;
        }
        return builder.toString();
    }

    private int getMaxBranch() {
        if (MAXBRANCH == null) {
            MAXBRANCH = getMaxBranch(root);
        }
        MAXBRANCH = MAXBRANCH % 2 == 0 ? MAXBRANCH : MAXBRANCH + 1;
        return MAXBRANCH;
    }

    private TreeNode<T> buildTree(Iterator<T> iterator) {
        if (iterator.hasNext()) {
            T value = iterator.next();
            if (value == null) {
                return null;
            }
            TreeNode<T> node = new TreeNode<>();
            node.val = value;
            MAXBRANCH = Math.max(MAXBRANCH, value.toString().length());
            node.left = buildTree(iterator);
            node.right = buildTree(iterator);
            return node;
        } else {
            throw new RuntimeException("wrong build");
        }
    }

    private int getMaxBranch(TreeNode<T> node) {
        if (node == null) {
            return 0;
        }
        return Math.max(node.val.toString().length(), Math.max(getMaxBranch(node.left), getMaxBranch(node.right)));
    }

    private int getHeight(TreeNode<T> node) {
        if (node == null) {
            return 0;
        }
        return Math.max(getHeight(node.left), getHeight(node.right)) + 1;
    }
}
```


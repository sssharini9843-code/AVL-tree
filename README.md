class Node:
def __init__(self, key):
self.key, self.left, self.right, self.height = key, None, None, 1

class AVLTree:
def get_height(self, node): return node.height if node else 0
def get_balance(self, node): return self.get_height(node.left) - self.get_height(node.right) if node else 0

def right_rotate(self, y):
x, T2 = y.left, y.left.right
x.right, y.left = y, T2
y.height = 1 + max(self.get_height(y.left), self.get_height(y.right))
x.height = 1 + max(self.get_height(x.left), self.get_height(x.right))
return x

def left_rotate(self, x):
y, T2 = x.right, x.right.left
y.left, x.right = x, T2
x.height = 1 + max(self.get_height(x.left), self.get_height(x.right))
y.height = 1 + max(self.get_height(y.left), self.get_height(y.right))
return y

def insert(self, root, key):
if not root: return Node(key)
if key < root.key: root.left = self.insert(root.left, key)
elif key > root.key: root.right = self.insert(root.right, key)
else: return root  # No duplicates

root.height = 1 + max(self.get_height(root.left), self.get_height(root.right))
balance = self.get_balance(root)

# Balancing logic
if balance > 1 and key < root.left.key: return self.right_rotate(root)
if balance < -1 and key > root.right.key: return self.left_rotate(root)
if balance > 1 and key > root.left.key:
root.left = self.left_rotate(root.left)
return self.right_rotate(root)
if balance < -1 and key < root.right.key:
root.right = self.right_rotate(root.right)
return self.left_rotate(root)
return root

def min_value_node(self, node):
while node.left: node = node.left
return node

def delete(self, root, key):
if not root: return root
if key < root.key: root.left = self.delete(root.left, key)
elif key > root.key: root.right = self.delete(root.right, key)
else:
if not root.left: return root.right
elif not root.right: return root.left
temp = self.min_value_node(root.right)
root.key, root.right = temp.key, self.delete(root.right, temp.key)

root.height = 1 + max(self.get_height(root.left), self.get_height(root.right))
balance = self.get_balance(root)

if balance > 1:
if self.get_balance(root.left) >= 0: return self.right_rotate(root)
root.left = self.left_rotate(root.left)
return self.right_rotate(root)
if balance < -1:
if self.get_balance(root.right) <= 0: return self.left_rotate(root)
root.right = self.right_rotate(root.right)
return self.left_rotate(root)
return root

def inorder(self, root):
if root:
self.inorder(root.left)
print(f"{root.key}(h:{root.height})", end=" ")
self.inorder(root.right)

# Real-time Driver Code
if __name__ == "__main__":
avl, root = AVLTree(), None
for k in [10, 20, 30, 40, 50, 25]: root = avl.insert(root, k)
print("Inorder after inserts: ", end=""); avl.inorder(root)
root = avl.delete(root, 30)
print("\nInorder after deleting 30: ", end=""); avl.inorder(root)


output:
Inorder after inserts: 10(h:1) 20(h:2) 25(h:1) 30(h:3) 40(h:1) 50(h:2)
Inorder after deleting 30: 10(h:1) 20(h:2) 25(h:1) 40(h:3) 50(h:1)


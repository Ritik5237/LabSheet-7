# LabSheet-7
#include <iostream>
#include <string>
using namespace std;

struct Node {
    string region;
    Node* left;
    Node* right;
    int height;
};

int getHeight(Node* node) {
    return node == nullptr ? 0 : node->height;
}

int getBalanceFactor(Node* node) {
    return node == nullptr ? 0 : getHeight(node->left) - getHeight(node->right);
}

Node* createNode(const string& region) {
    Node* newNode = new Node();
    newNode->region = region;
    newNode->left = nullptr;
    newNode->right = nullptr;
    newNode->height = 1;
    return newNode;
}

Node* rotateRight(Node* y) {
    Node* x = y->left;
    Node* T = x->right;

  x->right = y;
  y->left = T;

  y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
  x->height = max(getHeight(x->left), getHeight(x->right)) + 1;

  return x;
}

Node* rotateLeft(Node* x) {
    Node* y = x->right;
    Node* T = y->left;

  y->left = x;
  x->right = T;

   x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
   y->height = max(getHeight(y->left), getHeight(y->right)) + 1;

   return y;
}

Node* insert(Node* node, const string& region) {
    if (node == nullptr) 
        return createNode(region);
    if (region < node->region)
        node->left = insert(node->left, region);
    else if (region > node->region)
        node->right = insert(node->right, region);
    else
        return node;

   node->height = 1 + max(getHeight(node->left), getHeight(node->right));
   int balance = getBalanceFactor(node);

   if (balance > 1 && region < node->left->region)
        return rotateRight(node);

   if (balance < -1 && region > node->right->region)
        return rotateLeft(node);

   if (balance > 1 && region > node->left->region) {
        node->left = rotateLeft(node->left);
        return rotateRight(node);
    }

   if (balance < -1 && region < node->right->region) {
        node->right = rotateRight(node->right);
        return rotateLeft(node);
    }

   return node;
}

void inOrder(Node* root) {
    if (root != nullptr) {
        inOrder(root->left);
        cout << root->region << " ";
        inOrder(root->right);
    }
}

int main() {
    Node* root = nullptr;
    root = insert(root, "Region3");
    root = insert(root, "Region2");
    root = insert(root, "Region1");

   cout << "In-order traversal: ";
   inOrder(root);
   cout << endl;

   return 0;
}

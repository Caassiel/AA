#include <iostream>

static const int k = 2;

class Tree {
private:
    struct Node {
    int p[k];
    Node* left;
    Node* right;

    Node(const int point[k]) {
    for (int i = 0; i < k; i++) {
        p[i] = point[i];
        left = right = 0;
        }
    }
};

Node* root = 0;

Node* InsertRecursive(Node* node, const int p[k], unsigned d) {
    if (!node) return new Node(p);
    unsigned c = d % k;

    if (p[c] < node->p[c]) node->left = InsertRecursive(node->left, p, d + 1);
    else node->right = InsertRecursive(node->right, p, d + 1);

    return node;
}

bool Equal(const int p1[k], const int p2[k]) const {
    for (int i = 0; i < k; ++i) if (p1[i] != p2[i]) return false;
    return true;
}

bool SearchRecursive(const Node* node, const int p[k], unsigned d) const {
    if (!node) return false;
    if (Equal(node->p, p)) return true;

    unsigned c = d % k;

    if (p[c] < node->p[c]) return SearchRecursive(node->left, p, d + 1);
    else return SearchRecursive(node->right, p, d + 1);
}

void EmptyTree(Node* node) {
    if (!node) return;
    EmptyTree(node->left);
    EmptyTree(node->right);
    delete node;
}

void PrintTree(const Node* node, std::string prefix = "", bool isLeft = true) const {
    if (!node) return;
    std::cout << prefix << (isLeft ? "|-- " : " `-- ") << "(" << node->p[0] << ", " << node->p[1] << ")\n";

    std::string newPrefix = prefix + (isLeft ? "|   " : "        ");
    if (node->left || node->right) {
        PrintTree(node->left, newPrefix, true);
        PrintTree(node->right, newPrefix, false);
    }
}

void SearchAllRecursive(const Node* node) const {
    if (!node) return;
    std::cout << "(" << node->p[0] << ", " << node->p[1] << ")\n";
    SearchAllRecursive(node->left);
    SearchAllRecursive(node->right);
}


public:

void Insert(const int point[k]) {
    root = InsertRecursive(root, point, 0);
}

Tree Test(){
    Tree t;
    int p1[k] = {3, 4};
    int p2[k] = {2, 2};
    int p3[k] = {5, 3};
    int p4[k] = {1, 3};
    int p5[k] = {4, 4};

    t.Insert(p1);
    t.Insert(p2);
    t.Insert(p3);
    t.Insert(p4);
    t.Insert(p5);

    return t;
}

bool Search(const int point[k]) const {
    return SearchRecursive(root, point, 0);
}

void Print() const {
    if (!root) {
        std::cout << "Tree is empty.\n";
    }
    PrintTree(root, "", false);
}

void SearchAll() const {
    if (!root) {
        std::cout << "Tree is empty.\n";
    }
    SearchAllRecursive(root);
}

~Tree() {
    EmptyTree(root);
}
};

int main() {

Tree t;
t.Test().Print();
int px[k] = {2, 4};
std::cout << (t.Test().Search(px) ? "TRUE" : "FALSE") << "\n";
t.Test().SearchAll();

return 0;
}

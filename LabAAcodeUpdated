#include <iostream>
#include <string>
#include <regex>
#include <map>
#include <limits>
#include <vector>

using namespace std;

constexpr int k = 2;

class Tree {
private:
    struct Node {
        int Point[k];
        Node* left;
        Node* right;

        Node(const int P[k]) {
            for (int i = 0; i < k; i++) {
                Point[i] = P[i];
                left = right = 0;
            }
        }
    };

    Node* root = 0;
    Tree* tree = 0;

Node* InsertRecursive(Node* node, const int P[k], unsigned Depth) {
    if (!node) return new Node(P);
    unsigned CurrentDimension = Depth % k;

    if (P[CurrentDimension] < node->Point[CurrentDimension]) node->left = InsertRecursive(node->left, P, Depth + 1);
    else node->right = InsertRecursive(node->right, P, Depth + 1);

    return node;
}

bool Equal(const int P1[k], const int P2[k]) const {
    for (int i = 0; i < k; i++) if (P1[i] != P2[i]) return false;
    return true;
}

bool SearchRecursive(const Node* node, const int P[k], unsigned Depth) const {
    if (!node) return false;
    if (Equal(node->Point, P)) return true;

    unsigned CurrentDimension = Depth % k;

    if (P[CurrentDimension] < node->Point[CurrentDimension]) return SearchRecursive(node->left, P, Depth + 1);
    else return SearchRecursive(node->right, P, Depth + 1);
}

void EmptyTree(Node* node) {
    if (!node) return;
    EmptyTree(node->left);
    EmptyTree(node->right);
    delete node;
}

void PrintTree(const Node* node, string prefix = "", bool isLeft = true) const {
    if (!node) return;
    cout << prefix << (isLeft ? "|-- " : " `-- ") << "(" << node->Point[0] << ", " << node->Point[1] << ")\n";

    string newPrefix = prefix + (isLeft ? "|   " : "        ");
    if (node->left || node->right) {
        PrintTree(node->left, newPrefix, true);
        PrintTree(node->right, newPrefix, false);
    }
}

void SearchAllRecursive(const Node* node) const {
    if (!node) return;
    cout << "(" << node->Point[0] << ", " << node->Point[1] << ")\n";
    SearchAllRecursive(node->left);
    SearchAllRecursive(node->right);
}

double DistanceSquared(const int P1[k], const int P2[k]) const {
    double distance = 0;
    for (int i = 0; i < k; i++)
        distance += (P1[i] - P2[i]) * (P1[i] - P2[i]);
    return distance;
}

void NearestRecursive(Node* node, const int target[k], unsigned Depth, Node*& BestNode, double& BestDistance) const {
    if (!node) return;
    double distance = DistanceSquared(node->Point, target);

    if (distance < BestDistance) {
        BestDistance = distance;
        BestNode = node;
    }

    unsigned CurrentDimension = Depth % k;

    Node* next = (target[CurrentDimension] < node->Point[CurrentDimension]) ? node->left : node->right;
    Node* other = (target[CurrentDimension] < node->Point[CurrentDimension]) ? node->right : node->left;

    NearestRecursive(next, target, Depth + 1, BestNode, BestDistance);

    double difference = target[CurrentDimension] - node->Point[CurrentDimension];
    if (difference * difference < BestDistance) NearestRecursive(other, target, Depth + 1, BestNode, BestDistance);
}

void AboveRecursive(Node* node, int Min, unsigned Depth, vector<Node*>& result) const {
    if (!node) return;
    if (node->Point[1] >= Min) result.push_back(node);

    unsigned CurrentDimension = Depth % k;

    if (CurrentDimension == 1) {
        if (Min <= node->Point[CurrentDimension]){
            AboveRecursive(node->right, Min, Depth + 1, result);
            AboveRecursive(node->left, Min, Depth + 1, result);
            }
        }
    else {
        AboveRecursive(node->left, Min, Depth + 1, result);
        AboveRecursive(node->right, Min, Depth + 1, result);
    }
}

void RangeSearchRecursive(Node* node, const int BottomLeft[k], const int TopRight[k], unsigned Depth, vector<Node*>& result) const {
    if (!node) return;

    bool inside = true;
    for (int i = 0; i < k; i++) {
        if (node->Point[i] < BottomLeft[i] || node->Point[i] > TopRight[i]) {
            inside = false;
            break;
        }
    }
    if (inside) result.push_back(node);

    unsigned CurrentDimension = Depth % k;

    if (BottomLeft[CurrentDimension] <= node->Point[CurrentDimension]) RangeSearchRecursive(node->left, BottomLeft, TopRight, Depth + 1, result);
    if (TopRight[CurrentDimension] >= node->Point[CurrentDimension]) RangeSearchRecursive(node->right, BottomLeft, TopRight, Depth + 1, result);
}

public:

void Insert(const int point[k]) {
    root = InsertRecursive(root, point, 0);
}

bool Search(const int point[k]) const {
    return SearchRecursive(root, point, 0);
}

void Print() const {
    if (!root) {
        cout << "Tree is empty.\n";
    }
    PrintTree(root, "", false);
}

void SearchAll() const {
    if (!root) {
        cout << "Tree is empty.\n";
    }
    SearchAllRecursive(root);
}

Node* Nearest(const int target[k]) const {
    Node* BestNode = 0;
    double BestDistance = numeric_limits<double>::infinity();
    NearestRecursive(root, target, 0, BestNode, BestDistance);
    return BestNode;
}

vector<Node*> SearchAbove(int Min) const {
    vector<Node*> result;
    AboveRecursive(root, Min, 0, result);
    return result;
}

vector<Node*> RangeSearch(const int BottomLeft[k], const int TopRight[k]) const {
    vector<Node*> result;
    RangeSearchRecursive(root, BottomLeft, TopRight, 0, result);
    return result;
}

void Interface(){
    map<string, Tree*> Forest;
    string Input;
    smatch Match;

    static const regex Create(R"(^\s*create\s+([A-Za-z_]\w*)\s*;\s*$)", regex::icase);
    static const regex Insert(R"(^\s*insert\s+([A-Za-z_]\w*)\s*\(\s*(-?\d+)\s*,\s*(-?\d+)\s*\)\s*;\s*$)", regex::icase);
    static const regex Contains(R"(^\s*contains\s+([A-Za-z_]\w*)\s*\(\s*(-?\d+)\s*,\s*(-?\d+)\s*\)\s*;\s*$)", regex::icase);
    static const regex Print(R"(^\s*print\s+([A-Za-z_]\w*)\s*;\s*$)", regex::icase);
    static const regex Delete(R"(^\s*delete\s+([a-zA-Z]\w*);\s*$)", regex::icase);

    static const regex SearchSimple(R"(^\s*search\s+([a-zA-Z]\w*);\s*$)", regex::icase);
    static const regex SearchInside(R"(^\s*search\s+([A-Za-z_]\w*)\s+where\s+inside\s*\(\s*(-?\d+)\s*,\s*(-?\d+)\s*\)\s*,\s*\(\s*(-?\d+)\s*,\s*(-?\d+)\s*\)\s*;\s*$)", regex::icase);
    static const regex SearchAbove(R"(^\s*search\s+([A-Za-z_]\w*)\s+where\s+above_to\s*(-?\d+)\s*;\s*$)", regex::icase);
    static const regex SearchNN(R"(^\s*search\s+([A-Za-z_]\w*)\s+where\s+nn\s*\(\s*(-?\d+)\s*,\s*(-?\d+)\s*\)\s*;\s*$)", regex::icase);

    static const regex Exit(R"(^\s*exit\s*;\s*$)", regex::icase);

    cout << "This is an interface for a rudimentary database. Here's your allowed set of commands:\n";
    cout << "Create <tree_name>; - creates a new 2D tree;\nInsert <tree_name> (x, y); - inserts a 2-Dimensional point with integer values into the tree;\n";
    cout << "Contains <tree_name> (x, y); - checks if a point is in the tree;\nPrint <tree_name>; - outputs the tree onto the screen;\n";
    cout << "Search <tree_name>; - outputs all points of the tree;\nSearch <tree_name> where inside (x1, y1), (x2, y2); - outputs all points inside the rectangle;\n";
    cout << "Search <tree_name> where above_to <const>; - outputs all points whose y coordinate is greater than const;\n";
    cout << "Search <tree_name> where NN (x, y); - outputs the nearest neighbour to the point;\n";
    cout << "Delete <tree_name>; - deletes the tree;\nExit; - exits the program.\n\n";

    while(true){
        cout << ">";
        if(!getline(cin, Input)) break;

        if (regex_match(Input, Match, Create)) {
            string Name = Match[1];
            if (Forest.count(Name)) {
                cout << "Set '" << Name << "' already exists.\n";
            } else {
                Forest[Name] = new Tree();
                cout << "Set '" << Name << "' has been created.\n";
            }
        }
        else if (regex_match(Input, Match, Insert)) {
            string Name = Match[1];
            int x = stoi(Match[2]), y = stoi(Match[3]);
            if (!Forest.count(Name)) {
                cout << "No such set: " << Name << ".\n";
            } else {
                int p[k] = {x, y};
                Forest[Name]->Insert(p);
                cout << "Point (" << x << ", " << y << ") has been added into " << Name << ".\n";
            }
        }
        else if (regex_match(Input, Match, Contains)) {
            string Name = Match[1];
            int x = stoi(Match[2]), y = stoi(Match[3]);
            if (!Forest.count(Name)) {
                cout << "No such set: " << Name << ".\n";
            } else {
                int p[k] = {x, y};
                bool found = Forest[Name]->Search(p);
                cout << "Point (" << x << ", " << y << ") " << (found ? "found" : "not found") << " in " << Name << ".\n";
            }
        }
        else if (regex_match(Input, Match, Print)) {
            string Name = Match[1];
            if (!Forest.count(Name)) {
                cout << "No such tree: " << Name << ".\n";
            } else {
                cout << "Tree '" << Name << "':\n";
                Forest[Name]->Print();
            }
        }
        else if (regex_match(Input, Match, Delete)) {
            string Name = Match[1];
            if (!Forest.count(Name)) {
                cout << "No such tree: " << Name << ".\n";
            } else {
                delete Forest[Name];
                Forest.erase(Name);
                cout << "Deleted tree '" << Name << "'.\n";
            }
        }
        else if (regex_match(Input, Match, SearchSimple)) {
             string Name = Match[1];
            if (!Forest.count(Name)) {
                cout << "No such tree: " << Name << ".\n";
            }
            else {
                cout << "Existing elements:\n";
                Forest[Name]->SearchAll();
            }
        }
        else if (regex_match(Input, Match, SearchInside)) {
            string name = Match[1];
            int x1 = stoi(Match[2]), y1 = stoi(Match[3]);
            int x2 = stoi(Match[4]), y2 = stoi(Match[5]);

            if (!Forest.count(name)) {
                cout << "No such tree: " << name << ".\n";
                }
            else {
                int bottomLeft[k] = {min(x1, x2), min(y1, y2)};
                int topRight[k] = {max(x1, x2), max(y1, y2)};
                auto points = Forest[name]->RangeSearch(bottomLeft, topRight);

                cout << "Points in rectangle [(" << bottomLeft[0] << ", " << bottomLeft[1] << "), (" << topRight[0] << ", " << topRight[1] << ")]:\n";
                for (auto* p : points)
                cout << "(" << p->Point[0] << ", " << p->Point[1] << ")\n";
            }
        }
        else if (regex_match(Input, Match, SearchAbove)) {
            string name = Match[1];
            int Min = stoi(Match[2]);

            if (!Forest.count(name)) {
                cout << "No such tree: " << name << ".\n";
            } else {
                auto points = Forest[name]->SearchAbove(Min);
                cout << "Points with y >= " << Min << ":\n";
                for (auto* p : points)
                cout << "(" << p->Point[0] << ", " << p->Point[1] << ")\n";
            }
        }
        else if (regex_match(Input, Match, SearchNN)) {
            string name = Match[1];
            int x = stoi(Match[2]); int y = stoi(Match[3]);
            if (!Forest.count(name)) {
                cout << "No such tree: " << name << ".\n";
            } else {
                int target[k] = {x, y};
                auto* best = Forest[name]->Nearest(target);
                if (best) cout << "Nearest neighbor to (" << x << ", " << y << ") is (" << best->Point[0] << ", " << best->Point[1] << ").\n";
                else cout << "Tree is empty.\n";
            }
        }
        else if (regex_match(Input, Match, Exit)) {
            cout << "Process ended.\n";
            break;
        }
        else {
            cout << "Unrecognized command.\n";
        }
    }
}

~Tree() {
    EmptyTree(root);
    delete[] tree;
    }
};


int main() {

Tree T;
T.Interface();

return 0;
}

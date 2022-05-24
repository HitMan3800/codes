# codes
Consider telephone book database of N clients. Make use of a hash table implementation to 
quickly look up client‘s telephone number. Make use of two collision handling techniques 
and compare them using number of comparisons required to find a set of telephone numbers
#include <iostream>
#include <string.h>
using namespace std;
struct node
{
int value;
node *next;
} * HashTable[10];
class hashing
{
public:
hashing()
{
for (int i = 0; i < 10; i++)
{
HashTable[i] = NULL;
}
}
int HashFunction(int value)
{
return (value % 10);
}
node *create_node(int x)
{
node *temp = new node;
temp->next = NULL;
temp->value = x;
return temp;
}
void display()
{
for (int i = 0; i < 10; i++)
{
node *temp = new node;
temp = HashTable[i];
cout << "a[" << i << "] : ";
while (temp != NULL)
{
cout << " ->" << temp->value;
temp = temp->next;
}
cout << "\n";
}
}
int searchElement(int value)
{
bool flag = false;
int hash_val = HashFunction(value);
node *entry = HashTable[hash_val];
cout << "\nElement found at : ";
while (entry != NULL)
{
if (entry->value == value)
{
cout << hash_val << " : " << entry->value << endl;
flag = true;
}
entry = entry->next;
}
if (!flag)
return -1;
}
void deleteElement(int value)
{
int hash_val = HashFunction(value);
node *entry = HashTable[hash_val];
if (entry == NULL)
{
cout << "No Element found ";
return;
}
if (entry->value == value)
{
HashTable[hash_val] = entry->next;
return;
}
while ((entry->next)->value != value)
{
entry = entry->next;
}
entry->next = (entry->next)->next;
}
void insertElement(int value)
{
int hash_val = HashFunction(value);
// node* prev = NULL;
// node* entry =
HashTable[hash_val];
node *temp = new node;
node *head = new node;
head = create_node(value);
temp = HashTable[hash_val];
if (temp == NULL)
{
HashTable[hash_val] = head;
}
else
{
while (temp->next != NULL)
{
temp = temp->next;
}
temp->next = head;
}
}
};
int main()
{
int ch;
int data, search, del;
hashing h;
do
{
cout << "\nTelephone : \n1.Insert \n2.Display \n3.Search\n4.Delete 
\n5.Exit";
cin >> ch;
switch (ch)
{
case 1:
cout << "\nEnter phone no. to be inserted : ";
cin >> data;
h.insertElement(data);
break;
case 2:
h.display();
break;
case 3:
cout << "\nEnter the no to be searched : ";
cin >> search;
if (h.searchElement(search) == -1)
{
cout << "No element found at key ";
continue;
}
break;
case 4:
cout << "\nEnter the phno. to be deleted : ";
cin >> del;
h.deleteElement(del);
cout << "Phno. Deleted" << endl;
break;
}
} while (ch != 5);
return 0;
}
Implement all the functions of a dictionary (ADT) using hashing and handle collisions using 
chaining with / without replacement. Data: Set of (key, value) pairs, Keys are mapped to 
values, Keys must be comparable, Keys must be unique Standard Operations: Insert(key, 
value), Find(key), Delete(key)
#include <iostream>
#include <string.h>
using namespace std;
class HashFunction
{
typedef struct hash
{
long key;
char name[10];
} hash;
hash h[10];
public:
HashFunction();
void insert();
void display();
int find(long);
void Delete(long);
};
HashFunction::HashFunction()
{
int i;
for (i = 0; i < 10; i++)
{
h[i].key = -1;
strcpy(h[i].name, "NULL");
}
}
void HashFunction::Delete(long k)
{
int index = find(k);
if (index == -1)
{
cout << "\n\tKey Not Found";
}
else
{
h[index].key = -1;
strcpy(h[index].name, "NULL");
cout << "\n\tKey is Deleted";
}
}
int HashFunction::find(long k)
{
int i;
for (i = 0; i < 10; i++)
{
if (h[i].key == k)
{
cout << "\n\t" << h[i].key << " is Found at " << i << " Location 
With Name " << h[i].name;
return i;
}
}
if (i == 10)
{
return -1;
}
}
void HashFunction::display()
{
int i;
cout << "\n\t\tKey\t\tName";
for (i = 0; i < 10; i++)
{
cout << "\n\th[" << i << "]\t" << h[i].key << "\t\t" << h[i].name;
}
}
void HashFunction::insert()
{
char ans, n[10], ntemp[10];
long k, temp;
int v, hi, cnt = 0, flag = 0, i;
do
{
if (cnt >= 10)
{
cout << "\n\tHash Table is FULL";
break;
}
cout << "\n\tEnter a Telephone No: ";
cin >> k;
cout << "\n\tEnter a Client Name: ";
cin >> n;
hi = k % 10; // hash function
if (h[hi].key == -1)
{
h[hi].key = k;
strcpy(h[hi].name, n);
}
else
{
if (h[hi].key % 10 != hi)
{
temp = h[hi].key;
strcpy(ntemp, h[hi].name);
h[hi].key = k;
strcpy(h[hi].name, n);
for (i = hi + 1; i < 10; i++)
{
if (h[i].key == -1)
{
h[i].key = temp;
strcpy(h[i].name, ntemp);
flag = 1;
break;
}
}
for (i = 0; i < hi && flag == 0; i++)
{
if (h[i].key == -1)
{
h[i].key = temp;
strcpy(h[i].name, ntemp);
break;
}
}
}
else
{
for (i = hi + 1; i < 10; i++)
{
if (h[i].key == -1)
{
h[i].key = k;
strcpy(h[i].name, n);
flag = 1;
break;
}
}
for (i = 0; i < hi && flag == 0; i++)
{
if (h[i].key == -1)
{
h[i].key = k;
strcpy(h[i].name, n);
break;
}
}
}
}
flag = 0;
cnt++;
cout << "\n\t..... Do You Want to Insert More Key: y/n";
cin >> ans;
} while (ans == 'y' || ans == 'Y');
}
int main()
{
long k;
int ch, index;
char ans;
HashFunction obj;
do
{
cout << "\n\t*** Telephone (ADT) *****";
cout << "\n\t1. Insert\n\t2. Display\n\t3. Find\n\t4. Delete\n\t5. 
Exit";
cout << "\n\t..... Enter Your Choice: ";
cin >> ch;
switch (ch)
{
case 1:
obj.insert();
break;
case 2:
obj.display();
break;
case 3:
cout << "\n\tEnter a Key Which You Want to Search: ";
cin >> k;
index = obj.find(k);
if (index == -1)
{
cout << "\n\tKey Not Found";
}
break;
case 4:
cout << "\n\tEnter a Key Which You Want to Delete: ";
cin >> k;
obj.Delete(k);
break;
case 5:
break;
}
cout << "\n\t..... Do You Want to Continue in Main Menu:y/n ";
cin >> ans;
} whil
e (ans == 'y' || ans == 'Y');
}
A book consists of chapters, chapters consist of sections and sections consist of 
subsections. Construct a tree and print the nodes. Find the time and space requirements 
of your method. 
#include <iostream>
#include <stdlib.h>
#include <string.h>
using namespace std;
struct node
{
char name[20];
node *next;
node *down;
int flag;
};
class Gll
{
char ch[20];
int n, i;
node *head = NULL, *temp = NULL, *t1 = NULL, *t2 = NULL;
public:
node *create();
void insertb();
void insertc();
void inserts();
void insertss();
void displayb();
};
node *Gll::create()
{
node *p = new (struct node);
p->next = NULL;
p->down = NULL;
p->flag = 0;
cout << "\n enter the name";
cin >> p->name;
return p;
}
void Gll::insertb()
{
if (head == NULL)
{
t1 = create();
head = t1;
}
else
{
cout << "\n book exist";
}
}
void Gll::insertc()
{
if (head == NULL)
{
cout << "\n there is no book";
}
else
{
cout << "\n how many chapters you want to insert";
cin >> n;
for (i = 0; i < n; i++)
{
t1 = create();
if (head->flag == 0)
{
head->down = t1;
head->flag = 1;
}
else
{
temp = head;
temp = temp->down;
while (temp->next != NULL)
temp = temp->next;
temp->next = t1;
}
}
}
}
void Gll::inserts()
{
if (head == NULL)
{
cout << "\n there is no book";
}
else
{
cout << "\n Enter the name of chapter on which you want to enter the 
section";
cin >> ch;
temp = head;
if (temp->flag == 0)
{
cout << "\n their are no chapters on in book";
}
else
{
temp = temp->down;
while (temp != NULL)
{
if (!strcmp(ch, temp->name))
{
cout << "\n how many sections you want to enter";
cin >> n;
for (i = 0; i < n; i++)
{
t1 = create();
if (temp->flag == 0)
{
temp->down = t1;
temp->flag = 1;
cout << "\n******";
t2 = temp->down;
}
else
{
cout << "\n#####";
while (t2->next != NULL)
{
t2 = t2->next;
}
t2->next = t1;
}
}
break;
}
temp = temp->next;
}
}
}
}
void Gll::insertss()
{
if (head == NULL)
{
cout << "\n there is no book";
}
else
{
cout << "\n Enter the name of chapter on which you want to enter the 
section";
cin >> ch;
temp = head;
if (temp->flag == 0)
{
cout << "\n their are no chapters on in book";
}
else
{
temp = temp->down;
while (temp != NULL)
{
if (!strcmp(ch, temp->name))
{
cout << "\n enter name of section in which you want to 
enter the sub section";
cin >> ch;
if (temp->flag == 0)
{
cout << "\n their are no sections ";
}
else
{
temp = temp->down;
while (temp != NULL)
{
if (!strcmp(ch, temp->name))
{
cout << "\n how many subsections you want to 
enter";
cin >> n;
for (i = 0; i < n; i++)
{
t1 = create();
if (temp->flag == 0)
{
temp->down = t1;
temp->flag = 1;
cout << "\n******";
t2 = temp->down;
}
else
{
cout << "\n#####";
while (t2->next != NULL)
{
t2 = t2->next;
}
t2->next = t1;
}
}
break;
}
temp = temp->next;
}
}
}
temp = temp->next;
}
}
}
}
void Gll::displayb()
{
if (head == NULL)
{
cout << "\n book not exist";
}
else
{
temp = head;
cout << "\n NAME OF BOOK: " << temp->name;
if (temp->flag == 1)
{
temp = temp->down;
while (temp != NULL)
{
cout << "\n\t\tNAME OF CHAPTER: " << temp->name;
t1 = temp;
if (t1->flag == 1)
{
t1 = t1->down;
while (t1 != NULL)
{
cout << "\n\t\t\t\tNAME OF SECTION: " << t1->name;
t2 = t1;
if (t2->flag == 1)
{
t2 = t2->down;
while (t2 != NULL)
{
cout << "\n\t\t\t\t\t\tNAME OF SUBSECTION: "
<< t2->name;
t2 = t2->next;
}
}
t1 = t1->next;
}
}
temp = temp->next;
}
}
}
}
int main()
{
Gll g;
int x;
while (1)
{
cout << "\n\n enter your choice";
cout << "\n 1.insert book";
cout << "\n 2.insert chapter";
cout << "\n 3.insert section";
cout << "\n 4.insert subsection";
cout << "\n 5.display book";
cout << "\n 6.exit"<<endl;
cin >> x;
switch (x)
{
case 1:
g.insertb();
break;
case 2:
g.insertc();
break;
case 3:
g.inserts();
break;
case 4:
g.insertss();
break;
case 5:
g.displayb();
break;
case 6:
exit(0);
}
}
return 0;
}
Beginning with an empty binary search tree, Construct binary search tree by inserting 
the values in the order given. After constructing a binary tree –
i. Insert new node 
ii. Find number of nodes in longest path from root 
iii. Minimum data value found in the tree 
iv. Change a tree so that the roles of the left and right pointers are swapped at every 
node 
v. Search a value
#include <iostream>
#include <cstdlib>
using namespace std;
class node
{
public:
int info;
struct node *left;
struct node *right;
} * root;
class BST
{
public:
node *root;
void insert(node *, node *);
void display(node *, int);
int min(node *);
int height(node *);
void mirror(node *);
void preorder(node *);
void inorder(node *);
void postorder(node *);
void search(node *, int);
BST() { root = NULL; }
};
int main()
{
int choice, num;
BST bst;
node *temp;
while (1)
{
cout << "-----------------" << endl;
cout << "Operations on BST" << endl;
cout << "-----------------" << endl;
cout << "1.Insert Element " << endl;
cout << "2.Display" << endl;
cout << "3.Min value find" << endl;
cout << "4.Height" << endl;
cout << "5.Mirror of node" << endl;
cout << "6.Preorder" << endl;
cout << "7.Inorder" << endl;
cout << "8.Postorder" << endl;
cout << "9.No.of nodes in longest path " << endl;
cout << " 10.Search an element " << endl;
cout << " 11.Quit " << endl;
cout << " Enter your choice : ";
cin >> choice;
switch (choice)
{
case 1:
temp = new node();
cout << " Enter the number to be inserted : ";
cin >> temp->info;
bst.insert(bst.root, temp);
break;
case 2:
cout << " Display BST : " << endl;
bst.display(bst.root, 1);
cout << endl;
break;
case 3:
cout << " Min value of tree " << endl;
cout << temp->info;
bst.min(bst.root);
cout << endl;
break;
case 4:
int h;
h = bst.height(bst.root);
cout << " Height of tree = " << h;
cout << endl;
break;
case 5:
cout << " Mirror ";
bst.mirror(bst.root);
bst.display(bst.root, 1);
break;
case 6:
cout << " \n Display preorder Binary tree = ";
bst.preorder(bst.root);
cout << endl;
break;
case 7:
cout << " \n Display inorder Binary tree = ";
bst.inorder(bst.root);
cout << endl;
break;
case 8:
cout << " \n Display postorder Binary tree = ";
bst.postorder(bst.root);
cout << endl;
break;
case 9:
int nodes;
nodes = bst.height(bst.root);
cout << " No.of nodes in longest path from root is " << nodes;
cout << endl;
break;
case 10:
int searchdata;
cout << " Enter the element to ne searched : ";
cin >> searchdata;
bst.search(bst.root, searchdata);
cout << endl;
break;
case 11:
exit(1);
default:
cout << " Wrong choice " << endl;
}
}
}
void BST::insert(node *tree, node *newnode)
{
if (root == NULL)
{
root = new node;
root->info = newnode->info;
root->left = NULL;
root->right = NULL;
cout << " Root Node is Added " << endl;
return;
}
if (tree->info == newnode->info)
{
cout << " Element already in the tree " << endl;
return;
}
if (tree->info > newnode->info)
{
if (tree->left != NULL)
{
insert(tree->left, newnode);
}
else
{
tree->left = newnode;
(tree->left)->left = NULL;
(tree->left)->right = NULL;
cout << " Node Added To Left " << endl;
return;
}
}
else
{
if (tree->right != NULL)
{
insert(tree->right, newnode);
}
else
{
tree->right = newnode;
(tree->right)->left = NULL;
(tree->right)->right = NULL;
cout << " Node Added To Right " << endl;
return;
}
}
}
void BST::display(node *ptr, int level)
{
int i;
if (ptr != NULL)
{
display(ptr->right, level + 1);
cout << endl;
if (ptr == root)
cout << " Root-> : ";
else
{
for (i = 0; i < level; i++)
cout << "";
}
cout << ptr->info;
display(ptr->left, level + 1);
}
}
int BST::min(node *root)
{
node *temp;
if (root == NULL)
{
cout << " Tree is empty ";
}
else
{
temp = root;
while (temp->left != NULL)
{
temp = temp->left;
}
return (temp->info);
}
}
int BST::height(node *root)
{
int htleft, htright;
if (root == NULL)
{ // cout<<" Tree is empty "<<endl;
return (0);
}
else if (root->left == NULL && root->right == NULL)
{
return (1);
}
htleft = height(root->left);
htright = height(root->right);
if (htright >= htleft)
{
return (htright + 1);
}
else
{
return (htleft + 1);
}
}
void BST::mirror(node *root)
{
node *temp;
if (root != NULL)
{
temp = root->left;
root->left = root->right;
root->right = temp;
mirror(root->left);
mirror(root->right);
}
}
void BST::preorder(node *ptr)
{
if (ptr != NULL)
{
cout << ptr->info << "\t ";
preorder(ptr->left);
preorder(ptr->right);
cout << endl;
}
}
void BST::inorder(node *ptr)
{
if (ptr != NULL)
{
inorder(ptr->left);
cout << ptr->info << "\t ";
inorder(ptr->right);
cout << endl;
}
}
void BST::postorder(node *ptr)
{
if (ptr != NULL)
{
postorder(ptr->left);
postorder(ptr->right);
cout << ptr->info << "\t ";
cout << endl;
}
}
void BST::search(node *ptr, int searchdata)
{
if (ptr->info == searchdata)
{
cout << " Element Not Found... " << endl;
}
else if (ptr->info < searchdata && ptr->right != NULL)
{
search(ptr->right, searchdata);
}
else if (ptr->info > searchdata && ptr->left != NULL)
{
search(ptr->left, searchdata);
}
else
{cout << " Element found... " << endl;
}
}
A Dictionary stores keywords & its meanings. Provide facility for adding new 
keywords, deleting keywords, updating values of any entry. Provide facility to display 
whole data sorted in ascending/ Descending order. Also find how many maximum 
comparisons may require for finding any keyword. Use Binary Search Tree for 
implementation.
#include<iostream>
#include<string.h>
using namespace std;
typedef struct node
{
char k[20];
char m[20];
class node *left;
class node * right;
}node;
class dict
{
public:
node *root;
void create();
void disp(node *);
void insert(node * root,node *temp);
int search(node *,char []);
int update(node *,char []);
node* del(node *,char []);
node * min(node *);
};
void dict :: create()
{
class node *temp;
int ch;
do
{
temp = new node;
cout<<"\nEnter Keyword:";
cin>>temp->k;
cout<<"\nEnter Meaning:";
cin>>temp->m;
temp->left = NULL;
temp->right = NULL;
if(root == NULL)
{
root = temp;
}
else
{
insert(root, temp);
}
cout<<"\nDo u want to add more (y=1/n=0):";
cin>>ch;
}
while(ch == 1);
}
void dict :: insert(node * root,node *temp)
{
if(strcmp (temp->k, root->k) < 0 )
{
if(root->left == NULL)
root->left = temp;
else
insert(root->left,temp);
}
else
{ 
if(root->right == NULL)
root->right = temp;
else
insert(root->right,temp);
}
}
void dict:: disp(node * root)
{
if( root != NULL)
{
disp(root->left);
cout<<"\n Key Word :"<<root->k;
cout<<"\t Meaning :"<<root->m;
disp(root->right);
}
}
int dict :: search(node * root,char k[20])
{
int c=0;
while(root != NULL)
{
c++;
if(strcmp (k,root->k) == 0)
{
cout<<"\nNo of Comparisons:"<<c;
return 1;
}
if(strcmp (k, root->k) < 0)
root = root->left;
if(strcmp (k, root->k) > 0)
root = root->right;
}
return -1;
}
int dict :: update(node * root,char k[20])
{
while(root != NULL)
{
if(strcmp (k,root->k) == 0)
{
cout<<"\nEnter New Meaning ofKeyword"<<root->k;
cin>>root->m;
return 1;
}
if(strcmp (k, root->k) < 0)
root = root->left;
if(strcmp (k, root->k) > 0)
root = root->right;
}
return -1;
}
node* dict :: del(node * root,char k[20])
{
node *temp;
if(root == NULL)
{
cout<<"\nElement No Found";
return root;
}
if (strcmp(k,root->k) < 0)
{
root->left = del(root->left, k);
return root;
}
if (strcmp(k,root->k) > 0)
{
root->right = del(root->right, k);
return root;
}
if (root->right==NULL&&root->left==NULL)
{
temp = root;
delete temp;
return NULL;
}
if(root->right==NULL)
{
temp = root;
root = root->left;
delete temp;
return root;
}
else if(root->left==NULL)
{
temp = root;
root = root->right;
delete temp;
return root;
}
temp = min(root->right);
strcpy(root->k,temp->k);
root->right = del(root->right, temp->k);
return root;
}
node * dict :: min(node *q)
{
while(q->left != NULL)
{
q = q->left;
}
return q;
}
int main()
{
int ch;
dict d;
d.root = NULL;
do
{
cout<<"\nMenu\n 1.Create\n 2.Disp\n 3.Search\n 4.Update\n 5.Delete\n 
Enter Ur CH:";
cin>>ch;
switch(ch)
{
case 1: d.create();
break;
case 2: if(d.root == NULL)
{
cout<<"\nNo any Keyword";
}
else
{
d.disp(d.root);
}
break;
case 3: if(d.root == NULL)
{
cout<<"\nDictionary is Empty. First add keywords then try again ";
}
else
{
cout<<"\nEnter Keyword which u want to search:";
char k[20];
cin>>k;
if( d.search(d.root,k) == 1)
cout<<"\nKeyword Found";
else
cout<<"\nKeyword Not Found";
}
break;
case 4:
if(d.root == NULL)
{
cout<<"\nDictionary is Empty. First add keywords then try again ";
}
else
{
cout<<"\nEnter Keyword which meaning want to update:";
char k[20];
cin>>k;
if(d.update(d.root,k) == 1)
cout<<"\nMeaning Updated";
else
cout<<"\nMeaning Not Found";
}
break;
case 5:
if(d.root == NULL)
{
cout<<"\nDictionary is Empty. First add keywords then try again ";
}
else
{
cout<<"\nEnter Keyword which u want to delete:";
char k[20];
cin>>k;
if(d.root == NULL)
{
cout<<"\nNo any Keyword";
}
else
{
d.root = d.del(d.root,k);
}
}
}
}
while(ch<=5);
return 0;
}
There are flight paths between cities. If there is a flight between city A and city B then 
there is an edge between the cities. The cost of the edge can be the time that flight take 
to reach city B from A, or the amount of fuel used for the journey. Represent this as a 
graph. The node can be represented by airport name or name of the city. Use adjacency 
list representation of the graph or use adjacency matrix representation of the graph. 
Check whether the graph is connected or not. Justify the storage representation used.
#include <iostream>
#include <string.h>
using namespace std;
class flight
{
public:
int am[10][10];
char city_index[10][10];
flight();
int create();
void display(int city_count);
};
flight::flight()
{
int i, j;
for (i = 0; i < 10; i++)
{
strcpy(city_index[i], "xx");
}
for (i = 0; i < 10; i++)
{
for (j = 0; j < 10; j++)
{
am[i][j] = 0;
}
}
}
int flight::create()
{
int city_count = 0, j, si, di, wt;
char s[10], d[10], c;
do
{
cout << "\n\tEnter Source City : ";
cin >> s;
cout << "\n\tEnter Destination City : ";
cin >> d;
for (j = 0; j < 10; j++)
{
if (strcmp(city_index[j], s) == 0)
break;
}
if (j == 10)
{
strcpy(city_index[city_count], s);
city_count++;
}
for (j = 0; j < 10; j++)
{
if (strcmp(city_index[j], d) == 0)
break;
}
if (j == 10)
{
strcpy(city_index[city_count], d);
city_count++;
}
cout << "\n\t Enter Distance From " << s << " And " << d << ": ";
cin >> wt;
for (j = 0; j < 10; j++)
{
if (strcmp(city_index[j], s) == 0)
si = j;
if (strcmp(city_index[j], d) == 0)
di = j;
}
am[si][di] = wt;
cout << "\n\t Do you want to add more cities.....(y/n) : ";
cin >> c;
} while (c == 'y' || c == 'Y');
return (city_count);
}
void flight::display(int city_count)
{
int i, j;
cout << "\n\t Displaying Adjacency Matrix :\n\t";
for (i = 0; i < city_count; i++)
cout << "\t" << city_index[i];
cout << "\n";
for (i = 0; i < city_count; i++)
{
cout << "\t" << city_index[i];
for (j = 0; j < city_count; j++)
{
cout << "\t" << am[i][j];
}
cout << "\n";
}
}
int main()
{
flight f;
int n, city_count;
char c;
do
{
cout << "\n\t*** Flight Main Menu *****";
cout << "\n\t1. Create \n\t2. Adjacency Matrix\n\t3. Exit";
cout << "\n\t.....Enter your choice : ";
cin >> n;
switch (n)
{
case 1:
city_count = f.create();
break;
case 2:
f.display(city_count);
break;
case 3:
return 0;
}
cout << "\n\t Do you Want to Continue in Main Menu....(y/n) : ";
cin >> c;
} while (c == 'y' || c == 'Y');
return 0;
}
You have a business with several offices; you want to lease phone lines to connect them 
up with each other; and the phone company charges different amounts of money to 
connect different pairs of cities. You want a set of lines that connects all your offices 
with a minimum total cost. Solve the problem by suggesting appropriate data 
structures.
#include <iostream>
using namespace std;
class tree
{
int a[20][20], l, u, w, i, j, v, e, visited[20];
public:
void input();
void display();
void minimum();
};
void tree::input()
{
cout << "Enter the no. of branches: ";
cin >> v;
for (i = 0; i < v; i++)
{
visited[i] = 0;
for (j = 0; j < v; j++)
{
a[i][j] = 999;
}
}
cout << "\nEnter the no. of connections: ";
cin >> e;
for (i = 0; i < e; i++)
{
cout << "Enter the end branches of connections: " << endl;
cin >> l >> u;
cout << "Enter the phone company charges for this connection: ";
cin >> w;
a[l - 1][u - 1] = a[u - 1][l - 1] = w;
}
}
void tree::display()
{
cout << "\nAdjacency matrix:";
for (i = 0; i < v; i++)
{
cout << endl;
for (j = 0; j < v; j++)
{
cout << a[i][j] << " ";
}
cout << endl;
}
}
void tree::minimum()
{
int p = 0, q = 0, total = 0, min;
visited[0] = 1;
for (int count = 0; count < (v - 1); count++)
{
min = 999;
for (i = 0; i < v; i++)
{
if (visited[i] == 1)
{
for (j = 0; j < v; j++)
{
if (visited[j] != 1)
{
if (min > a[i][j])
{
min = a[i][j];
p = i;
q = j;
}
}
}
}
}
visited[p] = 1;
visited[q] = 1;
total = total + min;
cout << "Minimum cost connection is" << (p + 1) << " -> " << (q + 1) 
<< " with charge : " << min << endl;
}
cout << "The minimum total cost of connections of all branches is: " << 
total << endl;
}
int main()
{
int ch;
tree t;
do
{
cout << "==========PRIM'S ALGORITHM=================" << endl;
cout << "\n1.INPUT\n \n2.DISPLAY\n \n3.MINIMUM\n"
<< endl;
cout << "Enter your choice :" << endl;
cin >> ch;
switch (ch)
{
case 1:
cout << "*******INPUT YOUR VALUES*******" << endl;
t.input();
break;
case 2:
cout << "*******DISPLAY THE CONTENTS********" << endl;
t.display();
break;
case 3:
cout << "*********MINIMUM************" << endl;
t.minimum();
break;
}
} while (ch != 4);
return 0;
}
Given sequence k = k1 < … <kn of n sorted keys, with a search probability pi for each 
key ki . Build the Binary search tree that has the least search cost given the access 
probability for each key?
#include <iostream>
using namespace std;
void con_obst(void);
void print(int, int);
float a[20], b[20], wt[20][20], c[20][20];
int r[20][20], n;
int main()
{
int i;
cout << "\n**** PROGRAM FOR OBST ******\n";
cout << "\nEnter the no. of nodes : ";
cin >> n;
cout << "\nEnter the probability for successful search :: ";
cout << "\n*************************************************\n";
for (i = 1; i <= n; i++)
{
cout << "p[" << i << "]";
cin >> a[i];
}
cout << "\nEnter the probability for unsuccessful search :: ";
cout << "\n*******************************************************\n";
for (i = 0; i <= n; i++)
{
cout << "q[" << i << "]";
cin >> b[i];
}
con_obst();
print(0, n);
cout << endl;
}
void con_obst(void)
{
int i, j, k, l, min;
for (i = 0; i < n; i++)
{ // Initialisation
c[i][i] = 0.0;
r[i][i] = 0;
wt[i][i] = b[i];
// for j-i=1 can be j=i+1
wt[i][i + 1] = b[i] + b[i + 1] + a[i + 1];
c[i][i + 1] = b[i] + b[i + 1] + a[i + 1];
r[i][i + 1] = i + 1;
}
c[n][n] = 0.0;
r[n][n] = 0;
wt[n][n] = b[n];
// for j-i=2,3,4....,n
for (i = 2; i <= n; i++)
{
for (j = 0; j <= n - i; j++)
{
wt[j][j + i] = b[j + i] + a[j + i] + wt[j][j + i - 1];
c[j][j + i] = 9999;
for (l = j + 1; l <= j + i; l++)
{
if (c[j][j + i] > (c[j][l - 1] + c[l][j + i]))
{
c[j][j + i] = c[j][l - 1] + c[l][j + i];
r[j][j + i] = l;
}
}
c[j][j + i] += wt[j][j + i];
}
cout << endl;
}
cout << "\n\nOptimal BST is :: ";
cout << "\nw[0][" << n << "] :: " << wt[0][n];
cout << "\nc[0][" << n << "] :: " << c[0][n];
cout << "\nr[0][" << n << "] :: " << r[0][n];
}
void print(int l1, int r1)
{
if (l1 >= r1)
return;
if (r[l1][r[l1][r1] - 1] != 0)
cout << "\n Left child of " << r[l1][r1] << " :: " << r[l1][r[l1][r1] 
- 1];
if (r[r[l1][r1]][r1] != 0)
cout << "\n Right child of " << r[l1][r1] << " :: " << 
r[r[l1][r1]][r1];
print(l1, r[l1][r1] - 1);
print(r[l1][r1], r1);
return;
}
Dictionary stores keywords & its meanings. Provide facility for adding new keywords, 
deleting keywords, updating values of any entry. Provide facility to display whole data 
sorted in ascending/ Descending order. Also find how many maximum comparisons 
may require for finding any keyword. Use Height balance tree and find the complexity 
for finding a keyword
#include <iostream>
using namespace std;
class node
{
public:
string key;
string meaning;
node *left;
node *right;
};
class AVL
{
node *root;
public:
AVL() 
{ 
root = NULL; 
}
void create();
node *insert(node *cur, node *temp);
node *balance(node *temp);
int dif(node *temp);
int height(node *temp);
int maximum(int a, int b);
node *LL(node *par);
node *RR(node *par);
node *LR(node *par);
node *RL(node *par);
void ascending(node *temp);
node *delete_n(node *root, string key1);
void deleten();
node *extractmin(node *t);
void descending(node *temp);
void display();
bool search(node *cur, string key1);
void search_value();
};
void AVL::create()
{
char answer;
node *temp;
do
{
temp = new node();
cout << "\n Enter the keyword:";
cin >> temp->key;
cout << "\n Enter the meaning:";
cin >> temp->meaning;
temp->left = temp->right = NULL;
root = insert(root, temp);
cout << "\n Do you want to add another word?(y/n)";
cin >> answer;
} 
while (answer == 'y' || answer == 'Y');
}
node *AVL::insert(node *cur, node *temp)
{
if (cur == NULL)
{
return temp;
}
if (temp->key < cur->key)
{
cur->left = insert(cur->left, temp);
cur = balance(cur);
}
else if (temp->key > cur->key)
{
cur->right = insert(cur->right, temp);
cur = balance(cur);
}
return cur;
}
node *AVL::balance(node *temp)
{
int bal;
bal = dif(temp);
if (bal >= 2)
{
if (dif(temp->left) < 0)
temp = LR(temp);
else
temp = LL(temp);
}
else if (bal <= -2)
{
if (dif(temp->right) < 0)
temp = RR(temp);
else
temp = RL(temp);
}
return temp;
}
int AVL::dif(node *temp)
{
int l, r;
l = height(temp->left);
r = height(temp->right);
return (l - r);
}
int AVL::height(node *temp)
{
if (temp == NULL)
return (-1);
else
return (max(height(temp->left), height(temp->right)) + 1);
}
int AVL::maximum(int a, int b)
{
if (a > b)
return a;
else
return b;
}
node *AVL::LL(node *par)
{
node *temp, *temp1;
temp = par->left;
temp1 = temp->right;
temp->right = par;
par->left = temp1;
return temp;
}
node *AVL::RR(node *par)
{
node *temp, *temp1;
temp = par->right;
temp1 = temp->left;
temp->left = par;
par->right = temp1;
return temp;
}
node *AVL::LR(node *par)
{
par->left = RR(par->left);
return (LL(par));
}
node *AVL::RL(node *par)
{
par->right = LL(par->right);
return (RR(par));
}
void AVL::ascending(node *temp)
{
if (temp != NULL)
{
ascending(temp->left);
cout << "\n\t" << temp->key << " : " << temp->meaning;
ascending(temp->right);
}
}
void AVL::descending(node *temp)
{
if (temp != NULL)
{
descending(temp->right);
cout << "\n\t" << temp->key << " : " << temp->meaning;
descending(temp->left);
}
}
void AVL::display()
{
cout << "\n The keywords in ascending order are : \n";
ascending(root);
cout << "\n The keywords in descending order are : \n";
descending(root);
}
bool AVL::search(node *cur, string key1)
{
if (cur)
{
if (cur->key == key1)
return true;
if (cur->key > key1)
return search(cur->left, key1);
else
return search(cur->right, key1);
}
return false;
}
void AVL::search_value()
{
string key2;
cout << "\n Enter the keyword you wish to search : ";
cin >> key2;
if (search(root, key2))
cout << "\n The entered keyword is present in the AVL tree";
else
cout << "\n The entered keyword is not present in the AVL tree";
}
node *AVL::delete_n(node *cur, string key1)
{
if (!cur)
return cur;
if (key1 < cur->key)
cur->left = delete_n(cur->left, key1);
else if (key1 > cur->key)
cur->right = delete_n(cur->right, key1);
else
{
node *l = cur->left;
node *r = cur->right;
delete cur;
if (!r)
return l;
node *m = r;
while (m->left)
m = m->left;
m->right = extractmin(r);
m->left = l;
return balance(m);
}
return balance(cur);
}
node *AVL::extractmin(node *t)
{
if (!t->left)
return t->right;
t->left = extractmin(t->left);
return balance(t);
}
void AVL::deleten()
{
string key;
cout << "\n Enter the keyword to be deleted : ";
cin >> key;
root = delete_n(root, key);
}
int main()
{
char c;
int ch;
AVL a;
do
{
cout << "*********************************";
cout << "\n 1.Insert a keyword in AVL tree.";
cout << "\n 2.Display the AVL tree.";
cout << "\n 3.Search a keyword "; 
cout<<"\n 4.Delete a keyword."; 
cout<<"\n Enter your choice : "; 
cin>>ch; 
switch(ch) 
{ 
case 1 : a.create(); 
break; 
case 2 : a.display(); break; case 3 : a.search_value(); break; 
case 4 : a.deleten(); break; default : cout<<"\n Wrong choice !"; } 
cout<<"\n Do you want to continue? (y / n): "; 
cin>>c; } 
while(c=='y'||c=='Y'); 
return 0; 
}
Read the marks obtained by students of second year in an online examination of 
particular subject. Find out maximum and minimum marks obtained in that subject. 
Use heap data structure. Analyze the algorithm
#include<iostream>
using namespace std;
# define max 20
class stud
{
int mks[max];
public:
 stud()
 {
 for(int i=0;i<max;i++)
 mks[i]=0;
 }
 void insertheap(int tot);
 void displayheap(int tot);
 void showmax(int tot);
 void showmin();
};
void stud::insertheap(int tot)
{
for(int i=1;i<=tot;i++)
{
 cout<<"enter marks";
 cin>>mks[i];
 int j=i;
 int par=j/2;
 while(mks[j]<=mks[par] && j!=0)
 {
 int tmp = mks[j];
 mks[j]=mks[par];
 mks[par]=tmp;
 j=par;
 par=j/2;
 } 
}
}
void stud::displayheap(int tot)
{
int i=1,space=6;
cout<<endl;
while(i<=tot)
{
 if(i==1 || i==2 || i==4 || i==8 || i==16)
 {
 cout<<endl<<endl;
 for(int j=0;j<space;j++)
 cout<<" ";
 space-=2; 
 }
 cout<<" "<<mks[i];i++;
 
}
}
void stud::showmax(int tot)
{
int max1=mks[1];
for(int i=2;i<=tot;i++)
{
if(max1<mks[i])
 max1= mks[i];
}
cout<<"Max marks:"<<max1;
}
void stud::showmin()
{
cout<<"Min marks:"<<mks[1];
}
int main()
{
int ch,cont,total,tmp;
int n;
stud s1;
do
{
cout<<endl<<"Menu";
cout<<endl<<"1.Read marks of the student ";
cout<<endl<<"2.Display Min heap";
cout<<endl<<"3.Find Max Marks";
cout<<endl<<"4.Find Min Marks";
cout<<endl<<"Enter Choice";
cin>>ch;
switch(ch)
{
case 1:
 cout<<"how many students";
 cin>>total;
 s1.insertheap(total);
 break;
case 2:
 s1.displayheap(total);
 break;
case 3: s1.showmax(total);
 break;
case 4:
 s1.showmin();
 break;
}
cout<<endl<<"do u want to continue?(1 for continue)";
cin>>cont;
}while(cont==1);
return 0;
}

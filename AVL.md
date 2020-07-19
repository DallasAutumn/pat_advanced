# 平衡二叉树

## 1.结构体定义
```cpp
struct node
{
    int data, height;
    node *left, *right;
    node(int v)
    {
        data = v;
        height = 1;
        left = right = NULL;
    }
};
```
比正常的二叉树结构体多了一个height，存储结点高度。之后计算balance factor时就方便了。

## 2.辅助函数
```cpp
// 获取当前结点高度
int getH(node *root)
{
    if(!root) 
        return 0;
    return root->height;
}
// 更新结点高度
void updateH(node *&root)
{
    int LH = getH(root->left);
    int RH = getH(root->right);

    root->height = max(LH, RH) + 1;
}
// 计算平衡因子
int getBF(node *root)
{
    int LH = getH(root->left);
    int RH = getH(root->right);

    return LH - RH;
}
// LL情况，右单旋
void LL(node *&root)
{
    node *temp = root->left;
    root->left = temp->right;
    temp->right = root;
    updateH(root);
    updateH(temp);
    root = temp;
}
// RR情况，左单旋
void RR(node *&root)
{
    node *temp = root->right;
    root->right = temp->left;
    temp->left = root;
    updateH(root);
    updateH(temp);
    root = temp;
}
// LR情况，先对左孩子左单旋，再对根右单旋
void LR(node *&root)
{
    RR(root->left);
    LL(root);
}
// RL情况，先对右孩子右单旋，再对根左单旋
void RL(node *&root)
{
    LL(root->right);
    RR(root);
}
// 将平衡调整操作整合到一起
void adjust(node *&root)
{
    if(getBF(root) == 2) // LL或LR
    {
        if(getBF(root->left) == 2) //LL
            LL(root);
        else if(getBF(root->left) == -2) //LR
            LR(root);
    }

    if(getBF(root) == -2) // RR或RL
    {
        if(getBF(root->right) == -2) //RR
            RR(root);
        else if(getBF(root->right) == 2) //RL
            RL(root);
    }
}
```
## 3.基本操作
```cpp
// 插入操作：每一个节点插入后都要更新高度，进行平衡调整
void insert(node *&root, int v)
{
    if(!root)
    {
        root = new node(v);
        return;
    }

    if(v < root->data)
    {
        insert(root->left, v);
        updateH(root);
        adjust(root);
    }
    else
    {
        insert(root->right, v);
        updateH(root);
        adjust(root);
    }
}
```
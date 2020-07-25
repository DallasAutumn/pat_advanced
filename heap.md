# 堆及相关操作、堆排序

## 1.上下调整（以最大堆为例）
```cpp
void downAdjust(int low, int high)
{
    int i = low, j = 2 * i;

    while(j <= high)
    {
        if(j + 1 <= high && heap[j] < heap[j + 1])
            j++;

        if(heap[i] < heap[j])
        {
            swap(heap[i], heap[j]);
            i = j;
            j = i * 2;
        }
        else
            break;
    }
}

void upAdjust(int low, int high)
{
    int i = high, j = i / 2;

    while(j >= low)
    {
        if(heap[i] > heap[j])
        {
            swap(heap[i], heap[j]);
            i = j;
            j = i / 2;
        }
        else
            break;
    }
}
```

## 2.建堆
### 2.1 整体建堆
```cpp
void createHeap()
{
    for(int i = n / 2; i >= 1; i--)
        downAdjust(i, n);
}
```

### 2.2 插入成堆
```cpp
void insert(int x)
{
    heap[++n] = x;
    upAdjust(1, n);
}
```

## 3.堆排序
```cpp
void heapSort()
{
    for(int i = n; i > 1; i--)
    {
        swap(heap[i], heap[1]);
        downAdjust(1, i - 1);
    }
}
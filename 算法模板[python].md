## python 模板
### KMP
```
def kmp(mom_string,son_string):
    # 传入一个母串和一个子串
    # 返回子串匹配上的第一个位置，若没有匹配上返回-1
    test=''
    if type(mom_string)!=type(test) or type(son_string)!=type(test):
        return -1
    if len(son_string)==0:
        return 0
    if len(mom_string)==0:
        return -1
    #求next数组
    next=[-1]*len(son_string)
    if len(son_string)>1:# 这里加if是怕列表越界
        next[1]=0
        i,j=1,0
        while i<len(son_string)-1:#这里一定要-1，不然会像例子中出现next[8]会越界的
            if j==-1 or son_string[i]==son_string[j]:
                i+=1
                j+=1
                next[i]=j
            else:
                j=next[j]
 
    # kmp框架
    m=s=0#母指针和子指针初始化为0
    while(s<len(son_string) and m<len(mom_string)):
        # 匹配成功,或者遍历完母串匹配失败退出
        if s==-1 or mom_string[m]==son_string[s]:
            m+=1
            s+=1
        else:
            s=next[s]
 
    if s==len(son_string):#匹配成功
        return m-s
    #匹配失败
    return -1

```


### 并查集
```
class WeightUnionFindSet:
    def __init__(self):
        self.parent = {}  # 存放父亲节点
        self.parent_div_cur = {} # 存放父亲节点除以自己的商

    def union(self, x, y, x_div_y):
        # 增加新节点
        if x not in self.parent:
            self.parent[x] = x
            self.parent_div_cur[x] = 1.0
        if y not in self.parent:
            self.parent[y] = y
            self.parent_div_cur[y] = 1.0
        
        # 寻找祖宗节点
        root_x, root_x_div_x = self.find_root(x)
        root_y, root_y_div_y = self.find_root(y)

        # 祖宗不同则进行合并
        if (root_x != root_y):
            self.parent[root_y] = root_x
            self.parent_div_cur[root_y] = root_x_div_x / root_y_div_y * x_div_y 
            # root_x / root_y = root_x / x / (root_y / y) * (x / y)
    
    def find_root(self, x):
        if self.parent[x] == x:
            return x, self.parent_div_cur[x]
        else:
            # 找到祖宗以及祖宗除以当前父节点的商
            root, root_div_parent = self.find_root(self.parent[x])
            # 路径压缩，并得到祖宗除以当前节点的商
            self.parent[x] = root
            self.parent_div_cur[x] = root_div_parent * self.parent_div_cur[x]
            return self.parent[x], self.parent_div_cur[x]
```
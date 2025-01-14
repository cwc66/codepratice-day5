# codepratice-day5
代码打卡思考笔记5

代码随想录第五天
今天的内容是补的，因为当天得肠胃炎了。这部分内容也没多写几遍，因为担心今天的内容完不成。
哈希表基础
哈希表文章讲解的一些内容有必要多看看，set,map，用的原理是红黑树还是哈希表，查询效率是o(logn)还是o(1)

[有效的字母异位词 ](https://leetcode.cn/problems/valid-anagram/description/)
这道题是用数组模拟哈希表的用途，用于理解哈希表的用法。记录数据出现的次数。
```CPP
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26]={0};
        for(int i=0;i<s.size();i++)
        {
            record[s[i]-'a']+=1;
        }
        for(int j=0;j<t.size();j++)
        {
            record[t[j]-'a']-=1;
        }
        for(int i=0;i<26;i++)
        {
            if(record[i]!=0)
                return false;
        }
        return true;
    }
};
```

[两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)
这题可以用数组也可以用set，先将一个数组的元素放入哈希表set中，然后再遍历另外一个数组，判断是否出现过，出现过的话就加入到结果中，最后返回向量，所以用vector<int>(result_set.begin(),result_set.end())
```CPP
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set;
        unordered_set<int> nums_set(nums1.begin(),nums1.end());
        for(int num:nums2)
        {
            if(nums_set.find(num)!=nums_set.end())
            {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(),result_set.end());
    }
};
```

用数组的解法，速度更快
```CPP
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set;
        int hash[1005]={0};
        for(int num:nums1)
        {
            hash[num]=1;
        }
        for(int num:nums2)
        {
            if(hash[num]==1)
            {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(),result_set.end());
    }
};
```

[快乐数 ](https://leetcode.cn/problems/intersection-of-two-arrays/description/)
这题的难点在于取每个数的平方，以及因为非快乐数是重复出现的，因此多次计算平方和，如果重复出现数，则不是快乐数，如果结果是1，则是。
```CPP
class Solution {
public:
    int getsum(int n)//取数值的各个位上单个数之和
    {
        int sum=0;
        while(n)
        {
            sum+=(n%10)*(n%10);
            n=n/10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> s;
        while(1)
        {
            int sum=getsum(n);
            if(sum==1) return true;

            if(s.find(sum)!=s.end())
            {
                return false;
            }
            else
            {
                s.insert(sum);
            }
            n=sum;
        }
    }
};
```


[两数之和 ](https://leetcode.cn/problems/two-sum/)
哈希表的用法。
为什么会想到用哈希表？
因为这里需要保存某个数以及另外一个数，如果他们之和为target，则需要返回他们的下标。也就是说我们需要快速判断一个元素是否出现在集合里。

哈希表为什么用map？
因为我们这道题需要用到两个数，一个是数据值，一个是数据的下标，这样的话我们需要存储的就是两个值。一个作为key，在每次遍历时查找，一个value，作为key对应的值，是我们需要的答案（下标）。

本题map是用来存什么的？
存值和下标。

map中的key和value用来存什么的？
存值和下标。
```CPP
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> p;
        for(int i=0;i<nums.size();i++)
        {
            auto it=p.find(target-nums[i]);
            if(it!=p.end())
            {
                return {it->second,i};
            }
            else
            {
                p.insert(pair<int,int>(nums[i],i));
            }
        }
        return {};
    }
};
```
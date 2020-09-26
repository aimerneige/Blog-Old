---
title: LeetCode Group Anagrams
date: 2020-08-30 10:47:57
tags: LeetCode
category: Program
---
# Group Anagrams

> https://leetcode.com/problems/group-anagrams/

> Given an array of strings, group anagrams together.
>
> **Example:**
>
> ```
> Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
> Output:
> [
>   ["ate","eat","tea"],
>   ["nat","tan"],
>   ["bat"]
> ]
> ```
>
> **Note:**
>
> - All inputs will be in lowercase.
> - The order of your output does not matter.

```c++
uint64_t get_anagrams_key(const string& word) {
    uint64_t ret = 1;
    int magic_numbers[26] = { 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101 };
    for (char i : word) {
        ret *= magic_numbers[i - 'a'];
    }
    return ret;
}

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        map<uint64_t, vector<string>> storage;

        for (auto& str : strs) {
            uint64_t key = get_anagrams_key(str);
            storage[key].push_back(str);
        }

        vector<vector<string>> ret;
        ret.reserve(storage.size());
        for (auto& each : storage) {
            ret.push_back(each.second);
        }

        return ret;
    }
};
```

```java
class Solution {
    private long getKey(String str) {
        long ret = 1;
        int[] magic_numbers = { 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101 };
        char[] chars = str.toCharArray();
        for (char c : chars) {
            ret *= magic_numbers[c - 'a'];
        }
        return ret;
    }

    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) {
            return null;
        }

        Map<Long, List<String>> map = new HashMap<>();

        List<String> list = null;
        for (String str : strs) {
            long key = getKey(str);
            if (map.containsKey(key)) {
                list = map.get(key);
                list.add(str);
            }
            else {
                list = new ArrayList<String>();
                list.add(str);
                map.put(key, list);
            }
        }

        if (map.isEmpty()) {
            return null;
        }

        return new ArrayList<>(map.values());
    }
}
```

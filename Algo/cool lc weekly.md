
A fun trie problem

problem: https://leetcode.com/problems/longest-common-suffix-queries/description/

```python
class TrieNode:
    def __init__(self, c):
        self.c = c
        self.children = defaultdict(int)
        self.indices = []
    
class Solution:
    def stringIndices(self, wordsContainer: List[str], wordsQuery: List[str]) -> List[int]:
        def addWord(root, index, word):
            curr = root
            curr.indices.append(index)
            for letter in word:
                if letter not in curr.children:
                    curr.children[letter] = TrieNode(letter)
                curr = curr.children[letter]
                curr.indices.append(index)
                
        root = TrieNode('-')
        for i in range(len(wordsContainer)):
            wordsContainer[i] = wordsContainer[i][::-1]
        
        for i in range(len(wordsQuery)):
            wordsQuery[i] = wordsQuery[i][::-1]
            
        for i, word in enumerate(wordsContainer):
            addWord(root, i, word)

        def dfs(node):
            node.indices.sort(key = lambda x : (len(wordsContainer[x]), x))
            for child in node.children:
                dfs(node.children[child])

        dfs(root)

        def ans(root, word):
            curr = root
            for letter in word:
                if letter not in curr.children:
                    return curr.indices[0]
                else:
                    curr = curr.children[letter]
            return curr.indices[0]


        res = []
        for word in wordsQuery:
            res.append(ans(root, word))
        return res
```

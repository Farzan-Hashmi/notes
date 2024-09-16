another fun trie problem (just testing similarity)

link: https://leetcode.com/problems/word-break-ii/?envType=problem-list-v2&envId=trie

```python
class TrieNode:
    def __init__(self):
        self.children = [None] * 26
        self.is_word = False

class Trie:
    def __init__(self):
        # root is a dummy node
        self.root = TrieNode()
        
    def insert(self, word):
        """insert the word into Trie"""
        # alway start from root node
        n = self.root
        # traverse each character in the word
        for char in word:
            # get index of char
            index = ord(char) - ord("a")
            # to see if current node has this character in his children
            if not n.children[index]:
                # if None, we create new Trie node as a child of current node
                n.children[index] = TrieNode()
            # move node to its child
            n = n.children[index]
        n.is_word = True
        
    def search(self, word):
        """to search if there's a word in Trie"""
        n = self.find(word)
        return n is not None and n.is_word
    
    def startsWith(self, prefix):
        """return if there's a word in Tre that starts with given prefix"""
        return self.find(prefix) is not None
    
    def find(self, prefix):
        """helper function to return node with given prefix if exists. Otherwise, return None"""
        n = self.root
        for char in prefix:
            index = ord(char) - ord("a")
            if not n.children[index]:
                return None
            n = n.children[index]
        return n # return node with given prefix if exists


class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:   

        words = set(wordDict)
        # wordTree = Trie()
        # for word in words:
        #     wordTree.insert(word)

        path = []
        res = []
        
        def explore(index):
            if index == len(s):
                print('ran')
                res.append(path.copy())

            builder = ""
            for i in range(index, len(s)):
                builder += s[i]
                if builder in words:
                    path.append(builder)
                    explore(i+1)
                    path.pop()
                # if not wordTree.startsWith(builder):
                #     break

        explore(0)
        newRes = []
        for element in res:
            newRes.append(' '.join(element))
        
        return newRes

                

```
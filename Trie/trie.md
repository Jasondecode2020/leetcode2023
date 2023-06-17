### 14. Longest Common Prefix

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        cur = self.root
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
        cur.endOfWord = True
        
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        trie = Trie()
        for word in strs:
            trie.insert(word)
            
        root = trie.root
        res = ''
        while root:
            if len(root.children) > 1 or root.endOfWord:
                break
            c = list(root.children)[0]
            root = root.children[c]
            res += c
        return res
```

### 208. Implement Trie (Prefix Tree)

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False

class Trie:

    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        cur = self.root
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
        cur.endOfWord = True

    def search(self, word: str) -> bool:
        cur = self.root
        for c in word:
            if c not in cur.children:
                return False
            cur = cur.children[c]
        return cur.endOfWord

    def startsWith(self, prefix: str) -> bool:
        cur = self.root
        for c in prefix:
            if c not in cur.children:
                return False
            cur = cur.children[c]
        return True
```

### 648. Replace Words

### set

```python
class Solution:
    def replaceWords(self, dictionary: List[str], sentence: str) -> str:
        d = set(dictionary)
        def replace(word):
            for i in range(1, len(word)):
                if word[:i] in d:
                    return word[:i]
            return word
        return " ".join(map(replace, sentence.split()))
```

### Trie

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        cur = self.root
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
        cur.endOfWord = True

    def replace(self, word: str) -> bool:
        cur = self.root
        for i, c in enumerate(word):
            if c not in cur.children:
                break
            cur = cur.children[c]
            if cur.endOfWord:
                return word[: i + 1]
        return word

class Solution:
    def replaceWords(self, dictionary: List[str], sentence: str) -> str:
        trie = Trie()
        for prefix in dictionary:
            trie.insert(prefix)
        return " ".join(map(trie.replace, sentence.split()))
```

### 1268. Search Suggestions System

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.words = []
        self.n = 0
        
class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word):
        cur = self.root
        for c in word:
            if c not in cur.children: 
                cur.children[c] = TrieNode()
            cur = cur.children[c] 
            if cur.n < 3:
                cur.words.append(word)
                cur.n += 1
        
    def find(self, prefix):
        cur = self.root
        for c in prefix:
            if c not in cur.children: 
                return ''
            cur = cur.children[c] 
        return cur.words
            
class Solution:
    def suggestedProducts(self, products: List[str], searchWord: str) -> List[List[str]]:
        products.sort()
        trie = Trie()
        for word in products: 
            trie.insert(word)
        ans, cur = [], ''
        for c in searchWord:
            cur += c 
            ans.append(trie.find(cur))
        return ans    
```

### 1233. Remove Sub-Folders from the Filesystem

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        cur = self.root
        for ch in word:
            if ch not in cur.children:
                cur.children[ch] = TrieNode()
            cur = cur.children[ch]
        cur.endOfWord = True
    
    def find(self, word):
        cur = self.root
        for c in word:
            if cur.endOfWord:
                return True
            if c not in cur.children:
                return False
            cur = cur.children[c]
        return cur.endOfWord
        
class Solution:
    def removeSubfolders(self, folder: List[str]) -> List[str]:
        folder.sort()
        res, trie = [], Trie()
        for f in folder:
            chars = f.split('/')
            if not trie.find(chars):
                trie.insert(chars)
                res.append(f)
        return res
```

### 820. Short Encoding of Words

```python
class TrieNode:
    def __init__(self):
        self.children = {}

class Trie:
    def __init__(self):
        self.root = TrieNode()
        self.count = 0

    def insert(self, word):
        cur = self.root
        is_new_word = False
        for c in word:
            if c not in cur.children:
                is_new_word = True
                cur.children[c] = TrieNode()
            cur = cur.children[c]
        if is_new_word:
            self.count += 1 + len(word)
    
class Solution:
    def minimumLengthEncoding(self, words: List[str]) -> int:
        trie = Trie()
        for i in range(len(words)):
            words[i] = ''.join(reversed(list(words[i])))
        words.sort(reverse = True)
        for word in words:
            trie.insert(word)
        return trie.count
```

### 677. Map Sum Pairs (trie)

```python
class MapSum:

    def __init__(self):
        self.d = {}

    def insert(self, key: str, val: int) -> None:
        self.d[key] = val

    def sum(self, prefix: str) -> int:
        return sum(self.d[i] for i in self.d if i.startswith(prefix))
```

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.num = 0

class Trie:
    def __init__(self):
        self.root = TrieNode()
        self.d = {}

    def insert(self, word, val):
        diff = val - self.d.get(word, 0)
        self.d[word] = val
        cur = self.root
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
            cur.num += diff

    def search(self, word):
        cur = self.root
        for c in word:
            if c not in cur.children:
                return 0
            cur = cur.children[c]
        return cur.num

class MapSum:

    def __init__(self):
        self.trie = Trie()

    def insert(self, key: str, val: int) -> None:
        self.trie.insert(key, val)

    def sum(self, prefix: str) -> int:
        return self.trie.search(prefix)
```
```python
```
```python
```
```python
```
```python
```
```python
```
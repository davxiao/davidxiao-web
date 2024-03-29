---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Python3 CheatSheet"
subtitle: "My collection of python3 code snippets"
summary: "This post is my collection of python3 code snippets including string and list manipulation."
authors:
  - david-xiao
tags:
  - python
  - cheatsheet
categories:
  - Python
date: 2020-10-02
lastmod: 2020-10-02
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

TLDR;

This post is my collection of python3 code snippets. It comes in handy when manipulating string and list.

## single or double quoted string

In Python, such sequence of characters is included inside single or double quotes. There is no difference in single or double quoted string. Both can be used interchangeably.

## Remove a few elements from the beginning

Slice out the first 2 elements and return the rest.

```python
s = 'abcdef'
res = s[2:]
# res = 'cdef'
```

## Remove a few elements from the end

Slice out the last 3 elements and return the rest.

```python
s = 'abcdef'
res = s[:-3]
# res = 'abc'
```

## Retrieve the first few elements only

```python
s = 'abcdef'
res = s[:3]
# res = 'abc'
```

## Retrieve the last few elements only

```python
s = 'abcdef'
res = s[-3:]
# res = 'def'
```

## Retrieve elements from the middle

```python
s = '0123456789'
res = s[3:7]
# res = '3456'
```

## Remove single element by zero-index

Remove 'd' by its index: 3

> If one-based position is given instead of zero-based index, convert from position to index. e.g. `index = position - 1`

```python
s = 'abcdef'
res = s[:3]+s[4:]
# res = 'abcef'
```

## Remove multiple elements by zero-index

Remove 4 element by its index starting from index 3

```python
s = ['0123456789']
start_index = 3
to_cut = 4
res = s[:start_index]+s[start_index+to_cut:]
# res = '012789'
# zero-index 4 to 7 are removed
```

## Remove/Replace element by value

string version

```python
s = 'abcdef'
res = s.replace('c', 'w') # replace 'c' with 'w'
# res = 'abwdef'
```

list version

```python
s = ['a','b','c','d','e','f']
res = s
try:  
  pos = res.index('c')
except:
  pos = -1

if (pos != -1):
  res.pop(pos) #remove 'c'
  res.insert(pos, 'w') #replace with 'w'
# res = ['a', 'b', 'w', 'd', 'e', 'f']
```

## convert string to list character-wise

```python
s = 'abcdef'
res=[]
res[:0]=s
# res = ['a', 'b', 'c', 'd', 'e', 'f']
```

## convert list to string

```python
s = ['a', 'b', 'c', 'd', 'e', 'f']
res = ''.join(s)
# res = 'abcdef'
```

## Split string into word list

```python
s = 'welcome  to the  jungle'
res = s.split()
# res = ['welcome', 'to', 'the', 'jungle']
```

## Join words into a string with whitespace

```python
s = ['welcome', 'to', 'the', 'jungle']
res = ' '.join(s)
# res = 'welcome to the jungle'
```

## iterate list items to generate a new list

```python
s = ['welcome', 'to', 'the', 'jungle']
res = [x*2 for x in s]
#res = ['welcomewelcome', 'toto', 'thethe', 'junglejungle']
```

## swap case of a string (uppercase to lowercase and vice versa)

> python provides a string function that does exactly that:
>
> res = s.swapcase()

```python
def swapcase(c):
  if c.isupper():
    return c.lower()
  if c.islower():
    return c.upper()
  return c

s = 'Welcome   To The Jungle'
res = [swapcase(x) for x in s]
res = ''.join(res)
# res = 'wELCOME   tO tHE jUNGLE'
```

## produce list or string in reverse order

both string and list

```python
s = ['welcome', 'to', 'the', 'jungle']
res = s[::-1]
# res = ['jungle', 'the', 'to', 'welcome']
```

## find all occurances in a string

```python
s = 'welcome to to the jungle'
find_s = 'to'
find_len = len(find_s)

pointer = 0
while True:
  index = s.find(find_s,pointer)
  if index == -1:
    break
  print('found at index ', index)
  pointer += index
  pointer += find_len
# found at index 8
# found at index 11
```

## replace all occurances in a string

```python
s = 'welcome to to the jungle'
find_s = 'to'
replace_s = 'what'

res = s.replace(find_s, replace_s)
# res = 'welcome what what the jungle'
```

## detect duplicate items in a list

method 1: loop

```python
def has_dup(lst):
  flag = 0
  for i in range (len(lst)):
    for j in range (i+1,len(lst)):
        if (lst[i] == lst[j]):
          flag = 1
  if (flag == 1):
     return True
  else:
     return False
  
l=[1, 2, 3, 4, 5, 6]  
print(has_dup(l))
# False
```

method 2: create a temp set

`set` is a series of hashable objects, in a way it's like `dict` but the main difference between the two is that a dict item contains both key and value while a set item contains only key.

```python
s = [1,2,3,1,5,1]
res = set(s)
if (len(s) == len(res)):
  print('no duplicates found')
else:
  print('duplicates found')
# duplicates found
```

## remove duplicate words in a string

```python
def unique_list(l):
  ulist = []
  [ulist.append(x) for x in l if x not in ulist]
  return ulist

s = 'calvin klein design dress calvin klein'
res = ' '.join(unique_list(s.split()))
# res = 'calvin klein design dress'
```

## ROT13-like conversion over a string

Be aware ASCII code can be outside of printable range.

```python
s = 'This is a plaintext message'
offset = 1
res = ''.join([chr(ord(c)+offset) for c in s])
# res = 'Uijt!jt!b!qmbjoufyu!nfttbhf'
```

## Use regex to split complicated string into words

Compared to string.split() method, regex approach preserves separators in the result so that it's possible to re-construct the original string from the result.

```python
import re
s = 'Words, words, words.   '
res=re.split('([,. ]+)', s) # '(...)' enables the matched separators preserved in the result list.
# res = ['Words', ', ', 'words', ', ', 'words', '.   ', '']
```

## Use regex to search a pattern

```python
import re
s = 'Welcome to the jungle...'
match = re.search('to +the +(\w+)', s)
if match:
  res = match.group(0) # match 'to +the +(\w+)' as a whole
  # res = 'to the jungle'
  res = match.group(1) # match '(\w+)' portion
  # res = 'jungle'
else:
  print('no match found')
```

## count number of occurances of substring

string.count() approach

```python
s = 'jungle and jungle and another jungle...'
res = s.count('jungle'))
# res = 3
```

regular expression approach

```python
import re
s = 'Welcome to the jungle. It is a big jungle with many animals. Lion is the king of the jungle.'
match = re.findall('jungle', s)
if match:
  res = len(match) # return a list of string
else:
  print('no match found')
```

## escape string into html text

For HTML, it needs to escape the following:

- `<` to `&lt;`

- `>` to `&gt;`
- `&` to `&amp;`

```python
s = 'escape html string <body>&</body>'
res = s.replace('&', '&amp;').replace('>', '&gt;').replace('<', '&lt;')
# res = 'escape html string &lt;body&gt;&amp;&lt;/body&gt;'
```

## Use global variable

```python
maxlen = 0

def wordlength(x):
  global maxlen # to access global variable within a function
  if len(x) > maxlen:
    maxlen = len(x)

s = 'unsafe html string <body>&</body>'
words = s.split()
[wordlength(x) for x in words]
# maxlen = 14
```

## get both index and value when looping a list

for both string and list

```python
s = 'abcdef'

for i, value in enumerate(s):
  print ("index ", i, "value ", value)
```

## create a list of empty items

```python
res = [None]*4
# res = [None, None, None, None]

# Note this is different
res = ['']*4
# res = ['', '', '', '']
```

## determine if a list is sorted

```python
s = [1,2,3,4,5,6]
if (s == sorted(s)):
  print("sorted")
else:
  print("not sorted")
```

## looping a dict

```python
ages = {
    "Peter": 10,
    "Isabel": 11,
    "Anna": 9,
    "Thomas": 10,
    "Bob": 10,
    "Joseph": 11,
    "Maria": 12,
    "Gabriel": 10,
}

# loop to get all keys
for x in ages:
  print(x)

# loop to get all value
for x in ages:
  print(ages[x])

# loop to get both keys and values
for name, age in ages.items():
  print(name, age)
```

## nested dict

```python
students = {
    "Peter": {"age": 10, "address": "Lisbon"},
    "Isabel": {"age": 11, "address": "Sesimbra"},
    "Anna": {"age": 9, "address": "Lisbon"},
}

for p_id, p_info in students.items():
    print("\nPerson Name:", p_id)
    for key in p_info:
        print(key + ':', p_info[key])
```

## find the max value in a dict and return the key

```python
ages = {
     "Peter": 10,
     "Isabel": 11,
     "Anna": 9,
     "Thomas": 10,
     "Bob": 10,
     "Joseph": 11,
     "Maria": 12,
     "Gabriel": 10,
  }

value = list(ages.values())
key = list(ages.keys())
print (key[value.index(max(value))])
# Maria
```

## use nested list to cache multiplication results

```python
matrix = []
for i in range(10): #0-9
  row = []
  for j in range(10): #0-9
    row.append(i*j)
  matrix.append(row)


def multiply(x,y):
  try:
    return matrix[x][y]
  except:
    return x*y

print(multiply(6,9))
# 54
print(multiply(66,99))
# 6534
```

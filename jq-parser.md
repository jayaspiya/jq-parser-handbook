---
marp: true
theme: nord
---

# JQ parser

![bg left fit](./assets/jq-nord.png)

Command-line JSON processor

---

# Introduction

- jq is a lightweight and flexible command-line JSON processor.

-  jq is like `sed` for JSON data - you can use it to slice and filter and map and transform structured data with the same ease that `sed`, `awk`, `grep` and friends let you play with text.

---

## Sed

Sed is a advance text editor for filtering and transforming text input.

### Features of Sed
- Select text
- Subsitute text
- Add lines to text
- Delete lines from text
- Modify an original File 

---

## Sed Application

```bash
cat samples/zen_of_python.txt
```

![](./assets/zen-of-python-cat.png)

---
## Sed Application

```bash
sed -n '3,8p' samples/zen_of_python.txt
```

![](./assets/zen-of-python-sed.png)


---

## Sed Application

```bash
sed -n '3,8p' samples/zen_of_python.txt | sed 's/better/fancy/'
```

![](./assets/zen-of-python-sed-2.png)

---
# Getting Started

![bg right fit](./assets/jq-test.png)

```bash
sudo apt install -y jq
```

```bash
jq '.' example.json
```

---
# Selection


<!-- Absolute simplest filter: '.'
Object Identifier-Index: '.sample' -->


```bash
jq '.sample' example.json
jq '.data' example.json
jq '.data[0]' example.json
jq '.data[0].name' example.json
```

---

# Array Indexing
Indexing in `jq` is similar to like `python`
```bash
jq '.data[2:4]' example.json
```
```python
num_list = [1,2,3,4,5]
num_list[2:4]
```

---

## Array Selecting

![](./assets/array-value-iterator.png)

---

# Selecting Multiple Index

```bash
jq '.data[] | .name, .age' example.json
```

---

# Construction

### 1. Array Construction: `[]`
### 2. Object Construction: `{}`

---

# Array & Object constructor


```bash
jq '[.data[] | { name: .title, cost: .price}]' books.json
```

![](./assets/array-object-constructor.png)

---

## JQ built-in functions
Some useful functions
1. sort
2. sum
3. length
4. keys
5. type
6. min
7. max
8. unique

---

## JQ built-in functions

```bash
jq '.data | length' books.json
```

```bash
jq '[.data[].price] | max, min' books.json
```

```bash
echo [9,3,2,6] | jq 'sort'
```

```bash
 jq '.data[0] | (.price | type), (.title | type)' books.json
```
---

## Filtering

```bash
jq '.data[] | select(.price < 500)' books.json
jq '.data[] | select((.tags | length) > 2)' books.json
```

---

## Advance I

Get authors and their book count

```bash
jq '.data | group_by(.author) | .[] | {author: .[0].author, books: . | length }' books.json
```
---

## Advance II

Handle a property value with different datatype

```json
[
    {
        "author": "Nnedi Okorafor",
        "books": "Lagoon"
    },
    {
        "author": "Natasha Farrant",
        "books": ["The Children Of Castle Rock","Voyage Of The Sparrowhawk"]
    }
]
```

```bash
jq '.[].books as $books | if $ books | type == "string" then [$books] else $books end' author.json
```

---

## Advance III

Referencing value from another property

```json
{
    "books": [
        {
            "title": "Lord Of The Flies",
            "authorId": "101"
        }
    ],
    "author": {
        "101": "William Golding"
    }
}
```

```bash
jq '.author as $author | .books[] | {title, author: $author[.authorId]}' store.json
```

---

# References
- https://stedolan.github.io/jq/manual/
- https://lindevs.com/install-jq-on-ubuntu/
- http://www.compciv.org/recipes/cli/jq-for-parsing-json/
- https://earthly.dev/blog/jq-select/


---

# Thank you

### Any Questions???

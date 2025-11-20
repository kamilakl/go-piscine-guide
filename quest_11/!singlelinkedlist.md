# Quest 11 by jlokerov

## Односвязный список (Single Linked List): структура, работа, применение

Этот квест использует структуру данных **Single Linked List** (односвязный список).
Перед тем как переходить к заданиям, важно разобрать, что это за структура данных, как она работает и почему она полезна.

---

# 1. Что такое связный список?

Если абстрагироваться от программирования и представить это в жизни — вообразите цепочку из звеньев, где каждое звено знает только о следующем.

**Связный список — это структура данных, состоящая из узлов (nodes), каждый из которых хранит данные и ссылку на следующий узел.**

### Основные термины:

* **Head (голова)** — первый элемент списка.
* **Node (узел)** — элемент, который содержит данные.
* **Next** — ссылка на следующий узел в списке.
* **Tail (хвост)** — последний узел (его `Next == nil`).
* **Length** — количество элементов (не хранится автоматически: вычисляется вручную).

---

# 2. Что такое *Single* Linked List?

**Single Linked List** — это связный список, в котором каждый узел имеет ссылку *только на следующий*.

Структурно это выглядит так:

```
[Data|Next] → [Data|Next] → [Data|Next] → nil
```

В отличие от двусвязного (double linked list), у нас нет ссылки на предыдущий элемент. Это облегчает структуру, но усложняет некоторые операции.

---

# 3. Зачем нужен односвязный список?

Односвязный список — одна из базовых структур данных, которая хорошо объясняет:

* указатели
* динамические структуры
* память и хранение данных
* поиск, вставку и удаление без массива

В реальных задачах односвязные списки применяются в связных хеш-таблицах, очередях, LR-кэшах, обработке потоков, сетевых структурах.

---

# 4. Как выглядит узел в Go?

Обычно структура узла выглядит так:

```go
type Node struct {
    Data int
    Next *Node
}
```

Или если данные строковые:

```go
type Node struct {
    Data string
    Next *Node
}
```

---

# 5. Как хранится список?

В простейшем варианте список хранится через указатель на его *голову*:

```go
var head *Node
```

При этом сам список не хранит длину или хвост — только первый элемент.

---

# 6. Как работает вставка?

Вставка зависит от того, куда именно она выполняется.

---

## 6.1. Вставка в начало (push front)

Самая простая операция:

```go
func PushFront(head **Node, data int) {
    newNode := &Node{Data: data, Next: *head}
    *head = newNode
}
```

Результат:

```
New → OldHead → ... → nil
```

O(1) — мгновенно.

---

## 6.2. Вставка в конец (push back)

Чтобы попасть в конец, нужно пройти весь список.

```go
func PushBack(head **Node, data int) {
    newNode := &Node{Data: data}

    if *head == nil {
        *head = newNode
        return
    }

    current := *head
    for current.Next != nil {
        current = current.Next
    }

    current.Next = newNode
}
```

O(n) — нужно пройти весь список.

---

## 6.3. Вставка в отсортированный список (SortListInsert)

Это классическая задача из 01Edu (и вашего предыдущего квеста):

```go
func SortListInsert(head **Node, data int) {
    newNode := &Node{Data: data}

    if *head == nil || data <= (*head).Data {
        newNode.Next = *head
        *head = newNode
        return
    }

    current := *head
    for current.Next != nil && current.Next.Data < data {
        current = current.Next
    }

    newNode.Next = current.Next
    current.Next = newNode
}
```

---

# 7. Удаление элементов

## 7.1. Удаление первого элемента

```go
func PopFront(head **Node) {
    if *head != nil {
        *head = (*head).Next
    }
}
```

---

## 7.2. Удаление по значению

```go
func Remove(head **Node, data int) {
    if *head == nil {
        return
    }

    if (*head).Data == data {
        *head = (*head).Next
        return
    }

    current := *head
    for current.Next != nil && current.Next.Data != data {
        current = current.Next
    }

    if current.Next != nil {
        current.Next = current.Next.Next
    }
}
```

---

# 8. Обход списка

Чтобы вывести все элементы:

```go
func PrintList(head *Node) {
    for head != nil {
        fmt.Print(head.Data, " ")
        head = head.Next
    }
    fmt.Println()
}
```

---

# 9. Поиск элемента

```go
func Search(head *Node, value int) bool {
    for head != nil {
        if head.Data == value {
            return true
        }
        head = head.Next
    }
    return false
}
```

---

# 10. Почему односвязный список полезен студентам Go?

Потому что он отлично помогает понять:

### ✔ Указатели

Вы работаете с `*Node`, `**Node`, меняете указатели и передаёте ссылки.

### ✔ Динамические структуры

Список растёт в памяти так, как нужно — без массивов фиксированного размера.

### ✔ Алгоритмы вставки и удаления

Нужно корректно перенаправлять ссылки, чтобы не "потерять" часть списка.

### ✔ Рекурсию (если используется)

Можно реализовать многие операции рекурсивно.

### ✔ Чистоту данных

Каждый узел — отдельный объект.

---

# 11. Пример готовой реализации

```go
package main

import "fmt"

type Node struct {
    Data int
    Next *Node
}

func PushBack(head **Node, data int) {
    newNode := &Node{Data: data}

    if *head == nil {
        *head = newNode
        return
    }

    current := *head
    for current.Next != nil {
        current = current.Next
    }
    current.Next = newNode
}

func PrintList(n *Node) {
    for n != nil {
        fmt.Print(n.Data, " ")
        n = n.Next
    }
    fmt.Println()
}

func main() {
    var head *Node
    values := []int{3, 8, 1, 4}

    for _, v := range values {
        PushBack(&head, v)
    }

    PrintList(head)
}
```

Вывод:

```
3 8 1 4
```

---

# 12. Где используется SingleLinkedList в реальной практике?

* реализация очередей и стеков
* цепочки коллизий в hash-таблицах
* кэширование
* обработка потоков данных
* low-level структуры в OS и сетевых драйверах
* реализация memory-pool’ов

---

# 13. Итог

Односвязный список — это:

* простая по структуре
* но очень важная по смыслу
* практическая
* базовая
  структура данных.

Понимание SingleLinkedList развивает понимание указателей, динамической памяти и алгоритмического мышления — то, без чего трудно двигаться дальше в изучении компьютинга.


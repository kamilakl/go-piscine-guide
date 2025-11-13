# Quest 6.5 Nbrconvertalpha

## Название файла Go
`nbrconvertalpha/main.go`  

---

## Условия задания

| **Instruction (оригинал)** | **Перевод** |
|-----------------------------|--------------|
| *Write a program that prints the corresponding letter in the n position of the latin alphabet, where n is each argument received.* | Напишите программу, которая выводит букву латинского алфавита, соответствующую позиции **n**, где **n** — это каждый аргумент командной строки. |
| *For example 1 matches a, 2 matches b, etc. If n does not match a valid position of the alphabet or if the argument is not an integer, the program should print a space (" ").* | Например: 1 → a, 2 → b и т.д. Если число не валидно (не 1–26) или аргумент не является числом — выводится пробел `" "`. |
| *A flag `--upper` should be implemented. When used, the program prints the result in upper case. The flag will always be the first argument.* | Нужно реализовать флаг `--upper`. Если он указан, буквы выводятся **заглавными**. Флаг всегда идёт **первым** аргументом. |

---

## Пример
```
$ go run . 8 5 12 12 15
hello

$ go run . --upper 8 5 12 12 15
HELLO
```

---

## Решение

```go
package main

import (
	"os"
	"github.com/01-edu/z01"
)

func stringToInt(str string) (int, bool) {
	result := 0
	for _, char := range str {
		if char < '0' || char > '9' {
			return 0, false
		}
		result = result*10 + int(char-'0')
	}
	return result, true
}

func main() {
	args := os.Args[1:]
	upper := false

	if len(args) > 0 && args[0] == "--upper" {
		upper = true
		args = args[1:]
	}

	if len(args) == 0 {
		return
	}

	for _, arg := range args {
		n, valid := stringToInt(arg)
		if !valid || n < 1 || n > 26 {
			z01.PrintRune(' ')
		} else {
			letter := rune('a' + n - 1)
			if upper {
				letter = rune('A' + n - 1)
			}
			z01.PrintRune(letter)
		}
	}
	z01.PrintRune('\n')
}
```

---

## Ключевые понятия

| **Термин** | **Объяснение** |
|------------|----------------|
| **os.Args** | Содержит аргументы командной строки. `os.Args[0]` — имя программы, `os.Args[1:]` — реальные аргументы. |
| **Преобразование строки в число вручную** | Используется `stringToInt` — нельзя использовать `strconv.Atoi`. |
| **Диапазоны символов '0'–'9'** | Преобразование делается через: `int(char - '0')`. |
| **Коды латинских букв** | `'a'` → 97, `'b'` → 98 …, `'A'` → 65. Поэтому используется `'a' + n - 1`. |
| **Флаг `--upper`** | Если включён — используем `'A'` вместо `'a'`. |
| **Вывод через z01.PrintRune** | В Piscine нельзя использовать fmt.Print — только `PrintRune`. |

---

## Пошаговый разбор кода

### 1. Функция `stringToInt`
```go
func stringToInt(str string) (int, bool) {
```
Возвращает **число** и **признак успеха** (`true/false`).

---

### 2. Перебор символов строки

```go
    for _, char := range str {
```

- `char` — это `rune`.
- Перебираем каждый символ, убеждаясь, что это цифра.

---

### 3. Проверка: является ли символ цифрой
```go
if char < '0' || char > '9' {
	return 0, false
}
```

- Всё, что не в диапазоне `'0'–'9'` → ошибка.
- Немедленно возвращаем `(0, false)`.

---

### 4. Построение числа вручную
```go
result = result*10 + int(char-'0')
```

- `result*10` — сдвигаем число влево.
- `char - '0'` превращает символ в цифру.
- Например `'7' - '0' = 7`.

---

### 5. Основная функция `main`

---

### 5.1 Получение аргументов
```go
args := os.Args[1:]
```

Пропускаем имя программы.

---

### 5.2 Проверка флага `--upper`
```go
if len(args) > 0 && args[0] == "--upper" {
	upper = true
	args = args[1:]
}
```

- Если первый аргумент — `--upper`, включаем заглавный режим.
- Убираем этот аргумент из списка.

---

### 5.3 Если нет чисел — просто завершаем программу
```go
if len(args) == 0 {
	return
}
```

---

### 5.4 Обработка каждого аргумента
```go
for _, arg := range args {
```

---

### 5.5 Преобразуем строку в число
```go
n, valid := stringToInt(arg)
```

Пример:  
- `"8"` → (8, true)  
- `"abc"` → (0, false)

---

### 5.6 Проверка валидности числа

```go
if !valid || n < 1 || n > 26 {
	z01.PrintRune(' ')
}
```

Выводим пробел, если:

- аргумент не число  
- число вне диапазона 1–26

---

### 5.7 Вычисление буквы
```go
letter := rune('a' + n - 1)
```

Пример:

- n = 1 → `'a' + 0 = 'a'`
- n = 2 → `'a' + 1 = 'b'`

---

### 5.8 Если включён флаг `--upper`
```go
if upper {
	letter = rune('A' + n - 1)
}
```

---

### 5.9 Печать буквы
```go
z01.PrintRune(letter)
```

---

### 5.10 Завершающая новая строка

```go
z01.PrintRune('\n')
```

---

## Примеры

### 1. Обычный режим
```
$ go run . 12 5 7 5 14 56 4 1 18 25
legen dary
```

(56 → пробел)

### 2. Заглавные буквы
```
$ go run . --upper 8 5 12 12 15
HELLO
```

### 3. Некорректные аргументы
```
$ go run . 3 hello 2 %
c a b
```

---

## Заключение

Программа `nbrconvertalpha`:

- принимает числа как аргументы,
- переводит их в буквы латинского алфавита,
- корректно обрабатывает ошибки и неверные символы,
- поддерживает флаг `--upper` для заглавных букв,
- выводит пробел вместо неправильных значений.

Это задание учит:

- разбирать аргументы командной строки,
- работать с рунами и диапазонами символов,
- вручную конвертировать строку в число,
- корректно управлять флагами,
- печатать результат через `z01.PrintRune`.


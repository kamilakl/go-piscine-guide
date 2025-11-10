# Quest 6. flags

## Название файла Go
`flags/main.go`

---

## Условия задания 

Эта программа помогает потренироваться в работе с аргументами командной строки в Go.  
Вы сможете вставлять строки, сортировать их и выводить справку о флагах.

| **Instruction** | **Перевод** |
|---------------------------|--------------|
| Write a program that can take `--insert` (or `-i`), `--order` (or `-o`) and a `string` as arguments. | Напишите программу, которая принимает флаги `--insert` (или `-i`), `--order` (или `-o`) и строку. |
| **This program should:** | **Эта программа должна:** |
| - Insert the string given to `--insert` (or `-i`), into the main string. | - Вставить строку, указанную через `--insert` (или `-i`), в аргумент. |
| - If the flag `--order` (or `-o`) is given, order the string (ASCII order). | - Если указан `--order` (или `-o`), отсортировать строку в порядке ASCII. |
| - If no arguments or `--help` (or `-h`), print help. | - Если нет аргументов или указан `--help`, вывести справку. |

**Формат вывода:**  
Перед коротким флагом должно быть **два пробела**, перед описанием — **табуляция и пробел**.

---

**Нужно написать функцию, которая:**
- Считывает аргументы из командной строки (`os.Args`);
- Проверяет наличие флагов `--help`, `--insert`, `--order`;
- В зависимости от флагов:
  - выводит справку (`--help` / `-h`);
  - вставляет строку (`--insert` / `-i`);
  - сортирует результат (`--order` / `-o`);
- Если аргументов нет, выводит справку.

---

### Примеры
**Пример 1**
- **Ввод:** `$ go run . --insert=4321 --order asdad`
- **Вывод:** `1234aadds`

*Строка `4321` добавлена и отсортирована вместе с `asdad`.*

**Пример 2**
- **Ввод:** `$ go run . --insert=4321 asdad`
- **Вывод:** `asdad4321`

*Добавление без сортировки.*

**Пример 3**
- **Ввод:** `$ go run . asdad`
- **Вывод:** `asdad`

*Без флагов — просто вывод строки.*

**Пример 4**
- **Ввод:** `$ go run .` или `$ go run . -h` или `$ go run . --help`
- **Вывод:** 
```
--insert
  -i
         This flag inserts the string into the string passed as argument.
--order
  -o
         This flag will behave like a boolean, if it is called it will order the argument.
```

---

**Что нужно запомнить:**
- `--insert` добавляет строку;
- `--order` сортирует символы;
- `--help` показывает подсказку;
- Аргументы передаются через `os.Args`.

## Решение

```go
package main

import (
	"fmt"
	"os"

	"github.com/01-edu/z01"
)

func main() {
	args := os.Args[1:] // сразу убираем имя программы

	if len(args) == 0 || args[0] == "-h" || args[0] == "--help" {
		printHelp()
		return
	}

	insertValue := ""
	orderFlag := false
	var input string

    // Разбираем аргументы
	for _, arg := range args {
		if len(arg) > 9 && arg[:9] == "--insert=" {
			insertValue = arg[9:]
		} else if len(arg) > 3 && arg[:3] == "-i=" {
			insertValue = arg[3:]
		} else if arg == "--order" || arg == "-o" {
			orderFlag = true
		} else {
			input = arg
		}
	}

    // Добавляем вставку
	result := input + insertValue

    // Сортировка, если нужно
	if orderFlag {
		result = sortString(result)
	}

    // Вывод результата
	for _, r := range result {
		z01.PrintRune(r)
	}
	z01.PrintRune('\n')
}

// Вывод справки
func printHelp() {
	fmt.Println("--insert")
	fmt.Println("  -i")
	z01.PrintRune('\t')
	fmt.Println(" This flag inserts the string into the string passed as argument.")
	fmt.Println("--order")
	fmt.Println("  -o")
	z01.PrintRune('\t')
	fmt.Println(" This flag will behave like a boolean, if it is called it will order the argument.")
}

// Простая сортировка символов по ASCII
func sortString(s string) string {
	runes := []rune(s)
	for i := 0; i < len(runes)-1; i++ {
		for j := 0; j < len(runes)-1-i; j++ {
			if runes[j] > runes[j+1] {
				runes[j], runes[j+1] = runes[j+1], runes[j]
			}
		}
	}
	return string(runes)
}
```

---

## Ключевые понятия

| **Инструмент / элемент кода** | **Объяснение и назначение** |
|-------------------------------|------------------------------|
| **Пакет (`package main`)** | Объявляет, что программа является исполняемой. Все программы Go начинаются с `package main`. |
| **Импорт (`import (...)`)** | Позволяет использовать внешние пакеты. Здесь импортируются стандартные пакеты `fmt`, `os` и учебный пакет `github.com/01-edu/z01` для вывода символов. |
| **Функция `main()`** | Главная точка входа программы. Код внутри неё выполняется при запуске. |
| **Срез аргументов (`os.Args`)** | Содержит аргументы, переданные программе из командной строки. Первый элемент — имя программы. |
| **Условные операторы (`if`, `else if`, `else`)** | Используются для проверки условий — например, указан ли флаг `--help` или `--insert`. |
| **Цикл `for` с `range`** | Позволяет пройтись по всем аргументам или символам строки. |
| **Функции `printHelp()` и `sortString()`** | Вспомогательные функции: первая выводит справку, вторая сортирует символы в строке. |
| **Тип `rune`** | Представляет один символ Unicode. Используется при сортировке и выводе символов. |

---

## Логика и пошаговый разбор кода

### 1. Объявление пакета и импортов

```go
package main

import (
	"fmt"
	"os"
	"github.com/01-edu/z01"
)
```
**Здесь подключаются:**

- `fmt` — стандартный вывод текста;
- `os` — доступ к аргументам командной строки;
- `z01` — библиотека для посимвольного вывода (`z01.PrintRune`).

---

### 2. Главная функция программы

```go
func main() {
	args := os.Args[1:] // Убираем имя программы
```
`os.Args[1:]` — получаем все аргументы, кроме имени программы.
Пример: если запустить `go run . --insert=abc hello`, то **`args`** будет:
`["--insert=abc", "hello"]`.

---

### 3. Проверка помощи (--help)
```go
	if len(args) == 0 || args[0] == "-h" || args[0] == "--help" {
		printHelp()
		return
	}
```

Если аргументов нет или указан `-h`/`--help`, вызывается функция `printHelp()` и программа завершается.

---

### 4. Разбор аргументов

```go
	insertValue := ""
	orderFlag := false
	var input string

	for _, arg := range args {
		if len(arg) > 9 && arg[:9] == "--insert=" {
			insertValue = arg[9:]
		} else if len(arg) > 3 && arg[:3] == "-i=" {
			insertValue = arg[3:]
		} else if arg == "--order" || arg == "-o" {
			orderFlag = true
		} else {
			input = arg
		}
	}
```

**Цикл перебирает все аргументы:**
- `--insert=` или `-i=` → берётся строка для вставки;
- `--order` или `-o` → включается флаг сортировки;
- всё остальное → считается входной строкой.

**Пример:**
```bash
go run . --insert=42 -o hello
```

**Результат**
`insertValue = "42"`, `orderFlag = true`, `input = "hello"`

---

### 5. Склеивание строки

```go
	result := input + insertValue
```
Объединяет введённую строку с вставляемой (если указана).

**Пример**
`"hello" + "42" → "hello42"`

---

### 6. Сортировка строки

```go
	if orderFlag {
		result = sortString(result)
	}
```

Если флаг `--order` активен, строка сортируется по ASCII.
(Например, `"cba"` → `"abc"`.)

---

### 7. Вывод результата

```go
	for _, r := range result {
		z01.PrintRune(r)
	}
	z01.PrintRune('\n')
}
```

Каждый символ печатается с помощью `z01.PrintRune()`, чтобы программа полностью соответствовала формату тестов школы 01.

---

### 8. Функция справки
```go
func printHelp() {
	fmt.Println("--insert")
	fmt.Println("  -i")
	z01.PrintRune('\t')
	fmt.Println(" This flag inserts the string into the string passed as argument.")
	fmt.Println("--order")
	fmt.Println("  -o")
	z01.PrintRune('\t')
	fmt.Println(" This flag will behave like a boolean, if it is called it will order the argument.")
}
```

Выводит описание флагов и их коротких форм.  
Тут используется **табуляция** (`\t`) и **два пробела перед коротким флагом**, как по заданию.

---

### 9. Функция сортировки
```go
func sortString(s string) string {
	runes := []rune(s)
	for i := 0; i < len(runes)-1; i++ {
		for j := 0; j < len(runes)-1-i; j++ {
			if runes[j] > runes[j+1] {
				runes[j], runes[j+1] = runes[j+1], runes[j]
			}
		}
	}
	return string(runes)
}
```

**Простая сортировка **методом пузырька:****
- проходит по символам;
- если текущий больше следующего — меняет их местами;
- повторяет, пока всё не отсортируется.

**Пример:**  
`"go"` → `"og"` (потому что `g < o` в ASCII).

---

## Глоссарий

| **Термин** | **Понятное объяснение** |
|-------------|--------------------------|
| **Аргументы командной строки** | Данные, которые пользователь передаёт программе при запуске. |
| **Флаг** | Параметр, начинающийся с `-` или `--`, изменяющий поведение программы. |
| **Срез (`slice`)** | Массив переменной длины. Удобен для хранения аргументов. |
| **Bubble sort (пузырьковая сортировка)** | Простой способ упорядочить элементы, сравнивая пары соседних. |
| **Rune** | Символ в кодировке Unicode. В Go тип rune равен `int32`. |
| **Функция `PrintRune()`** | Выводит по одному символу за раз — используется для точного контроля вывода. |
| **Условие (`if ...`)** | Позволяет выполнять разные действия в зависимости от входных данных. |
| **Return (`return`)** | Завершает выполнение функции и возвращает значение. |

---

## Заключение

Из этого задания важно запомнить:
1. Как работать с **аргументами командной строки** через `os.Args`.
2. Как использовать **флаги** (`--insert`, `--order`) для управления поведением программы.
3. Как использовать **циклы и условия** для обработки аргументов.
4. Как сделать код понятным, разделив его на **логические функции** (`printHelp`, `sortString`).
5. Как сортировать строку вручную и выводить результат по символам.

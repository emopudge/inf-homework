# Лабораторная работа: Шифрование файлов с помощью OpenSSL
## Цель работы
Освоить основы утилиты OpenSSL для шифрования и дешифрования файлов
## Материалы
 [Официальная страница OpenSSL](https://www.openssl.org/)
 
 [Руководство по OpenSSL](https://www.openssl.org/docs/manmaster/) 
 
 [Учебник по OpenSSL](https://linuxhint.com/openssl_command/) 
 ## Простое задание
 Используя утилиту OpenSSL (_полноценная криптографическая библиотека_), зашифровать файл `secret.txt` с помощью алгоритма [AES-256](https://baofeng.ru/blog/What-is-AES-256-encryption/) (_симметричный алгоритм шифрования, то есть имеет общий ключ для шифрования и дешифрования_) в [CBC режиме](https://ru.wikipedia.org/w/index.php?title=%D0%A0%D0%B5%D0%B6%D0%B8%D0%BC_%D1%81%D1%86%D0%B5%D0%BF%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F_%D0%B1%D0%BB%D0%BE%D0%BA%D0%BE%D0%B2_%D1%88%D0%B8%D1%84%D1%80%D0%BE%D1%82%D0%B5%D0%BA%D1%81%D1%82%D0%B0&stable=1) (_Каждый блок открытого текста (кроме первого) побитово складывается по модулю 2 (операция XOR) с предыдущим результатом шифрования._), используя пароль, и затем расшифровать его.
 ## Шаги
 ### 1. Создание файла
 
 Создаем файл `secret.txt` с конфиденциальной информацией (например, ваш любимый анекдот про басистов)
 
### 2. Шифрование: 
Выполняем команду
 ```bash
   openssl aes-256-cbc -a -salt -pbkdf2 -in secret.txt -out secret.enc
 ```
* **`aes-256-cbc`**: Указание алгоритма шифрования (AES-256 в режиме CBC).  Вы можете изменить алгоритм на другой, но AES-256 — хороший выбор для сильной защиты.
* **`-a`**:  Эта опция указывает OpenSSL использовать [Base64](https://en.wikipedia.org/wiki/Base64) (_стандарт кодирования двоичных данных при помощи только 64 символов (алфавит, цифры + 2 допсимвола)_) для кодирования выходного файла.  Это делает зашифрованный файл более читаемым (без непечатных символов), но увеличивает его размер.
* **`-salt`**: Эта опция добавляет случайную [соль](https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D0%BB%D1%8C_(%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%8F)) к паролю перед использованием его для генерации ключа шифрования.  Соль делает атаку методом "словарного подбора" значительно сложнее, даже если злоумышленник знает используемый алгоритм.
* **`-pbkdf2`**:  Эта опция указывает использовать алгоритм Password-Based Key Derivation Function 2 [(PBKDF2)](https://ru.wikipedia.org/wiki/PBKDF2).  Это важная опция безопасности, которая предотвращает быстрый подбор ключа.
* **`-in secret.txt`**:  Эта опция указывает на входной файл, который нужно зашифровать. В данном случае это файл `secret.txt`.
* **`-out secret.enc`**:  Эта опция указывает на имя выходного файла, в который будет записан зашифрованный результат. В данном случае это файл `secret.enc`.

Создаем пароль от файла

### 3. Проверка: 
Убедитесь, что файл `secret.enc` создан и имеет размер, отличный от `secret.txt`. Он должен быть больше, чем у изначального файла, ведь шифрование добавляет избыточность - так мы поймем, что шифрование прошло успешно.
```bash
  ls -l test.enc
```
Вывод будет выглядеть примерно так:

```bash
-rw-r--r-- 1 пользователь группа 12345  1 Янв 00:00 test.txt
```
Здесь:
* **`12345`**:  Размер файла в байтах.

Также можно посмотреть содержимое зашифрованного файла
 ```bash
cat secret.enc
```
Например, мой выглядит вот так:

![image](https://github.com/user-attachments/assets/98c844be-b65f-47f3-9b4f-17f62d84714c)

### 4. Расшифрование. 
Выполняем команду
   ```bash
   openssl aes-256-cbc -d -a -pbkdf2 -in secret.enc -out secret.dec
   ```
* **`-d`**:  режим дешифрования
 Вводим пароль, заданный при шифровании

 **Важно!** если вводили `-a -pbkdf2` при шифровании, при дешифровке важно также их указать, иначе будет ошибка:
 ```bad magic number```

### 5. Сравнение: 
Содержимое файлов secret.txt и secret.dec должно быть идентично

## Задача на выполнение

Напишите скрипт Bash, который выполняет следующие действия:

1. Запрашивает у пользователя путь к файлу, который нужно зашифровать. Скрипт должен проверять существование файла и его доступность для чтения.  Если файл не существует или недоступен, скрипт должен выводить сообщение об ошибке и завершаться.

2. Запрашивает у пользователя пароль для шифрования.  Пароль должен быть защищен от отображения на экране (использование read -s)

3. Шифрует файл с помощью OpenSSL, используя алгоритм AES-256-CBC.  Результат шифрования должен быть сохранен в файл с расширением .enc.  Имя зашифрованного файла должно быть таким же, как у исходного, но с добавленным расширением.

Требования:

* Скрипт должен использовать OpenSSL для шифрования и дешифрования.
* Скрипт должен обрабатывать ошибки, например, неправильный пароль или проблемы с доступом к файлу.
* Скрипт должен быть хорошо документирован (комментарии).

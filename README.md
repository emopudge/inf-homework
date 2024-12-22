# Лабораторная работа: Шифрование файлов с помощью OpenSSL
## Цель работы
Освоить основы утилиты OpenSSL для шифрования и дешифрования файлов
## Материалы
 [Официальная страница OpenSSL](https://www.openssl.org/)
 
 [Руководство по OpenSSL](https://www.openssl.org/docs/manmaster/) 
 
 [Учебник по OpenSSL](https://linuxhint.com/openssl_command/) 
 ## Задание
 Зашифровать файл `secret.txt` с помощью алгоритма AES-256 в CBC режиме _((англ. Cipher Block Chaining, CBC) — один из режимов шифрования для симметричного блочного шифра с использованием механизма обратной связи)_, используя пароль, и затем расшифровать его.
 ### Шаги
 Создаем файл `secret.txt` с конфиденциальной информацией (например, ваш любимый анекдот про басистов)
 Выполняем команду
 ```bash
   openssl aes-256-cbc -salt -in test.txt -out test.enc
 ```
_где 
* **`-salt`**: Эта опция добавляет случайную соль к паролю перед использованием его для генерации ключа шифрования.  Соль делает атаку методом "словарного подбора" значительно сложнее, даже если злоумышленник знает используемый алгоритм.
* **`-in secret.txt`**:  Эта опция указывает на входной файл, который нужно зашифровать. В данном случае это файл `secret.txt`.
* **`-out secret.enc`**:  Эта опция указывает на имя выходного файла, в который будет записан зашифрованный результат. В данном случае это файл `secret.enc`._ 

# Создание SSH ключа и добавление его в TortoiseGit

1.  Качаем puttyGEN
    https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
    выбираем puttygen.exe

2. Запускаем, нажимаем кнопку Generate, не забываем трясти мышью, ждём завершения.
   Вводим проверочную фразу, подтверждаем, сохраняем публичный и приватный ключи.

3. Заходим https://gitlab.kozhindev.com/profile/keys
   Вводим текст из файла с публичным ключом (или копируем из окошка puttyGEN)

4. При клонировании репозитория (используем TortoiseGIT) включаем флажок Load Putty Key и указываем файл с приватным ключом (.ppk)
   В подтверждающем окошке вводим фразу

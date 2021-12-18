## git config мои изменения и причины
[Вернуться](./)

Актуально для винды


Итак,изначальный конфиг гита **в винде** выглядит так:

    [diff "astextplain"]
	    textconv = astextplain
    [filter "lfs"]
        clean = git-lfs clean -- %f
	    smudge = git-lfs smudge -- %f
	    process = git-lfs filter-process
	    required = true
    [http]
	    sslBackend = openssl
	    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
    [core]
	    autocrlf = true
	    symlinks = false
	    editor = \"C:\\\\Program Files\\\\Notepad++\\\\notepad++.exe\" -multiInst -notabbar -nosession -noPlugin
    [pull]
	    rebase = false
    [credential]
	    helper = manager-core
    [credential "https://dev.azure.com"]
	    useHttpPath = true
    [init]
	    defaultBranch = master

Вроде, все хорошо. Но это работает из рук вон плохо:
1. Русские имена файлов отображаются крокозябрами
1. Русские коммиты - тоже
1. По https (пользователь, пароль) на github не льется

### Имена файлов

Чтобы кириллица в именах файла правильно отображалась:

    [core]
	    quotepath = false
		
### Коммиты

Чтобы кириллица читалась в коммитах:
    
	[i18n]
	    commitencoding = cp1251

И в выводе log:
    
	[i18n]
	    logoutputencoding = cp1251

### Подключение https

Если теперь все оставить как есть, то запушить коммит на github по https не получится. Мешает секция *[http]* - грохнем ее. Заодно - грохнем все секции *[credential]* (я пока не разобрался зачем они нужны).

### Мой текущий config

    [diff "astextplain"]
	    textconv = astextplain
    [filter "lfs"]
	    clean = git-lfs clean -- %f
	    smudge = git-lfs smudge -- %f
	    process = git-lfs filter-process
	    required = true
    [core]
	    autocrlf = true
	    symlinks = false
	    editor = \"C:\\\\Program Files\\\\Notepad++\\\\notepad++.exe\" -multiInst -notabbar -nosession -noPlugin
	    quotepath = false
    [pull]
	    rebase = false
    [init]
	    defaultBranch = master
    [i18n]
	    commitencoding = cp1251
	    logoutputencoding = cp1251
		
*Поменять можно либо глобальный файл настроек либо локальный. Глобальный файл настроек находится здесь C:\Program Files\Git\etc\gitconfig, локальный в каталоге репозитория .git\config.*
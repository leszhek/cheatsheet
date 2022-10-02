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
1. ~~По https (пользователь, пароль) на github не льется~~ *Нам это и не надо*

### Имена файлов

Чтобы кириллица в именах файла правильно отображалась нужно в .gitconfig.* (локальный или глобальный) в секцию *core* добавить отключение параметра *quotepath*:

    [core]
	    quotepath = off

Либо отключить его из командной строки:

```
$ git config --global core.quotepath off
```
		
### Коммиты

А вот с коммитами - все иначе. Там править в *.gitconfig* не получится. Нужно в теминале прописать две команды:

```
$ git config --global --unset i18n.commitencoding
$ git config --global --unset i18n.logoutputencoding
```
Чтобы git сам мог управлять кодировкой.

*Поменять можно либо глобальный файл настроек либо локальный. Глобальный файл настроек находится здесь C:\Program Files\Git\etc\gitconfig, локальный в каталоге репозитория .git\config.*
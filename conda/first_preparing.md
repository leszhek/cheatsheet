## Подготовка среды conda.
[Вернуться](./)

В этом разделе все, чтобы организовать удобное рабочее место.<br>
Речь пойдет о macos (и Intel, и Silicon, о чем будут особые отметки, если инструкции отличаются).

Инструкции приведены в порядке установки на чистую платформу. Поехали.

### Подготовка

1. Устанавливаем [homebrew](./brew.md), по инструкции из этого раздела

2. Установим git и miniconda:
```
brew insatll git
brew install --cask miniconda
```

### Первичная настройка

В документации советуют оставить среду (base) нетронутой, а пользоваться другими средами.

1. Для начала conda нужно инициализировать (о чем homebrew вам обязательно напишет):
```
==> Caveats
Please run the following to setup your shell:
  conda init "$(basename "${SHELL}")"
```

Что ж - это и делаем:
```
$ conda init "$(basename "${SHELL}")"
```

Если вы не хотите, чтобы конда каждый раз грузилась по умолчанию, просто поставьте ее в "тихий режим":
```
conda config --set auto_activate_base false
```
Но тогда, для работы со средами потребуется ее активировать вручную:
```
conda activate
```

2. Настраиваем источники пакетов (добавим ветку conda-forge и сделаем ее главной)
```
$ conda config --add channels conda-forge
$ conda config --set channel_priority true
```

Эти команды запишут в файл конфигурации **~/.condarc**:

```
channels:
  - conda-forge
  - defaults
channel_priority: flexible
```
Где **channels_priority** может принимать значения:
- strict (строго)
- flexible (гибко)
- disabled (отключено)

При **strict** пакеты в каналах с более низким приоритетом не рассматриваются, если пакет с тем же именем есть в канале с более высоким приоритетом.

Благодаря **flexible** приоритету установщик может использовать каналы с более низким приоритетом для удовлетворения зависимостей, а не вызывать недопустимую ошибку.

И, наконец, в **disable** приоритет имеет версия пакета.

3. Если нужно, создаем новую среду, например - dev, и переключаемся на нее:
```
$ conda create --name dev
$ conda activate dev
```
*Думаю, что (base) не зря предлагают оставить нетронутой;

4. Устанавливаем все, что нам нужно (вручную):
```
$ conda install python=3.9.1 nbconvert=5.6.1 jinja2=3.0.3 notebook=6.4.8 jupyterlab_widgets=1.0.2 widgetsnbextension=3.5.2 jupyter_contrib_nbextensions ipywidgets=7.6.5 re2 requests numpy pandas scikit-learn matplotlib seaborn openpyxl tqdm
```

Если все пройдет правильно - при запуске jupyter notebook ошибок не будет.

Если ошибка, все же возникает - необходимо переустановить и активировать *wiidgetsnbextension*

```
$ jupyter nbextension disable widgetsnbextension --py
$ jupyter nbextension disable widgetsnbextension --py --user
$ jupyter nbextension disable widgetsnbextension --py --sys-prefix

$ jupyter nbextension uninstall --py widgetsnbextension --user
$ jupyter nbextension uninstall --py widgetsnbextension --sys-prefix

$ jupyter nbextension install --py widgetsnbextension --sys-prefix
$ jupyter nbextension enable widgetsnbextension --py --sys-prefix
```

5. И, наконец, для автоматической активации среды, прописываем в .zshrc (после блока активации conda):

```
conda activate dev  # dev - имя среды для активации
```

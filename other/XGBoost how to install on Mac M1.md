Последовательность установки основных компонентов разработки, аналитики и ML

- Инструменты разработчика
	xcode-select --install
- Homebrew
- Git (homebrew install git)
- R и RStudio
- Anaconda +R ядро и xgboost
- ну и потом - просто


Другой путь:

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
$ brew help
$ brew install cmake libomp git

Здесь ставим anaconda

$ conda install xgboost
$ python
$ pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install
```
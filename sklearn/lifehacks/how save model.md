Сохранить обученную модель - очень просто. Для этого достаточно ее серилизовать. Самый простой путь - воспользоваться [pickle](../../python/pickle.md)


Для записи моделинужно воспользоваться командами:
```
filename = 'filename.sav'
pickle.dump(model, open(filename, 'wb'))
```

Для загрузки:
```
loaded_model = pickle.load(open(filename, 'rb'))
```
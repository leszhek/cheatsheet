Сохранить обученную модель - очень просто. Для этого достаточно ее серилизовать. Самый простой путь - воспользоваться [pickle](../../python/pickle.md)


Для записи моделинужно воспользоваться командами:
<code>
filename = 'filename.sav'
pickle.dump(model, open(filename, 'wb'))</code>

Для загрузки:
<code>loaded_model = pickle.load(open(filename, 'rb'))</code>
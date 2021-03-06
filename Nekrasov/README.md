Этот скрипт позволяет искать новые статьи сотрудников организации на ИСТИНЕ https://istina.msu.ru/

По умолчанию поиск производится по организации ГАИШ МГУ (https://istina.msu.ru/organizations/department/275695/), но опционально возможен поиск по любой другой организации. Можно выбрать любую организацию на истине или любое подразделение ГАИШ (они тоже классифицируются как организации) путем запуска скрипта из cmd с ключом -r (reference) и указанием после этого ключа адреса организации на истине. Пример:

python -m istina -r https://istina.msu.ru/organizations/department/275740/

При таком запуске будет выполнен поиск по списку сотрудников отдела релятивистской астрофизики.

Скрипт сначала проходит через список сотрудников организации, по умолчанию начиная с первой страницы (то есть в алфавитном порядке), но опционально можно начинать поиск с любой страницы списка сотрудников (https://istina.msu.ru/organizations/department/275695/workers/?&p=* - здесь на место звездочки вставляется номер интересующей страницы для начала поиска, по умолчанию =1). Также возможен поиск не до конца списка, а до указанной конечной страницы поиска (по умолчанию я также задал =1).

Пример вызова из cmd с указанием начальной и конечной страниц поиска (эти аргументы можно вызвать по ключам -a и -b):

python -m istina -a 2 -b 3

При таком вызове будет выполнен поиск по 2 и 3 страницам из списка сотрудников.

После получения списка сотрудников, скрипт проверяет профиль каждого сотрудника и определяет наличие новых статей, проходя их список сверху вниз.

Является ли статья новой или нет - определяется по DOI статьи, поэтому если на истине DOI не указан, то скрипт укажет на это и продолжит поиск.

"Новыми" считаются статьи из текущего месяца и из прошлого. Тем не менее по умолчанию скрипт производит поиск только по статьям из предыдущего месяца. Текущий месяц можно включить в поиск опционально (если производить поиск, например, 1-2 числа нового месяца, то поиск по текущему месяцу, логично, почти не даст результатов). Пример поиска с включением результатов из текущего месяца (за это отвечает булевый аргумент, вызываемый по ключу -c (от слова "current"), по умолчанию он =False:

python -m istina -c

Аналогично можно выключить поиск по предыдущему месяцу, за это отвечает булевый аргумент -p (от слова "previous"), по умолчанию =True:

python -m istina -c -p

Но тогда надо не забыть включить поиск по текущему месяцу, иначе код просто ничего не будет искать.

Можно использовать при запуске любую комбинацию ключей a, b, c, p, но надо помнить, что по логике a <= b, а если запустить код с обоими булевыми ключами False (т.е., например, просто запустить python -m istina -p), то ничего найти не удастся.

Скрипт получает метаданные статьи в формате json, поэтому если для данной статьи этот формат метаданных недоступен, то скрипт ничего из нее получить не сможет и пропустит.

После поиска данных на странице сотрудника найденные подходящие статьи добавляются в list и программа начинает поиск по следующим страницам.

В конце поиска по всем профилям учитывается то, что некоторые статьи могли быть найдены несколько раз, поэтому list из статей преобразывается в set, так мы получаем только список уникальных статей.

Пример запуска скрипта со всеми возможными ключами:

python -m istina -r https://istina.msu.ru/organizations/department/275740/ -a 1 -b 3 -c

Будет выполнен поиск по отделу релятивистской астрофизики. Всего этот отдел на истине содержит две страницы сотрудников, поэтому значение ключа b >= 2 выполняет поиск по всему отделу. В данном случае поиск будет выполнен и по текущему месяцу, и по предыдущему.

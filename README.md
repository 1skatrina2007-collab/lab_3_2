# Задание task_03_02_09.
#
# Выполнил: Павлова Е.А.
# Группа: ЦИБ-251


import random
import time


def timeit(func, *args, **kw):
    """Выполнить функицю 'func' с параметрами '*args', '**kw' и
    вернуть время выполнения в мс."""

    time_start = time.time()
    res = func(*args, **kw)
    time_end = time.time()

    return (time_end - time_start) * 1000.0, res


def is_ok_to_pass_1(db, passport_id):
    """Вернуть отметку о допуске из 'db' для паспорта с номером 'passport_id'.

    Если паспорт не найден в базе данных, возвращается отказ в допуске.

    Алгоритм:
        Последовательный перебор списка.

    Сложность: O(N).

    Параметры:
        - db (list of dict): база данных;
        - passport_id (str): номер паспорта.

    Результат:
        bool: отметка о допуске."""
    for i in db:
        if i["id"] == passport_id:
            return i["ok"]
    return False


def is_ok_to_pass_2(db_opt, passport_id):
    """Вернуть отметку о допуске из 'db_opt' для паспорта с номером 'passport_id'.

    Если паспорт не найден в базе данных, возвращается отказ в допуске.

    Алгоритм:
        Прямой доступ по ключу в словаре.

    Сложность: O(1).

    Параметры:
        - db_opt (dict): оптимизированная база данных для поиска;
        - passport_id (str): номер паспорта.

    Результат:
        bool: отметка о допуске."""
    return db_opt.get(passport_id, False)


def ok_str(flag):
    return "Да" if flag else "Нет"


if __name__ == "__main__":
    print("Загрузка базы данных... ")

    GEN_LIMIT = 1000000
    # База данных
    db = [{"ok": random.random() < 0.9, "id": "{:07d}".format(i)}
          for i in random.sample(range(GEN_LIMIT, 10 * GEN_LIMIT), GEN_LIMIT)]

    # Структура db_opt должна быть оптимизированным хранилищем
    db_opt = {i['id']: i["ok"] for i in db}

    print("Загружено записей: {:,}.\n\nПримеры:".format(GEN_LIMIT))
    print("Первый элемент:", db[0])
    print("Последний элемент:", db[-1])
    print("\n")

    passport_id = input("Введите номер паспорта (7 цифр): ").strip()
    try:
        a = int(passport_id)
        if len(passport_id) != 7:
            print('Ошибка ввода: Введите номер паспорта(число из 7 цифр)')
        else:
            output = """
                  1 способ   2 способ
    Допущен     {:>10s} {:>10s}
    Время (мс.) {:>10.2f} {:>10.2f}
    """

            res1 = timeit(is_ok_to_pass_1, db, passport_id)
            res2 = timeit(is_ok_to_pass_2, db_opt, passport_id)

            print(output.format(ok_str(res1[1]),
                                ok_str(res2[1]),
                                res1[0],
                                res2[0]))
    except ValueError:
        print('Ошибка ввода: Введите номер паспорта(число из 7 цифр)')

# -------------
# Пример вывода:
#
# Загрузка базы данных...
# Загружено записей: 1,000,000.
#
# Примеры:
# Первый элемент: {'ok': True, 'id': '4998753'}
# Последний элемент: {'ok': True, 'id': '6280964'}
#
#
# Введите номер паспорта (7 цифр): 6280964
#
#               1 способ   2 способ
# Допущен             Да         Да
# Время (мс.)     145.10       0.00

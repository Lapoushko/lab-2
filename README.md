# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Довгий Вадим Игоревич
- РИ210942
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity. При выполнении задания используйте видео-материалы и исходные данные, предоставленные преподавателя курса.
Ход работы:
- **Так как задание является копированием действий преподавателя на видео, то вставлять код я не вижу смысла, поскольку его много. Но в качестве доказательства моей работы, я прилагаю ниже скриншоты моих действий.**

![2022-10-11_19-35-14](https://user-images.githubusercontent.com/45125347/195133736-90cf3ef2-00da-426f-a2c9-84b4ef0386ea.png)
![2022-10-11_19-37-47](https://user-images.githubusercontent.com/45125347/195133820-47a56e48-c39b-4725-a134-4c06f54477bc.png)
![2022-10-11_19-38-31](https://user-images.githubusercontent.com/45125347/195133897-243d9f76-e96d-4265-a040-c01325cb274b.png)
![2022-10-11_19-57-30](https://user-images.githubusercontent.com/45125347/195133939-96cd001d-1194-499b-a059-59b5741e7ebb.png)

____


## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1
Ход работы:
- Для начала я решил создать новую Google-таблицу "LinearRegr". В ней будут записываться данные, которые появляются в результате работы кода. Код предоставлен ниже. Сам код взял из Лабораторной работы №1 линейной регрессии
```py
import numpy as np
import gspread

def model(a, b, x):
    return a*x+b

def func_loss(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5/num) * (np.square(prediction - y)).sum()

def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0/num) * ((prediction - y)*x).sum()
    db = (1.0/num) * ((prediction - y).sum())
    a -= Lr*da
    b -= Lr*db
    return a, b

def iter(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b


a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
Lr = 0.000001

x = np.array([3, 21, 22, 34, 54, 34, 55, 67, 89, 99])
y = np.array([2, 22, 24, 65, 79, 82, 55, 130, 150, 199])


a, b = iter(a, b, x, y, 100)
prediction = model(a, b, x)
loss = func_loss(a, b, x, y)
print(a, b, loss)
# plt.scatter(x, y)
# plt.plot(x, prediction)
# plt.show()


gc = gspread.service_account(filename='prefab-list-365214-c1133bb62018.json')
sh = gc.open("LinearRegr")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1, 11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        a, b = iter(a, b, x, y, 100)
        prediction = model(a, b, x)
        loss = func_loss(a, b, x, y)
        tempInf = str(loss)
        tempInf = tempInf.replace('.', ',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(tempInf))
        print(tempInf)
```
![2022-10-11_20-10-01](https://user-images.githubusercontent.com/45125347/195134790-bab601bd-22ee-4e91-bc56-2761d1805c33.png)

____

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2
Ход работы:
- Чтобы не переписывать код заново, я решил просто изменить скрипт, который отвечает за воспроизведение звуков в зависимости от полученных данных.

```cs
     void Update()
    {
        if (dataSet.Count == 0 || dataSet.Count <= i){
            return;
        }
        
        if (dataSet["Mon_" + i.ToString()] <= 200 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] > 200 & dataSet["Mon_" + i.ToString()] < 1000 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        
        if (dataSet["Mon_" + i.ToString()] >= 1000 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }
    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1uGsokpbfe9YWQUjgl2YnhgCUdCIHzmnJGHz_dhqLyhQ/values/Лист1?key=AIzaSyAX8ysCLFqIPEyPKk8-n1CFU4pRrW5gpiQ");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[1]));  // selectRow[2] -> selectRow[1]
        }
    }
```
![2022-10-11_20-11-58](https://user-images.githubusercontent.com/45125347/195135015-bdceae81-d929-4435-a6cd-8b3f69f4eee4.png)

![2022-10-11_20-12-54](https://user-images.githubusercontent.com/45125347/195135027-b399b27f-eff7-403f-a8f3-04a9ff7e5b6a.png)

____

## Выводы

- Хочется сказать, что эта лабораторная работа с самого начала меня порадовала приятнейшей отсылкой на Stronghold Crusader. В ходе самой работы меня порадовала работа с сервисами, с которыми я был не знаком. И способ связать несколько языков одним проектом меня многому научил.

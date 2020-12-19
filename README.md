# Репозиторий для проекта по глубокому обучению

## О проекте

### Цель

Решить задачу извлечения определенных именованных сущностей (локаций, персон и организаций) с метриками, не сильно уступающими state-of-the-art. 

### Бейзлайн-решение

CNN/RNN/что угодно

## Данные

### Датасет

Для выполнения проекта я нашла два почти одинаковых датасета: [CoNLL2003](https://github.com/pfliu-nlp/Named-Entity-Recognition-NER-Papers/tree/master/ner_dataset/CoNLL2003) и [CoNLL++](https://github.com/ZihanWangKi/CrossWeigh/tree/master/data). Разница между ними в том, что в CoNLL++ версии 
чуть более точно размечен файл для тестирования модели. Более точная тестовая выборка – причина, по которой я использовала датасет CoNLL++ для обучения
своей модели.

### Формат

Каждая строка данных состоит из слова, частеречного тега, тега для построения синтаксических деревьев и, собственно, NER-тега. Формат разметки – BIO. Пример разметки:

```
CHINA NNP I-NP B-LOC
IN IN I-PP O
SURPRISE DT I-NP O
DEFEAT NN I-NP O
. . O O
```

### Таргет

Предсказываем NER-тег по слову.

### State-of-the-art метрика

F-мера от 90.94 до 94.3 в зависимости от датасета (CoNLL2003 / CoNLL++) и подхода.

## Результаты и итоги

### С какими проблемами я столкнулась

**1. Предсказание по токену PAD токена неименованной сущности**

Так как все последовательности в датасете разной длины, нужно было прибегнуть к паддингу. При этом токены
части паддинга предсказывались как O. Значение f-меры из-за этого было 0.44.

**Решение**: добавление токенов '<START>' и '<END>'.

[Пример из туториала PyTorch](https://pytorch.org/tutorials/beginner/nlp/advanced_tutorial.html)

### Метрика

### Итоги

## Обзор способов решения задачи


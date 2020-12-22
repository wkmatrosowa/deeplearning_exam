# Репозиторий для проекта по глубокому обучению

## О проекте

### Цель

Решить задачу извлечения определенных именованных сущностей (локаций, персон и организаций) с метриками, не сильно уступающими state-of-the-art. 

### Решение

LSTM с параметрами

| Параметр      | Значение |
| ----------- | ----------- |
| number of layers | 4  |
| hidden dimension  | 128 |
| spatial dropout   | 0.3  |
| embedding dimension   | 300  |
| embedding vocab length  | 100 000  |

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

### Метрики

| Метрика      | Значение |
| ----------- | ----------- |
| micro-F1 (on test)     | 0.86      |
| macro-F1 (on test)   | 0.56        |
| accuracy (on test)   | 0.86        |
| mean train loss   | 0.04        |
| mean test loss   | 0.05        |

### Проблемы и решение

Проблема 1. Несбалансированность тегов в датасете приводит к тому, что сеть предсказывает всё как неименованную сущность.

Решение 1: добавление нормированных весов тегов и изменение параметра reduction у функции лосса

Решение 2: добавление искуственно заданных весов и изменение параметра reduction у функции лосса – дало лучший результат.

### Итоги

## Способы решения задачи

Neural Architectures for Named Entity Recognition End-toEnd Sequence labeling via BLSTM-CNN-CRF
* [code](https://github.com/ZhixiuYe/NER-pytorch)
* [paper](https://arxiv.org/abs/1603.01354)

Hybrid semi-Markov CRF
* [code](https://github.com/ZhixiuYe/HSCRF-pytorch)
* [paper](https://www.aclweb.org/anthology/P18-2038.pdf)

LSTM-CRF Model for Named Entity Recognition (or Sequence Labeling)
* [code](https://github.com/allanj/pytorch_lstmcrf/tree/v0.2.0)
* [paper reference from authors of the code](https://arxiv.org/abs/1603.01360)

LM-LSTM-CRF
* [code](https://github.com/LiyuanLucasLiu/LM-LSTM-CRF)
* [paper](https://arxiv.org/abs/1709.04109)

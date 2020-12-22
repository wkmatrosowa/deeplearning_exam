# Репозиторий для проекта по глубокому обучению

## О проекте

### Цель

Решить задачу извлечения именованных сущностей с метриками, не сильно уступающими state-of-the-art. 

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

## Проблемы и их решение

**Несбалансированность тегов в датасете приводит к тому, что сеть предсказывает всё как неименованную сущность.**

*Решение 1*: добавление нормированных весов тегов, изменение параметра reduction у функции лосса и перемножение лосса на веса ([пример](https://discuss.pytorch.org/t/per-class-and-per-sample-weighting/25530/4)).

Это решение дало модели возможность больше предсказывать теги. Однако оно не дало хорошего прироста по метрикам и качеству.

*Решение 2*: добавление искуственно заданных весов, изменение параметра reduction у функции лосса и перемножение лосса на веса.

Поменяла нормированные веса на искусственные. Этот подход дал лучший результат.

## Результаты

| Метрика      | Значение |
| ----------- | ----------- |
| micro-F1 (on test)     | 0.86      |
| macro-F1 (on test)   | 0.56        |
| accuracy (on test)   | 0.86        |
| mean train loss   | 0.04        |
| mean test loss   | 0.05        |

## Способы решения задачи

Судя по тому, какие материалы я находила, можно сделать вывод, что задачу NER решать можно абсолютно по-разному: с помощью сверточных и реккурентных сетей и
их комбинаций, а также используя [Марковские случайные поля, или Марковские сети](https://habr.com/ru/post/241317/).

*Neural Architectures for Named Entity Recognition End-toEnd Sequence labeling via BiLSTM-CNN-CRF*
* [code](https://github.com/ZhixiuYe/NER-pytorch)
* [paper](https://arxiv.org/abs/1603.01354)

*Hybrid semi-Markov CRF*
* [code](https://github.com/ZhixiuYe/HSCRF-pytorch)
* [paper](https://www.aclweb.org/anthology/P18-2038.pdf)

*LSTM-CRF Model for Named Entity Recognition (or Sequence Labeling)*
* [code](https://github.com/allanj/pytorch_lstmcrf/tree/v0.2.0)
* [paper reference from the authors of the code](https://arxiv.org/abs/1603.01360)

*LM-LSTM-CRF*
* [code](https://github.com/LiyuanLucasLiu/LM-LSTM-CRF)
* [paper](https://arxiv.org/abs/1709.04109)

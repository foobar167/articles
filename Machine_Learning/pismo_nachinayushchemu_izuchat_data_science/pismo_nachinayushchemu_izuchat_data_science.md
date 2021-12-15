# Письмо начинающему изучать Data Science
Дата публикации: 31.12.2019

[Статья опубликована на Хабре](https://habr.com/ru/post/482652/)

Я бы хотел получить такое письмо три года назад, когда только начинал изучать [Data Science](https://youtu.be/xC-c7E5PK0Y) (DS). Чтобы там были необходимые ссылки на полезные материалы. Статья не претендует на полноту охвата необъятной области DS. Однако для начинающего специалиста будет полезна.

![Нейронные сети – это...](data/2019.12.27_neural_networks_are.jpg)

В DS наиболее часто используются следующие технологии:
  * свободное владение английским языком;
  * операционная система [Ubuntu](https://ubuntu.com) Linux, так исторически сложилось;
  * язык программирования [Python](https://www.python.org), но лучше [Anaconda Python](https://www.anaconda.com);
  * интегрированная среда разработки (IDE) [PyCharm](https://www.jetbrains.com/pycharm/download) , Community Edition бесплатная;
  * инфраструктура (framework) для машинного обучения (ML machine learning), глубокого обучения (DL deep learning) и создания нейросетей ([PyTorch](https://pytorch.org), [TensorFlow](https://www.tensorflow.org) и [десятки других](https://en.wikipedia.org/wiki/Comparison_of_deep-learning_software));
  * если нет своей [мощной видеокарты](https://catalog.onliner.by/videocard?desktop_gpu%5B0%5D=rtx2080ti&desktop_gpu%5Boperation%5D=union&order=price:asc) (GPU graphical processing unit), тогда следует пользоваться бесплатными облачными технологиями на основе [Jupyter Notebook](https://www.dataschool.io/cloud-services-for-jupyter-notebook/);
  * умение пользоваться распределенной системой управления версиями [Git](https://ru.wikipedia.org/wiki/Git) ([GitHub](https://github.com), [GitLab](https://about.gitlab.com), [Bitbucket](https://bitbucket.org) и т.д.);
  * иметь учетную запись на StackOverflow и [всех его ответвлениях](https://meta.stackexchange.com/questions/130524/which-stack-exchange-website-for-machine-learning-and-computational-algorithms).

Также со временем вам понадобятся множество различных дополнительных к Python библиотек и инструментов обработки изображений и данных. Их десятки. Наиболее полезные в **обработке изображений** для меня в порядке убывания важности:
  * [Virtual Environment](https://github.com/foobar167/articles/blob/master/Ubuntu/05_Virtual_environments.md) – виртуальная среда разработки для различных проектов, которая инкапсулирует в себе разные версии библиотек и инструментов.
  * [NumPy](https://numpy.org) – работа с матрицами, линейная алгебра.
  * [OpenCV](https://opencv.org) – множество различных алгоритмов для работы с изображениями.
  * [Jupyter Notebook](https://jupyter.org) – веб-приложение для разработки и выполнения программ Python в браузере и в облаке.
  * [Tensorflow-gpu](https://www.tensorflow.org/install/gpu) – конфигурация нейронных сетей и вычисления на графических картах.
  * [iPython](https://ipython.org) – более удобная консольная работа с командами Python, советую использовать её вместо консоли по-умолчанию.
  * [Matplotlib](https://matplotlib.org) – рисование графиков и диаграмм.
  * [Pillow](https://pillow.readthedocs.io/en/stable) – работа со всеми популярными форматами изображений.
  * [Pandas](https://pandas.pydata.org) – работа с данными.
  * [Re](https://docs.python.org/3/library/re.html) – регулярные выражения для работы с не-графическими данными.
  * [SciPy](https://www.scipy.org) – продвинутая работа с алгоритмами, бесплатная альтернатива программе MatLab.
  * [Scikit-learn](https://scikit-learn.org/stable) – алгоритмы машинного обучения.
  * [Scikit-image](https://scikit-image.org) – продвинутая обработка изображений.
  * [K3D](https://github.com/K3D-tools/K3D-jupyter) – работа с трехмерными графиками и изображениями в Jupyter Notebook.

Машинное обудение (ML machine learning), а особенно глубокое обучение (Deep Learning) невозможны без данных. Необходимые базы данных (датасеты, datasets) можно поискать через сервис [Google Dataset Search](https://toolbox.google.com/datasetsearch) или среди [25-ти тысяч датасетов Kaggle](https://www.kaggle.com/datasets).

![Ну, давай, покажи нам примеры](data/2019.12.31_nu_davay_pokazhi_nam_primery.jpg)

Что у меня есть:
  * [Различные программы Python](https://github.com/foobar167/junkyard). Начните с [простых скриптов](https://github.com/foobar167/junkyard/tree/master/simple_scripts) и продолжите более сложными.
  * [Набор инструкций по настройке Ubuntu Linux](https://github.com/foobar167/articles/tree/master/Ubuntu). Из них самая важная инструкция по установке и настройке [виртуальной среды](https://github.com/foobar167/articles/blob/master/Ubuntu/05_Virtual_environments.md).
  * Брошюра «[Введение в машинное обучение и искусственные нейронные сети](https://foobar167.github.io/page/vvedeniye-v-mashinnoye-obucheniye-i-iskusstvennyye-neyronnyye-seti.html)». Самые основы, которые собраны со всего Интернета, но в моем «уникальном» исполнении.
  * [Курсы и видео для начинающих](https://github.com/foobar167/articles/blob/master/Ubuntu/13_Keras_and_TensorFlow_how-tos.md#exercises), с которых стоит начинать изучать нейросети.
  * [Полезные инструменты](https://github.com/foobar167/articles/blob/master/Ubuntu/13_Keras_and_TensorFlow_how-tos.md#tools), где каждый найдет что-нибудь интересное для себя.
  * [Общий список курсов](https://github.com/foobar167/articles/blob/master/Machine_Learning/courses_on_machine_learning.md), которые прошел и которые хотел бы пройти.

Спасибо за внимание!

**Теги:** нейросети, искусственные нейронные сети, машинное обучение, распознавание изображений, machine learning, deep learning, Data Science, глубокое обучение, Python, Anaconda Python, 

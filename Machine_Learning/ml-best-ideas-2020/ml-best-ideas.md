# Ideas of the leading Machine Learning researches

![Under construction](../data/2019.09.25-under-construction-icon.png)
**Under construction...**

TODO:
- Swift
- J
- BERT
- GPT-3
- capsule network
- Jeremy Howard
- Andrew Ng


_<b>Disclaimer</b>_: ideas that were expressed in these videos may be wrong or misunderstood and incorrectly conveyed by me.

[Published on Medium]()

Article provides fresh interviews (mostly 2020) with the world's leading researchers in the field of Machine Learning. I've tried to find the most interesting ideas for thinking and developing new ML algorithms.

[Geoffrey Hinton: The Foundations of Deep Learning](https://youtu.be/zl99IZvW7rE)

![Geoffrey Hinton](data/geoffrey-hinton-photo.jpg)

* [7:57](https://youtu.be/N0ER1MC9cqM?t=477), [27:18](https://youtu.be/N0ER1MC9cqM?t=1638), [36:44](https://youtu.be/N0ER1MC9cqM?t=2204), [42:50](https://youtu.be/N0ER1MC9cqM?t=2570), [49:07](https://youtu.be/N0ER1MC9cqM?t=2829) Brain learns without a "teacher", so the future of ML is in unsupervised learning algorithms. [25:25](https://youtu.be/N0ER1MC9cqM?t=1525) For now BERT, GPT-3, capsule networks and other transformer models use unsupervised pre-training. [48:07](https://youtu.be/N0ER1MC9cqM?t=2887) So **most of the learning is unsuperfised**.
* The main idea of the [capsule network](https://youtu.be/_-RU9Yoca84) is that features are better represented as a directional vector, rather than an undirected array of numbers. According to Hinton capsule neural networks are "finally something that works well".
* [18:04](https://youtu.be/N0ER1MC9cqM?t=1084), [19:42](https://youtu.be/N0ER1MC9cqM?t=1182) Brain does not use backpropagation, at least not like in convolutional neural networks. [20:52](https://youtu.be/N0ER1MC9cqM?t=1252) There is no need to backpropagate through all the layers, but reach an agreement between neighbouring layers in the layer stack. However for now it is not better than simple greedy bottom-up learning algorithm.
* [31:55](https://youtu.be/N0ER1MC9cqM?t=1915) Neural networks are very good at recognizing textures. That's why there are adversarial examples where two things looks totally different to us, but very similar to neural net and vice versa.
* [37:01](https://youtu.be/N0ER1MC9cqM?t=2221), [38:53](https://youtu.be/N0ER1MC9cqM?t=2333) Big model (trained directly on the data) can teach smaller and faster model, that would be as good as the big model. Models that are good at sucking structure out of the data are not necessarily the same as the models that are going to be small, agile and easy to use on the cell phone.

[Jeremy Howard: fast.ai Deep Learning Courses and Research | Lex Fridman Podcast](https://youtu.be/J6XcP4JOHmk)

![Jeremy Howard](data/jeremy-howard-photo.jpg)

* Python is not the best language for ML, its "for" loops are slow. Array-oriented programming languages such as Swift and J are better.

If you want to do ML for yourself there are several useful courses worth to study. I'm a practice fan of "[eating my own dog food](https://en.wikipedia.org/wiki/Eating_your_own_dog_food)", so I have studied these courses myself in the last six months or finishing them right now.

* [Machine Learning](https://www.coursera.org/learn/machine-learning) course by [Andrew Ng](https://en.wikipedia.org/wiki/Andrew_Ng) gives math knowledge behind the stuff. 11 weeks.
* [Deep Learning with PyTorch: Zero to GANs](https://jovian.ai/learn/deep-learning-with-pytorch-zero-to-gans). 6 lessons. If you're familiar with PyTorch, then review only the last lesson about [GANs and transfer learning](https://jovian.ai/learn/deep-learning-with-pytorch-zero-to-gans/lesson/lesson-6-image-generation-using-gans).
* [Practical Deep Learning for Coders](https://course.fast.ai/videos/?lesson=1) course by [Jeremy Howard](https://en.wikipedia.org/wiki/Jeremy_Howard_(entrepreneur)). 8 lessons.
* [CNN Architectures implementations](https://www.youtube.com/playlist?list=PLaPdEEY26UXyE3UchW0C742xh542yh0yI) on Keras with the [source code](https://github.com/Machine-Learning-Tokyo/CNN-Architectures/tree/master/Implementations) and [my review](https://colab.research.google.com/drive/10oWdIVyPTeF0C50OVZeTFwXJdNWMgCSw) on CoLab.

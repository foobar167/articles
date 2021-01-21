# Ideas of the leading Machine Learning researches

![Under construction](../data/2019.09.25-under-construction-icon.png)
**Under construction...**

TODO:
- [Leslie Smith paper](https://arxiv.org/abs/1708.07120)
- [Swift for TensorFlow](https://www.tensorflow.org/swift) tutorials
- J
- Julia
- BERT
- GPT-3
- capsule network
- one-shot learning
- future learning
- data generation
- selfsupervised learning
- The rise of small data or low data.


_<b>Disclaimer</b>_: ideas that were expressed in these videos may be wrong or misunderstood and incorrectly conveyed by me.

[Published on Medium]()

Article provides fresh interviews (mostly 2020) with the world's leading researchers in the field of Machine Learning. I've tried to find the most interesting ideas for thinking and developing new ML algorithms.


[Geoffrey Hinton: The Foundations of Deep Learning](https://youtu.be/zl99IZvW7rE)

![Geoffrey Hinton](data/geoffrey-hinton-photo.jpg)

* [7:57](https://youtu.be/N0ER1MC9cqM?t=477), [27:18](https://youtu.be/N0ER1MC9cqM?t=1638), [36:44](https://youtu.be/N0ER1MC9cqM?t=2204), [42:50](https://youtu.be/N0ER1MC9cqM?t=2570), [47:09](https://youtu.be/N0ER1MC9cqM?t=2829) Brain learns without a "teacher", so the future of ML is unsupervised learning. [25:25](https://youtu.be/N0ER1MC9cqM?t=1525) For now BERT, GPT-3, capsule networks and other transformer models use unsupervised pre-training. [48:07](https://youtu.be/N0ER1MC9cqM?t=2887) Most of the learning in the brain is unsuperfised.
* The main idea of the [capsule network](https://youtu.be/_-RU9Yoca84) is that features are better represented as a directional vector, rather than an undirected array of numbers. According to Hinton capsule neural networks are "finally something that works well".
* [18:04](https://youtu.be/N0ER1MC9cqM?t=1084), [19:42](https://youtu.be/N0ER1MC9cqM?t=1182) Brain does not use backpropagation, at least not like in convolutional neural networks. [20:52](https://youtu.be/N0ER1MC9cqM?t=1252) So idea of distillation networks appeared. There is no need to backpropagate through all the layers, but reach an agreement between neighbouring layers in the layer stack. Like in TCP/IP protocol: neighbouring layers communicate with each other, but don't communicate with further layers. However for now it is not better than simple greedy bottom-up learning algorithm.
* [31:55](https://youtu.be/N0ER1MC9cqM?t=1915) Neural networks are very good at recognizing textures. That's why there are adversarial examples where two things looks totally different to us, but very similar to neural net and vice versa.
* [37:01](https://youtu.be/N0ER1MC9cqM?t=2221), [38:53](https://youtu.be/N0ER1MC9cqM?t=2333) **Big model (trained directly on the data) can teach smaller and faster models**, that would be as good as the big model. Models that are good at sucking structure out of the data are not necessarily the same as the models that are going to be small, agile and easy to use on the cell phones.


[Jeremy Howard: fast.ai Deep Learning Courses and Research | Lex Fridman Podcast](https://youtu.be/J6XcP4JOHmk)

![Jeremy Howard](data/jeremy-howard-photo.jpg)

* [12:59](https://youtu.be/J6XcP4JOHmk?t=779) Python is not the future programming language for ML. Python is a lot less elegant language in nearly every way, but it has data science libraries. There are better array-oriented programming languages ([8:16](https://youtu.be/J6XcP4JOHmk?t=496)) such as [Swift](https://youtu.be/comQ1-x2a1Q) ([CoLab](https://colab.research.google.com/github/tensorflow/swift/blob/master/notebooks/blank_swift.ipynb) with Swift, [Swift for TensorFlow](https://www.tensorflow.org/swift), [Swift AI](https://github.com/Swift-AI/Swift-AI)), J and Julia.
* [24:05](https://youtu.be/J6XcP4JOHmk?t=1445) There is about 10x shortage of the number of doctors in the world. It would like to take about 300 years to train enough doctors to meet that gap. Maybe if we used deep learning for some of the analytics maybe we don't need so many highly trained doctors for diagnosis and treatment planning. [25:28](https://youtu.be/J6XcP4JOHmk?t=1528) In South Africa there is only 5 pediatric radiologists for the entire continent. The person that looks at medical imaging for kids will be a nurse at best. In India and China almost no X-rays are read by any trained professional. We need algorithms for preliminary diagnosis. They have money: developing world is not a poor world. There is an expensive diagnostic equipment, but no expertise. [28:28](https://youtu.be/J6XcP4JOHmk?t=1708) AI in medicine started from zero at 2014.
* [33:00](https://youtu.be/J6XcP4JOHmk?t=1980) One of the research areas now is **doing more with less data**: [transfer learning](https://jovian.ai/aakashns/transfer-learning-pytorch), [active learning](https://youtu.be/ANIw1Mz1SRI) ([41:49](https://youtu.be/J6XcP4JOHmk?t=2509)), cooperative learning, one-shot learning, future learning, data augmentation, data generation, selfsupervised learning, ect. The rise of small data or low data.
* [40:55](https://youtu.be/J6XcP4JOHmk?t=2455) Most of the research in deep learning world is a total waste of time :-)
* [57:01](https://youtu.be/J6XcP4JOHmk?t=3421) Computational photography: three cheap lenses plus a little bit intentional movement (take several frames) gives enough information to get excellent pixel resolution. The same thing with audio which is not done yet.
* [58:57](https://youtu.be/J6XcP4JOHmk?t=3537), [1:01:00](https://youtu.be/J6XcP4JOHmk?t=3660) Leslie Smith discovered **[super-convergence](https://arxiv.org/abs/1708.07120)**. There are certain networks that with certain settings of hyperparameters could be trained ten times faster by using ten times higher learning rate. No one published that paper, because it's not an area of academic research and it's not explained yet.
* [1:20:55](https://youtu.be/J6XcP4JOHmk?t=4855), [1:27:02](https://youtu.be/J6XcP4JOHmk?t=5222) The key differentiator between people that succeed and people that fail is tenacity.
* [1:24:37](https://youtu.be/J6XcP4JOHmk?t=5077) We don't need more experts like create slightly evolutionary research in areas that everybody is studying. We need experts at using deep learning to diagnose malaria, to analyse fisheries to identify problem areas in ocean, etc. 


[Andrew Ng: Deep Learning, Education, and Real-World AI](https://youtu.be/0jspaMLxBig)

![Andrew Ng](data/andrew-ng-photo.jpg)

* [23:33](https://youtu.be/0jspaMLxBig?t=1413) The thing we really got wrong was the early importance of unsupervised learning. No way that most of what we know is through supervised learning, but modern ML is based on supervised learning. [46:10](https://youtu.be/0jspaMLxBig?t=2770) Unsupervised learning is a beautiful idea.
* [50:54](https://youtu.be/0jspaMLxBig?t=3054) **Make learning a habit**. Just like brushing your teeth. [57:52](https://youtu.be/0jspaMLxBig?t=3472), [1:14:33](https://youtu.be/0jspaMLxBig?t=4473), [1:18:22](https://youtu.be/0jspaMLxBig?t=4702) **Start small** (article [AI Transformation Playbook](https://landing.ai/wp-content/uploads/2020/05/LandingAI_Transformation_Playbook_11-19.pdf)), but remember that the ML model is about less than 5% of the entire system.
* [1:00:32](https://youtu.be/0jspaMLxBig?t=3632), [1:03:10](https://youtu.be/0jspaMLxBig?t=3790), [career advice](https://youtu.be/733m6qBH-jI?t=1757) Try to find a good team for daily communication. Who are these 5-10 people you'll interact with every day?


## Several useful courses

If you want to do ML for yourself there are several useful courses worth to study. I'm a practice fan of "[eating my own dog food](https://en.wikipedia.org/wiki/Eating_your_own_dog_food)", so I have studied these courses myself in the last six months or finishing them right now.

* Start with [Machine Learning](https://www.coursera.org/learn/machine-learning) course by [Andrew Ng](https://en.wikipedia.org/wiki/Andrew_Ng), which gives math knowledge behind the stuff. 11 weeks.
* Continue with [Deep Learning with PyTorch: Zero to GANs](https://jovian.ai/learn/deep-learning-with-pytorch-zero-to-gans). 6 lessons. If you're familiar with [PyTorch](https://madewithml.com/courses/ml-foundations/pytorch/), then review only the last lesson about [GANs and transfer learning](https://jovian.ai/learn/deep-learning-with-pytorch-zero-to-gans/lesson/lesson-6-image-generation-using-gans).
* Deepen knowledge with [Practical Deep Learning for Coders](https://course.fast.ai/videos/?lesson=1) course by [Jeremy Howard](https://en.wikipedia.org/wiki/Jeremy_Howard_(entrepreneur\)). 8 lessons.
* Understand model architectures with [CNN Architectures implementations](https://www.youtube.com/playlist?list=PLaPdEEY26UXyE3UchW0C742xh542yh0yI) on Keras with the [source code](https://github.com/Machine-Learning-Tokyo/CNN-Architectures/tree/master/Implementations) and [my review](https://colab.research.google.com/drive/10oWdIVyPTeF0C50OVZeTFwXJdNWMgCSw) on CoLab.
* Stay in touch with newsletters: [deep learning weekly](https://www.deeplearningweekly.com) and [deep learning batch](https://www.deeplearning.ai/thebatch).


## Quick summary

Do more with less data. Use Machine Learning in your expert domain area and solve everyday life practical problems with this tool. Make learning a habit. Keep learning even if you don't understand something, because the key difference between people that succeed and people that fail is tenacity.

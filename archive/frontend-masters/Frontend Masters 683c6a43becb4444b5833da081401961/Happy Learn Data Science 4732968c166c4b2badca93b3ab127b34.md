# Happy Learn: Data Science

**#fw-happy-learn: Data Science**

# **Purpose**

To outline a learning path to learn the basics of Data Science and its associated techniques & tools

## **Approach**

This document is NOT a book and we are NOT preparing you for an exam. It is a collection of information provided by experts at Freshworks from their work experience to have a learning path that will help one learn the basics of Data Science in an incremental manner.

## **Learning Path**

Journey of learning Data Science would approximately look like:

- Learn Python (Python is a great choice due to it rich ecosystem)
- Dealing with data - collect, analyze, extract, manipulate, predict
- Understand the basics of Machine learning (supervised and unsupervised)
- Understand various Machine Learning Models
- Find realistic/realworld data sets (from platforms like Kaggle)
- Apply the learned concepts
- Advanced theory/concepts in Statistics, Probability and Linear algebra (for the Math inclined minds - you can do this step any order)

It will take a dedicated effort of about 3 months if one can spend 12-15 hours per week.

The most important thing is to stay the course and reach out when you feel progress hasn’t been made. We hope you have fun with learning Data Science *!!!*

# **Learning Data Science - Step-by-step**

Read the book [Data Science from Scratch](https://learning.oreilly.com/library/view/data-science-from/9781492041122/) by Joel Grus (Free if you have ACM/O'reilly subscription)

To make it easier for you, we have slightly modified the order:

- Chapter-1: To get a good understanding what the book is about (optional if you are impatient)
- Python:
    - Chapter-2: [A Crash Course in Python](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch02.html#python)
    - [Jupyter tutorial](https://www.dataquest.io/blog/jupyter-notebook-tutorial/) (Jupyter makes your Python learning easier, especially DS)
    - Another good [Jupyter tutorial](https://realpython.com/jupyter-notebook-introduction/)
- Data Visualisation:
    - Chapter-3: [Visualising Data](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch03.html#visualizing_data)
- Data Operations:
    - Chapter-9: [Getting Data](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch09.html#getting_data)
    - Chapter-10: [Working with Data](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch10.html#working_with_data)
    - Chapter-23: [Databases & SQL](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch23.html#databases)
- Machine learning models:
    - Chapter-11: [Machine Learning](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch11.html#machine_learning)
    - Chapter-12: [k-Nearest-Neighbours](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch12.html#nearest_neighbors)
    - Chapter-17: [Decision Trees](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch17.html#decision_trees)
- Advanced theory/concepts in Mathematics related to Data Science:
    - Chapter-4: [Linear Algebra](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch04.html#linear_algebra)
    - Chapter-5: [Statistics](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch05.html#statistics)
    - Chapter-6: [Probability](https://learning.oreilly.com/library/view/data-science-from/9781491901410/ch06.html#probability)

Chapters 7-8 are optional. However, please come back and read it when you are comfortable with other sections.

Once you finish the above items, we recommend you go through it once again quickly. This will help you understand various concepts much better. It is also recommended that you learn the concepts in Chapters 7-8

**Machine Learning Techniques**

There are two types of techniques:

1. **Supervised learning:** which trains a model on known input and output data so that it can predict future outputs
2. **Unsupervised learning**: which finds hidden patterns in input data.

### **Supervised Learning**

- Hands-on with **Linear regression** - Get a feel for implementing an ML algorithm from scratch rather than using a library
    1. [Implementing Linear regression using least squares method](https://nbviewer.jupyter.org/github/rasbt/algorithms_in_ipython_notebooks/blob/master/ipython_nbs/statistics/linregr_least_squares_fit.ipynb)
    2. [Learn some advanced ML concepts (basis functions, regularisation, etc.) using Linear regression](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/05.06-Linear-Regression.ipynb)
- **Naive Bayes classifier**
    1. [Introduction to Naive Bayes](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/05.05-Naive-Bayes.ipynb)
    2. [Recipe classification and few other examples using NB](http://jonathansoma.com/lede/foundations/classes/classification/naive-bayes/)

### **Unsupervised Learning**

- **K-means clustering**
    1. [Introduction to K-means](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/05.11-K-Means.ipynb)
    2. [More clustering examples](https://colab.research.google.com/github/saskeli/data-analysis-with-python-summer-2019/blob/master/clustering.ipynb)
- Dimensionality reduction using [Principal component analysis](https://www.kaggle.com/nirajvermafcb/principal-component-analysis-explained) (PCA)

### **Further reading**

- [https://jakevdp.github.io/PythonDataScienceHandbook/](https://jakevdp.github.io/PythonDataScienceHandbook/)

**Video Tutorials**

[Machine Learning A-Z™: Hands-On Python & R In Data Science](https://www.udemy.com/course/machinelearning/)

(R is a Programming Language that is also widely used in Statistical Computing and data mining and you may skip it and focus only on Python’s ecosystem)

**Kaggle**

[Kaggle](https://www.kaggle.com/) can be summarised as follows:

- The world's largest data science community
- Provides datasets (without data, there is no data scientist ;-))
- Conducts competitions and offers a variety of starter projects. Here are few interesting ones:
- [Classification Problem](https://www.kaggle.com/c/titanic)
- [Regression Problem](https://www.kaggle.com/c/house-prices-advanced-regression-techniques)
- [Computer Vision](https://www.kaggle.com/c/digit-recognizer)
- [Image Processing](https://www.kaggle.com/c/facial-keypoints-detection)
- [Natural Language Processing](https://www.kaggle.com/c/word2vec-nlp-tutorial)

**Tools & IDEs**

- [Jupyter Notebooks](https://jupyter.org/)
- [Virtual environments - virtualenvwrapper](https://docs.python.org/3.4/library/venv.html)
- [PyCharm](https://www.jetbrains.com/pycharm/), [Visual Studio Code](https://code.visualstudio.com/) - best IDEs for Python
- [Scipy](https://www.scipy.org/) - Python-based ecosystem of open-source software for mathematics, science, and engineering
- [Pandas](https://pandas.pydata.org/) - Data analysis & manipulation tool, built on top of the [Python](https://www.python.org/)
- [Numpy](https://numpy.org/) - Package for Scientific Computing in Python
- [Matplotlib](https://matplotlib.org/) - Library for creating static, animated, and interactive visualizations in Python
- [Scikit-learn](https://scikit-learn.org/) - Tools for predictive analysis - based on NumPy, SciPy & Matplotlib

**Blogs**

super.ai has a series of blog posts on the hidden issues in machine learning:

- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-one](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-one)
- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-two](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-two)
- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-three](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-three)
- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-four](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-four)
- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-five](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-five)
- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-six](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-six)
- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-seven](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-seven)
- [https://super.ai/blog/7-costly-surprises-of-machine-learning-part-eight](https://super.ai/blog/7-costly-surprises-of-machine-learning-part-eight)

**For help**

If you have any questions in Data Science learning, please use our slack channel #fw-learn-datascience
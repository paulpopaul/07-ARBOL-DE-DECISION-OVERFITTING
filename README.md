# 07-ARBOL-DE-DECISION-OVERFITTING

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.datasets import load_breast_cancer, load_iris
from sklearn.model_selection import train_test_split
from sklearn.tree import export_graphviz

import graphviz
import matplotlib.pyplot as plt
import numpy as np
```


```python
iris = load_iris()
```


```python
X_entrenamiento, X_test, y_entrenamiento, y_test = train_test_split(iris.data, iris.target)
```


```python
arbol = DecisionTreeClassifier()
```


```python
arbol.fit(X_entrenamiento, y_entrenamiento)
```




    DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                           max_depth=None, max_features=None, max_leaf_nodes=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=1, min_samples_split=2,
                           min_weight_fraction_leaf=0.0, presort='deprecated',
                           random_state=None, splitter='best')




```python
arbol.score(X_test, y_test)
```




    0.9210526315789473




```python
arbol.score(X_entrenamiento, y_entrenamiento)
```




    1.0




```python
export_graphviz(arbol, out_file = 'arbol.dot', class_names = iris.target_names,
               feature_names = iris.feature_names, impurity = False, filled = True)
```


```python
with open('arbol.dot') as f:
    dot_graph = f.read()
graphviz.Source(dot_graph)
```




![svg](output_8_0.svg)




```python
caract = iris.data.shape[1]

plt.barh(range(caract), arbol.feature_importances_)
plt.yticks(np.arange(caract), iris.feature_names)
plt.xlabel('Importancia de las carasteristicas')
plt.ylabel('Carasteristicas')
plt.show()
```


![png](output_9_0.png)



```python
arbol = DecisionTreeClassifier(max_depth = 5)
```
arbol.fit(X_entrenamiento, y_entrenamiento)arbol.score(X_test, y_test)arbol.score(X_entrenamiento, y_entrenamiento)

```python
n_classes = 3

plot_colors = 'bry'
plot_step = 0.02

for pairidx, pair in enumerate([[0, 1], [0, 2], [0, 3],
                                [1, 2], [1, 3], [2, 3]]):
    
    X = iris.data[:, pair]
    Y = iris.target
    
    clf = DecisionTreeClassifier(max_depth=3).fit(X, y)
    
    plt.subplot(2, 3, pairidx + 1)
    
    x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
    y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, plot_step),
                         np.arange(y_min, y_max, plot_step))
    
    Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    cs = plt.contourf(xx, yy, Z, cmap = plt.cm.Paired)
    
    plt.xlabel(iris.feature_names[pair[0]])
    plt.ylabel(iris.feature_names[pair[1]])
    plt.axis('tight')
    
    for i, color in zip(range(n_classes), plot_colors):
        idx = np.where(y == i)
        plt.scatter(X[idx, 0], X[idx, 1], c=color, label=iris.target_names[i],
                   cmap=plt.cm.Paired)
        
    plt.axis('tight')
    
plt.suptitle('Ejemplos de clasificador de arboles')
plt.legend()
plt.show()
```


![png](output_14_0.png)



```python
clf.score(X, y)
```




    0.9733333333333334




```python

```


```python

```

# Monthly_dacon_202101

Um, T. T., Babakeshizadeh, V., & Kulić, D. (2017, September). Exercise motion classification from large-scale wearable sensor data using convolutional neural networks. In 2017 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS) (pp. 2385-2390). IEEE.

데이터의 구체적인 형식은 당연히 다르지만, task의 목적이 어느정도 일치하여 참고를 많이 하였다.

### 1. 어떤 모델을 쓸 것인가?

1dcnn 사용


### 2. 주어진 데이터를 어떻게 Input으로 넣을 것인가?

![image](https://user-images.githubusercontent.com/75729975/207480682-e1f4bc84-dddc-4cd6-8d04-fb10945fe3b6.png)\
데이터가 600단위의 시간동안 기록된 형태이며, 600행별로 Label이 하나씩 부여가 되어있다.

이 데이터를 그냥 원래 형태 그대로 사용 할 것인지, 아니면 논문의 그림처럼 reshape해서 사용할 것인지, 혹은 Flatten해서 사용할 것인지를 결정하는 과정이 있었고, 결론적으론 원래 형태 그대로 사용한 것에 1dcnn을 사용한 것의 효과가 좋았다.

### 그 외의 성능 향상을 위한 노력

## Data Augmentation

운동에 관련한 데이터이므로, 일정한 주기를 가지고 규칙적으로 움직인다. 이 과제의 목적은 운동 동작 분류였는데, 분류 Label의 Imbalance 이슈가 있었다. 그렇기 때문에, 간단한 방법으로 Over sampling을 진행하였다. 예를 들어, A-B-C-D-A-D의 규칙을 가지고 움직이는 운동이 있다고 하면, 이 운동을 일반적으로 수 회 반복하므로...

A-B-C-D-A-D-A-B-C-D-A-D-A-B-C-D-A-D-A-B-C-D-A-D-A-B-C-D-A-D-A-B-C-D-A-D 등의 기본 Trend 하에서 매 행동마다 약간의 Random noise가 더해진 형태일 것이다.

데이터의 기본 size에 유의한다면, 위의 sequence에서 중간의 "D-A-D-A-B-C-D-A-D-A-B-C-D-A-D-A-B" 정도만 따로 떼어내도, 똑같은 운동을 한다고 볼 수 있다. 즉, 해당 방법으로 Classification 과정에서 Label의 Imbalance 문제를 해소할 수 있다.

(지금 생각해보면, STL decomposition 같은 방법으로 Generation하는 것도 괜찮아 보인다.)


## Soft voting ensemble

모형에 Drop out을 적용하기도 했고, 얕은 모델과 깊은 모델을 각각 만들었었다.
이것들에 대해, 각각 여러 번 추론을 반복하고 그 결과에 대해 Soft voting을 하여 결과를 도출함으로써 조금 더 일반화 성능을 높일 수 있었다.

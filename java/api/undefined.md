# 함수형 인터페이스

{% hint style="info" %}
**Function\<T, R>**\
R apply(T t);\
\- T를 인자로 받고, R을 리턴합니다.\


**Consumer\<T>**\
****void accept(T t);\
\- T를 인자로 받고, 이를 소모합니다.(리턴 X)\


**Predicate\<T>**\
boolean test(T t);\
\- T를 인자로 받고, Boolean형을 리턴합니다.\
****

**Supplier\<T>**\
****T get();\
\- T를 리턴합니다.(Lazy Evaluation)
{% endhint %}


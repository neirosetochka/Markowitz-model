# Модель Марковица с безрисковым активом
Задача Марковица - это задача формирования инвестиционного портфеля. В самом простом виде она звучит так: имеем N рисковых активов, хотим получить определенную ожидаемую доходность всего портфеля при минимальных рисках. Какие активы и в каком количестве нужно задействовать? \
По формулам [этой статьи](https://www.researchgate.net/publication/226896075_Portfolio_Selection_Theory_with_Different_Interest_Rates_for_Borrowing_and_Leading) я написала класс модели Марковица, которая умеет работать с безрисковым активом. Практически это значит, что она умеет уменьшать риски при сохранении той же доходности путем добавления к рисковым активам одного безрискового.
## Формальная постановка задачи
Каждый рисковый актив характеризуется доходностью:
$$r_j = \frac{S_j(1) - S_j(0)}{S_j(0)}.$$
Здесь $S_j(t)$ - стоимость $j$-й ценной бумаги в момент времени $t$. Мы знаем начальную стоимость бумаги, но не знаем в момент времени $1$. Поэтому это случайная величина, и у нее есть матожидание и дисперсия.\
В задаче мы хотим найти вектор $w$: 
$$w_j =  \frac{N_j S_j(0)}{S_p(0)}, \\ S_p(0) = \sum_{n=1}^{N}{N_nS_n(0)}.$$
Здесь $N_j$ - число приобретённых бумаг j-го типа (может быть отрицательным, что означает короткую позицию).
Если добавить сюда безрисковый актив, получим формальную постановку задачи, которую решает моя модель:
$$\displaystyle\min_w \hspace{1mm}\frac{1}{2}w^TVw$$
$$w^Te+(1-w^T\mathbb{I})r(w)=e_p,$$
$$r(w) = \begin{cases} r_l, & 1 - w^T\mathbb{I} >= 0,\\ r_b, & 1 - w^T\mathbb{I} < 0.\end{cases}$$
Здесь $V$ - ковариационная матрица, $e_p$ - ожидаемая доходность портфеля, которую мы хотим получить, $r_l$ и $r_b$ - ставки доходности безрискового актива в случае длинной и короткой позиций соответственно.
## Возможности модели
Модель может не только выдавать оптимальные веса, но и строить график всех решений:
<p align="center">
<img src="https://github.com/neirosetochka/Markowitz-model/assets/72963340/4aa5bbaf-bfce-4b06-8e9d-9c782cb993aa" width=60%> 
</p>
Она также хранит все промежуточные результаты и все точки касания\перелома, что может быть полезно для исследований.

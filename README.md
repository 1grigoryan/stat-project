# t-критерий Уэлча

В рамках этого проекта мы подготовили:
 * Статью на Википедии по данной теме с вычислением статистики и примерами использованияt-критерия Стьюдента и t-критерия Уэлча: https://ru.wikipedia.org/wiki/Инкубатор:T-критерий_Уэлча ;
 * Код для проведения симуляций в Python;
 * Шаблон плаката


Код для симуляций содержит в себе следующие функции:
 * Выведение сравнения результатов тестов
 * Выводение списков p-value для различного количества сэмплов при заданных параметрах
 * Построение графиков взаимозависимости p-value от количества тестов
 
```{python}
def compare_tests(n_1, n_2, mu_1, mu_2, std_1, std_2):
    """
    Выводит сравнение результатов тестов
    """
    
    X_1 = np.random.normal(mu_1, std_1, n_1)
    X_2 = np.random.normal(mu_2, std_2, n_2)
    
    return [stats.ttest_ind(X_1, X_2, equal_var=False), stats.ttest_ind(X_1, X_2)]
```

```{python}
def p_values(n_1, n_2, mu_1, mu_2, std_1, std_2):
    """
    Выводит списки p-value для различного количества сэмплов при заданных параметрах
    """
    
    
    p_values_welch = []
    p_values_student = []
    total_samples = []
    for x in range(1, 20): #тут просто считает разные размеры выборок, шаг можно поменять 
        tests = compare_tests(n_1*x, n_2*x, mu_1, mu_2, std_1, std_2)
        p_values_welch.append(tests[0][1])
        p_values_student.append(tests[1][1])
        total_samples.append(20*x)
        
    return p_values_welch, p_values_student, total_samples
```

```{python}
np.random.seed(1337)
p_values_welch, p_values_student,total_samples = p_values(10, 20, 22, 22, 1, 5)
plt.figure(figsize=(13, 8))
plt.plot(total_samples, p_values_welch, linewidth='2', color='green', label='Welch')
plt.plot(total_samples, p_values_student, linewidth='2', color='blue', label='Student')
plt.legend()
plt.xlabel('p-value')
plt.ylabel('Number of samples total')
plt.title('Relation between p-value and number of tests')
plt.savefig('relation.png')
```

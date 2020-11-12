# Pyplot: basic plot types

## Basic plot types

- graph: `plt.plot(x, y)` / `plt.plot(y)`
- bar chart: `plt.bar(x, y)`
- scatter plot: `plt.plot(x, y, ".")` / `plt.scatter(x, y, size, color)`
- histogram: `plt.hist(x)`
- box plot: `plt.boxplot(x)`
- pie chart: `plt.pie(x, labels=...)`

## Graph

Graph of associated values of x and y:

```py
plt.plot(x, y)
```

Graph with automatic x (0, 1, ...):

```py
plt.plot(y)
```

## Graph

multiple data sets:

```py
x = [1, 2, 3, 4]

y1 = [1, 2, 3, 4]
y2 = [3, 0 , 1, 0]

plt.plot(x, [y1, y2])
```

## Bar charts

```py
plt.bar(x, y, width=0.6)
plt.bar(x, y, width=1, align="edge")

plt.bar(
    [0, 1, 2],
    [9.6, 17, 9.8],
    tick_label=["China", "Russia", "USA"]
)

plt.barh([0, 1, 2], [9.6, 17, 9.8])
```

## Scatter plot

creates data points with two (or more) values - one on the x-axis and the other on the y-axis

simple:

```py
plt.plot(x, y, ".")
```

advanced:

```py
plt.scatter(x, y, size, color)
```

## Histogram

Counts the occurence of certain values / ranges

```py
plt.hist(many_simulated_dice_rolls_with_10_dice)
```

```py
plt.hist(
    many_simulated_dice_rolls_with_10_dice,
    bins=[15, 20, 25, 30, 35, 40, 45, 50, 55]
)
```

```py
plt.hist(
    many_simulated_dice_rolls_with_10_dice,
    density=True
)
```

## Box plot

Visualization of statistical data in a diagram (minimum, median, maximum, ...)

```py
plt.boxplot(dice_simulation_1)
```

```py
plt.boxplot(
    [dice_simulation_1, dice_simulation_2],
    labels=["simulation 1", "simulation 2"]
)
```

## Pie chart

```py
plt.pie([3, 10, 17, 9], labels=["a", "b", "c", "d"])

plt.pie([3, 10, 17, 9], explode=[0, 0, 0, 0.1])
```

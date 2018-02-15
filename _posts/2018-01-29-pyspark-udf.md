---
layout: post
title: How to Turn Python Functions into PySpark Functions (UDF)
comments: true
lang: en
category: technical
header-img: "/figure/source/2018-01-29-pyspark-udf/pyspark-distributed-function.png"
tags: [data-science, pyspark, spark, udf]
description: How can I distribute a Python function in PySpark to speed up the computation with the least amount of work?
---


Here's the problem: I have a Python function that iterates over my data, but going through each row in the dataframe takes several days. If I have a computing cluster with many nodes, how can I distribute this Python function in PySpark to speed up this process --- maybe cut the total time down to less than a few hours --- with the least amount of work?

In other words, how do I turn a Python function into a Spark user defined function, or *UDF*? I'll explain my solution here.

![distribute-python-function](/figure/source/2018-01-29-pyspark-udf/pyspark-distributed-function.png)

You need will Spark installed to follow this tutorial. Windows users can check out my [previous post on how to install Spark](http://changhsinlee.com/install-pyspark-windows-jupyter/).

Spark version in this post is 2.1.1, and the Jupyter notebook from this post [can be found here](https://github.com/changhsinlee/changhsinlee.github.io/blob/master/notebook/20180129-python-function-pyspark-udf/example-notebook.ipynb).

## Registering a UDF

PySpark UDFs work in a similar way as the pandas `.map()` and `.apply()` methods for pandas series and dataframes. If I have a function that can use values from a row in the dataframe as input, then I can map it to the entire dataframe. The only difference is that with PySpark UDFs I have to specify the output data type.

As an example, I will create a PySpark dataframe from a pandas dataframe.

```python
# example data
df_pd = pd.DataFrame(
    data={'integers': [1, 2, 3],
     'floats': [-1.0, 0.5, 2.7],
     'integer_arrays': [[1, 2], [3, 4, 5], [6, 7, 8, 9]]}
)
df = spark.createDataFrame(df_pd)
df.printSchema()
```

    root
     |-- floats: double (nullable = true)
     |-- integer_arrays: array (nullable = true)
     |    |-- element: long (containsNull = true)
     |-- integers: long (nullable = true)


```python
df.show()
```



    +------+--------------+--------+
    |floats|integer_arrays|integers|
    +------+--------------+--------+
    |  -1.0|        [1, 2]|       1|
    |   0.5|     [3, 4, 5]|       2|
    |   2.7|  [6, 7, 8, 9]|       3|
    +------+--------------+--------+

### Primitive type outputs

Let's say I have a python function `square()` that squares a number, and I want to register this function as a Spark UDF.

```python
def square(x):
    return x**2
```

As long as the python function's output has a corresponding data type in Spark, then I can turn it into a UDF. When registering UDFs, I have to specify the data type using the types from `pyspark.sql.types`. All the types supported by PySpark [can be found here](http://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=types#module-pyspark.sql.types).

Here's a small gotcha --- because Spark UDF doesn't convert integers to floats, unlike Python function which works for both integers and floats, a Spark UDF will return a column of NULLs if the input data type doesn't match the output data type, as in the following example.


#### Registering UDF with integer type output

```python
# Integer type output
from pyspark.sql.types import IntegerType
square_udf_int = udf(lambda z: square(z), IntegerType())
```


```python
(
    df.select('integers',
              'floats',
              square_udf_int('integers').alias('int_squared'),
              square_udf_int('floats').alias('float_squared'))
    .show()
)
```

    +--------+------+-----------+-------------+
    |integers|floats|int_squared|float_squared|
    +--------+------+-----------+-------------+
    |       1|  -1.0|          1|         null|
    |       2|   0.5|          4|         null|
    |       3|   2.7|          9|         null|
    +--------+------+-----------+-------------+


#### Registering UDF with float type output

```python
# float type output
from pyspark.sql.types import FloatType
square_udf_float = udf(lambda z: square(z), FloatType())
```


```python
(
    df.select('integers',
              'floats',
              square_udf_float('integers').alias('int_squared'),
              square_udf_float('floats').alias('float_squared'))
    .show()
)
```

    +--------+------+-----------+-------------+
    |integers|floats|int_squared|float_squared|
    +--------+------+-----------+-------------+
    |       1|  -1.0|       null|          1.0|
    |       2|   0.5|       null|         0.25|
    |       3|   2.7|       null|         7.29|
    +--------+------+-----------+-------------+


#### Specifying float type output in the Python function

Specifying the data type in the Python function output is probably the safer way. Because I usually load data into Spark from Hive tables whose schemas were made by others, specifying the return data type means the UDF should still work as intended even if the Hive schema has changed.

```python
## Force the output to be float
def square_float(x):
    return float(x**2)
square_udf_float2 = udf(lambda z: square_float(z), FloatType())
```


```python
(
    df.select('integers',
              'floats',
              square_udf_float2('integers').alias('int_squared'),
              square_udf_float2('floats').alias('float_squared'))
    .show()
)
```

    +--------+------+-----------+-------------+
    |integers|floats|int_squared|float_squared|
    +--------+------+-----------+-------------+
    |       1|  -1.0|        1.0|          1.0|
    |       2|   0.5|        4.0|         0.25|
    |       3|   2.7|        9.0|         7.29|
    +--------+------+-----------+-------------+

### Composite type outputs

If the output of the Python function is a list, then the values in the list have to be of the same type, which is specified within `ArrayType()` when registering the UDF.

```python
from pyspark.sql.types import ArrayType

def square_list(x):
    return [float(val)**2 for val in x]

square_list_udf = udf(lambda y: square_list(y), ArrayType(FloatType()))

df.select('integer_arrays', square_list_udf('integer_arrays')).show()
```

    +--------------+------------------------+
    |integer_arrays|<lambda>(integer_arrays)|
    +--------------+------------------------+
    |        [1, 2]|              [1.0, 4.0]|
    |     [3, 4, 5]|       [9.0, 16.0, 25.0]|
    |  [6, 7, 8, 9]|    [36.0, 49.0, 64.0...|
    +--------------+------------------------+


For a function that returns a tuple of mixed typed values, I can make a corresponding `StructType()`, which is a composite type in Spark, and specify what is in the struct with `StructField()`. For example, if I have a function that returns the position and the letter from `ascii_letters`,

```python
import string

def convert_ascii(number):
    return [number, string.ascii_letters[number]]

convert_ascii(1)
```


    [1, 'b']



```python
array_schema = StructType([
    StructField('number', IntegerType(), nullable=False),
    StructField('letters', StringType(), nullable=False)
])

spark_convert_ascii = udf(lambda z: convert_ascii(z), array_schema)

df_ascii = df.select('integers', spark_convert_ascii('integers').alias('ascii_map'))
df_ascii.show()
```

    +--------+---------+
    |integers|ascii_map|
    +--------+---------+
    |       1|    [1,b]|
    |       2|    [2,c]|
    |       3|    [3,d]|
    +--------+---------+


Note that the schema looks like a tree, with nullable option specified as in `StructField()`.

```python
df_ascii.printSchema()
```

    root
     |-- integers: long (nullable = true)
     |-- ascii_map: struct (nullable = true)
     |    |-- number: integer (nullable = false)
     |    |-- letters: string (nullable = false)



## Some UDF problems I've seen

### Py4JJavaError
Most of the `Py4JJavaError` exceptions I've seen came from mismatched data types between Python and Spark, especially when the function uses a data type from a python module like `numpy`. so I'd first look into that if there's an error.

For example, if the output is a `numpy.ndarray`, then the UDF throws an exception.

```python
import numpy as np

# Example data
d_np = pd.DataFrame({'int_arrays': [[1,2,3], [4,5]]})
df_np = spark.createDataFrame(d_np)
df_np.show()
```

    +----------+
    |int_arrays|
    +----------+
    | [1, 2, 3]|
    |    [4, 5]|
    +----------+




```python
# squares with a numpy function, which returns a np.ndarray
def square_array_wrong(x):
    return np.square(x)

square_array_wrong([1,2,3])
```

This function returns a `numpy.ndarray` whose values are also numpy objects `numpy.int32` instead of Python primitives.


    array([1, 4, 9], dtype=int32)


When executed, it throws a `Py4JJavaError`.

```python
spark_square_array_wrong = udf(square_array_wrong, ArrayType(FloatType()))

df_np.withColumn('doubled', spark_square_array_wrong('int_arrays')).show()
```

```
---------------------------------------------------------------------------

Py4JJavaError                             Traceback (most recent call last)

<ipython-input-32-0a6762c11818> in <module>()
----> 1 df_np.withColumn('doubled', spark_square_array_wrong('int_arrays')).show()

...

Py4JJavaError: An error occurred while calling o150.showString.
: org.apache.spark.SparkException: Job aborted due to stage failure: Task 2 in stage 15.0 failed 1 times, most recent failure: Lost task 2.0 in stage 15.0 (TID 31, localhost, executor driver): net.razorvine.pickle.PickleException: expected zero arguments for construction of ClassDict (for numpy.core.multiarray._reconstruct)

...
```

The solution is to convert it back to a list whose values are Python primitives.

```python
def square_array_right(x):
    return np.square(x).tolist()

spark_square_array_right = udf(square_array_right, ArrayType(IntegerType()))
```

Now the UDF works as intended:

```python
zz = df_np.withColumn('squared', spark_square_array_right('int_arrays'))
zz.show()
```

    +----------+---------+
    |int_arrays|  squared|
    +----------+---------+
    | [1, 2, 3]|[1, 4, 9]|
    |    [4, 5]| [16, 25]|
    +----------+---------+

### Slowness

Another problem I've seen is that the UDF takes much longer to run than its Python counterpart. In this case, I took advice from [@JnBrymn](https://twitter.com/JnBrymn) and inserted several print statements to record time between each step in the Python function.

Unlike most Spark functions, however, those `print()` runs inside each executor, so the diagnostic logs also go into the executors' stdout instead of the driver stdout, which can be accessed [under the Executors tab in Spark Web UI](https://jaceklaskowski.gitbooks.io/mastering-apache-spark/content/spark-webui-executors.html). If you are in local mode, you can find the URL for the Web UI by running

```
spark.sparkContext.uiWebUrl
```

One reason of slowness I ran into was because my data was too small in terms of file size --- when the dataframe is small enough, Spark sends the entire dataframe to one and only one executor and leave other executors waiting. In other words, Spark doesn't distributing the Python function as desired if the dataframe is too small.

To fix this, I repartitioned the dataframe before calling the UDF. For example,

```
df_repartitioned = df.repartition(100)
```

When a dataframe is repartitioned, I think each executor processes one partition at a time, and thus reduce the execution time of the PySpark function to roughly the execution time of Python function times the reciprocal of the number of executors, barring the overhead of initializing a task.

```
time_for_udf ~ time_for_python_function / number_of_executors
```

So, I'd make sure the number of partition is at least the number of executors when I submit a job.

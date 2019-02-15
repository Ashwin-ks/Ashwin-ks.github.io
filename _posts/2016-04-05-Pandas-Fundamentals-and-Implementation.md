---
layout: post
title: Pandas Fundamentals and implementation.
subtitle: Includes fundamentals of Pandas and implementation of various functions, modules, and objects included in Pandas.
---

# Data Manipulation with Pandas
Pandas is a newer package built on top of NumPy, and provides an efficient implementation of a DataFrame. DataFrames are essentially multidimensional arrays with attached row and column labels, and often with heterogeneous types and/or missing data  
As well as offering a convenient storage interface for labeled data, Pandas implements a number of powerful data operations familiar to users of both database frameworks and spreadsheet programs.

As we saw, NumPy’s ndarray data structure provides essential features for the type of clean, well-organized data typically seen in numerical computing tasks. While it serves this purpose very well, its limitations become clear when we need more flexibility (attaching labels to data, working with missing data, etc.) and when attempting operations that do not map well to element-wise broadcasting (groupings, pivots, etc.), each of which is an important piece of analyzing the less structured data available in many forms in the world around us. Pandas, and in particular its Series and DataFrame objects, builds on the NumPy array structure and provides efficient access to these sorts of “data munging” tasks that occupy much of a data scientist’s time.

Pandas objects can be thought of as enhanced versions of NumPy structured arrays in which the rows and columns are identified with labels rather than simple integer indices. As we will see during the course of this chapter, Pandas provides a host of useful tools, methods, and functionality on top of the basic data structures, but nearly everything that follows will require an understanding of what these structures are. Thus, before we go any further, let’s introduce these three fundamental Pandas data structures: the Series, DataFrame, and Index.


```python
import numpy as np
import pandas as pd
```

### The Pandas Series Object
A Pandas Series is a one-dimensional array of indexed data. It can be created from a list or array as follows:


```python
data=pd.Series([0.25,0.5,0.75,1])
data
```




    0    0.25
    1    0.50
    2    0.75
    3    1.00
    dtype: float64




```python
data.values
```




    array([ 0.25,  0.5 ,  0.75,  1.  ])




```python
data.index
```




    RangeIndex(start=0, stop=4, step=1)




```python
data[1:3]
```




    1    0.50
    2    0.75
    dtype: float64



##### SERIES AS GENERALIZED NUMPY ARRAY
it may look like the Series object is basically interchangeable with a one-dimensional NumPy array. The essential difference is the presence of the index: while the NumPy array has an implicitly defined integer index used to access the values, the Pandas Series has an explicitly defined index associated with the values.

This explicit index definition gives the Series object additional capabilities. For example, the index need not be an integer, but can consist of values of any desired type. For example, if we wish, we can use strings as an index:



```python
data=pd.Series([0.25,0.5,0.75,1],index=['a','b','c','d'])
data
```




    a    0.25
    b    0.50
    c    0.75
    d    1.00
    dtype: float64




```python
data['b']
```




    0.5




```python
data = pd.Series([0.25, 0.5, 0.75, 1.0],
                        index=[2, 5, 3, 7])
data[5]
```




    0.5



##### SERIES AS SPECIALIZED DICTIONARY
In this way, you can think of a Pandas Series a bit like a specialization of a Python dictionary. A dictionary is a structure that maps arbitrary keys to a set of arbitrary values, and a Series is a structure that maps typed keys to a set of typed values. This typing is important: just as the type-specific compiled code behind a NumPy array makes it more efficient than a Python list for certain operations, the type information of a Pandas Series makes it much more efficient than Python dictionaries for certain operations.

We can make the Series-as-dictionary analogy even more clear by constructing a Series object directly from a Python dictionary:


```python
population_dict = {'California': 38332521,
                           'Texas': 26448193,
                           'New York': 19651127,
                           'Florida': 19552860,
                           'Illinois': 12882135}
population = pd.Series(population_dict)
population
```




    California    38332521
    Florida       19552860
    Illinois      12882135
    New York      19651127
    Texas         26448193
    dtype: int64



By default, a Series will be created where the index is drawn from the sorted keys. From here, typical dictionary-style item access can be performed:


```python
population['California']
```




    38332521



Unlike a dictionary, though, the Series also supports array-style operations such as slicing:


```python
population['California':'Illinois']
```




    California    38332521
    Florida       19552860
    Illinois      12882135
    dtype: int64



##### CONSTRUCTING SERIES OBJECTS
We’ve already seen a few ways of constructing a Pandas Series from scratch; all of them are some version of the following:
>pd.Series(data, index=index)


```python
pd.Series(5, index=[100, 200, 300])
```




    100    5
    200    5
    300    5
    dtype: int64




```python
pd.Series({2:'a', 1:'b', 3:'c'})
```




    1    b
    2    a
    3    c
    dtype: object




```python
pd.Series({2:'a', 1:'b', 3:'c'}, index=[3, 2])
```




    3    c
    2    a
    dtype: object



### The Pandas DataFrame Object
The next fundamental structure in Pandas is the DataFrame. Like the Series object discussed in the previous section, the DataFrame can be thought of either as a generalization of a NumPy array, or as a specialization of a Python dictionary. We’ll now take a look at each of these perspectives.

##### DATAFRAME AS A GENERALIZED NUMPY ARRAY
If a Series is an analog of a one-dimensional array with flexible indices, a DataFrame is an analog of a two-dimensional array with both flexible row indices and flexible column names. Just as you might think of a two-dimensional array as an ordered sequence of aligned one-dimensional columns, you can think of a DataFrame as a sequence of aligned Series objects. Here, by “aligned” we mean that they share the same index.


```python
area_dict = {'California': 423967, 'Texas': 695662, 'New York': 141297,
             'Florida': 170312, 'Illinois': 149995}
area = pd.Series(area_dict)
area
```




    California    423967
    Florida       170312
    Illinois      149995
    New York      141297
    Texas         695662
    dtype: int64



Now that we have this along with the population Series from before, we can use a dictionary to construct a single two-dimensional object containing this information:


```python
states = pd.DataFrame({'population': population,
                               'area': area})
states
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
    </tr>
  </tbody>
</table>
</div>




```python
population
```




    California    38332521
    Florida       19552860
    Illinois      12882135
    New York      19651127
    Texas         26448193
    dtype: int64




```python
area
```




    California    423967
    Florida       170312
    Illinois      149995
    New York      141297
    Texas         695662
    dtype: int64




```python
states.index
```




    Index(['California', 'Florida', 'Illinois', 'New York', 'Texas'], dtype='object')




```python
states.columns
```




    Index(['area', 'population'], dtype='object')



Thus the DataFrame can be thought of as a generalization of a two-dimensional NumPy array, where both the rows and columns have a generalized index for accessing the data

##### DATAFRAME AS SPECIALIZED DICTIONARY

Similarly, we can also think of a DataFrame as a specialization of a dictionary. Where a dictionary maps a key to a value, a DataFrame maps a column name to a Series of column data. For example, asking for the 'area' attribute returns the Series object containing the areas we saw earlier:


```python
states['area']
```




    California    423967
    Florida       170312
    Illinois      149995
    New York      141297
    Texas         695662
    Name: area, dtype: int64



##### CONSTRUCTING DATAFRAME OBJECTS


```python
####1)From a single Series object

pd.DataFrame(population, columns=['population'])

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>12882135</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>19651127</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>26448193</td>
    </tr>
  </tbody>
</table>
</div>




```python
###2a From list of lists
pd.DataFrame( [ ["Michigan","Ann Arbor"], ["Michigan", "Yipsilanti"] ], 
    columns=["State","RegionName"]  )
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>RegionName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Michigan</td>
      <td>Ann Arbor</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Michigan</td>
      <td>Yipsilanti</td>
    </tr>
  </tbody>
</table>
</div>




```python
####2)From a list of dicts .Any list of dictionaries can be made into a DataFrame

data = [{'a': i, 'b': 2 * i}
                for i in range(3)]
print(data)
pd.DataFrame(data)



```

    [{'a': 0, 'b': 0}, {'a': 1, 'b': 2}, {'a': 2, 'b': 4}]
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
 pd.DataFrame([{'a': 1, 'b': 2}, {'b': 3, 'c': 4}])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>3</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
####3)From a dictionary of Series objects

pd.DataFrame({"area":area,"population":population})
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
    </tr>
  </tbody>
</table>
</div>




```python
####4)From a two-dimensional NumPy array

pd.DataFrame(np.random.rand(3, 2),
                     columns=['foo', 'bar'],
                     index=['a', 'b', 'c'])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>foo</th>
      <th>bar</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.819422</td>
      <td>0.217693</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0.521680</td>
      <td>0.138218</td>
    </tr>
    <tr>
      <th>c</th>
      <td>0.091140</td>
      <td>0.504031</td>
    </tr>
  </tbody>
</table>
</div>




```python
####5)From a NumPy structured array

A = np.zeros(3, dtype=[('A', 'i8'), ('B', 'f8')])
print(A)
pd.DataFrame(A)
```

    [(0,  0.) (0,  0.) (0,  0.)]
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



##### The Pandas Index Object
We have seen here that both the Series and DataFrame objects contain an explicit index that lets you reference and modify data. This Index object is an interesting structure in itself, and it can be thought of either as an immutable array or as an ordered set 


```python
ind = pd.Index([2, 3, 5, 7, 11])
ind
```




    Int64Index([2, 3, 5, 7, 11], dtype='int64')



##### INDEX AS IMMUTABLE ARRAY
This immutability makes it safer to share indices between multiple DataFrames and arrays, without the potential for side effects from inadvertent index modification.


```python
 ind[1],ind[::2]
```




    (3, Int64Index([2, 5, 11], dtype='int64'))




```python
print(ind.size, ind.shape, ind.ndim, ind.dtype)
```

    5 (5,) 1 int64
    

##### INDEX AS ORDERED SET
Pandas objects are designed to facilitate operations such as joins across datasets, which depend on many aspects of set arithmetic. The Index object follows many of the conventions used by Python’s built-in set data structure, so that unions, intersections, differences, and other combinations can be computed in a familiar way:


```python
indA = pd.Index([1, 3, 5, 7, 9])
indB = pd.Index([2, 3, 5, 7, 11])
```


```python
indA & indB  # intersection
```




    Int64Index([3, 5, 7], dtype='int64')




```python
indA | indB  # union
```




    Int64Index([1, 2, 3, 5, 7, 9, 11], dtype='int64')




```python
indA ^ indB  # symmetric difference
```




    Int64Index([1, 2, 9, 11], dtype='int64')



### Data Indexing and Selection
Because of this potential confusion in the case of integer indexes, Pandas provides some special indexer attributes that explicitly expose certain indexing schemes. These are not functional methods, but attributes that expose a particular slicing interface to the data in the Series.

##### INDEXERS: LOC, ILOC, AND IX
These slicing and indexing conventions can be a source of confusion. For example, if your Series has an explicit integer index, an indexing operation such as >data[1] will use the explicit indices, while a slicing operation like >data[1:3] will use the implicit Python-style index.


```python
data = pd.Series(['a', 'b', 'c'], index=[1, 3, 5])
data
```




    1    a
    3    b
    5    c
    dtype: object




```python
# explicit index when indexing
data[1]
```




    'a'




```python
# implicit index when slicing
data[1:3]
```




    3    b
    5    c
    dtype: object



Because of this potential confusion in the case of integer indexes, Pandas provides some special indexer attributes that explicitly expose certain indexing schemes. These are not functional methods, but attributes that expose a particular slicing interface to the data in the Series

First, the loc attribute allows indexing and slicing that always references the explicit index:


```python
data.loc[1]
```




    'a'




```python
data.loc[1:3]
```




    1    a
    3    b
    dtype: object



The iloc attribute allows indexing and slicing that always references the implicit Python-style index:


```python
data.iloc[1]
```




    'b'




```python
data.iloc[1:3]
```




    3    b
    5    c
    dtype: object



One guiding principle of Python code is that “explicit is better than implicit.” The explicit nature of loc and iloc make them very useful in maintaining clean and readable code; especially in the case of integer indexes

### Data Selection in DataFrame
Recall that a DataFrame acts in many ways like a two-dimensional or structured array, and in other ways like a dictionary of Series structures sharing the same index. These analogies can be helpful to keep in mind as we explore data selection within this structure.

##### DATAFRAME AS A DICTIONARY
DataFrame as a dictionary of related Series objects


```python
area = pd.Series({'California': 423967, 'Texas': 695662,
                          'New York': 141297, 'Florida': 170312,
                          'Illinois': 149995})
pop = pd.Series({'California': 38332521, 'Texas': 26448193,
                         'New York': 19651127, 'Florida': 19552860,
                         'Illinois': 12882135})
data = pd.DataFrame({'area':area, 'pop':pop})
data
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
    </tr>
  </tbody>
</table>
</div>



The individual Series that make up the columns of the DataFrame can be accessed via dictionary-style indexing of the column name:


```python
data['area']
```




    California    423967
    Florida       170312
    Illinois      149995
    New York      141297
    Texas         695662
    Name: area, dtype: int64




```python
#Equivalently, we can use attribute-style access with column names that are strings:
data.area
```




    California    423967
    Florida       170312
    Illinois      149995
    New York      141297
    Texas         695662
    Name: area, dtype: int64




```python
data.area is data['area']
```




    True




```python
data.pop is data['pop']
```




    False




```python
#this dictionary-style syntax can also be used to modify the object, in this case to add a new column:
data['density'] = data['pop'] / data['area']
#This shows a preview of the straightforward syntax of element-by-element arithmetic between Series objects
data
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
      <td>90.413926</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
      <td>38.018740</td>
    </tr>
  </tbody>
</table>
</div>



##### DATAFRAME AS TWO-DIMENSIONAL ARRAY
As mentioned previously, we can also view the DataFrame as an enhanced two-dimensional array. We can examine the raw underlying data array using the values attribute:


```python
data.values
```




    array([[  4.23967000e+05,   3.83325210e+07,   9.04139261e+01],
           [  1.70312000e+05,   1.95528600e+07,   1.14806121e+02],
           [  1.49995000e+05,   1.28821350e+07,   8.58837628e+01],
           [  1.41297000e+05,   1.96511270e+07,   1.39076746e+02],
           [  6.95662000e+05,   2.64481930e+07,   3.80187404e+01]])



With this picture in mind, we can do many familiar array-like observations on the DataFrame itself. For example, we can transpose the full DataFrame to swap rows and columns:


```python
data.T
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>California</th>
      <th>Florida</th>
      <th>Illinois</th>
      <th>New York</th>
      <th>Texas</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>area</th>
      <td>4.239670e+05</td>
      <td>1.703120e+05</td>
      <td>1.499950e+05</td>
      <td>1.412970e+05</td>
      <td>6.956620e+05</td>
    </tr>
    <tr>
      <th>pop</th>
      <td>3.833252e+07</td>
      <td>1.955286e+07</td>
      <td>1.288214e+07</td>
      <td>1.965113e+07</td>
      <td>2.644819e+07</td>
    </tr>
    <tr>
      <th>density</th>
      <td>9.041393e+01</td>
      <td>1.148061e+02</td>
      <td>8.588376e+01</td>
      <td>1.390767e+02</td>
      <td>3.801874e+01</td>
    </tr>
  </tbody>
</table>
</div>



When it comes to indexing of DataFrame objects, however, it is clear that the dictionary-style indexing of columns precludes our ability to simply treat it as a NumPy array. In particular, passing a single index to an array accesses a row:


```python
data.values[0]
```




    array([  4.23967000e+05,   3.83325210e+07,   9.04139261e+01])




```python
#and passing a single “index” to a DataFrame accesses a column:
data['area']
```




    California    423967
    Florida       170312
    Illinois      149995
    New York      141297
    Texas         695662
    Name: area, dtype: int64



Thus for array-style indexing, we need another convention. Here Pandas again uses the loc, iloc, and ix indexers mentioned earlier. Using the iloc indexer, we can index the underlying array as if it is a simple NumPy array (using the implicit Python-style index), but the DataFrame index and column labels are maintained in the result:


```python
data.iloc[:3, :2]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.loc[:'Illinois', :'pop']
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
  </tbody>
</table>
</div>




```python
#The ix indexer allows a hybrid of these two approaches:
data.ix[:3, :'pop']
```

    C:\Users\Nithin\Anaconda3\lib\site-packages\ipykernel_launcher.py:2: DeprecationWarning: 
    .ix is deprecated. Please use
    .loc for label based indexing or
    .iloc for positional indexing
    
    See the documentation here:
    http://pandas.pydata.org/pandas-docs/stable/indexing.html#ix-indexer-is-deprecated
      
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
  </tbody>
</table>
</div>



Any of the familiar NumPy-style data access patterns can be used within these indexers. For example, in the loc indexer we can combine masking and fancy indexing as in the following:


```python
data.loc[data.density > 100, ['pop', 'density']]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Florida</th>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
  </tbody>
</table>
</div>



Any of these indexing conventions may also be used to set or modify values; this is done in the standard way that you might be accustomed to from working with NumPy:


```python
data.iloc[0, 2] = 90
data
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
      <td>90.000000</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
      <td>38.018740</td>
    </tr>
  </tbody>
</table>
</div>



##### ADDITIONAL INDEXING CONVENTIONS
There are a couple extra indexing conventions that might seem at odds with the preceding discussion, but nevertheless can be very useful in practice. First, while indexing refers to columns, slicing refers to rows:


```python
data['Florida':'Illinois']
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Such slices can also refer to rows by number rather than by index:
data[1:3]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Similarly, direct masking operations are also interpreted row-wise rather than column-wise:
data[data.density > 100]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
  </tbody>
</table>
</div>



### Operating on Data in Pandas
One of the essential pieces of NumPy is the ability to perform quick element-wise operations, both with basic arithmetic (addition, subtraction, multiplication, etc.) and with more sophisticated operations (trigonometric functions, exponential and logarithmic functions, etc.). Pandas inherits much of this functionality from NumPy, and the ufuncs that we introduced in “Computation on NumPy Arrays: Universal Functions” are key to this.

Pandas includes a couple useful twists, however: for unary operations like negation and trigonometric functions, these ufuncs will preserve index and column labels in the output, and for binary operations such as addition and multiplication, Pandas will automatically align indices when passing the objects to the ufunc. This means that keeping the context of data and combining data from different sources—both potentially error-prone tasks with raw NumPy arrays—become essentially foolproof ones with Pandas. We will additionally see that there are well-defined operations between one-dimensional Series structures and two-dimensional DataFrame structures.


###### Ufuncs: Index Preservation
Because Pandas is designed to work with NumPy, any NumPy ufunc will workon Pandas Series and DataFrame objects.





```python
rng=np.random.RandomState(42)
ser=pd.Series(rng.randint(0,10,4))
ser
```




    0    6
    1    3
    2    7
    3    4
    dtype: int32




```python
df = pd.DataFrame(rng.randint(0,10,(3,4)),
                 columns=['A','B','C','D'])
df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>4</td>
      <td>3</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
np.exp(ser)
```




    0     403.428793
    1      20.085537
    2    1096.633158
    3      54.598150
    dtype: float64




```python
np.sin(df * np.pi / 4)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1.000000</td>
      <td>7.071068e-01</td>
      <td>1.000000</td>
      <td>-1.000000e+00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.707107</td>
      <td>1.224647e-16</td>
      <td>0.707107</td>
      <td>-7.071068e-01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.707107</td>
      <td>1.000000e+00</td>
      <td>-0.707107</td>
      <td>1.224647e-16</td>
    </tr>
  </tbody>
</table>
</div>



##### UFuncs: Index Alignment
For binary operations on two Series or DataFrame objects, Pandas will align indices in the process of performing the operation. This is very convenient when you are working with incomplete data


```python
#INDEX ALIGNMENT IN SERIES
area = pd.Series({'Alaska': 1723337, 'Texas': 695662,
                         'California': 423967}, name='area')
population = pd.Series({'California': 38332521, 'Texas': 26448193,
                               'New York': 19651127}, name='population')
```


```python
population / area
```




    Alaska              NaN
    California    90.413926
    New York            NaN
    Texas         38.018740
    dtype: float64



The resulting array contains the union of indices of the two input arrays, which we could determine using standard Python set arithmetic on these indices


```python
area.index | population.index
```




    Index(['Alaska', 'California', 'New York', 'Texas'], dtype='object')




```python
A = pd.Series([2, 4, 6], index=[0, 1, 2])
B = pd.Series([1, 3, 5], index=[1, 2, 3])
A + B
```




    0    NaN
    1    5.0
    2    9.0
    3    NaN
    dtype: float64




```python
A.add(B, fill_value=0)
```




    0    2.0
    1    5.0
    2    9.0
    3    5.0
    dtype: float64




```python
#INDEX ALIGNMENT IN DATAFRAME
A = pd.DataFrame(rng.randint(0, 20, (2, 2)),
                         columns=list('AB'))
A
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
B = pd.DataFrame(rng.randint(0, 10, (3, 3)),
                         columns=list('BAC'))
B
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>B</th>
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>8</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9</td>
      <td>2</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
A + B
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>15.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.0</td>
      <td>6.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Notice that indices are aligned correctly irrespective of their order in the two objects, and indices in the result are sorted. As was the case with Series, we can use the associated object’s arithmetic method and pass any desired fill_value to be used in place of missing entries. Here we’ll fill with the mean of all values in A (which we compute by first stacking the rows of A):


```python
fill=A.stack().mean()
A.add(B,fill_value=fill)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>15.0</td>
      <td>13.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.0</td>
      <td>6.0</td>
      <td>4.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6.5</td>
      <td>13.5</td>
      <td>10.5</td>
    </tr>
  </tbody>
</table>
</div>



##### Ufuncs: Operations Between DataFrame and Series
When you are performing operations between a DataFrame and a Series, the index and column alignment is similarly maintained. Operations between a DataFrame and a Series are similar to operations between a two-dimensional and one-dimensional NumPy array. Consider one common operation, where we find the difference of a two-dimensional array and one of its rows:


```python
A = rng.randint(10, size=(3, 4))
A
```




    array([[3, 8, 2, 4],
           [2, 6, 4, 8],
           [6, 1, 3, 8]])




```python
A - A[0]
```




    array([[ 0,  0,  0,  0],
           [-1, -2,  2,  4],
           [ 3, -7,  1,  4]])



According to NumPy’s broadcasting rules (see “Computation on Arrays: Broadcasting”), subtraction between a two-dimensional array and one of its rows is applied row-wise.

In Pandas, the convention similarly operates row-wise by default:


```python
df = pd.DataFrame(A,columns=list('QRST'))
df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Q</th>
      <th>R</th>
      <th>S</th>
      <th>T</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>8</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>6</td>
      <td>4</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1</td>
      <td>3</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
df -df.iloc[0]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Q</th>
      <th>R</th>
      <th>S</th>
      <th>T</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1</td>
      <td>-2</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>-7</td>
      <td>1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



If you would instead like to operate column-wise, you can use the object methods mentioned earlier, while specifying the axis keyword:


```python
df.subtract(df['R'],axis=0)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Q</th>
      <th>R</th>
      <th>S</th>
      <th>T</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-5</td>
      <td>0</td>
      <td>-6</td>
      <td>-4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-4</td>
      <td>0</td>
      <td>-2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>0</td>
      <td>2</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
halfrow = df.iloc[0, ::2]
halfrow
```




    Q    3
    S    2
    Name: 0, dtype: int32



df - halfrow

### Handling Missing Data
different data sources may indicate missing data in different ways  
how Pandas chooses to represent it, and demonstrate some built-in Pandas tools for handling missing data in Python. Here and throughout the book, we’ll refer to missing data in general as null, NaN, or NA values.

#### Missing Data in Pandas
The way in which Pandas handles missing values is constrained by its reliance on the NumPy package, which does not have a built-in notion of NA values for non-floating-point data types.  
With these constraints in mind, Pandas chose to use sentinels for missing data, and further chose to use two already-existing Python null values: the special floating-point NaN value, and the Python None object. This choice has some side effects, as we will see, but in practice ends up being a good compromise in most cases of interest.  

##### NONE: PYTHONIC MISSING DATA
The first sentinel value used by Pandas is None, a Python singleton object that is often used for missing data in Python code. Because None is a Python object, it cannot be used in any arbitrary NumPy/Pandas array, but only in arrays with data type 'object' (i.e., arrays of Python objects):


```python
vals1 = np.array([1, None, 3, 4])
vals1
```




    array([1, None, 3, 4], dtype=object)



This dtype=object means that the best common type representation NumPy could infer for the contents of the array is that they are Python objects. While this kind of object array is useful for some purposes, any operations on the data will be done at the Python level, with much more overhead than the typically fast operations seen for arrays with native types:


```python
for dtype in ["object","int"]:
    print("dtype =",dtype)
    %timeit np.arange(1E6,dtype=dtype).sum()
    print()
```

    dtype = object
    188 ms ± 5.27 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
    
    dtype = int
    6.69 ms ± 85.4 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
    
    

The use of Python objects in an array also means that if you perform aggregations like sum() or min() across an array with a None value, you will generally get an error:


```python
vals1.sum()
#This reflects the fact that addition between an integer and None is undefined.
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-82-0b91035bdd89> in <module>()
    ----> 1 vals1.sum()
          2 #This reflects the fact that addition between an integer and None is undefined.
    

    ~\Anaconda3\lib\site-packages\numpy\core\_methods.py in _sum(a, axis, dtype, out, keepdims)
         30 
         31 def _sum(a, axis=None, dtype=None, out=None, keepdims=False):
    ---> 32     return umr_sum(a, axis, dtype, out, keepdims)
         33 
         34 def _prod(a, axis=None, dtype=None, out=None, keepdims=False):
    

    TypeError: unsupported operand type(s) for +: 'int' and 'NoneType'


##### NAN: MISSING NUMERICAL DATA
The other missing data representation, NaN (acronym for Not a Number), is different; it is a special floating-point value recognized by all systems that use the standard IEEE floating-point representation:


```python
vals2 = np.array([1, np.nan, 3, 4])
vals2.dtype
```

Notice that NumPy chose a native floating-point type for this array: this means that unlike the object array from before, this array supports fast operations pushed into compiled code. You should be aware that NaN is a bit like a data virus—it infects any other object it touches. Regardless of the operation, the result of arithmetic with NaN will be another NaN:  

Note that this means that aggregates over the values are well defined (i.e., they don’t result in an error) but not always useful:


```python
vals2.sum(), vals2.min(), vals2.max()
```

NumPy does provide some special aggregations that will ignore these missing values:


```python
np.nansum(vals2), np.nanmin(vals2), np.nanmax(vals2)
```

Keep in mind that NaN is specifically a floating-point value; there is no equivalent NaN value for integers, strings, or other types.

##### NAN AND NONE IN PANDAS
NaN and None both have their place, and Pandas is built to handle the two of them nearly interchangeably, converting between them where appropriate:


```python
 pd.Series([1, np.nan, 2, None])
```


```python
x = pd.Series(range(2), dtype=int)
x
```


```python
x[0] = None
x
```

Keep in mind that in Pandas, string data is always stored with an object dtype.

##### Operating on Null Values
As we have seen, Pandas treats None and NaN as essentially interchangeable for indicating missing or null values. To facilitate this convention, there are several useful methods for detecting, removing, and replacing null values in Pandas data structures. They are:  
>isnull()  
Generate a Boolean mask indicating missing values

>notnull()  
Opposite of isnull()

>dropna()  
Return a filtered version of the data

>fillna()  
Return a copy of the data with missing values filled or imputed

##### DETECTING NULL VALUES
Pandas data structures have two useful methods for detecting null data: isnull() and notnull(). Either one will return a Boolean mask over the data. For example:


```python
data = pd.Series([1, np.nan, 'hello', None])
```


```python
data.isnull()
```


```python
data[data.notnull()]
```

The isnull() and notnull() methods produce similar Boolean results for DataFrames.

##### DROPPING NULL VALUES
In addition to the masking used before, there are the convenience methods, dropna() (which removes NA values) and fillna() (which fills in NA values). For a Series, the result is straightforward:


```python
data.dropna()
```


```python
#For a DataFrame, there are more options. Consider the following DataFrame:
df = pd.DataFrame([[1,      np.nan, 2],
                           [2,      3,      5],
                           [np.nan, 4,      6]])
df
```

We cannot drop single values from a DataFrame; we can only drop full rows or full columns. Depending on the application, you might want one or the other, so dropna() gives a number of options for a DataFrame.  

By default, dropna() will drop all rows in which any null value is present:


```python
df.dropna()
```


```python
df.dropna(axis=1)
```

But this drops some good data as well; you might rather be interested in dropping rows or columns with all NA values, or a majority of NA values. This can be specified through the how or thresh parameters, which allow fine control of the number of nulls to allow through.

The default is how='any', such that any row or column (depending on the axis keyword) containing a null value will be dropped. You can also specify how='all', which will only drop rows/columns that are all null values:


```python
df[3] = np.nan
df
```


```python
df.dropna(axis=1,how='all')
```

For finer-grained control, the thresh parameter lets you specify a minimum number of non-null values for the row/column to be kept:


```python
df.dropna(axis='rows', thresh=3)
```

##### FILLING NULL VALUES
because it is such a common operation Pandas provides the fillna() method, which returns a copy of the array with the null values replaced.



```python
data = pd.Series([1, np.nan, 2, None, 3], index=list('abcde'))
data
```


```python
data.fillna(0)
```


```python
# forward-fill
data.fillna(method='ffill')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
      <td>90.000000</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
      <td>38.018740</td>
    </tr>
  </tbody>
</table>
</div>




```python
# backward-fill
data.fillna(method='bfill')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
      <td>90.000000</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
      <td>38.018740</td>
    </tr>
  </tbody>
</table>
</div>




```python
df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Q</th>
      <th>R</th>
      <th>S</th>
      <th>T</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>8</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>6</td>
      <td>4</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1</td>
      <td>3</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.fillna(method='ffill',axis=1)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Q</th>
      <th>R</th>
      <th>S</th>
      <th>T</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>8</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>6</td>
      <td>4</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1</td>
      <td>3</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



#### Hierarchial Indexing
>Multi indexing and how to operate -- refer Python Data Science Handbook


```python
pop = pd.DataFrame([['California',2000,33871648],
                    ['California',2010,37253956],
                    ['New York',2000,18976457],
                    ['New York',2010,19378102],
                    ['Texas',2000,20851820],
                    ['Texas',2010,25145561]])
pop
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>California</td>
      <td>2000</td>
      <td>33871648</td>
    </tr>
    <tr>
      <th>1</th>
      <td>California</td>
      <td>2010</td>
      <td>37253956</td>
    </tr>
    <tr>
      <th>2</th>
      <td>New York</td>
      <td>2000</td>
      <td>18976457</td>
    </tr>
    <tr>
      <th>3</th>
      <td>New York</td>
      <td>2010</td>
      <td>19378102</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Texas</td>
      <td>2000</td>
      <td>20851820</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Texas</td>
      <td>2010</td>
      <td>25145561</td>
    </tr>
  </tbody>
</table>
</div>




```python
newcols = {
    0:'state',
    1:'year',
    2:'population'
}
```


```python
pop.rename(columns=newcols,inplace=True)
```


```python
pop
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>year</th>
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>California</td>
      <td>2000</td>
      <td>33871648</td>
    </tr>
    <tr>
      <th>1</th>
      <td>California</td>
      <td>2010</td>
      <td>37253956</td>
    </tr>
    <tr>
      <th>2</th>
      <td>New York</td>
      <td>2000</td>
      <td>18976457</td>
    </tr>
    <tr>
      <th>3</th>
      <td>New York</td>
      <td>2010</td>
      <td>19378102</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Texas</td>
      <td>2000</td>
      <td>20851820</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Texas</td>
      <td>2010</td>
      <td>25145561</td>
    </tr>
  </tbody>
</table>
</div>



Often when you are working with data in the real world, the raw input data looks like this and it’s useful to build a MultiIndex from the column values. This can be done with the set_index method of the DataFrame, which returns a multiply indexed DataFrame:


```python
pop.set_index('state',inplace=True)
```


```python
pop
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>population</th>
    </tr>
    <tr>
      <th>state</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>2000</td>
      <td>33871648</td>
    </tr>
    <tr>
      <th>California</th>
      <td>2010</td>
      <td>37253956</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>2000</td>
      <td>18976457</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>2010</td>
      <td>19378102</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>2000</td>
      <td>20851820</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>2010</td>
      <td>25145561</td>
    </tr>
  </tbody>
</table>
</div>




```python
pop.reset_index(inplace=True)
```


```python
pop.set_index(['state','year'])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>population</th>
    </tr>
    <tr>
      <th>state</th>
      <th>year</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">California</th>
      <th>2000</th>
      <td>33871648</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>37253956</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">New York</th>
      <th>2000</th>
      <td>18976457</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>19378102</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Texas</th>
      <th>2000</th>
      <td>20851820</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>25145561</td>
    </tr>
  </tbody>
</table>
</div>



In practice, I find this type of reindexing to be one of the more useful patterns when I encounter real-world datasets.




```python
# hierarchical indices and columns
index = pd.MultiIndex.from_product([[2013, 2014], [1, 2]],
                                   names=['year', 'visit'])
columns = pd.MultiIndex.from_product([['Bob', 'Guido', 'Sue'], ['HR', 'Temp']],
                                     names=['subject', 'type'])
data = np.round(np.random.randn(4, 6), 1)
```


```python
# create the DataFrame
health_data = pd.DataFrame(data, index=index, columns=columns)
health_data
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>subject</th>
      <th colspan="2" halign="left">Bob</th>
      <th colspan="2" halign="left">Guido</th>
      <th colspan="2" halign="left">Sue</th>
    </tr>
    <tr>
      <th></th>
      <th>type</th>
      <th>HR</th>
      <th>Temp</th>
      <th>HR</th>
      <th>Temp</th>
      <th>HR</th>
      <th>Temp</th>
    </tr>
    <tr>
      <th>year</th>
      <th>visit</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">2013</th>
      <th>1</th>
      <td>-0.5</td>
      <td>2.4</td>
      <td>-1.8</td>
      <td>-0.5</td>
      <td>-0.7</td>
      <td>-0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.5</td>
      <td>1.2</td>
      <td>1.6</td>
      <td>1.8</td>
      <td>1.5</td>
      <td>0.3</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2014</th>
      <th>1</th>
      <td>1.3</td>
      <td>-0.0</td>
      <td>0.1</td>
      <td>-1.8</td>
      <td>-0.7</td>
      <td>1.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.7</td>
      <td>-0.5</td>
      <td>0.2</td>
      <td>-0.8</td>
      <td>1.1</td>
      <td>0.3</td>
    </tr>
  </tbody>
</table>
</div>



##### Data Aggregations on Multi-Indices
We’ve previously seen that Pandas has built-in data aggregation methods,such as mean(), sum(), and max(). For hierarchically indexeddata, these can be passed a level parameter that controls whichsubset of the data the aggregate is computed on.For example, let’s return to our health data:


```python
health_data
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>subject</th>
      <th colspan="2" halign="left">Bob</th>
      <th colspan="2" halign="left">Guido</th>
      <th colspan="2" halign="left">Sue</th>
    </tr>
    <tr>
      <th></th>
      <th>type</th>
      <th>HR</th>
      <th>Temp</th>
      <th>HR</th>
      <th>Temp</th>
      <th>HR</th>
      <th>Temp</th>
    </tr>
    <tr>
      <th>year</th>
      <th>visit</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">2013</th>
      <th>1</th>
      <td>-0.5</td>
      <td>2.4</td>
      <td>-1.8</td>
      <td>-0.5</td>
      <td>-0.7</td>
      <td>-0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.5</td>
      <td>1.2</td>
      <td>1.6</td>
      <td>1.8</td>
      <td>1.5</td>
      <td>0.3</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2014</th>
      <th>1</th>
      <td>1.3</td>
      <td>-0.0</td>
      <td>0.1</td>
      <td>-1.8</td>
      <td>-0.7</td>
      <td>1.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.7</td>
      <td>-0.5</td>
      <td>0.2</td>
      <td>-0.8</td>
      <td>1.1</td>
      <td>0.3</td>
    </tr>
  </tbody>
</table>
</div>



Perhaps we’d like to average out the measurements in the two visits each year. We can do this by naming the index level we’d like to explore, in this case the year:


```python
data_mean = health_data.mean(level='year')
data_mean
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>subject</th>
      <th colspan="2" halign="left">Bob</th>
      <th colspan="2" halign="left">Guido</th>
      <th colspan="2" halign="left">Sue</th>
    </tr>
    <tr>
      <th>type</th>
      <th>HR</th>
      <th>Temp</th>
      <th>HR</th>
      <th>Temp</th>
      <th>HR</th>
      <th>Temp</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013</th>
      <td>0.5</td>
      <td>1.80</td>
      <td>-0.10</td>
      <td>0.65</td>
      <td>0.4</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>0.3</td>
      <td>-0.25</td>
      <td>0.15</td>
      <td>-1.30</td>
      <td>0.2</td>
      <td>0.95</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_mean.mean(axis=1, level='type')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>type</th>
      <th>HR</th>
      <th>Temp</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013</th>
      <td>0.266667</td>
      <td>0.866667</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>0.216667</td>
      <td>-0.200000</td>
    </tr>
  </tbody>
</table>
</div>



Thus in two lines, we’ve been able to find the average heart rate and temperature measured among all subjects in all visits each year. This syntax is actually a shortcut to the GroupBy functionality, which we will discuss in “Aggregation and Grouping”. While this is a toy example, many real-world datasets have similar hierarchical structure.

##### Combining Datasets: Concat and Append
Some of the most interesting studies of data come from combiningdifferent data sources. These operations can involve anything from verystraightforward concatenation of two different datasets, to morecomplicated database-style joins and merges that correctly handle anyoverlaps between the datasets. Series and DataFrames are builtwith this type of operation in mind, and Pandas includes functions andmethods that make this sort of data wrangling fast and straightforward.  
Here we’ll take a look at simple concatenation of Series and DataFrameswith the pd.concat function; later we’ll dive into more sophisticatedin-memory merges and joins implemented in Pandas.


```python
def make_df(cols, ind):
   """Quickly make a DataFrame"""
   data = {c: [str(c) + str(i) for i in ind]
           for c in cols}
   return pd.DataFrame(data, ind)

# example DataFrame
make_df('ABC', range(3))
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
  </tbody>
</table>
</div>



###### Simple Concatenation with pd.concat
>Signature in Pandas v0.18
>pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,keys=None, levels=None, names=None,verify_integrity=False,copy=True)


```python
ser1 = pd.Series(['A', 'B', 'C'], index=[1, 2, 3])
ser2 = pd.Series(['D', 'E', 'F'], index=[4, 5, 6])
pd.concat([ser1, ser2])
```




    1    A
    2    B
    3    C
    4    D
    5    E
    6    F
    dtype: object




```python
df1 = make_df('AB', [1, 2])
df2 = make_df('AB', [3, 4])
print(df1); print(df2); print(pd.concat([df1, df2]))
```

        A   B
    1  A1  B1
    2  A2  B2
        A   B
    3  A3  B3
    4  A4  B4
        A   B
    1  A1  B1
    2  A2  B2
    3  A3  B3
    4  A4  B4
    


```python
df3 = make_df('AB', [0, 1])
df4 = make_df('CD', [0, 1])
print(df3); print(df4); print(pd.concat([df3, df4], axis='columns'))
```

        A   B
    0  A0  B0
    1  A1  B1
        C   D
    0  C0  D0
    1  C1  D1
        A   B   C   D
    0  A0  B0  C0  D0
    1  A1  B1  C1  D1
    

##### DUPLICATE INDICES
One important difference between np.concatenate and pd.concat is that Pandas concatenation preserves indices, even if the result will have duplicate indices! Consider this simple example:


```python
x = make_df('AB', [0, 1])
y = make_df('AB', [2, 3])
y.index = x.index  # make duplicate indices!
print(x); print(y); print(pd.concat([x, y]))
```

        A   B
    0  A0  B0
    1  A1  B1
        A   B
    0  A2  B2
    1  A3  B3
        A   B
    0  A0  B0
    1  A1  B1
    0  A2  B2
    1  A3  B3
    

Notice the repeated indices in the result. While this is valid within DataFrames, the outcome is often undesirable. pd.concat() gives us a few ways to handle it.


```python
#Catching the repeats as an error

try:
    pd.concat([x, y], verify_integrity=True)
except ValueError as e:
    print("ValueError:", e)
```

    ValueError: Indexes have overlapping values: [0, 1]
    


```python
#Ignoring the index

print(x); print(y); print(pd.concat([x, y], ignore_index=True))
```

        A   B
    0  A0  B0
    1  A1  B1
        A   B
    0  A2  B2
    1  A3  B3
        A   B
    0  A0  B0
    1  A1  B1
    2  A2  B2
    3  A3  B3
    


```python
#Adding MultiIndex keys
print(x); print(y); print(pd.concat([x, y], keys=['x', 'y']))
```

        A   B
    0  A0  B0
    1  A1  B1
        A   B
    0  A2  B2
    1  A3  B3
          A   B
    x 0  A0  B0
      1  A1  B1
    y 0  A2  B2
      1  A3  B3
    

##### CONCATENATION WITH JOINS
In the simple examples we just looked at, we were mainly concatenating DataFrames with shared column names. In practice, data from different sources might have different sets of column names, and pd.concat offers several options in this case. Consider the concatenation of the following two DataFrames, which have some (but not all!) columns in common:


```python
df5 = make_df('ABC', [1, 2])
df6 = make_df('BCD', [3, 4])
print(df5); print(df6); print(pd.concat([df5, df6]))
```

        A   B   C
    1  A1  B1  C1
    2  A2  B2  C2
        B   C   D
    3  B3  C3  D3
    4  B4  C4  D4
         A   B   C    D
    1   A1  B1  C1  NaN
    2   A2  B2  C2  NaN
    3  NaN  B3  C3   D3
    4  NaN  B4  C4   D4
    

By default, the entries for which no data is available are filled with NA values. To change this, we can specify one of several options for the join and join_axes parameters of the concatenate function. By default, the join is a union of the input columns (join='outer'), but we can change this to an intersection of the columns using join='inner':


```python
print(df5); print(df6);
print(pd.concat([df5, df6], join='inner'))
```

        A   B   C
    1  A1  B1  C1
    2  A2  B2  C2
        B   C   D
    3  B3  C3  D3
    4  B4  C4  D4
        B   C
    1  B1  C1
    2  B2  C2
    3  B3  C3
    4  B4  C4
    

Another option is to directly specify the index of the remaining colums using the join_axes argument, which takes a list of index objects. Here we’ll specify that the returned columns should be the same as those of the first input:


```python
print(df5); print(df6);
print(pd.concat([df5, df6], join_axes=[df5.columns]))
```

        A   B   C
    1  A1  B1  C1
    2  A2  B2  C2
        B   C   D
    3  B3  C3  D3
    4  B4  C4  D4
         A   B   C
    1   A1  B1  C1
    2   A2  B2  C2
    3  NaN  B3  C3
    4  NaN  B4  C4
    

##### THE APPEND() METHOD
Because direct array concatenation is so common, Series and DataFrame objects have an append method that can accomplish the same thing in fewer keystrokes. For example, rather than calling pd.concat([df1, df2]), you can simply call df1.append(df2):


```python
print(df1); print(df2); print(df1.append(df2))
```

        A   B
    1  A1  B1
    2  A2  B2
        A   B
    3  A3  B3
    4  A4  B4
        A   B
    1  A1  B1
    2  A2  B2
    3  A3  B3
    4  A4  B4
    

Keep in mind that unlike the append() and extend() methods of Python lists, the append() method in Pandas does not modify the original object—instead, it creates a new object with the combined data. It also is not a very efficient method, because it involves creation of a new index and data buffer. Thus, if you plan to do multiple append operations, it is generally better to build a list of DataFrames and pass them all at once to the concat() function.

#### Combining Datasets: Merge and Join
One essential feature offered by Pandas is its high-performance, in-memory join and merge operations. If you have ever worked with databases, you should be familiar with this type of data interaction. The main interface for this is the pd.merge function, and we’ll see a few examples of how this can work in practice.

>##### Relational Algebra  
The behavior implemented in pd.merge() is a subset of what is known as relational algebra, which is a formal set of rules for manipulating relational data, and forms the conceptual foundation of operations available in most databases. The strength of the relational algebra approach is that it proposes several primitive operations, which become the building blocks of more complicated operations on any dataset. With this lexicon of fundamental operations implemented efficiently in a database or other program, a wide range of fairly complicated composite operations can be performed.

>Pandas implements several of these fundamental building blocks in the pd.merge() function and the related join() method of Series and DataFrames. As we will see, these let you efficiently link data from different sources.

>##### Categories of Joins  
The pd.merge() function implements a number of types of joins: the one-to-one, many-to-one, and many-to-many joins. All three types of joins are accessed via an identical call to the pd.merge() interface; the type of join performed depends on the form of the input data. Here we will show simple examples of the three types of merges, and discuss detailed options further below.

>##### ONE-TO-ONE JOINS  
Perhaps the simplest type of merge expression is the one-to-one join, which is in many ways very similar to the column-wise concatenation seen in “Combining Datasets: Concat and Append”. As a concrete example, consider the following two DataFrames, which contain information on several employees in a company:


```python
df1 = pd.DataFrame({'employee': ['Bob', 'Jake', 'Lisa', 'Sue'],
                    'group': ['Accounting', 'Engineering', 'Engineering', 'HR']})
df2 = pd.DataFrame({'employee': ['Lisa', 'Bob', 'Jake', 'Sue'],
                    'hire_date': [2004, 2008, 2012, 2014]})
print(df1); print(df2)
```

      employee        group
    0      Bob   Accounting
    1     Jake  Engineering
    2     Lisa  Engineering
    3      Sue           HR
      employee  hire_date
    0     Lisa       2004
    1      Bob       2008
    2     Jake       2012
    3      Sue       2014
    


```python
df3=pd.merge(df1,df2)
df3
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>employee</th>
      <th>group</th>
      <th>hire_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>Accounting</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jake</td>
      <td>Engineering</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lisa</td>
      <td>Engineering</td>
      <td>2004</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sue</td>
      <td>HR</td>
      <td>2014</td>
    </tr>
  </tbody>
</table>
</div>



The pd.merge() function recognizes that each DataFrame has an “employee” column, and automatically joins using this column as a key. The result of the merge is a new DataFrame that combines the information from the two inputs. Notice that the order of entries in each column is not necessarily maintained: in this case, the order of the “employee” column differs between df1 and df2, and the pd.merge() function correctly accounts for this. Additionally, keep in mind that the merge in general discards the index, except in the special case of merges by index (see “The left_index and right_index keywords”).

> ##### MANY-TO-ONE JOINS
Many-to-one joins are joins in which one of the two key columns contains duplicate entries. For the many-to-one case, the resulting DataFrame will preserve those duplicate entries as appropriate. Consider the following example of a many-to-one join:


```python
df4 = pd.DataFrame({'group': ['Accounting', 'Engineering', 'HR'],
                           'supervisor': ['Carly', 'Guido', 'Steve']})
print(df3); print(df4); print(pd.merge(df3, df4))
```

      employee        group  hire_date
    0      Bob   Accounting       2008
    1     Jake  Engineering       2012
    2     Lisa  Engineering       2004
    3      Sue           HR       2014
             group supervisor
    0   Accounting      Carly
    1  Engineering      Guido
    2           HR      Steve
      employee        group  hire_date supervisor
    0      Bob   Accounting       2008      Carly
    1     Jake  Engineering       2012      Guido
    2     Lisa  Engineering       2004      Guido
    3      Sue           HR       2014      Steve
    

The resulting DataFrame has an additional column with the “supervisor” information, where the information is repeated in one or more locations as required by the inputs.

>##### MANY-TO-MANY JOINS
Many-to-many joins are a bit confusing conceptually, but are nevertheless well defined. If the key column in both the left and right array contains duplicates, then the result is a many-to-many merge. This will be perhaps most clear with a concrete example. Consider the following, where we have a DataFrame showing one or more skills associated with a particular group.

By performing a many-to-many join, we can recover the skills associated with any individual person:


```python
df5 = pd.DataFrame({'group': ['Accounting', 'Accounting',
                                     'Engineering', 'Engineering', 'HR', 'HR'],

                           'skills': ['math', 'spreadsheets', 'coding', 'linux',
                                      'spreadsheets', 'organization']})
print(df1); print(df5); print(pd.merge(df1, df5))
```

      employee        group
    0      Bob   Accounting
    1     Jake  Engineering
    2     Lisa  Engineering
    3      Sue           HR
             group        skills
    0   Accounting          math
    1   Accounting  spreadsheets
    2  Engineering        coding
    3  Engineering         linux
    4           HR  spreadsheets
    5           HR  organization
      employee        group        skills
    0      Bob   Accounting          math
    1      Bob   Accounting  spreadsheets
    2     Jake  Engineering        coding
    3     Jake  Engineering         linux
    4     Lisa  Engineering        coding
    5     Lisa  Engineering         linux
    6      Sue           HR  spreadsheets
    7      Sue           HR  organization
    

These three types of joins can be used with other Pandas tools to implement a wide array of functionality. But in practice, datasets are rarely as clean as the one we’re working with here. In the following section, we’ll consider some of the options provided by pd.merge() that enable you to tune how the join operations work.

>##### Specification of the Merge Key
We’ve already seen the default behavior of pd.merge(): it looks for one or more matching column names between the two inputs, and uses this as the key. However, often the column names will not match so nicely, and pd.merge() provides a variety of options for handling this.

##### THE ON KEYWORD
Most simply, you can explicitly specify the name of the key column using the on keyword, which takes a column name or a list of column names:


```python
print(df1); print(df2); print(pd.merge(df1, df2, on='employee'))
```

      employee        group
    0      Bob   Accounting
    1     Jake  Engineering
    2     Lisa  Engineering
    3      Sue           HR
      employee  hire_date
    0     Lisa       2004
    1      Bob       2008
    2     Jake       2012
    3      Sue       2014
      employee        group  hire_date
    0      Bob   Accounting       2008
    1     Jake  Engineering       2012
    2     Lisa  Engineering       2004
    3      Sue           HR       2014
    

This option works only if both the left and right DataFrames have the specified column name.



##### THE LEFT_ON AND RIGHT_ON KEYWORDS
At times you may wish to merge two datasets with different column names; for example, we may have a dataset in which the employee name is labeled as “name” rather than “employee”. In this case, we can use the left_on and right_on keywords to specify the two column names:


```python
df3 = pd.DataFrame({'name': ['Bob', 'Jake', 'Lisa', 'Sue'],
                    'salary': [70000, 80000, 120000, 90000]})
print(df1); print(df3);
print(pd.merge(df1, df3, left_on="employee", right_on="name"))
```

      employee        group
    0      Bob   Accounting
    1     Jake  Engineering
    2     Lisa  Engineering
    3      Sue           HR
       name  salary
    0   Bob   70000
    1  Jake   80000
    2  Lisa  120000
    3   Sue   90000
      employee        group  name  salary
    0      Bob   Accounting   Bob   70000
    1     Jake  Engineering  Jake   80000
    2     Lisa  Engineering  Lisa  120000
    3      Sue           HR   Sue   90000
    

The result has a redundant column that we can drop if desired—for example, by using the drop() method of DataFrames:


```python
pd.merge(df1, df3, left_on="employee", right_on="name").drop('name', axis=1)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>employee</th>
      <th>group</th>
      <th>salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bob</td>
      <td>Accounting</td>
      <td>70000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jake</td>
      <td>Engineering</td>
      <td>80000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lisa</td>
      <td>Engineering</td>
      <td>120000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sue</td>
      <td>HR</td>
      <td>90000</td>
    </tr>
  </tbody>
</table>
</div>



##### THE LEFT_INDEX AND RIGHT_INDEX KEYWORDS
Sometimes, rather than merging on a column, you would instead like to merge on an index. For example, your data might look like this:


```python
df1a = df1.set_index('employee')
df2a = df2.set_index('employee')
print(df1a); print(df2a)
```

                    group
    employee             
    Bob        Accounting
    Jake      Engineering
    Lisa      Engineering
    Sue                HR
              hire_date
    employee           
    Lisa           2004
    Bob            2008
    Jake           2012
    Sue            2014
    

You can use the index as the key for merging by specifying the left_index and/or right_index flags in pd.merge():


```python
print(df1a); print(df2a);
print(pd.merge(df1a, df2a, left_index=True, right_index=True))
```

                    group
    employee             
    Bob        Accounting
    Jake      Engineering
    Lisa      Engineering
    Sue                HR
              hire_date
    employee           
    Lisa           2004
    Bob            2008
    Jake           2012
    Sue            2014
                    group  hire_date
    employee                        
    Bob        Accounting       2008
    Jake      Engineering       2012
    Lisa      Engineering       2004
    Sue                HR       2014
    

For convenience, DataFrames implement the join() method, which performs a merge that defaults to joining on indices:


```python
print(df1a); print(df2a); print(df1a.join(df2a))
```

                    group
    employee             
    Bob        Accounting
    Jake      Engineering
    Lisa      Engineering
    Sue                HR
              hire_date
    employee           
    Lisa           2004
    Bob            2008
    Jake           2012
    Sue            2014
                    group  hire_date
    employee                        
    Bob        Accounting       2008
    Jake      Engineering       2012
    Lisa      Engineering       2004
    Sue                HR       2014
    

If you’d like to mix indices and columns, you can combine left_index with right_on or left_on with right_index to get the desired behavior:


```python
print(df1a); print(df3);
print(pd.merge(df1a, df3, left_index=True, right_on='name'))
```

                    group
    employee             
    Bob        Accounting
    Jake      Engineering
    Lisa      Engineering
    Sue                HR
       name  salary
    0   Bob   70000
    1  Jake   80000
    2  Lisa  120000
    3   Sue   90000
             group  name  salary
    0   Accounting   Bob   70000
    1  Engineering  Jake   80000
    2  Engineering  Lisa  120000
    3           HR   Sue   90000
    

##### Specifying Set Arithmetic for Joins
In all the preceding examples we have glossed over one important consideration in performing a join: the type of set arithmetic used in the join. This comes up when a value appears in one key column but not the other. Consider this example:


```python
df6 = pd.DataFrame({'name': ['Peter', 'Paul', 'Mary'],
                            'food': ['fish', 'beans', 'bread']},
                           columns=['name', 'food'])
df7 = pd.DataFrame({'name': ['Mary', 'Joseph'],
                    'drink': ['wine', 'beer']},
                   columns=['name', 'drink'])
print(df6); print(df7); print(pd.merge(df6, df7))
```

        name   food
    0  Peter   fish
    1   Paul  beans
    2   Mary  bread
         name drink
    0    Mary  wine
    1  Joseph  beer
       name   food drink
    0  Mary  bread  wine
    

Here we have merged two datasets that have only a single “name” entry in common: Mary. By default, the result contains the intersection of the two sets of inputs; this is what is known as an inner join. We can specify this explicitly using the how keyword, which defaults to 'inner':


```python
pd.merge(df6, df7, how='inner')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>food</th>
      <th>drink</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mary</td>
      <td>bread</td>
      <td>wine</td>
    </tr>
  </tbody>
</table>
</div>



Other options for the how keyword are 'outer', 'left', and 'right'. An outer join returns a join over the union of the input columns, and fills in all missing values with NAs:


```python
print(df6); print(df7); print(pd.merge(df6, df7, how='outer'))
```

        name   food
    0  Peter   fish
    1   Paul  beans
    2   Mary  bread
         name drink
    0    Mary  wine
    1  Joseph  beer
         name   food drink
    0   Peter   fish   NaN
    1    Paul  beans   NaN
    2    Mary  bread  wine
    3  Joseph    NaN  beer
    

The left join and right join return join over the left entries and right entries, respectively. For example:




```python
print(df6); print(df7); print(pd.merge(df6, df7, how='left'))
```

        name   food
    0  Peter   fish
    1   Paul  beans
    2   Mary  bread
         name drink
    0    Mary  wine
    1  Joseph  beer
        name   food drink
    0  Peter   fish   NaN
    1   Paul  beans   NaN
    2   Mary  bread  wine
    

The output rows now correspond to the entries in the left input. Using how='right' works in a similar manner.

All of these options can be applied straightforwardly to any of the preceding join types.

##### Overlapping Column Names: The suffixes Keyword
Finally, you may end up in a case where your two input DataFrames have conflicting column names. Consider this example:


```python
df8 = pd.DataFrame({'name': ['Bob', 'Jake', 'Lisa', 'Sue'],
                    'rank': [1, 2, 3, 4]})
df9 = pd.DataFrame({'name': ['Bob', 'Jake', 'Lisa', 'Sue'],
                    'rank': [3, 1, 4, 2]})
print(df8); print(df9); print(pd.merge(df8, df9, on="name"))
```

       name  rank
    0   Bob     1
    1  Jake     2
    2  Lisa     3
    3   Sue     4
       name  rank
    0   Bob     3
    1  Jake     1
    2  Lisa     4
    3   Sue     2
       name  rank_x  rank_y
    0   Bob       1       3
    1  Jake       2       1
    2  Lisa       3       4
    3   Sue       4       2
    

Because the output would have two conflicting column names, the merge function automatically appends a suffix _x or _y to make the output columns unique. If these defaults are inappropriate, it is possible to specify a custom suffix using the suffixes keyword:


```python
print(df8); print(df9);
print(pd.merge(df8, df9, on="name", suffixes=["_L", "_R"]))
```

       name  rank
    0   Bob     1
    1  Jake     2
    2  Lisa     3
    3   Sue     4
       name  rank
    0   Bob     3
    1  Jake     1
    2  Lisa     4
    3   Sue     2
       name  rank_L  rank_R
    0   Bob       1       3
    1  Jake       2       1
    2  Lisa       3       4
    3   Sue       4       2
    

These suffixes work in any of the possible join patterns, and work also if there are multiple overlapping columns.

For more information on these patterns, see “Aggregation and Grouping”, where we dive a bit deeper into relational algebra. Also see the “Merge, Join, and Concatenate” section of the Pandas documentation for further discussion of these topics.

##### Example: US States Data
Merge and join operations come up most often when one is combining data from different sources. Here we will consider an example of some data about US states and their populations. The data files can be found at http://github.com/jakevdp/data-USstates/:

>https://jakevdp.github.io/PythonDataScienceHandbook/03.07-merge-and-join.html

##### Aggregation and Grouping
>https://jakevdp.github.io/PythonDataScienceHandbook/03.08-aggregation-and-grouping.html

##### Pivot Tables
>https://jakevdp.github.io/PythonDataScienceHandbook/03.09-pivot-tables.html

##### Vectorized String Operations
>https://jakevdp.github.io/PythonDataScienceHandbook/03.10-working-with-strings.html

##### Working with Time Series
>https://jakevdp.github.io/PythonDataScienceHandbook/03.11-working-with-time-series.html

##### High-Performance Pandas: eval() and query()
>https://jakevdp.github.io/PythonDataScienceHandbook/03.12-performance-eval-and-query.html

##### Sources: 
Python Data Science Handbook - https://jakevdp.github.io/PythonDataScienceHandbook/    
Please note this is for reference.For detailed explanation of methods and complete understanding buy the book in link:- http://shop.oreilly.com/product/0636920034919.do

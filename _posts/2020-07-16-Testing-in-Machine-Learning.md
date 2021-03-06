---
title:  "Software Testing Practices in Data Science"
date:   2020-09-19 23:35:38 +0545
classes : wide
categories:
  - Data-Science
  - Testing

excerpt: Compilation of necessary  discussion on usefulness of unit testing in data science projects

header:
  og_image: /assets/images/sentence_embedding_image.jpg
  teaser: /assets/images/sentence_embedding_image.jpg
---

## Part 1 : General overview on testing

Testing is not a data scientist's best practice in software engineering. With constant experimentation as day to day job, especially on feature engineering, data scientits don't have a firm ground to begin with, where to test

On a day to day job as a data scientists, we generally have : 

- One off analysis ( some experimentation to data in jupyter notebook, which we generally document , for easier recall later on )
- Exploratory work ( Generally impossible to follow testing, but, there's always some importatn piece of code written during this phase that will be reused )
- Well-defined problem ( this is where testing shines ) 
- Working with Legacy code ( When using legacy codebase , try adding test to parts you have reused/modified from it and not whole codebase )


### Why test ?

With complicated production code involving feature extraction on  messy user input ,  and stochastic algorithm acting upon it, We make lot of assumptions on how data is like, and how it's being processed. Well, that's just assumptions. 

What if irrelevant values, columns were added in data warehouse, which is not common. System would break. 

Test helps to make sure, where it broke.

Key reason can be listed as : 

- Know code is during whats it's supposed to do ( are missing values in data affecting the preprocessing which we forget to account for )
- Make experimental changes rapidly without fearing to break the code
- Other people more confident in our code
- Helps catch bugs
- Understand the code for new users
- Help code development and refactoring
- Be less amazed at, like, don't get suprised when code breaks due to faulty production data in pipeline
- Code works not just in one computer, but across all the other users computer
- Show em data scientists can actually write good code !!



### When and what to test ?

It's an iterative process. You write code, you add test to make sure code does what you meant it to do. You make new changes, you add test.

Say a function that calculates mean, you write a test, and check it actually calculates mean. 

You test the output of your piece of code is corrent. Not  internal computation.

Say your code calculates weighted mean :

```python
## simple example.py
def weighted_mean( array , weights ) :
  assert array.istype(np.array)
  assert weights.istype(np.array)
  return  np.sum(array * weights) / len(array)

## test_mean.py
def test_weighted_array( ):
  assert(weighted_mean( [1,2,3,4,5] , [0.5,0.5,0.5,0.5,0.5] )) == 3

```



### Testing tool of choice : pytest

You can install it using any of  :
```
pip install pytest
conda install pytest
```

Wait no unit tests .. Umm no, as much as headache testing already is, I won't want
to add more boilerplates, which unit tests does.

Now into the example 

#### General Testing Scenario : 

Assume the folder structure : 

![](/assets/images/testing_machine_learning/2021-01-19-23-45-43.png)


```python
## capitalize.py
## we have  function `capitalize_reverse`
## The function is supposed to capitalize input and reverse it and ignore numbers

def capitalize_reverse(text):
    text = text[::-1].upper()
    text = [ i for i in text if not i.isdigit() ]
    return ''.join(text)
```

We make a separate file *test_demo.py* and write test for above function.

```python
## test_demo.py
## test our reverse function
from capitalize import capitalize_reverse

def test_capitalize_reverse( ):
    assert capitalize_reverse('age') == 'EGA'
    assert capitalize_reverse('1243') == ''
    assert capitalize_reverse(1245) == ''
    assert capitalize_reverse(12000.4500091) == '.'

def test_number_removal():
    assert capitalize_reverse('c1a2t3') == 'TAC'

```

In the shell, inside that folder , we now run the test as : **pytest test_demo.py**.

We find that test fails on thirds assertion  :

![](/assets/images/testing_machine_learning/2021-01-21-00-39-25.png)

As we wrote assertions we knew our function must, we unknowingly broke it (well .. XD ) .
We knew raw input of user will include integer/floating points, but forgot to account it in the code. 

And that's what test is for. We write code. Test it's behaviour on its supposed output. Iteratively update code when test breaks. 

Our updated function will be now, 

```python
## capitalize.py
def capitalize_reverse(text):
    ## typecast integer/floating points into string
    text = str(text)
    text = text[::-1].upper()
    text = [ i for i in text if not i.isdigit() ]
    return ''.join(text)
```

Testing this again, `pytest test_demo.py`. we pass the test.

![](/assets/images/testing_machine_learning/2021-01-21-00-49-31.png)


Well that was simple and not data science specific. But this tutorial was only meant to make you guys understand, what test driven development ( TDD ) actually is. 


## Part 2 :  Data science specific Testing
In Data Science domain , testing usually revolves around : 

- Dataframes
- Databases
- ML models with non-deterministic outcomes
- Acceptable tolerances on results
- Testing for properties, than exact values

#### CONTINE AT : jes ford, 13:07




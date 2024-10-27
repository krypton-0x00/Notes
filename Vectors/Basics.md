![[Pasted image 20240929134654.png]]
When describing vectors mathematicians generally prefer to describe vectors as character symbols with a little bar over their head like v¯v¯. Also, when displaying vectors in formulas they are generally displayed as follows:

![[Pasted image 20240929135140.png]]

## Scalar vector operations

A scalar is a single digit. When adding/subtracting/multiplying or dividing a vector with a scalar we simply add/subtract/multiply or divide each element of the vector by the scalar. For addition it would look like this:

![[Pasted image 20240929135212.png]]
## Vector negation

Negating a vector results in a vector in the reversed direction. A vector pointing north-east would point south-west after negation. To negate a vector we add a minus-sign to each component (you can also represent it as a scalar-vector multiplication with a scalar value of `-1`):
![[chrome_0LRqE16XSY.png]]
![[Pasted image 20240929135258.png]]
##### Just like normal addition and subtraction, vector subtraction is the same as addition with a negated second vector:
![[Pasted image 20240929135318.png]]
Subtracting two vectors from each other results in a vector that's the difference of the positions both vectors are pointing at. This proves useful in certain cases where we need to retrieve a vector that's the difference between two points
![[Pasted image 20240929135325.png]]
## Length

To retrieve the length/magnitude of a vector we use the Pythagoras theorem that you may remember from your math classes. A vector forms a triangle when you visualize its individual `x` and `y` component as two sides of a triangle:


![[Pasted image 20240929135356.png]]
## Vector-vector multiplication

Multiplying two vectors is a bit of a weird case. Normal multiplication isn't really defined on vectors since it has no visual meaning, but we have two specific cases that we could choose from when multiplying: one is the dot product denoted as v¯⋅k¯v¯⋅k¯ and the other is the cross product denoted as v¯×k¯v¯×k¯.

#### Dot product


![[Pasted image 20240929135457.png]]
# Cross product
gives a normal to the two vectors( ie a vector which is perpendicular to those vectors)

![[Pasted image 20240929135508.png]]
# Matrix-Vector multiplication

Up until now we've had our fair share of vectors. We used them to represent positions, colors and even texture coordinates. Let's move a bit further down the rabbit hole and tell you that a vector is basically a `Nx1` matrix where `N` is the vector's number of components (also known as an N-dimensional vector). If you think about it, it makes a lot of sense. Vectors are just like matrices an array of numbers, but with only 1 column. So, how does this new piece of information help us? Well, if we have a `MxN` matrix we can multiply this matrix with our `Nx1` vector, since the columns of the matrix are equal to the number of rows of the vector, thus matrix multiplication is defined.

But why do we care if we can multiply matrices with a vector? Well, it just so happens that there are lots of interesting 2D/3D transformations we can place inside a matrix, and multiplying that matrix with a vector then _transforms_ that vector. In case you're still a bit confused, let's start with a few examples and you'll soon see what we mean.

## Identity matrix

In OpenGL we usually work with `4x4` transformation matrices for several reasons and one of them is that most of the vectors are of size 4. The most simple transformation matrix that we can think of is the identity matrix. The identity matrix is an `NxN` matrix with only 0s except on its diagonal. As you'll see, this transformation matrix leaves a vector completely unharmed: 

![[Pasted image 20240929144616.png]]
The vector is completely untouched. This becomes obvious from the rules of multiplication: the first result element is each individual element of the first row of the matrix multiplied with each element of the vector. Since each of the row's elements are 0 except the first one, we get: `1⋅1 + 0⋅2 + 0⋅3 + 0⋅ 4 = 11⋅1 + 0⋅2 + 0⋅3 + 0⋅4 = 1` and the same applies for the other 3 elements of the vector.

# Scaling a vector using Matrices 
![[Pasted image 20240929144646.png]]
![[Pasted image 20240929144715.png]]
 
  # Rotation
![[Pasted image 20240929193027.png]]
![[Pasted image 20240929193107.png]]
![[Pasted image 20240929193125.png]]

# Combine Matrices

![[Pasted image 20240929193157.png]]

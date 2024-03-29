# Chapter 12 Exercises #

## Exercise 1 ##

### **Question** ##

Suppose the following declarations are in effect:

```C
int a[] = {5, 15, 34, 54, 14, 2, 52, 72};
int *p = &a[1], *q = &a[5];
```

    (a) What is the value of *(p+3)?
    (b) What is the value of *(q-3)?
    (c) What is the value of q - p?
    (d) Is the condition p < q true or false?
    (e) Is the condition *p < *q true of false


### **Answer**  ###

(a) The value of `*(p+3)` is `14`.

(b) The value of `*(q-3)` is `34`.

(c) The value of `q - p` is `4`. Remember that subtracting one pointer from another gives the distance between the pointers in array elements.

(d) The condition `p < q` will be `true` because the address of `q` should be higher than `p`.

(e) The condition `*p < *q` will be `false` because by dereferencing the pointer with the indirection operator (*) we are getting an actual value pointed to. Therefore, `*p` will be `15` and `*q` will be `2`. 

---

## Exercise 2 ##

### **Question** ###

Suppose that `high`, `low`, and `middle` are all pointer variables of the same type, and that `low` and `high` point to elements of an array. Why is the following statement illegal, and how could it be fixed?

```C
middle = (low + high) / 2;
```

### **Answer** ###

The issue with this statement is that we are attempting to add pointers together. Pointers are simply addresses in memory and adding them together has no defined meaning. This makes the statement illegal. We can solve this by instead rewriting the statement to subtract the pointers rather than add them.

```C
middle = low + (low - high) / 2;
```

Notice that in the rewritten statement we are able to add `low` after performing the subtraction because pointers **CAN** be added to integers.

---

## **Exercise 3** ##

### **Question** ###

What will the contents of the `a` array be after the following statements are executed?

```C
#define N 10

int a[N] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int *p = &a[0], *q = &a[N-1], temp;

while(p < q) {
    temp = *p;
    *p++ = *q;
    *q-- = temp;
}
```

### **Answer:** ###

Recall that the post-script increment (x++) operator is handled after other operations unlike the pre-script increment (++x) operator. Therefore, in this exercise when operations are being performed on `p` the increment only happens after the other operations which means that during the other operations `p++` is actually just `p`.

This code fragment will reverse the order of the `a` array as it runs and our final array will be:

`a[] = {10, 9, 8, 7, 6, 5, 4, 3, 2, 1};`

---

## **Exercise 4** ##

### **Question** ###

Rewrite the `make_empty`, `is_empty`, and `is_full` functions of Section 10.2 to use the pointer variable `top_ptr` instead of the integer variable `top`.

### **Answer:** ###

```C
    #include<stdbool.h>
    #define STACK_SIZE 100

    int contents[STACK_SIZE]
    int * top_ptr = 0;

    void make_empty(void) {
        //Set the top pointer to address of the first element of the array to make the
        //stack start at the beginning so it is empty.
        top_ptr = &contents[0];
    }

    bool is_empty(void) {
        //Return true if the top pointer is the same address as the first element of the
        //array indicating that the stack is empty.
        return top_ptr == &contents[0];
    }

    bool is_full(void) {
        //Return true if the address of the top pointer is the same as the array's final
        //element indicating that the stack is full.
        return top_ptr == &contents[STACK_SIZE];
    }


```
---

## **Exercise 5** ##

### **Question** ###

Suppose that `a` is a one-dimensional array and `p` is a pointer variable. Assuming that the assignment `p = a` has just been performed. Which of the following expressions are illegal because of mismatched types? Of the remaining expressions, which are true (have a nonzero value)?

```C
(a) p == a[0]
(b) p == &a[0]
(c) *p == a[0]
(d) p[0] == a[0]
```

### **Answer:** ###

`(a)` is illegal because you are comparing an address to a value. `(b)` is legal because you are comparing an address to an address and evaluates to `true` because `p` is an alias for `a` and when declaring an array as a pointer it points to the first element in the array. (Recall Section 12.3) `(c)` is legal because you are comparing a value to a value since `p` was dereferenced with the indirection operator and will evaluate to `true` for the same reason as the previous. `(d)` is also legal because we are comparing a value to another value and evaluates to `true` because we are comparing equivalent values.

---

## **Exercise 6** ##

### **Question** ###

Rewrite the following function to use pointer arithmetic instead of array subscripting. (In other words, eliminate the variable `i` and all uses of the `[]` operator.) Make as few changes as possible.

```C
int sum_array(const int a[], int n) {

    int i, sum;

    for (i = 0; i < n; i++) {
        sum += a[i];
    }

    return sum;

}
```

### **Answer:** ###

Recall that array parameters can be declared as a pointer since the compiler treats it as if it was identical to declaring it as an array.

```C
int sum_array(int * a, int n) {
    
    int sum = 0;
    int * p;

    for (p = a; p < a; p++) {
        sum += *p;
    }

    return sum;

}
```

---

## **Exercise 7** ##

### **Question** ###

Write the following function:

```C
bool search(const int a[], int n, int key);
```
`a` is an array to be searched, `n` is the number of elements in the array, and `key` is the search key. `search` should return `true` if `key` matches some elements of `a`, and `false` if it doesn't. Use pointer arithmetic--not subscripting--to visit array elements.

### **Answer:** ###

```C
bool search(const int a[], int n, int key) {

    //Defining p as a pointer.
    int * p;

    //Setup a boolean variable for us to return later for determining if a match is found. Defaults to false. Requires stdbool.h.
    bool match_found = false;

    //Setting p = a to set p = &a[0] before iterating through the array with pointer arithmetic.
    for (p = a; p < a + n; p++) {

        //Compare key value to the value of the current array element. If a match is found, set match_found to true.
        match_found = *p == key; 
    }

    return match_found;

}
```
Remember: We can use the array name alone as a pointer. (`a` in this case.) We can either use it alone for pointer arithmetic which will disallow us from incrementing `a`; therefore, we can declare a secondary pointer variable and copy `a` into it(`p`  in this case) to use for operations. Reread **Section 12.3** if you need review.

---

## **Exercise 8** ##

### **Question** ###

Rewrite the following function to use pointer arithmetic instead of array subscripting. (In other words, eliminate the variable `i` and all uses of the `[]` operator.) Make as few changes as possible.

```C
void store_zeros(int a[], int n) {
    
    int i;

    for (i = 0; i < n; i++) {
        a[i] = 0;
    }

}
```

### **Answer:** ###

```C
void store_zeros(int * a, int n) {

    int * p;

    for(p = a; p < a + n; p++) {
        *p = 0;
    }

}
```
We setup a pointer variable `p` that holds the same address as `a` so that we can make modifications to array which would not be possible if we simply used `a`. Re-read 12.3 if you are unsure of why this is the case.

---

## **Exercise 9** ##

### **Question** ###

Write the following function:

```C
double inner_product(const double *a, const double *b, int n);
```

`a` and `b` both point to arrays of length `n`. The function should return `a[0] * b[0] + a[1] * b[1] + ... + a[n-1] * b[n-1]`. Use pointer arithmetic--not subscripting--to visit array elements.

### **Answer:** ###

```C
double inner_product(const double *a, const double *b, int n) {

    //Create a double variable that will store the accumulated values.
    double final = 0.0;

    //Let's create two pointer variables to copy the array pointers into.
    double * p = a, * q = b;

    //We only need to conditionally loop through the array using one of the arrays since they are both presumed to have the same length and they will both be incremented on each successful loop so their indices should remain the same.
    while(p < a + n) {

        //Because of how the postscript increment side-effects work we can add the values of each element together.
        final += *p++ * *q++;

    }

    return final;

}
```
This can be a bit confusing at first blush. The crux of the issue to make sure you understand how combinations of the indirection (`*`) and increment (`++`) operators behave. Simply put, when we use `*p++` and `*q++`, for example, the multiplication will be using the current values of `*p` and `*q` respectively. It is only after this operation that the incrementing of `p` and `q` will take place. The latter part of **Section 12.2** explains this in great detail. Reread this as many times as necessary to absorb it.

---

## **Exercise 10** ##

### **Question** ###

Modify the `find_middle` function of **Section 11.5** so that it uses pointer arithmetic to calculate a return value.   

### **Answer:** ###

```C
int *find_middle(int * a, int n) {

    //Create a copy of the a array pointer as p.
    int * p = a;

    //Use pointer arithmetic to return the middle of the array.
    return p + (n / 2);

}
```
This exercise is rather simple if you understand the basics of how pointer arithmetic works. Creating the pointer `p` and giving the address of `a` we know that it now is set at `&a[0]`. If we divide the number of elements in the array by 2 and add that to `p` we will get the element at the middle of the array. However, if our array stores an even number of integers we will get the higher of the two middle values.

**e.g.** If the array being passed in to the function is `int a[6] = {1, 2, 3, 4, 5, 6};` we will get `4` as our return value.

---

## **Exercise 11** ##

### **Question** ###

Modify the `find_largest` function so that it uses pointer arithmetic--not subscripting--to visit array elements.

### **Answer:** ###

```C
int find_largest(int a[], int n) {
    
    int * p, max;
    p = a;

    max = *p;

    while (p < a + n) {
    
        if (*p > max) {

            max = *p++;
        
        }
        
    }
    
    return max;

}
```

As with the previous exercise we must understand how combinations of the indirection (`*`) and the increment (`++`) work in order to tackle this exercise.

---

## **Exercise 12** ##

### **Question** ###

Write the following function:

```C
void find_two_largest(const int *a, int n, int *largest, int *second_largest);
```
`a` points to an array of length `n`. The function searches the array for its largest and second largest elements, storing them in the variables poined to by `largest` and `second_largest`, respectively. Use pointer arithmetic--not subscripting--to visit the elements.

### **Answer:** ###

```C
void find_two_largest(const int *a, int n, int *largest, int *second_largest) {

    int * p = a;

    *largest = *p;
    *second_largest = *p;

    while (p++ < a + n) {

        if (*p > *largest) {

            *second_largest = *largest;
            *largest = *p;

        } else if (*p > *second_largest) {

            *second_largest = *p;

        }

    }

}

```

---

## **Exercise 13** ##

### **Question** ###

**Section 8.2** had a program fragment in which two nested `for` loops initialized the array `ident` for use as an identity matrix. Rewrite this code, using a single pointer to step through the array one element at a time. *Hint:* Since we won't be using `row` and `col` index variables, it won't be easy to tell where to store 1. Instead we can use the fact that the first element of the array should be 1, the next `N` elements should be 0, the next element should 1, and so forth. Use a variable to keep track of how many consecutive 0s have been stored; when the count reaches `N`, it's time to store 1.

### **Answer:** ###

**TODO**: This answer is incomplete.

```C
#define N 10

double ident[N][N];
int row, col;

for (row = 0; row < N; row++) {

}
```

---

## **Exercise 14** ##

### **Question** ###

Assume that the following array contains a week's worth of hourly temperature readings, with each row containing the readings for one day:

```C
int temperatures[7][24];
```
Write a statement that uses the `search` function (see **Exercise 7**) to search the entire `temperatures` array for the value `32`.

### **Answer:** ###

```C
bool search_for_32 = search(temperatures, 7 * 24, 32);
```
We need to pass in the array to the search function as the first argument. Since the second argument wants to know the total number of elements we must multiply the number of rows by the number of columns which are `7` and `24` respectively in this case. For our final argument we are passing in the search key (what we are looking for in the array) which in this case is `32`.

---

## **Exercise 15** ##

### **Question** ###

Write a loop that prints all temperature readings stored in row `i` of the `temperatures` array (see **Exercise 14**). Use a pointer to visit each element in the row.

### **Answer:** ###

```C
int i, *p;

for (p = a[i]; p < a[i] + 24; p++) {
    
    printf("%d", *p);

}
```
This can be quite complicated at first glance. Remember that our `search` function is expecting a 1-dimensional array as an argument. The expression `a[i]` is a pointer to the first element of row `i`. This is explained in **Section 12.4** in the subsection named: **Processing the Rows of a Multidimensional Array**.

---

## **Exercise 16** ##

### **Question** ###

Write a loop that prints the highest temperature in the `temperatures` array (see **Exercise 14**) for each day of the week. The loop body should call the `find_largest` function, passing it one row of the array at a time.

### **Answer:** ###



---
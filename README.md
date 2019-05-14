# mckinsey
A repo with solutions for some McKinsey technical test's task

## Beautiful numbers

Suppose you have N integers from 1 to N. We define a beautiful arrangement as an array that is constructed by these N numbers successfully if one of the following is true for the ith position (1 <= i <= N) in this array:

The number at the ith position is divisible by i.
i is divisible by the number at the ith position.
 
Now given N, how many beautiful arrangements can you construct?

__Example:__
```
Input: 2
Output: 2
Explanation: 

The first beautiful arrangement is [1, 2]:

Number at the 1st position (i=1) is 1, and 1 is divisible by i (i=1).

Number at the 2nd position (i=2) is 2, and 2 is divisible by i (i=2).

The second beautiful arrangement is [2, 1]:

Number at the 1st position (i=1) is 2, and 2 is divisible by i (i=1).

Number at the 2nd position (i=2) is 1, and i (i=2) is divisible by 1.
```

__Note:__
N is a positive integer and will not exceed 15.

__Solution:__
```python
import itertools
def beautiful(n):
    a = list(range(1, n+1))
    
    result = []
    
    for i in list((itertools.permutations(a, n))):
        
        flag = True
        for k in range(len(i)):
            if (i[k]%(k+1) == 0) | ((k+1)%i[k] == 0):
                pass
            else:
                flag = False
                break

        if flag:
            result.append(i)

    return len(result)
``` 

## Working Schedules
You just got a new job where you have to work exactly as many hours as you are told each week, working no more than a daily maximum number of hours per day. Some of the days, they tell you how many hours you will work. You get to choose the remainder of your schedule, within the limits.

A completed schedule consists of exactly 7 digits in the range 0 to 8 representing each day's hours to be worked. You will get a pattern string similar to the schedule, but with some of the digits replaced by a question mark, ?, (ascii 63 decimal). Given a maximum number of hours you can work in a day, replace the question marks with digits so that the sum of the scheduled hours is exactly the hours you must work in a week. Return a string array with all possible schedules you can create, ordered lexicographically.

For example, your partial schedule, pattern = 08??840, your required hours, work_hours = 24, and you can only work, at most, day_hours = 4 hours per day during the two days in question. You have two days on which you must work 24 - 20 = 4 more hours for the week. All of your possible schedules are listed below:
```
0804840
0813840
0822840
0831840
0840840
```

### Function Description
The function must return an array of strings that represents all possible valid schedules. The strings must be ordered lexicographically.

Function has the following parameter(s):

__work_hours:__ an integer that represents the hours you must work in the week

__day_hours:__ an integer that represents the maximum hours you may work in a day

__pattern:__ a string that represents the partially completed schedule

__Task constraints:__
1 ≤ work_hours ≤ 56
1 ≤ day_hours ≤ 8
| pattern | = 7
Each character of pattern ∈ {0, 1,...,8}
There is at least one correct schedule.

__Example:__

Input:
```
work_hours = 3
day_hours = 2
pattern = ??2??00
```

Expected output:

```
0020100
0021000
0120000
1020000
```

__Solution:__
```python
import itertools
def findSchedules(workHours, dayHours, pattern):
    # Write your code here

    total_hours = 0
    marks = 0
    result = []

    for i in pattern:
        try:
            total_hours += int(i)
        except ValueError:
            marks += 1

    hours_left = workHours - total_hours
    combinations = []
    marks_indexes = []
    
        
    for i in list(itertools.product(range(dayHours+1), repeat=marks)):
        if sum(i) == hours_left:
            combinations.append(i)
    
    for i, c in enumerate(pattern):
        if '?' == c:
            marks_indexes.append(i)
    
    for i in combinations:  
        proxy_pattern = pattern
        for j,k in zip(marks_indexes, i):
            a = list(proxy_pattern)
            a[j]=str(k)
            proxy_pattern = "".join(a)
        result.append(proxy_pattern)
        
    return result
```

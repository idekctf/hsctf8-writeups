Writeup for crypto/cyanocitta-cristata-cyanotephra-but-fixed


# Problem:
Only the ciphertext has changed from the original challenge.

The Blue Jay (Cyanocitta cristata) is a passerine bird in the family Corvidae, native to North America. It is resident through most of eastern and central United States and southern Canada, although western populations may be migratory. It breeds in both deciduous and coniferous forests, and is common near and in residential areas. It is predominately blue with a white chest and underparts, and a blue crest. It has a black, U-shaped collar around its neck and a black border behind the crest. Sexes are similar in size and plumage, and plumage does not vary throughout the year. Four subspecies of the Blue Jay are recognized.
Downloads: [output.txt] [cyanocitta-cristata-cyanotephra.sage]


# Solution:
From the category of this problem, we know that it is a crypto problem, so let's download both [output.txt] and [cyanocitta-cristata-cyanotephra.sage]. Opening both

-[output.txt]:
[(26, 66, 70314326037540683861066), (175, 242, 1467209789992686137450970), (216, 202, 1514632596049937965560228), (13, 227, 485439858137512552888191), (1, 114, 112952835698501736253972), (190, 122, 874047085530701865939630), (135, 12, 230058131262420942645110), (229, 220, 1743661951353629717753164), (193, 81, 704858158272534244116883)]
886191939093 589140258545
19440293474977244702108989804811578372332250

-[cyanocitta-cristata-cyanotephra.sage]:
```
import random
var("x y")
flag = int(open('flag.txt','rb').read().hex(),16)
xs = [random.randint(1,256) for i in range(9)]
ys = [random.randint(1,256) for i in range(9)]
assert not any([xs[i]==ys[i] for i in range(9)])
c = [random.randint(1,2^64) for i in range(len(xs))]
f(x,y)=c[0]*x^2+c[1]*y^2+c[2]*x*y+c[3]*x+c[4]*y+c[5]
solns = [int(f(xs[i],ys[i])) for i in range(len(xs))]
print([(xs[i],ys[i],solns[i]) for i in range(9)])
a,b = random.randint(1,2^40),random.randint(1,2^40)
print(a,b)
print((int(f(a,b)))^^flag)
```

-Let's breakdown [cyanocitta-cristata-cyanotephra.sage]:
xs = [random.randint(1,256) for i in range(9)]
The "xs" values create the first 9 values in each list. Let's group the xs values together from [output.txt] and put it in a matrix:
xs = [26, 175, 216, 13, 1, 190, 135, 229, 193]
Let's do the same for ys
ys = [66, 242, 202, 227, 114, 122, 12, 220, 81]

solns = [int(f(xs[i],ys[i])) for i in range(len(xs))]
For "solns", we get the last 9 values in each list. This is found by f(x,y)=c[0]*x^2+c[1]*y^2+c[2]*x*y+c[3]*x+c[4]*y+c[5] equation.

To find the values of c[0], c[1], c[2], c[3], c[4], c[5] we can create a system of equations by substiting 6 values of xs and 6 values of ys into the f(x,y) function. Let's change each c value to a variable.
c[0]=a | c[1]=b | c[2]=c | c[3]=d | c[4]=e | c[5]=f
Our 6 equations:
```
26**2a + 66**2b + 22*66c + 26d + 66e + f = 70314326037540683861066 
175**2a + 242**2b + 175*242c + 175d + 242e + f = 1467209789992686137450970
216**2a + 202**2b + 216*202c + 216d + 202e + f = 1514632596049937965560228
13**2a + 227**2b + 13*227c + 13d + 227e + f = 485439858137512552888191
1a + 114**2b + 114c + 1d + 114e + f = 112952835698501736253972
190**2a + 122**2b + 122*190c + 190d + 122e + f = 874047085530701865939630
```

Now we're ready to solve for the a,b,c,d,e,f. I used this solve script with mpmath and npumpy after putting each equation into matrix form:
```
from mpmath import *
import numpy as np

mp.prec = 1000
mp.dps = 1000
mp.pretty = False

print(mp)

A = matrix([[mpf(26**2), mpf(66**2), mpf(22*66), mpf(26), mpf(66), mpf(1)], 
[mpf(175**2), mpf(242**2), mpf(175*242), mpf(175), mpf(242), mpf(1)], 
[mpf(216**2), mpf(202**2), mpf(216*202), mpf(216), mpf(202), mpf(1)], 
[mpf(13**2), mpf(227**2), mpf(13*227), mpf(13), mpf(227), mpf(1)], 
[mpf(1), mpf(114**2), mpf(114), mpf(1), mpf(114), mpf(1)], 
[mpf(190**2), mpf(122**2), mpf(122*190), mpf(190), mpf(122),mpf(1)]])
B = np.array([mpf(70314326037540683861066), mpf(1467209789992686137450970), mpf(1514632596049937965560228), 
mpf(485439858137512552888191), mpf(112952835698501736253972), mpf(874047085530701865939630)])
X = np.array((A**-1).tolist()).dot(B)

print(X)
```
We obtain these values for c[0], c[1], c[2], c[3], c[4], c[5]:
[mpf('11227347570319680787')
 mpf('8521180195499215015')
 mpf('14706786521482826438')
 mpf('2369955372216026905') 
 mpf('4447776531968912934')
 mpf('14360386757903922932')]
c[0]=11227347570319680787, c[1]=8521180195499215015, c[2]=14706786521482826438, c[3]=2369955372216026905, c[4]=4447776531968912934, c[5]=14360386757903922932

Now that we know all neccesary values for c we can plug each of the numbers back into the original sagemath script with a little modification:
a = 886191939093, b = 589140258545
```
import random
var("x y")
#flag = int(open('flag.txt','rb').read().hex(),16)
#xs = [random.randint(1,256) for i in range(9)]
#ys = [random.randint(1,256) for i in range(9)]
#assert not any([xs[i]==ys[i] for i in range(9)])
c = [11227347570319680787, 8521180195499215015,14706786521482826438,2369955372216026905,4447776531968912934,14360386757903922932]
f(x,y)=c[0]*x^2+c[1]*y^2+c[2]*x*y+c[3]*x+c[4]*y+c[5]
#solns = [int(f(xs[i],ys[i])) for i in range(len(xs))]
#print([(xs[i],ys[i],solns[i]) for i in range(9)])
a,b = 886191939093,589140258545
#print(a,b)
print(int(f(a,b)))
```
Output: 19453112380317214095677318883741141782768295
We then XOR this with 19440293474977244702108989804811578372332250
To get: 34852863801140041977112467590591656571005
We convert this to hex, then ASCII to get our flag!

# Flag: 
flag{d8smdsx01a0}
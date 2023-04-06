## a) Bojanovo sporočilo

```py
uB = 34100010166843172
i=1909
m = uB * 2**123 + i
print(f"Bojanovo sporočilo: {pow(3,m,1353040922319896710729948440742113526140662069124237571)}")
```

```
Bojanovo sporočilo: 41008829042639875776957436734924455885091932366588404
```

## b) Kako lahko Cene izračuna Anitino skrivno število?

```
cj   = 164867525413631686108542244605590332657131844994186648
cj+1 = 669330273667331356154438891368274712878935740757554990
cj+2 = 853109242122207805055956523445529343243869885591747755
```

```
cj   = (uA * 2**123 + j  )^e % n
cj+1 = (uA * 2**123 + j+1)^e % n
cj+2 = (uA * 2**123 + j+2)^e % n
```

Izrazimo `cj+1`

```
cj+1 =
(uA * 2**123 + j + 1)^3 % n =
sum_i^3 (3 choose i) (uA * 2**123 + j)^i * 1^(3-i) % n =
sum_i^3 (3 choose i) (uA * 2**123 + j)^i =
1 + 3 * (uA * 2**123 + j)^1 3 * (uA * 2**123 + j)^2 + (uA * 2**123 + j)^e % n =
1 + 3 * (uA * 2**123 + j) + 3 * (uA * 2**123 + j)^2 + cj % n =
```

Izrazimo `cj+2`

```
cj+2 =
(uA * 2**123 + j + 2)^3 % n =
sum_i^3 (3 choose i) (uA * 2**123 + j)^i * 2^(3-i) % n =
8 + 4 * 3 * (uA * 2**123 + j) + 2 * 3 * (uA * 2**123 + j)^2 + cj % n =
```

Odštejemo enačbe (2xdruga - prva)

```
a = uA * 2**123 + j

cj+2 - cj % n = 8 + 12 * a + 6 * a^2 % n
cj+1 - cj % n = 1 + 3 * a + 3 * a^2 % n
-----------------------------------
cj+2 - 2cj+1 + cj % n = 6 + 6a % n

a = ((cj+2 - 2cj+1 + cj - 6) % n)/6
```

Sedaj smo izrazili `a`. Iz `a` lahko preprosto izračunamo `u(A)`, tako da delimo z `2^123` in zaokrožimo navzdol.

```
c = [
  164867525413631686108542244605590332657131844994186648,
  669330273667331356154438891368274712878935740757554990,
  853109242122207805055956523445529343243869885591747755
]

a = ((c[2] - 2*c[1] + c[0] - 6) % n)//6

print(f"a = {a}")
print(f"uA = {a // 2**123}")
print(f"j = {a % 2**123}")
```

```
a = 172059523753512248264261571009447296047298719699176998
uA = 16180399854194147
j = 3622
```
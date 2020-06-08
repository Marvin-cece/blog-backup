
## 原子atom
字母和除了括号”(“和“)”外的特殊字符开头的字符串、数字。
## 列表list
由一组原子加括号()组成。更进一步，由S-表达式加括号组成。
## S-表达式 S-expression
所有的原子和列表都是S-表达式，null/empty是特殊的表达式。

# 基本操作符
## car
取非空列表的第一个S-表达式表达式。仅对非空列表定义，否则没有答案。

l 是(((hotdogs)) (and) (picle) relish)。(car l)为((hotdogs))
## cdr
取列表l 中除了(car l)的部分，任何非空list的cdr都是另一个list。cdr仅为非空列表定义，否则没有答案。

l 是((x) t r)，(cdr l)为(t r)。(音could-er)。
## cons
把一个atom原子追加到列表最前头，第一个参数是任何S-表达式，第二个是任意列表，返回列表
。

a， l 是()，那么 (cons a l) 为(a)。
## null、atom、quote()
s 是 harrry，(atom? s)为true，判断是否是一个atom原子。

l 是 (),(null? l)、(null? quote())为true。quote()是空列表null的写法。()是空列表。
## eq
是否是同一个非数值atom原子。

a1 是 Harry，a2是 Harry。 (eq? a1 a2)为true。
## lat
lat是单纯由atom原子构成的列表。lat?判断列表中 S-expression 是否都是atom原子。

l 是 (Jack Sprat could eat no chicken fat)， (lat? l)为true。

函数定义：
(cond …) 提问

(lambda …) 产生函数

(define …) 给出函数名

```
(define lat?
  (lambda (l)
    (cond
     ((null? l) #t)
     ((atom? (car l)) (lat? (cdr l)))
     (else #f))))
```
## or
如果第一个是真那么停止并回答真。否则查询第二个问题并以第二个问题的回答作为答案。

(or (null? l1) (atom? l2))是真还是假， 其中 l1 是()， l2 是(d e f g)

真，因为(null? l1)是真。

## member
a 是否是lat的一个成员。
(member? a lat) 是真还是假，其中 a 是poached，lat是(fried eggs and scrambled eggs)。真
```
(define member?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? (car lat) a)
                (member? a (cdr lat)))))))
```
## rember
表示remove a member 删除第一个指定成员。
a 是mint，lat 是(lamb chops and mint jelly)。(rember a lat)是(lamb chops and jelly)
```
(define rember
  (lambda (a lat)
    (cond
      ((null? lat) '())
      (else (cond
              ((eq? a (car lat)) (cdr lat))
              (else (rember a (cdr lat))))))))
```
```
(define rember
 (lambda (a lat)
   (cond
    ((null? lat) '())
    ((eq? (car lat) a) (cdr lat))
    (else (cons (car lat)
                (rember a (cdr lat)))))))
```

## add1、sub1
n是67 ，(add1 n)为68
(sub1 n) 是多少，其中n是5。4
```
(define add1
 (lambda (n)
   (+ n 1)))
```
```
(define sub1
 (lambda (n)
   (- n 1)))
```
## zero、+、-、x
(zero? 1492) 为 false
(+ 46 12) 是58 。+的终止条件是0。
(- 14 3) 11。
(x n m) 做乘法，求n和m的乘积.终止条件((zero? m) 0)，n x 0 = 0。
只考虑正整数。
```
(define +
 (lambda (n m)
   (cond
     ((zero? m) n)
     (else (add1 (+ n (sub1 m)))))))
 ```
 ```
 (define -
 (lambda (n m)
   (cond
     ((zero? m) n)
     (else (sub1 (- n (sub1 m)))))))
 ```
 ```
 (define x
 (lambda n m
   (cond
     ((zero? m) 0)
     (else (+ n (x n (sub1 m)))))))
 ```
 ```
 (x 12 3) = 12 + (x 12 2)
          = 12 + 12 +( x 12 1)
          = 12 + 12 + 12 (x 12 0)
          = 12 + 12 + 12 + 0
          = 36
 ```

 ## tup
 数字组成的list。自然终止条件(null? tup)
 (3 (7 4) 13 9)不是一个tup，(7 4)不是一个数。()是一个特殊空tup

 ## addtup
 返回一个tuple参数的所有数成员的总和。
 tup 是 (3 5 2 8) 那么(addtup tup)是18。

 ## tup+
 每次查找两个tup的元素，即每次递归都是处理两个元素。要求列表等长。
 tup1是(3 6 9 11 4)，tup2是(8 5 2 0 7)，问(tup+ tup1 tup2)是(11 11 11 11 11)
 列表等长实现：
 ```
 (define tup+
 (lambda (tup1 tup2)
   (cond
    ((and (null? tup1) (null? tup2))
     (quote ()))
    (else
     (cons (+ (car tup1) (car tup2))
           (tup+
            (cdr tup1) (cdr tup2)))))))
 ```
列表不等长实现：
```
(define tup+
  (lambda (tup1 tup2)
    (cond
     ((null? tup1) tup2)
     ((null? tup2) tup1)
     (else
      (cons (+ (car tup1) (car tup2))
            (tup+
             (cdr tup1) (cdr tup2)))))))
```
## >、=、<、^、/
(> 12 133)为false
(^ 2 3)为8
```
(define >
 (lambda (n m)
   (cond
     ((zero? n) #f)
     ((zero? m) #t)
     (else (> (sub1 n) (sub1) m)))))
```
```
(define =
  (lambda (n m)
    (cond
      ((zero? m) (zero? n))
      ((zero? n) #f)
      (else (= (sub1 n) (sub1) m)))))

(define =
 (lambda (n m)
   (cond
     ((> n m) #f)
     ((< n m) #f)
     (else #t))))
```
```
(define ^
 (lambda (n m)
   (cond
     ((zero? m) 1)
     (else (x n (^ n (sub1 m)))))))
```
```
(define ÷
 (lambda (n m)
   (cond
     ((< n m) 0)
       (else (add1 (÷ (- n m) m))))))
```
## length
(length lat) 的值是什么，其中lat是(ham and cheese on rye) 5
```
(define length
 (lambda (lat)
   (cond
     ((null? lat) 0)
     (else (add1 (length (cdr lat)))))))
```
## pick、rempick
(pick n lat) 是什么
其中 n 是 4
lat 是 (lasagna spaghetti ravioli macaroni meatball)

(rempick n lat) 是什么，其中
n 是 3
lat 是 (hotdogs with hot mustard)

(hotdogs with mustard)
```
(define pick
 (lambda (n lat)
   (cond
     ((zero? (sub1 n)) (car lat))
     (else (pick (sub1 n) (cdr lat))))))
```
```
(define rempick
 (lambda (n lat)
   (cond
     ((zero? (sub1 n)) (cdr lat))
     (else (cons (car lat) 
                 (rempick (sub1 n) (cdr lat)))))))
```
## number
(number? a) 是真还是假
当
a 是 tomato 假 (number? 76) 真
无函数。number? 如同add1, sub1, zero?, car, cdr, cons, null?, eq?, 以及atom?, 都是元函数

## rember*
(rember* a l)是什么
其中
a 是suace
l 是(((tomato sauce)) ((bean) sauce) (and ((flying)) sauce))

(((tomato)) ((bean)) (and ((flying))))
```
(define rember*
 (lambda (a l)
   (cond
    ((null? l) (quote ()))
    ((atom? (car l))
     (cond
      ((eq? (car l) a)
       (rember* a (cdr l)))
      (else (cons (car l)
                  (rember* a (cdr l))))))
    (else (cons (rember* a (car l))
                (rember* a (cdr l)))))))
```

## 算术表达式
算术表达式可以是atom原子(包括数)，或者由+，×，或者\^连接的两个算术表达式。（cookie也算）

(quote +) 原子 +，而不是操作 +
(quote ×) 原子 ×，而不是操作 ×

(n + 3) 不是算术表达式，因为没有定义括号。如果定义了就可以。


## 化简和辅助函数
(value y) 的值是多少，其中 x 是(1 + (3 ^ 4))。82。
在子表达式上递归value。子部分不是一个算术表达式的表达。我们对一个list表做递归。但是不是所有的list都是算术表达式的表达。我们必须是对子表达式做递归。

```
(define value
 (lambda (nexp)
   (cond
    ((atom? nexp) nexp)
    ((eq? (car (cdr nexp)) (quote +))
     (+ (value (car nexp))
        (value (car (cdr (cdr nexp))))))
    ((eq? (car (cdr nexp)) (quote x))
     (x (value (car nexp))
        (value (car (cdr (cdr nexp))))))
    (else
     (^ (value (car nexp))
        (value (car (cdr (cdr nexp)))))))))
```

算术表达式的表达的第一个子表达式
我们来为算术表达式写一个函数1st-sub-exp
```
(define 1st-sub-exp
 (lambda (aexp)
   (cond
     (else (car (cdr aexp))))))

把(cond …)去掉

(define 1st-sub-exp
 (lambda (aexp)
   (car (cdr aexp))))
```
最后我们把(car nexp)替换为(operator nexp)
```
(define  operator
 (lambda (aexp)
   (car aexp)))
```

```
(define 2nd-sub-exp
 (lambda (aexp)
   (car (cdr (cdr aexp)))))
```
重写：
```
(define value
 (lambda (nexp)
   (cond
    ((atom? nexp) nexp)
    ((eq? (operator nexp) (quote +))
     (+ (value (1st-sub-exp nexp))
        (value (2nd-sub-exp nexp))))
    ((eq? (operator nexp) (quote x))
     (x (value (1st-sub-exp nexp))
        (value (2nd-sub-exp nexp))))
    (else
     (^ (value (1st-sub-exp nexp))
        (value (2nd-sub-exp nexp)))))))
```

-------------
练习

出现次数:
```
(define occur*
 (lambda (a l)
   (cond
     ((null? l) 0)
     ((atom? (car l))
      (cond
        ((eq? a (car l))
         (add1 (occur* a (cdr l))))
        (else (occur* a (cdr l)))))
     (else (+ (occur* a (car l))
              (occur* a (cdr l)))))))
```

1、“它有三个参数：atom原子new和old，还有一个列表lat。

函数insertR把new插入到第一个old后边创建一个lat。”

```
(define insertR
 (lambda (new old lat)
   (cond
     ((null? lat) (quote()))
     (else (cond
             ((eq? old (car lat)) 
              (cons old (cons new (cdr lat))))
             (else (cons (car lat) 
                       (insertR new old (cdr lat)))))))))
```

2、 (x m (sub1 n)) 是什么意思

把n(n = 12)加到自然递归上。如果x正确的话，那么(x 12 (sub1 3))应该是24
4、在所有old后面加上new
```
(define insertR*
 (lambda (new old l)
   (cond
    ((null? l) (quote ()))
    ((atom? (car l))
     (cond
      ((eq? (car l) old)
       (cons old
             (cons new
                   (insertR* new old
                             (cdr l)))))
      (else (cons (car l)
                  (insertR* new old
                            (cdr l))))))
    (else (cons (insertR* new old
                          (car l))
                (insertR* new old
                          (cdr l)))))))
```

3、
现在使用number?函数写一个no-nums，它删除一个lat中的所有出现的数。例如，lat是(5 pears 6 prunes 9 dates)，那么(no-nums lat)的值是(pears prunes dates)

(defin no-nums
 (lambda (lat)
   (cond
    ((null? lat) (quote ()))
    (else (cond
           ((number? (car lat))
            (no-nums (cdr lat)))
           (else (cons (car lat)
                       (no-nums (cdr lat))))))))) 
现在写一个函数 all-nums，它从一个lat中抽取出所有的数构成一个tup。

(define all-nums
 (lambda (lat)
   (cond
    ((null? lat) (quote ()))
    (else
     (cond
      ((number? (car lat))
       (cons (car lat)
             (all-nums (cdr lat))))
      (else
       (all-nums (cdr lat))))))))
写一个函数eqan?，当两个参数a1和a2是一样的原子时为真。

(define eqan?
 (lambda (a1 a2)
   (cond
     ((and (number? a1) (number? a2)) (= a1 a2))
     ((or (number? a1) (number? a2)) #f)
     (else (eq? a1 a2)))))

     现在写一个函数occur描述一个lat中出现原子a的次数

(define occur
 (lambda (a lat)
   (cond
    ((null? lat) 0)
    (else
     (cond
      ((eq? (car lat) a)
       (add1 (occur a (cdr lat))))
      (else
       (occur a (cdr lat))))))))
写一个函数one?仅当n为1时(one? n)为#t真，否则为#f假

(define one?
 (lambda (n)
   (cond
     ((zero? n) #f)
     (else (zero? (sub1 n))))))
或者

(define one?
 (lambda (n)
   (cond
     (else (= 1 n)))))
又或者直接

(define one?
 (lambda (n)
   (= 1 n)))

第一诫：
任何函数的第一个查询表达式都是null?

当在一个由原子构成的列表(lat)上递归时，问两个问题：(null? lat) 还有 else.
当在一个数字上递归时，问两个问题：(zero? n) 还有 else.
当在一个由S-表达式构成的列表（复合列表）上递归时，问三个问题：(null? l), (atom? (car l)),还有 else.
第一诫：
递归时至少要有一个参数变化，并且向终止条件方向变化。变化的参数必须有终止测试条件：当时用cdr时，用null?测试终止。

cons构建list表，add1构建数
当用+构建一个值时，使用0作为终止值，因为0不改变加法的值。

当用x构建一个值时，使用1作为终止值，因为1不改变乘法的值。

当用cons构建list表，终止条件是()

递归时至少要有一个参数变化，并且向终止条件方向变化。变化的参数必须有终止测试条件：当递归原子(lat)时使用(cdr lat)。当递归数n时使用(sub1 n)。当递归一个 S-expression的列表l,当(null? l)或者(atom? (car l))使用(car l)和(cdr l)。
递归时参数要向终止条件方向变化：当使用cdr时，用null?测试终止；
当使用sub1时，用zero?测试终止。

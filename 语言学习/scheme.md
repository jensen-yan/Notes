#### 介绍

1. scheme语言是古老的语言, 对python影响大. 学会后能更轻松掌握不同语言的差异
2. scheme 是lisp语言的方言, lisp很古老也很好用, 很美丽.
3. lisp很简单, 一天就能学会, 但难的地方也难



#### 语法

1. 原始表达: 2, 3.3, true, + , quotient
2. 组合:  (quotient 10 2) = 5, (not true)
3. quotient 求商 是内置函数/procedure. 运算符在最前面.
4. modulo 求余数
5. 不关心缩进, 换行
6. (number? 3)  -> true (3 是number). zero, integer 同理



#### special Forms

1. 一个组合, 不是call expression 是special forms

2. if: (if \<predicate> \<consequent> \<alternative>) 

3. ```scheme
   ; cond 表达, 类似switch
   (cond
       (<p1> <e1>)
       (<p2> <e2>)
       ...
       (<pn> <en>)
       [(else <else-expression>)])
   (cond
           ((> x 0) 'positive)
           ((< x 0) 'negative)
           (else 'zero))
   ```

4. 

5. and/ or: ( and  e1  e2 en)

6. binding 绑定符号: (define pi 3.14)

7. 新函数: (define (name  parameter)  body )

8. ```scheme
   (define (abs x)
     (if (< x 0)
         (-x)
          x))
   (abs -3)  # 3
   (define (average x y) (/ (+ x y) 2))
   ```

9. ```scheme
   (define (square x)  (* x x))
   (define (sqrt x)
                (define (update guess)
                  (if (= (square guess) x)
                        guess
                        (update  (average guess  (/ x guess))) ))
                (update 1))
   # 函数内部可以定义子函数
   ```

10. lambda:  (lambda  (parameter)  body )

11. ```scheme
    (define (plus4 x) (+ x 4))
    (define plus4 (lambda (x) (+ x 4)))
    ((lambda (x y z) (+ x y (square z))) 1 2 3  )	# 1 + 2 + 3*3
    ```

12. (line) 如果line函数没参数, 直接调用

    

#### list

1. scheme中也有list, 链表

2. cons: 两个参数的函数, 创建list

3. car:  返回list第一个元素

4. cdr: 返回list剩下的

5. nil: 空list

6. ```scheme
   (cons 2 nil)
   ```

7. list 的显示方法还是(1 2 3 4), 空格隔开

8. 可以在 https://code.cs61a.org/  中 (draw x) 来画图

9. (null? s) 可以判断list  s 是否是空链表

10. (list 1 2 3 4) 可以直接创建 list

11. (append \`(1 2 3) \`(4 5 6))  -> (1 2 3 4 5 6)

12. (length `(1 2 3))  -> 3 

13. cons( \`a \`b ) 就是点对, 是一个pair

#### symbolic programming

1. 操作 符号组成的list, 表示成结构化的对象

2. symbol(符号) 一般指向值. 怎么指向符号?

3. ```scheme
   (define a 1)
   (define b 2)
   (list a b)		; (1 2) 丢失了符号信息
   (list `a `b)	; (a b) 不丢失
   `a  == (quote a)
   `(a b c)		; 也可以组成list (a b c)
   `(a ,b c)		; (a 2 c)
   ; ,b = (unquote b)
   '(a ,b c) 		; (a (unquote b) c)
   ; 区别` ' 前者是解引用 quality quote, 后者是单引号
   ```

4. 

   ```scheme
   ; 中缀变前缀
   (define (cadr lst) (car (cdr lst)) )
   (define (caddr lst) (car (cdr (cdr lst))))
   ;(1 + 1)	=> (+ 1 1)
   ;((2 * 3) + 4) 	=> (+ (* 2 3) 4)
   (define (infix e)
     (if (not (list? e)) e
         `(,(cadr e) ,(infix(car e)) ,(infix(caddr e)))
         )
     )
   ```

5. 

#### 蛇形三角

1. fd  forward 
2. rt   turn right
3. (load 'ex.scm)  加载文件



#### programming language

1. 编写interpreter , 输入
---
layout: post
title:  "깃허브 블로그에 적용되는 markdown 문법"
categories: markdown
img: use-post/markdown.jpg
toc: true
---
<br/>
github 블로그에 포스팅을 하려면 마크다운 언어를 사용해야 합니다 :D

마크다운 언어는 구조적으로 유효한 XHTML 혹은 HTML 로 선택적 변환이 가능하게 하는 것이 목표라네요.

두고두고 볼 요량으로 정리해봅니다.


<br/><br/>

# **h1 ~ h6**
***

글자 앞에 #을 붙여줍니다. #의 갯수로 크기를 판단합니다.

{% highlight markdown %}
# h1
## h2
### h3
#### h4
##### h5
###### h6
{% endhighlight %}

![image]({{ site.url }}/upload/images/2017-10-08-first-posting/header.jpg)

<br/><br/>

# **문단 제목**
***

구분선을 통하여 문단의 제목을 작성할 수 있습니다. = 는 <h1> 과 같고 - 는 <h2> 정도의 크기입니다.

{% highlight markdown %}
문단의 제목
==============
문단의 제목
--------------
{% endhighlight %}

![image]({{ site.url }}/upload/images/2017-10-08-first-posting/title.jpg)

<br/><br/>

# **이탤릭체, 볼드, 취소선, 밑줄**
***

이탤릭체(기울이기)는 `*` 혹은 `_`, 볼드(굵게) 는 `**` 혹은 `__`, 취소선은 `~~` 기호를 사용합니다. 밑줄은 `<U></U>` 태그를 붙여줍니다.(대문자 U입니다.)

{% highlight markdown %}
*이탤릭체*
_이탤릭체_
**볼드**
__볼드__
~~취소선~~
<U>밑줄</U>
{% endhighlight %}

*이탤릭체*

**볼드**

~~취소선~~

<U>밑줄</U>

<br/><br/>

# **하이퍼링크**
***

글 작성 중 하이퍼링크가 필요하다면 아래 형태로 사용할 수 있습니다

{% highlight markdown %}
[naver](https://www.naver.com)
[naver](https://www.naver.com "네이버")
{% endhighlight %}

적용 시 [sirupe 의 깃허브로 이동](https://github.com/sirupe) 하게 됩니다.

<br/><br/>
# **이미지 삽입**
***

포스팅을 할 때 이미지 삽입이 필요한 경우가 많을텐데요, 아래의 형태로 사용하면 됩니다.

{% highlight markdown %}
![img태그 alt속성](이미지경로)
![poco](/images/poco.jpeg)
{% endhighlight %}

이미지 경로에는 이미지태그 삽입시 입력해야 하는 경로와 동일한 경로를 입력해주시면 됩니다.

<br/>

이미지 경로 정상 입력시 >

![poco](/images/poco.jpeg)

<br/>

이미지 경로가 부정확할 때 >

![poco](images/poco.jpeg)

<br/><br/>

# **인용구**

***

인용구 블럭은 문단 앞에 > 를 붙여줌으로써 사용할 수 있습니다.

인용구 표시 두번 입력하게 되면 인용구 블럭 안에 인용구 블럭을 넣을수도 있지요~

{% highlight markdown %}
> 인용구
>> 인용구
{% endhighlight %}

> 인용구
>> 인용구

<br/><br/>

# **리스트 사용**
***

순서가 있는 리스트 사용은 앞에 숫자를 붙여줍니다.

순서대로 번호를 붙이지 않아도 자동으로 순서가 증가합니다.

{% highlight markdown %}
1. 순서 1
1. 순서 2
1. 순서 3
{% endhighlight %}

1. 순서 1
1. 순서 2
1. 순서 3

순서가 없는 리스트 사용은 -, * 등의 기호를 사용합니다.

{% highlight markdown %}
* 순서 1
    - 순서 1-1
    - 순서 1-2
* 순서 2
* 순서 3
{% endhighlight %}

* 순서 1
    - 순서 1-1
    - 순서 1-2
* 순서 2
* 순서 3

<br/><br/>

# **가로줄**
***

여러가지 표현법이 있지만 모두 가로줄을 그려줍니다.

{% highlight markdown %}
* * *
***
*****
- - -
------------------
{% endhighlight %}

* * *

***

*****

- - -

------------------

<br/><br/>

# **테이블(표 그리기)**
***

표를 그릴 필요가 있을 때에는 `|`, `:`, `-` 세 가지 기호가 사용됩니다.

기호   | 설명
:----:|:-------:
`|`   |컬럼 구분
`-`   |`<th>` 와 `<tr>` 구분
`-:`  |오른쪽 정렬
`:-`  |왼쪽 정렬
`:-:` |중앙정렬

<br/>

{% highlight markdown %}
|    |right   |left    |center
|----|-------:|:-------|:------:
|row1|data    |data    |data
|row2|data2-1 |data2-2 |data2-3 
|row3|data 3-1|data 3-2|data 3-3
{% endhighlight %}

|    |right   |left    |center
|----|-------:|:-------|:------:
|row1|data    |data    |data
|row2|data2-1 |data2-2 |data2-3 
|row3|data 3-1|data 3-2|data 3-3

<br/>

테이블을 표현할 때 대쉬 특수문자 `---` 로 `<th>` 와 `<tr>` 를 구분합니다. `---` <U>선 위에 있으면 모두</U> `<th>` 가 되어버리죠. 아래의 경우 `<th>` 태그에만 적용되어야 하는 css 가 `---` 기준선 위의 두 줄에 전부 적용된 것을 볼 수 있습니다.

{% highlight markdown %}
|    |right   |left    |center
|row1|data    |data    |data
|----|-------:|:-------|:------:
|row2|data2-1 |data2-2 |data2-3 
|row3|data 3-1|data 3-2|data 3-3
{% endhighlight %}

|    |right   |left    |center
|row1|data    |data    |data
|----|-------:|:-------|:------:
|row2|data2-1 |data2-2 |data2-3 
|row3|data 3-1|data 3-2|data 3-3

<br/>

그리고 정렬기준인 `-:`, `:-`, `:-:` 기호를 사용하게 되면 <U>한 열의 정렬기준에 모두 적용</U>되는 것도 확인이 됩니다. `<th>` 태그와 `<tr>` 태그에 해당하는 요소가 모두 한꺼번에 적용이 되죠.

만약 내용에 해당하는 `<tr>` 태그는 와 제목에 해당하는 `<th>` 태그의 정렬을 다르게 하고 싶다면 제목에 해당하는 부분에 따로 `<center>` 태그를 적용시켜주어야 합니다.

{% highlight markdown %}
|    |<center>right</center>|<center>left</center>|center
|----|---------------------:|:--------------------|:---------------------:
|row1|data                  |data                 |data
|row2|data2-1               |data2-2              |data2-3
|row3|data 3-1              |data 3-2             |data 3-3
{% endhighlight %}

|    |<center>right</center>|<center>left</center>|center
|----|---------------------:|:--------------------|:---------------------:
|row1|data                  |data                 |data
|row2|data2-1               |data2-2              |data2-3
|row3|data 3-1              |data 3-2             |data 3-3

<br/><br/>
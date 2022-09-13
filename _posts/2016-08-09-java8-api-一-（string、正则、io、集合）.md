---
layout: post
title: java8 API (一) （String、正则、IO、集合）
date: 2016-08-09
tags: ["2016","@有光","collection","java8","lambda","article"]
---

# 前言

## 目的

*   java8自14年发布已经过去2年多了，越来越多的应用，开始升级到java8,升级不是目的，目的是升级之后，我们就可以使用java8新增的特性，从而提高开发效率、性能、稳定性等。所以伴随着升级，在团队内分享了java8的新特性，不过光了解新特性，知道lambda是啥，真正实践的时候还是有些迷茫（我自己现在就是这样），所以最近开始慢慢的整理java8新增的api，一方面字节在学习，另一方面通过例子的形式，希望可以帮助大家了解新增的api，以及api如何与lambda&stream等新特性应用在开发中。文章中不包含的内容：

    *   java8新特性和语法讲很少，如果对java8还不太了解可以先自己查阅学习。
    *   文中例子都比较简单，代码以lambda和stream的代码为主，java8之前的代码例子基本没有，脑补一下，大家都能想出来。

*   文中例子，都是自己敲过一遍，并且运行出的结果，如果大家发现问题，或者有更好的例子等都欢迎大家随时反馈给我@有光。

## java8新增的API

*   非官方数据

    *   195个新的文件加入到 JDK8 API
    *   93 新类, 89 新接口 , 13 新enum
    *   2699 新方法, 56 新构造函数
    *   46 个接口标记为函数式接口
    *   213 接口默认方法
    *   68 接口静态方法

# 基础

## String

### join()

*   字符串拼接。

            <span class="hljs-keyword">String</span>.<span class="hljs-built_in">join</span>(<span class="hljs-string">":"</span>, <span class="hljs-string">"alibaba"</span>, <span class="hljs-string">"icbu"</span>, <span class="hljs-string">"youguang"</span>);
            <span class="hljs-keyword">String</span> <span class="hljs-built_in">join</span> = <span class="hljs-keyword">String</span>.<span class="hljs-built_in">join</span>(<span class="hljs-string">":"</span>, Lists.newArrayList(<span class="hljs-string">"alibaba"</span>, <span class="hljs-string">"icbu"</span>, <span class="hljs-string">"youguang"</span>));
            System.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">join</span>);<span class="hljs-comment">// => alibaba:icbu:youguang</span>

### chars()

*   创建一个字符流。
*   例子：统计去重后的字符数.

            long collect = <span class="hljs-string">"alibaba:icbu:youguang"</span>.chars()//
                                                  .<span class="hljs-keyword">distinct</span>()//
                                                  .count();
            <span class="hljs-type">System</span>.<span class="hljs-keyword">out</span>.println(collect);// => <span class="hljs-number">11</span>

### StringJoiner

*   字符串拼接。
*   PS:String.join方法内部就是用StringJoiner实现的。

             StringJoiner sj = <span class="hljs-keyword">new</span> StringJoiner(<span class="hljs-string">":"</span>);
            sj.<span class="hljs-built_in">add</span>(<span class="hljs-string">"alibaba"</span>);
            sj.<span class="hljs-built_in">add</span>(<span class="hljs-string">"icbu"</span>);
            sj.<span class="hljs-built_in">add</span>(<span class="hljs-string">"youguang"</span>);
            <span class="hljs-keyword">String</span> result = sj.toString(); <span class="hljs-comment">//alibaba:icbu:youguang</span>

            sj = <span class="hljs-keyword">new</span> StringJoiner(<span class="hljs-string">":"</span>,<span class="hljs-string">"prefix-"</span>, <span class="hljs-string">"-suffix"</span>);
            sj.<span class="hljs-built_in">add</span>(<span class="hljs-string">"alibaba"</span>);
            sj.<span class="hljs-built_in">add</span>(<span class="hljs-string">"icbu"</span>);
            sj.<span class="hljs-built_in">add</span>(<span class="hljs-string">"youguang"</span>);
            System.out.<span class="hljs-built_in">println</span>(sj.toString()); <span class="hljs-comment">//prefix-alibaba:icbu:youguang-suffix</span>

## 正则

### Pattern.splitAsStream()

*   正则分割后的结果，通过流来操作。
*   例子：冒号分割，过滤掉youguang

            <span class="hljs-keyword">String</span> <span class="hljs-built_in">bar</span> = Pattern.<span class="hljs-keyword">compile</span>(<span class="hljs-string">":"</span>)
                                .splitAsStream(<span class="hljs-string">"alibaba:icbu:youguang"</span>)
                                .filter(s -> !s.contains(<span class="hljs-string">"youguang"</span>))
                                .collect(Collectors.joining(<span class="hljs-string">":"</span>));
            <span class="hljs-keyword">System</span>.out.println(<span class="hljs-built_in">bar</span>);<span class="hljs-comment">// => alibaba:icbu</span>

### Pattern.asPredicate()

*   通过正则生成Predicate。
*   例子:结合stream来过滤字符串。

          <span class="hljs-built_in">Pattern</span> pattern = <span class="hljs-built_in">Pattern</span>.compile(<span class="hljs-string">".*@gmail\\.com"</span>);
            Stream.of(<span class="hljs-string">"tony.liw@gmail.com"</span>, <span class="hljs-string">"tony.liw@alibaba-inc.com"</span>)
                  .filter(pattern.asPredicate())
                  .count();<span class="hljs-comment">// => 1</span>

## 数学操作

## 输入&输出

*   java8提供了更简单的方式进行文件操作。

### Files.list

*   Files类从1.7引入，1.8增加list方法生成stream来操作文件/目录。
*   PS:此处使用了1.7中的try-with-resource语法（Stream实现了AutoCloseable接口）

            <span class="hljs-built_in">try</span> (<span class="hljs-built_in">Stream</span><Path> stream = Files.list(Paths.<span class="hljs-built_in">get</span>(<span class="hljs-string">"/opt"</span>))) {
                <span class="hljs-keyword">String</span> joined = stream.<span class="hljs-built_in">map</span>(<span class="hljs-keyword">String</span>::valueOf)<span class="hljs-comment">//</span>
                        .filter(path -> !path.startsWith(<span class="hljs-string">"."</span>))<span class="hljs-comment">//</span>
                         .sorted().collect(Collectors.joining(<span class="hljs-string">"; "</span>));
                System.out.<span class="hljs-built_in">println</span>(<span class="hljs-string">"List: "</span> + joined);<span class="hljs-comment">// => List: /opt/cisco; /opt/local</span>

### Files.find

*   查找文件
*   例子: 找到目录下的所有JavaScript文件（目录查找深度不超过5）。

            Path start = Paths.<span class="hljs-built_in">get</span>(<span class="hljs-string">"/home/admin/"</span>);
            <span class="hljs-keyword">int</span> maxDepth = <span class="hljs-number">5</span>;<span class="hljs-comment">//目录树层数</span>
            <span class="hljs-built_in">try</span> (<span class="hljs-built_in">Stream</span><Path> stream = Files.<span class="hljs-built_in">find</span>(start, maxDepth, (path, attr) ->
                    <span class="hljs-keyword">String</span>.valueOf(path).endsWith(<span class="hljs-string">".js"</span>))) {

                <span class="hljs-keyword">String</span> joined = stream
                        .sorted()
                        .<span class="hljs-built_in">map</span>(<span class="hljs-keyword">String</span>::valueOf)
                        .collect(Collectors.joining(<span class="hljs-string">"; "</span>));
                System.out.<span class="hljs-built_in">println</span>(<span class="hljs-string">"Found: "</span> + joined);
            }

### Files.walk

*   遍历文件，下面例子实现与上面的find同样的功能，不同的是过滤JavaScript文件通过stream的filter实现。

            Path start = Paths.<span class="hljs-built_in">get</span>(<span class="hljs-string">"/home/admin/"</span>);
            <span class="hljs-keyword">int</span> maxDepth = <span class="hljs-number">5</span>;<span class="hljs-comment">//目录树层数</span>
            <span class="hljs-built_in">try</span> (<span class="hljs-built_in">Stream</span><Path> stream = Files.walk(start, maxDepth)) {
                <span class="hljs-keyword">String</span> joined = stream
                        .<span class="hljs-built_in">map</span>(<span class="hljs-keyword">String</span>::valueOf)
                        .filter(path -> path.endsWith(<span class="hljs-string">".js"</span>))
                        .sorted()
                        .collect(Collectors.joining(<span class="hljs-string">"; "</span>));
                System.out.<span class="hljs-built_in">println</span>(<span class="hljs-string">"walk(): "</span> + joined);
            }

### Files.readAllLines

*   读取文件的所有内容

    List<span class="hljs-symbol"><String></span> lines = Files.readAllLines(Paths.<span class="hljs-built_in">get</span>(<span class="hljs-string">"res/nashorn.js"</span>));
    lines.<span class="hljs-built_in">add</span>(<span class="hljs-string">"print('foobar');"</span>);
    Files.<span class="hljs-keyword">write</span>(Paths.<span class="hljs-built_in">get</span>(<span class="hljs-string">"res/nashorn1-modified.js"</span>), lines);

PS:使用该方法的时候要考虑到文件的大小，该方法会将所有内容一次读入内存。

### Files.lines

*   将文件内容转成stream，元素是每行的内容。

           <span class="hljs-keyword">try</span> (Stream<<span class="hljs-keyword">String</span>> stream = Files.lines(Paths.<span class="hljs-built_in">get</span>(<span class="hljs-string">"res/nashorn.js"</span>))) {
                stream
                        .<span class="hljs-built_in">filter</span>(<span class="hljs-built_in">line</span> -> <span class="hljs-built_in">line</span>.contains(<span class="hljs-string">"print"</span>))
                        .<span class="hljs-built_in">map</span>(<span class="hljs-keyword">String</span>::<span class="hljs-built_in">trim</span>)
                        .forEach(System.out::<span class="hljs-built_in">println</span>);
            }

### Files.newBufferedReader&Files.newBufferedWriter

*   带buffer的读写操作

    Path path = Paths.<span class="hljs-built_in">get</span>(<span class="hljs-string">"res/nashorn1.js"</span>);
    <span class="hljs-keyword">try</span> (<span class="hljs-keyword">BufferedReader</span> reader = Files.newBufferedReader(path)) {
        System.out.<span class="hljs-built_in">println</span>(reader.readLine());
    }

    Path path = Paths.<span class="hljs-keyword">get</span>(<span class="hljs-string">"res/output.js"</span>);
    <span class="hljs-keyword">try</span> (BufferedWriter writer = Files.newBufferedWriter(path)) {
        writer.<span class="hljs-built_in">write</span>(<span class="hljs-string">"print('Hello World');"</span>);
    }

## NULL（空指针）

*   如何避免NullPointerException,为了避免NPE,写代码过程!=null检查会出现在各种地方，java8引入了更好的解决方案（Optional+Lambda）。[其他语言解决NPE的语法糖](https://en.wikipedia.org/wiki/Null_coalescing_operator)

### Optional基本

        //不要这样,这与!=null没什么区别
        <span class="hljs-keyword">if</span>(stringOptional.isPresent()){
             <span class="hljs-type">System</span>.<span class="hljs-keyword">out</span>.println(stringOptional.get().length());
        }
        //下面是推荐的常用操作

        optionalValue.ifPresent(s -> <span class="hljs-type">System</span>.<span class="hljs-keyword">out</span>.println(s + <span class="hljs-string">" contains red"</span>));
        //增加到集合汇总
        optionalValue.ifPresent(results::add);
        //增加到集合中，并返回操作结果
        <span class="hljs-type">Optional</span><<span class="hljs-type">Boolean</span>> added = optionalValue.map(results::add);

        //无值的optional
        <span class="hljs-type">Optional</span><<span class="hljs-type">String</span>> optionalString = <span class="hljs-type">Optional</span>.empty();
        //不存在值，返回"<span class="hljs-type">No</span> word"
        <span class="hljs-type">String</span> <span class="hljs-literal">result</span>=optionalValue.orElse(<span class="hljs-string">"No word"</span>);
        //没值，计算一个默认值
        <span class="hljs-literal">result</span> = optionalString.orElseGet(() -> <span class="hljs-type">System</span>.getProperty(<span class="hljs-string">"user.dir"</span>));
        //无值，抛一个异常
        <span class="hljs-keyword">try</span> {
            <span class="hljs-literal">result</span> = optionalString.orElseThrow(<span class="hljs-type">NoSuchElementException</span>::new);
        } catch (<span class="hljs-type">Throwable</span> t) {
        }   

### Optional.map

*   例子：看代码应该秒懂了，就是取foo值，但是为了取这个值，正常的逻辑里面需要增加一串!=null检查。java8中可以通过map函数避免。

    //老的写法
    <span class="hljs-type">Outer</span> outer = new <span class="hljs-type">Outer</span>();
    <span class="hljs-keyword">if</span> (outer != null && outer.nested != null && outer.nested.inner != null) {
        <span class="hljs-type">System</span>.<span class="hljs-keyword">out</span>.println(outer.nested.inner.foo);
    }
    //新的写法
    <span class="hljs-type">Optional</span>.<span class="hljs-keyword">of</span>(new <span class="hljs-type">Outer</span>())
        .map(<span class="hljs-type">Outer</span>::getNested)
        .map(<span class="hljs-type">Nested</span>::getInner)
        .map(<span class="hljs-type">Inner</span>::getFoo)
        .ifPresent(<span class="hljs-type">System</span>.<span class="hljs-keyword">out</span>::println);

    //第二种lambda的方式
    resolve(() -> obj.getNested().getInner().getFoo())
        .ifPresent(<span class="hljs-type">System</span>.<span class="hljs-keyword">out</span>::println);

    public <span class="hljs-keyword">static</span> <T> <span class="hljs-type">Optional</span><T> resolve(<span class="hljs-type">Supplier</span><T> resolver) {
        <span class="hljs-keyword">try</span> {
            T <span class="hljs-literal">result</span> = resolver.get();
            <span class="hljs-keyword">return</span> <span class="hljs-type">Optional</span>.ofNullable(<span class="hljs-literal">result</span>);
        }
        catch (<span class="hljs-type">NullPointerException</span> e) {
            <span class="hljs-keyword">return</span> <span class="hljs-type">Optional</span>.empty();
        }
    }

# 集合

## Collection

### removeIf

*   通过传入Predicate删除集合里面符合条件的元素。

            <span class="hljs-type">Collection</span><<span class="hljs-type">String</span>> <span class="hljs-built_in">c</span> = new <span class="hljs-type">HashSet</span><>();
            <span class="hljs-built_in">c</span>.add(<span class="hljs-string">"Content 1"</span>);
            <span class="hljs-built_in">c</span>.add(<span class="hljs-string">"Content 2"</span>);
            <span class="hljs-built_in">c</span>.add(<span class="hljs-string">"Content 3"</span>);
            <span class="hljs-built_in">c</span>.removeIf(s -> s.<span class="hljs-built_in">contains</span>(<span class="hljs-string">"2"</span>));
            <span class="hljs-type">System</span>.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">c</span>);<span class="hljs-comment">// => [Content 3, Content 1]</span>

*   PS:如果这个接口可以满足过滤的要求，就没必要使用stream了，这个效率更好。
*   扩展：充分利用Predicate,直接看例子

    <span class="hljs-comment">//基本操作</span>
     <span class="hljs-built_in">list</span> =<span class="hljs-literal">new</span> ArrayList(Arrays.asList(<span class="hljs-number">1</span>,<span class="hljs-number">2</span>,<span class="hljs-number">3</span>,<span class="hljs-number">4</span>,<span class="hljs-number">5</span>,<span class="hljs-number">6</span>,<span class="hljs-number">7</span>,<span class="hljs-number">8</span>,<span class="hljs-number">9</span>,<span class="hljs-number">10</span>,<span class="hljs-number">11</span>));
     <span class="hljs-built_in">list</span>.removeIf(a->{ <span class="hljs-keyword">return</span> a%<span class="hljs-number">3</span>==<span class="hljs-number">0</span>;});

     <span class="hljs-comment">//OR 操作</span>
     Predicate<<span class="hljs-built_in">Integer</span>> predicate2 = a->{ <span class="hljs-keyword">return</span> a % <span class="hljs-number">3</span> == <span class="hljs-number">0</span>;};
     Predicate<<span class="hljs-built_in">Integer</span>> predicate3 = a->{ <span class="hljs-keyword">return</span> a % <span class="hljs-number">5</span> == <span class="hljs-number">0</span>;};
     <span class="hljs-built_in">list</span> =<span class="hljs-literal">new</span> ArrayList(Arrays.asList(<span class="hljs-number">1</span>,<span class="hljs-number">2</span>,<span class="hljs-number">3</span>,<span class="hljs-number">4</span>,<span class="hljs-number">5</span>,<span class="hljs-number">6</span>,<span class="hljs-number">7</span>,<span class="hljs-number">8</span>,<span class="hljs-number">9</span>,<span class="hljs-number">10</span>,<span class="hljs-number">11</span>));
     <span class="hljs-built_in">list</span>.removeIf(predicate2.<span class="hljs-keyword">or</span>(predicate3));

     <span class="hljs-comment">//AND 操作</span>
     Predicate<<span class="hljs-built_in">Integer</span>> predicate2 = a->{ <span class="hljs-keyword">return</span> a % <span class="hljs-number">3</span> == <span class="hljs-number">0</span>;};
     Predicate<<span class="hljs-built_in">Integer</span>> predicate3 = a->{ <span class="hljs-keyword">return</span> a % <span class="hljs-number">5</span> == <span class="hljs-number">0</span>;};
     <span class="hljs-built_in">list</span> =<span class="hljs-literal">new</span> ArrayList(Arrays.asList(<span class="hljs-number">1</span>,<span class="hljs-number">2</span>,<span class="hljs-number">3</span>,<span class="hljs-number">4</span>,<span class="hljs-number">5</span>,<span class="hljs-number">6</span>,<span class="hljs-number">7</span>,<span class="hljs-number">8</span>,<span class="hljs-number">9</span>,<span class="hljs-number">10</span>,<span class="hljs-number">11</span>));
     <span class="hljs-built_in">list</span>.removeIf(predicate2.<span class="hljs-keyword">and</span>(predicate3));

### stream、parallelStream

*   大家都知道了，就不说了，记住所有集合类都是通过Collection接口继承来即可。

## Iterable

### forEach

*   遍历每个元素，继承了Iterable接口的都可以使用。

     List<String> stringList = Arrays.asList(<span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>)<span class="hljs-comment">;</span>
            stringList.<span class="hljs-keyword">forEach</span>(<span class="hljs-keyword">System</span>.out::println)<span class="hljs-comment">;</span>

## iterator

### forEachRemaining

*   遍历iterator，并根据指定的action进行处理。开发中可能会出现，需要对集合中的第一个或者前几个元素进行特别的操作，然后继续遍历剩余的元素执行另一个操作（action），这个时候使用forEachRemaining非常合适。例如下面字符串拼接的例子。

            <span class="hljs-keyword">if</span> (!<span class="hljs-keyword">iterator</span>.hasNext()) {
                <span class="hljs-keyword">return</span>;
            }
            <span class="hljs-type">StringBuilder</span> builder = new <span class="hljs-type">StringBuilder</span>();
            builder.append(<span class="hljs-keyword">iterator</span>.next());
            <span class="hljs-keyword">iterator</span>.forEachRemaining( element -> {
                builder.append(<span class="hljs-string">", "</span>).append(element);
            });

*   PS:这里只是说明一下用法，字符串拼接，当然用String.join就够啦。

## List

### replaceAll

*   传入UnaryOperator，将元素替换为一元操作之后的值

       <span class="hljs-built_in">List</span><<span class="hljs-built_in">String</span>> stringList = Arrays.asList(<span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>);
            stringList.replaceAll(<span class="hljs-built_in">String</span><span class="hljs-type">::toUpperCase</span>);
            System.out.println(stringList);<span class="hljs-comment">//[A, B, C]</span>

### sort

*   通过指定Comparator进行排序，参数为空则根据自然排序。

            <span class="hljs-built_in">List</span><<span class="hljs-built_in">String</span>> stringList = Arrays.asList(<span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>);
            stringList.sort(<span class="hljs-built_in">String</span><span class="hljs-type">::compareTo</span>);

*   PS: 可能会有人问这个和Collections.sort()的区别。下面是Collections的sort方法代码。内部调用的就是list.sort

      <span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> <T <span class="hljs-keyword">extends</span> Comparable<? <span class="hljs-keyword">super</span> T>> <span class="hljs-keyword">void</span> <span class="hljs-keyword">sort</span>(List<T> list) {
            list.<span class="hljs-keyword">sort</span>(<span class="hljs-keyword">null</span>);
        }

## Set

*   接口没什么变化。

## Map

### foreach

           Map<<span class="hljs-keyword">String</span>, Integer> <span class="hljs-built_in">map</span> = new HashMap<>();
            <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-string">"A"</span>, <span class="hljs-number">10</span>);
            <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-string">"B"</span>, <span class="hljs-number">20</span>);
            <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-string">"C"</span>, <span class="hljs-number">30</span>);
            <span class="hljs-built_in">map</span>.forEach((k, v) -> System.out.<span class="hljs-built_in">println</span>(<span class="hljs-string">"Item : "</span> + k + <span class="hljs-string">" Count : "</span> + v));

### getOrDefault

*   根据key取value，没有返回一个默认值(这个一直是很多人想要的方法)。

    System.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">map</span>.getOrDefault(<span class="hljs-string">"D"</span>,<span class="hljs-number">40</span>));<span class="hljs-comment">// => 40</span>

*   PS:如果还在使用Apache Commons Collections包中的DefaultedMap类，更换了。

### putIfAbsent

*   javadoc中提供了与putIfAbsent的等价方法，
*   特别说明put方法返回值：对应的key曾经有值返回老的value，否则返回null

    //The default implementation <span class="hljs-built_in">is</span> equivalent to, <span class="hljs-keyword">for</span> this <span class="hljs-built_in">map</span>:
       V v = <span class="hljs-built_in">map</span>.<span class="hljs-built_in">get</span>(<span class="hljs-built_in">key</span>);
      <span class="hljs-keyword">if</span> (v == null)
          v = <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-built_in">key</span>, value);
      <span class="hljs-built_in">return</span> v;

*   例子:

        System.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">map</span>.putIfAbsent(<span class="hljs-string">"B"</span>,<span class="hljs-number">40</span>));<span class="hljs-comment">// => 20</span>
        System.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">map</span>.putIfAbsent(<span class="hljs-string">"D"</span>,<span class="hljs-number">40</span>));<span class="hljs-comment">// => null</span>

### remove(Object.Object)

*   key和valu都相等的时候才删除，下面是javadoc中原来等价方法实现。

          <span class="hljs-keyword">if</span> (<span class="hljs-built_in">map</span>.containsKey(<span class="hljs-built_in">key</span>) && Objects.equals(<span class="hljs-built_in">map</span>.<span class="hljs-built_in">get</span>(<span class="hljs-built_in">key</span>), value)) {
              <span class="hljs-built_in">map</span>.<span class="hljs-built_in">remove</span>(<span class="hljs-built_in">key</span>);
              <span class="hljs-built_in">return</span> <span class="hljs-literal">true</span>;
          } <span class="hljs-keyword">else</span>
              <span class="hljs-built_in">return</span> <span class="hljs-literal">false</span>;

*   例子：

     System.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">map</span>.<span class="hljs-built_in">remove</span>(<span class="hljs-string">"D"</span>,<span class="hljs-number">40</span>));<span class="hljs-comment">// => true</span>

### replace(K,V)

*   map中包含这个key，替换对应的value。javadoc中原来等价的方法实现

          <span class="hljs-keyword">if</span> (<span class="hljs-built_in">map</span>.containsKey(<span class="hljs-built_in">key</span>)) {
              <span class="hljs-built_in">return</span> <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-built_in">key</span>, value);
          } <span class="hljs-keyword">else</span>
              <span class="hljs-built_in">return</span> null;

*   例子：`Map<String, Integer> map = new HashMap<>(); map.put("A", 10); System.out.println(map.replace("A",20));//=> 10`
*   PS：注意与putIfAbsent区别

### replace(K,V,V)

*   map中包含这个key，并且value相等时，替换对应的value。javadoc中原来等价的方法实现

          <span class="hljs-keyword">if</span> (<span class="hljs-built_in">map</span>.containsKey(<span class="hljs-built_in">key</span>) && Objects.equals(<span class="hljs-built_in">map</span>.<span class="hljs-built_in">get</span>(<span class="hljs-built_in">key</span>), value)) {
              <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-built_in">key</span>, newValue);
              <span class="hljs-built_in">return</span> <span class="hljs-literal">true</span>;
          } <span class="hljs-keyword">else</span>
              <span class="hljs-built_in">return</span> <span class="hljs-literal">false</span>;

*   例子:

            Map<<span class="hljs-keyword">String</span>, Integer> <span class="hljs-built_in">map</span> = new HashMap<>();
            <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-string">"A"</span>, <span class="hljs-number">10</span>);
            <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-string">"B"</span>, <span class="hljs-number">20</span>);
            System.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">map</span>.replace(<span class="hljs-string">"B"</span>,<span class="hljs-number">10</span>,<span class="hljs-number">100</span>));<span class="hljs-comment">//=>false</span>
            System.out.<span class="hljs-built_in">println</span>(<span class="hljs-built_in">map</span>.replace(<span class="hljs-string">"B"</span>,<span class="hljs-number">20</span>,<span class="hljs-number">100</span>));<span class="hljs-comment">//=> true</span>

### compute(K,remappingFunction)

*   通过remappingFunction函数，根据指定的key和对应的value，通过计算生成新的value。javadoc中原来等价的方法实现

          V oldValue = <span class="hljs-built_in">map</span>.<span class="hljs-built_in">get</span>(<span class="hljs-built_in">key</span>);
          V newValue = remappingFunction.<span class="hljs-built_in">apply</span>(<span class="hljs-built_in">key</span>, oldValue);
          <span class="hljs-keyword">if</span> (oldValue != null ) {
             <span class="hljs-keyword">if</span> (newValue != null)
                <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-built_in">key</span>, newValue);
             <span class="hljs-keyword">else</span>
                <span class="hljs-built_in">map</span>.<span class="hljs-built_in">remove</span>(<span class="hljs-built_in">key</span>);
          } <span class="hljs-keyword">else</span> {
             <span class="hljs-keyword">if</span> (newValue != null)
                <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-built_in">key</span>, newValue);
             <span class="hljs-keyword">else</span>
                <span class="hljs-built_in">return</span> null;
          }

*   例子：统计单词出现次数/统计图书借阅次数/遇到同样的key，创建或者append一个msg信息等。

            <span class="hljs-meta">Map</span><<span class="hljs-keyword">String, </span>Integer> countMap = Maps.newHashMap()<span class="hljs-comment">;</span>
            List<<span class="hljs-keyword">String> </span><span class="hljs-keyword">bookList= </span>Arrays.asList(<span class="hljs-string">"Book-A"</span>, <span class="hljs-string">"Book-B"</span>, <span class="hljs-string">"Book-A"</span>)<span class="hljs-comment">;</span>
            for (<span class="hljs-keyword">String </span><span class="hljs-keyword">book </span>: <span class="hljs-keyword">bookList) </span>{
                countMap.compute(<span class="hljs-keyword">book, </span>(k, v) -> v == null ? <span class="hljs-number">1</span> : v + <span class="hljs-number">1</span>)<span class="hljs-comment">;</span>
            }
            System.out.println(countMap)<span class="hljs-comment">;</span>

*   PS:要注意如果remappingFunction计算结果为空，会从map中remove

### computeIfAbsent(K,mappingFunction)vs computeIfPresent(K,remappingFunction)

*   这两个方法比较相近容易混淆，对比一下，下面是javadoc中两个新方法等同老代码。左侧:computeIfAbsent,右侧：computeIfPresent [![screenshot](http://img3.tbcdn.cn/L1/461/1/d5e62e17ef3b63a131ac999761def121f52850e9 "screenshot")](d5e62e17ef3b63a131ac999761def121f52850e9)
*   区别：

    *   computeIfAbsent value根据key计算。computeIfPresent value基于key和oldValue计算（参数名也可以看出点意思mappingFunction/remappingFunction）
    *   newValue为空，computeIfPresent会删除map中的元素。

*   例子：这两个方法很容易想到的一个场景是本地缓存（考虑到多线程场景可以使用ConcurrentHashMap）,下面以computeIfAbsent方法为例，同时与java8之前的版本代码做一下对比。

    static <span class="hljs-built_in">Map</span><<span class="hljs-built_in">String</span>, <span class="hljs-built_in">Integer</span>> <span class="hljs-keyword">CACHE</span> = <span class="hljs-literal">new</span> ConcurrentHashMap<>();
    <span class="hljs-keyword">public</span> <span class="hljs-built_in">Integer</span>  getJAVA8(<span class="hljs-built_in">String</span> key){
        <span class="hljs-comment">//java8会使用thread-safe的方式从cache中存取记录</span>
       <span class="hljs-keyword">return</span> <span class="hljs-keyword">CACHE</span>.computeIfAbsent(key, s -> {
           int rt=<span class="hljs-number">0</span>;
           <span class="hljs-comment">//xxx 计算 rt=</span>
           <span class="hljs-keyword">return</span> rt;
       });
    }
    <span class="hljs-keyword">public</span> <span class="hljs-built_in">Integer</span>  getJAVA7(<span class="hljs-built_in">String</span> key){
        <span class="hljs-built_in">Integer</span> rt = <span class="hljs-keyword">CACHE</span>.get(key);
        <span class="hljs-keyword">if</span> (rt == <span class="hljs-built_in">null</span>) {
            <span class="hljs-comment">//java8 中ConcurrentHashMap复写了computeIfAbsent方法做了线程安全控制</span>
            synchronized (<span class="hljs-keyword">CACHE</span>) {
                rt = <span class="hljs-keyword">CACHE</span>.get(key);
                <span class="hljs-keyword">if</span> (rt == <span class="hljs-built_in">null</span>) {
                    <span class="hljs-comment">//xxxx计算 rt=</span>
                    <span class="hljs-keyword">CACHE</span>.put(key, rt);
                }
            }
        }
        <span class="hljs-keyword">return</span> rt;
    }

### merge(K,V,remappingFunction)

*   看名字已经理解80%了，直接看javadoc中与老版本等价的代码好了。

          V oldValue = <span class="hljs-built_in">map</span>.<span class="hljs-built_in">get</span>(<span class="hljs-built_in">key</span>);
          V newValue = (oldValue == null) ? value :
                       remappingFunction.<span class="hljs-built_in">apply</span>(oldValue, value);
          <span class="hljs-keyword">if</span> (newValue == null)
              <span class="hljs-built_in">map</span>.<span class="hljs-built_in">remove</span>(<span class="hljs-built_in">key</span>);
          <span class="hljs-keyword">else</span>
              <span class="hljs-built_in">map</span>.<span class="hljs-built_in">put</span>(<span class="hljs-built_in">key</span>, newValue);

*   是不是一眼看过去和computeIfPresent很像。此处diff一下就清晰了，computeIfPresent是在key对应的value存在时才有效。 [![screenshot](http://img1.tbcdn.cn/L1/461/1/dce357656cb7411a07903bd13adbff48e309c26d "screenshot")](dce357656cb7411a07903bd13adbff48e309c26d)

### replaceAll(function)

*   根据function计算结果，替换map所有entry的value

## Comparator

*   Comparator在平时开发中用的比较多，java8针对该接口，增加了16个默认方法和静态方法，下面挑一些跟大家分享。

### comparing(keyExtractor)

*   先看例子，生成根据firstName对Person进行排序的Comparator:

        <span class="hljs-comment">//老写法</span>
        Comparator<Person> comparator=<span class="hljs-keyword">new</span> Comparator<Person>() {
            <span class="hljs-meta">@Override</span>
            <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">int</span> <span class="hljs-title">compare</span><span class="hljs-params">(<span class="hljs-keyword">final</span> Person o1, <span class="hljs-keyword">final</span> Person o2)</span> </span>{
                <span class="hljs-keyword">return</span> o1.name.compareTo(o2.name);
            }
        };
        <span class="hljs-comment">//lambda 写法</span>
        comparator= (o1, o2) -> o1.name.compareTo(o2.name);
        <span class="hljs-comment">//Comparator.comparing方法，提供提取比较key的function即可</span>
        comparator= Comparator.comparing(Person::getName);

*   PS:该方法，要求提取的key实现了 Comparable接口。key没有Comparable接口可以使用重载实现comparing(keyExtractor,keyComparator)
*   重载方法:

    *   comparing(keyExtractor,keyComparator)
    *   comparingInt(keyExtractor) 提取的key是Integer类型
    *   comparingLong(keyExtractor)
    *   comparingDouble(keyExtractor)

### reversed()

*   反转比较规则

            <span class="hljs-built_in">List</span><<span class="hljs-built_in">Integer</span>> integerList = Arrays.asList(<span class="hljs-number">1</span>, <span class="hljs-number">3</span>, <span class="hljs-number">2</span>, <span class="hljs-number">5</span>, <span class="hljs-number">4</span>);
            <span class="hljs-comment">//正序</span>
            integerList.sort(<span class="hljs-built_in">Integer</span><span class="hljs-type">::compareTo</span>);
            System.out.println(integerList); <span class="hljs-comment">// => [1, 2, 3, 4, 5]</span>
            <span class="hljs-comment">//倒序</span>
            integerList.sort(Comparator.comparing(<span class="hljs-built_in">Integer</span><span class="hljs-type">::intValue</span>).reversed()); <span class="hljs-comment">// => [5, 4, 3, 2, 1]</span>

### thenComparing(other)

*   链式比较，以后不用在使用第三方的ComparisonChain类了。
*   例子:先按字符串长度，再按字符默认顺序排序（忽略大小写）。

     Comparator<<span class="hljs-keyword">String> </span><span class="hljs-keyword">cmp </span>= Comparator.comparingInt(<span class="hljs-keyword">String::length)
    </span>                                              .thenComparing(<span class="hljs-keyword">String.CASE_INSENSITIVE_ORDER);
    </span>

*   重载方法

    *   thenComparing(keyExtractor,keyComparator)
    *   thenComparing(keyExtractor)
    *   thenComparingInt(keyExtractor)
    *   thenComparingLong(keyExtractor)
    *   thenComparingDouble(keyExtractor)

### reverseOrder

*   静态方法，返回与自然排序相反的Comparator（比较元素要实现Comparator接口）

            List<Integer> integerList2 = Arrays.<span class="hljs-keyword">asList</span>(<span class="hljs-number">1</span>, <span class="hljs-number">3</span>, <span class="hljs-number">2</span>, <span class="hljs-number">5</span>, <span class="hljs-number">4</span>);
            integerList.<span class="hljs-keyword">sort</span>(Comparator.reverseOrder());
            System.out.<span class="hljs-keyword">println</span>(integerList);<span class="hljs-comment">// => [5, 4, 3, 2, 1]</span>

### naturalOrder

*   与reverseOrder相反，默认的自然排序器。等价//c1.compareTo(c2);

### nullsFirst(comparator) & nullsLast(comparator)

*   针对null值指定策略，排在前面（nullsFirst），还是后面（nullsLast）

            List<Integer> integerList3 = Lists.newArrayList(<span class="hljs-number">1</span>, <span class="hljs-number">3</span>, <span class="hljs-keyword">null</span>, <span class="hljs-number">5</span>, <span class="hljs-number">4</span>);
            integerList3.<span class="hljs-keyword">sort</span>(Comparator.nullsFirst(Integer::<span class="hljs-keyword">compareTo</span>));
            System.out.<span class="hljs-keyword">println</span>(integerList3);<span class="hljs-comment">// => [null, 1, 3, 4, 5]</span>
            integerList3.<span class="hljs-keyword">sort</span>(Comparator.nullsLast(Integer::<span class="hljs-keyword">compareTo</span>));
            System.out.<span class="hljs-keyword">println</span>(integerList3);<span class="hljs-comment">// => [1, 3, 4, 5, null]</span>

*   这两个比较器一个很大的作用是可以处理结合中有null值的情况，一般默认的比较器遇到null都会抛出NPE，如果不想NPE可以使用nullsFirst/nullsLast包装一下。上面例子：如果不通过Comparator.nullsFirst(Integer::compareTo)生成新的比较器，直接使用Integer::compareTo就会抛NPE异常。

## Arrays

*   工具类中增加了一些对数组相关的流和并行处理的操作。

### Stream

*   将数组转成流进行操作，这样Stream Api就都可以用了[stream api](http://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)。

    //数组内容求和
    int[]<span class="hljs-built_in"> array </span>=<span class="hljs-built_in"> new </span>int[]{1,2,3,4,5};
    //求和
    Arrays.stream(array).sum()
    //过滤
    int[] ms = Arrays.stream(ns).map(n -> n * 2).filter(n -> n % 4 == 0).toArray();

### setAll

*   setAll方法提供一个 int -> T的函数接口做参数，int是数组的索引，T是数组的新值。
*   例子：通过该接口对数组的索引进行操作，然后将指定数组当前索引位置的值赋值为操作后的值。

            int[]<span class="hljs-built_in"> array </span>=<span class="hljs-built_in"> new </span>int[10];
            Arrays.setAll(array, i -> i * 10);
            System.out.println(Arrays.toString(array));// => 输出 [0, 10, 20, 30, 40, 50, 60, 70, 80, 90]        

### spliterator

## Collections

*   emptySortedSet、emptySortedMap、emptyNavigableSet、emptyNavigableMap、unmodifiableNavigableSet、unmodifiableNavigableMap、synchronizedNavigableSet、synchronizedNavigableMap等。

## 集合&数组转化

### 数组转集合

*   没发现java8有什么新增的方式，如果有同学知道的求推荐。老的方式(Arrays/Guava包)：

            List<Integer> integerList = Arrays.asList(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>)<span class="hljs-comment">;</span>
            <span class="hljs-comment">// guava</span>
            integerList = Lists.newArrayList(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>)<span class="hljs-comment">;</span>

### 集合转数组

*   看例子

            <span class="hljs-comment">//原始方式</span>
            Integer[] integerArray = integerList.toArray(new Integer[integerList.size()])<span class="hljs-comment">;</span>
            <span class="hljs-comment">// guava</span>
            <span class="hljs-keyword">int</span>[] intArray = Ints.toArray(integerList)<span class="hljs-comment">;</span>
            <span class="hljs-comment">// java8-基本类型</span>
            intArray = integerList.stream().mapToInt(Integer::intValue).toArray()<span class="hljs-comment">;</span>
             <span class="hljs-comment">// java8-对象类型</span>
            stringArray = stringList.stream().toArray(String[]::new)<span class="hljs-comment">;</span>

*   PS: java8新增了可以通过stream将元素转成数组，但是对一般的集合转数组操作没有比以前更好。不过结合新增的stream操作可以轻松实现List<a>转为B[]的功能。例如：将字符串结合，转成字符串大写</a>

    <span class="hljs-built_in">Character</span>[] characterArray = stringList.stream().<span class="hljs-keyword">map</span>(s -> s.charAt(<span class="hljs-number">0</span>)).toArray(<span class="hljs-built_in">Character</span>[]::<span class="hljs-keyword">new</span>);

# 未完待续

*   java8 API （二） stream整理中
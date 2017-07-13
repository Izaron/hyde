<h1><a id="C____0"></a>Cпортивное программирование и киберпанк</h1>
<p><img src="https://media.giphy.com/media/NKEt9elQ5cR68/giphy.gif" alt=""></p>
<p><em>В мире будущего многое изменилось, но алгоритмы все так же популярны</em></p>
<p>Я довольно долго занимался спортивным программированием, и не пожалел, что потратил довольно много времени на это дело. Спортивное программирование это круто! Если вы не верите, то позвольте доказать это вам.</p>
<h2><a id="____8"></a>Спортивное программирование это интересно</h2>
<p>На самом деле, если человек серьезно вникает в спортивное программирование, то он учит множество интересных вещей. Структуры данных, теория графов, строковые алгоритмы, а также динамическое программирование, теория игр, комбинаторика… Список довольно большой.</p>
<p><img src="https://static1.squarespace.com/static/543593e4e4b0bf8b316933e3/t/55224459e4b0ab9c8e348de9/1428309084880/firstattempt.jpg" alt=""></p>
<p><em>А еще поймет, почему этот код тормозит как телефон с Android 2.3.2</em></p>
<h2><a id="__________16"></a>Спортивное программирование помогает лучше разбираться в коде и прокачивает мозги</h2>
<p><img src="https://s-media-cache-ak0.pinimg.com/originals/be/82/8c/be828c40bb5a13d25c1e2579514ed6f3.gif" alt=""></p>
<p><em>Что-то пошло не так, и я не могу с этим справиться уже два часа</em></p>
<h4><a id="Rubber_Duck___22"></a>Rubber Duck на стероидах</h4>
<p>Соревнования по СП имеют жесткое ограничение по времени. Если вы не отдебажили свое решение вовремя, то вы теряете ценное время/баллы/получаете бОльший штраф. Мозг, настроенный на СП, не будет размазывать поиск бага на час, он его найдет за несколько минут. На самом деле многие задания такие комплексные, что ошибку в них можно допустить в десяти местах. Так что скилл дебаггинга сильно прокачивается.</p>
<h4><a id="________26"></a>Серьезно, вы должны понимать, как это все работает</h4>
<p>Огромное количество программистов не имеет понятия, как работает на самом деле std::map/TreeMap и std::set/TreeSet, не говоря о других вещах. Для них это черная магия. Из-за этого они допускают разные нелепости в виде кода, который работает намного медленнее, чем мог бы работать.</p>
<p><img src="https://68.media.tumblr.com/001f6b1a313463ac3a0b4cbaf21a98cb/tumblr_nvi1n5aODQ1udwzyuo1_500.gif" alt=""></p>
<h4><a id="_____32"></a>А что такое “сдвиг массива”?</h4>
<p>Хотя я не буду далеко ходить за примером - допустим, у нас есть std::vector/ArrayList из 100000 элементов. Нужно написать код, который в цикле постоянно просматривает крайний элемент и затем удаляет его. Логично и “удобно” написать код, удаляющий первый элемент. И тут нас ждет сюрприз - оказывается, что этот код будет работать несколько секунд! А если бы мы удаляли всегда последний элемент, это бы отработало практически мгновенно. Опытный программист сразу понимает, почему так происходит. (Хотя, если бы мы использовали double-linked list, это работало бы быстро в обеих случаях).</p>
<h4><a id="To_be_continued_36"></a>To be continued</h4>
<p>Есть и другие побочные эффекты, как появившаяся способность <em>быстро</em> писать код, но это все выходит за рамки данного поста.</p>
<h2><a id="____40"></a>Спортивное программирование это престижно</h2>
<p><img src="https://static.tumblr.com/0c4c1044875d8d4b73b616699b4f4c37/35ras8e/dfin2bmo6/tumblr_static_bg_cyberjam_night.png" alt=""></p>
<p>СП никогда не было чем-то андерграундным. В мире постоянно проводятся контесты с интересными задачами. Самые известные соревнования это ACM ICPC - олимпиада для студентов, и IOI - для школьников. Также в интернете регулярно проводятся крупные контесты как Google Code Jam, Facebook Hacker Cup, где лучшим участникам оплачивается поездка на “онсайт” - финальный раунд. В России проводятся похожие ежегодные олимпиады VK Cup, Яндекс.Алгоритм, Russian Code Cup (от MailRu). Не говоря уже про регулярные соревнования на сайтах, самые популярные из которых Codeforces и TopCoder. Высокий рейтинг на этих сайтах по итогам участия в кратковременных контестах украшает резюме.</p>
<p>Контестов много, и системы в них используются абсолютно разные, но суть одинаковая - надо решать задания.</p>
<h2><a id="____47"></a>Как выглядит типичное задание</h2>
<p><img src="https://68.media.tumblr.com/6e5bf2cc7f2574aed534af35700ac94e/tumblr_opgyaiBTJo1v63h88o1_500.gif" alt=""></p>
<h4><a id="___51"></a>Легенды и саги</h4>
<p>Что такое задача - problem? Мы получаем набор чисел и строк из входного потока или файла, и нам надо написать программу, которая читает эти данные, как-то обрабатывает их и выводит другие данные (ответ) в выходной поток/файл. К задаче прилагается условие, которое называется “легендой”. В условии также обычно бывает 1-3 примера входных и выходных данных. Легенда устанавливает модель задачи и условное обозначение входных и выходных данных, то есть она как будто говорит “представьте, что следующие N чисел являются элементами массива A, а следующие M чисел это элементы массива B”.</p>
<p><img src="http://i.imgur.com/IbQ84iF.gif" alt=""></p>
<h4><a id="___57"></a>Рекомендовано к решению</h4>
<p>Когда инженеры Google Code Jam давали лекцию про СП, они разобрали неплохое задание с одного из своих раундов - <a href="https://code.google.com/codejam/contest/6274486/dashboard#s=p2">Evaluation</a>.</p>
<p>Дается набор выражений с присваиваниями. Надо узнать, можно ли так упорядочить строки, чтобы все переменные были бы успешно вычислены?</p>
<p>Например, даются эти строки:</p>
<pre><code>a=f(b,c)
b=g()
c=h()
</code></pre>
<p>Ответ “можно”. Вот порядок, в котором каждое выражение корректно:</p>
<pre><code>b=g()
c=h()
a=f(b,c)
</code></pre>
<p>А этот порядок был бы невалидным, так как на момент вычисления <code>a</code> переменная <code>c</code> является undefined:</p>
<pre><code>b=g()
a=f(b,c)
c=h()
</code></pre>
<p><img src="http://i.imgur.com/dbidMbl.gif" alt=""></p>
<h4><a id="__89"></a>Система оценки</h4>
<p>В каждом файле до 20 тестов. Если мы сможем найти ответ, когда в каждом тесте до сотни строк (n &lt;= 100), то мы получим 12 баллов. Если строк тысяча (n &lt;= 1000), то получим в сумме 12+15 баллов. Разделение на две группы сделано не зря. Часто бывает так, что решение придумывается недостаточно оптимальное, тогда оно получает только часть баллов. Предполагается, что 12 баллов получит решение за <a href="https://en.wikipedia.org/wiki/Big_O_notation">O(n^3)</a>, а 27 баллов дается за <a href="https://en.wikipedia.org/wiki/Big_O_notation">O(n^2)</a>. Лучше сразу написать решение быстрее.</p>
<h4><a id="Hack_93"></a>Hack!</h4>
<pre><code class="language-cpp"><span class="hljs-preprocessor">#<span class="hljs-keyword">include</span> <span class="hljs-string">&lt;bits/stdc++.h&gt;</span></span>
<span class="hljs-keyword">using</span> <span class="hljs-keyword">namespace</span> <span class="hljs-built_in">std</span>;

<span class="hljs-function"><span class="hljs-keyword">int</span> <span class="hljs-title">main</span><span class="hljs-params">(<span class="hljs-keyword">int</span> argc, <span class="hljs-keyword">char</span> *argv[])</span> </span>{
    <span class="hljs-keyword">int</span> t;
    <span class="hljs-built_in">cin</span> &gt;&gt; t;
    <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; t; i++) {
        <span class="hljs-built_in">cout</span> &lt;&lt; <span class="hljs-string">"Case #"</span> &lt;&lt; i + <span class="hljs-number">1</span> &lt;&lt; <span class="hljs-string">": "</span>;
        <span class="hljs-built_in">cerr</span> &lt;&lt; <span class="hljs-string">"Test #"</span> &lt;&lt; i + <span class="hljs-number">1</span> &lt;&lt; endl;
        <span class="hljs-built_in">cout</span> &lt;&lt; (solve() ? <span class="hljs-string">"GOOD"</span> : <span class="hljs-string">"BAD"</span>) &lt;&lt; endl;
    }
}
</code></pre>
<p><code>solve()</code> будет читать очередной тест и говорить нам ответ.</p>
<p>Лучше сразу написать вспомогательную функцию для “разрезания” строки:</p>
<pre><code class="language-cpp"><span class="hljs-built_in">vector</span>&lt;<span class="hljs-built_in">string</span>&gt; crop(<span class="hljs-built_in">string</span>&amp; s) {
    <span class="hljs-built_in">vector</span>&lt;<span class="hljs-built_in">string</span>&gt; vec;
    <span class="hljs-built_in">string</span> cur = <span class="hljs-string">""</span>;

    <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; (<span class="hljs-keyword">int</span>) s.size(); i++) {
        <span class="hljs-keyword">if</span> (s[i] &lt; <span class="hljs-string">'a'</span> || s[i] &gt; <span class="hljs-string">'z'</span>) {
            <span class="hljs-comment">// if s[i] is not a lowercase english letter</span>
            <span class="hljs-keyword">if</span> (!cur.empty()) {
                vec.push_back(cur);
                cur = <span class="hljs-string">""</span>;
            }
        } <span class="hljs-keyword">else</span> {
            cur.push_back(s[i]);
        }
    }

    <span class="hljs-keyword">return</span> vec;
}
</code></pre>
<h4><a id="__135"></a>Теория графов</h4>
<p>Как нам вообще решать это? Тут на сцену выходит популярная и легендарная <strong>теория графов</strong>.</p>
<p><img src="https://s-media-cache-ak0.pinimg.com/originals/37/71/58/377158fca2b73b083fd0aa4a4f703930.gif" alt=""></p>
<p>Просто представим нашу задачу в виде графа! Пусть каждая переменная это <strong>вершина</strong> графа, из которой в каждую зависимую переменную (то есть соотв. вершину) идет направленное <strong>ребро</strong>.</p>
<h4><a id="__143"></a>Построение модели</h4>
<p>Наш пример выглядит так - у нас есть точки-вершины в пространстве <code>a</code>, <code>b</code>, <code>c</code>, и есть две стрелки-рёбра, направленные из <code>a</code> в другие вершины. Так же можно представить любой тест. Интуитивно задачу можно решить так - мы будем постоянно просматривать граф и смотреть, есть ли где-нибудь вершина, из которой не исходит <strong>ни одного</strong> ребра. Если такая вершина есть, то ее переменную мы “определяем”. Саму вершину из графа и ребра к ней выкидываем, и просматриваем граф заново.</p>
<p><img src="http://i.imgur.com/noiejkr.gif" alt=""></p>
<h4><a id="__149"></a>Нахождение ответа</h4>
<p>Если у нас еще остались неопределенные переменные, но мы не можем найти никакой вершины, откуда не ведет ни одного ребра, это значит что мы имеем <strong>цикл</strong>, то есть зависимости, которых никак разрешить нельзя, потому что где-то <code>x</code> зависит от <code>y</code>, <code>y</code> зависит от <code>z</code>, <code>z</code> зависит от <code>x</code>, грубо говоря. Значит, для этого теста решения нет. Если мы все переменные определили, то решение есть.</p>
<h4><a id="__153"></a>Дописываем код</h4>
<pre><code class="language-cpp"><span class="hljs-function"><span class="hljs-keyword">bool</span> <span class="hljs-title">solve</span><span class="hljs-params">()</span> </span>{
    <span class="hljs-keyword">int</span> n;
    <span class="hljs-built_in">cin</span> &gt;&gt; n;

    <span class="hljs-built_in">vector</span>&lt;<span class="hljs-built_in">string</span>&gt; sources(n);  <span class="hljs-comment">// source lines</span>
    <span class="hljs-built_in">map</span>&lt;<span class="hljs-built_in">string</span>, <span class="hljs-keyword">int</span>&gt; indexes;   <span class="hljs-comment">// map of "variable name" -&gt; "index"</span>
    <span class="hljs-built_in">vector</span>&lt; <span class="hljs-built_in">vector</span>&lt;<span class="hljs-built_in">string</span>&gt; &gt; deps_raw(n);   <span class="hljs-comment">// list of dependencies (as variable string)</span>
    <span class="hljs-built_in">vector</span>&lt; <span class="hljs-built_in">set</span>&lt;<span class="hljs-keyword">int</span>&gt; &gt; deps(n);   <span class="hljs-comment">// list of dependencies (as indexes)</span>

    <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; n; i++) {
        <span class="hljs-built_in">string</span> s;
        <span class="hljs-built_in">cin</span> &gt;&gt; s;

        sources[i] = s;

        <span class="hljs-built_in">vector</span>&lt;<span class="hljs-built_in">string</span>&gt; variables = crop(s);
        <span class="hljs-built_in">string</span> name = variables[<span class="hljs-number">0</span>];
        indexes[name] = i;

        <span class="hljs-built_in">vector</span>&lt;<span class="hljs-built_in">string</span>&gt; var_names;
        <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> z = <span class="hljs-number">2</span>; z &lt; (<span class="hljs-keyword">int</span>) variables.size(); z++)
            var_names.push_back(variables[z]);
        deps_raw[i] = var_names;
    }

    <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; n; i++) {
        <span class="hljs-keyword">for</span> (<span class="hljs-keyword">auto</span> dep : deps_raw[i]) {
            <span class="hljs-keyword">if</span> (!indexes.count(dep))
                <span class="hljs-keyword">return</span> <span class="hljs-literal">false</span>;
        }
    }

    <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; n; i++) {
        <span class="hljs-built_in">set</span>&lt;<span class="hljs-keyword">int</span>&gt; deps_set;
        <span class="hljs-keyword">for</span> (<span class="hljs-keyword">auto</span> dep : deps_raw[i])
            deps_set.insert(indexes[dep]);
        deps[i] = deps_set;
    }

    <span class="hljs-comment">// main loop here!</span>
    <span class="hljs-built_in">vector</span>&lt;<span class="hljs-keyword">bool</span>&gt; used(n);
    <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> count = <span class="hljs-number">0</span>; count &lt; n; count++) {
        <span class="hljs-keyword">int</span> v = -<span class="hljs-number">1</span>;
        <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; n; i++) {
            <span class="hljs-keyword">if</span> (used[i])
                <span class="hljs-keyword">continue</span>;
            <span class="hljs-keyword">if</span> (deps[i].empty()) {
                v = i;
                <span class="hljs-keyword">break</span>;
            }
        }
        <span class="hljs-keyword">if</span> (v == -<span class="hljs-number">1</span>)
            <span class="hljs-keyword">return</span> <span class="hljs-literal">false</span>;

        <span class="hljs-built_in">cerr</span> &lt;&lt; count &lt;&lt; <span class="hljs-string">"-th line: "</span> &lt;&lt; sources[v] &lt;&lt; endl;

        used[v] = <span class="hljs-literal">true</span>;
        <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; n; i++) {
            <span class="hljs-keyword">if</span> (!used[i]) {
                <span class="hljs-keyword">if</span> (deps[i].count(v))
                    deps[i].erase(v);
            }
        }
    }

    <span class="hljs-keyword">return</span> <span class="hljs-literal">true</span>;
}
</code></pre>
<p><a href="https://gist.github.com/Izaron/43c521e5597550029de928c5fb8f3ef9">Полный исходный код</a>.</p>
<p><img src="https://media.giphy.com/media/IojlRkMFIgZVu/giphy.gif" alt="" align="right"></p>
<p>Скомпилировать можно в вашей IDE или в консоли <code>g++ -std=c++11 main.cpp -o main</code>, смотреть работу программы <code>./main &lt;input_file &gt;output_file</code>.</p>
<p>Выхлоп в консоли из примера на странице с задачей:</p>
<pre><code>Test #1
0-th line: b=g()
1-th line: c=h()
2-th line: a=f(b,c)
Test #2
Test #3
Test #4
0-th line: x=f()
1-th line: y=g(x,x)
</code></pre>
<p><img src="http://4.bp.blogspot.com/-3pgg2B2VrG0/U7ndQIaSTPI/AAAAAAAAAIM/jU3yDsl_wDk/s1600/title.gif" alt=""></p>
<p>На этом все, спасибо за внимание! Но не пропадайте надолго…</p>
<p>:wq</p>

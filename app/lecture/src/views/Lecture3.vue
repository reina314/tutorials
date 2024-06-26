<script setup>
    import { ref, onMounted, onUnmounted } from 'vue';
    import Title from '../components/Title.vue';
    import Header from '../components/Header.vue';
    import SubHeader from '../components/SubHeader.vue';
    import Paragraph from '../components/Paragraph.vue';
    import PyScript from '../components/PyScript.vue';
    import Hint from '../components/Hint.vue';
    import ProgressFooter from '../components/ProgressFooter.vue';

    const completion = ref(0);

    const updateScrollCompletion = () => {
        const currentProgress = window.scrollY;
        const scrollHeight = document.body.scrollHeight - window.innerHeight;
        if (scrollHeight) {
            completion.value = Math.round((currentProgress / scrollHeight) * 100);
            if (completion.value > 100) {
                completion.value = 100;
            }
        }
    };

    onMounted(() => {
        window.addEventListener("scroll", updateScrollCompletion);
    });

    onUnmounted(() => {
        window.removeEventListener("scroll", updateScrollCompletion);
    });
</script>

<template>
    <Title>第3講 Python: 配列、辞書</Title>

    <div class="container w-full sm:w-5/6 md:w-4/6 mx-auto mb-10 text-start">
        <Header>1.配列</Header>
        <Paragraph>
            配列(<span class="text-yellow-400">list</span>, <span class="text-yellow-400">array</span>)とは、「複数の要素（値）の集合を格納・管理するのに用いられるデータ構造」(<a href="https://ja.wikipedia.org/wiki/%E9%85%8D%E5%88%97" class="underline text-blue-200">Wikipedia「配列」</a>より引用)のことである。
            より簡便に、いろいろなデータが入った容器だと捉えても構わない。

            <SubHeader>配列の基本</SubHeader>
            <Paragraph>
                まずは、実際の例を見て配列を理解しよう。

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    print(food[0])
                </PyScript>

                上の例では、1行目でstr型のデータを格納したlistを定義している。
                listに格納されたデータは、food[0]のように、listの変数名の後ろにインデックス(<span class="text-yellow-400">index</span>)を指定することでアクセスできる。
                ここで、indexが0オリジン(<span class="text-yellow-400">zero-based</span>)であることに十分注意してほしい。
            </Paragraph>

            <Paragraph>
                次に、配列の要素を書き換えてみよう。

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    food[0] = 'Avocado'
                    print(food)
                </PyScript>

                上の例では、foodの0番目の要素を'Avocado'に書き換えている。
                この場合も、listの変数名の後にindexを書けば良い。
            </Paragraph>

            <Paragraph>
                補足: Pythonにおけるlistは、一般的なCやJavaの配列とは異なり、動的配列(<span class="text-yellow-400">dynamic array</span>)である。これは、list内のデータが増減するたびに、自動的にメモリ領域が再割り当てされるということである。したがって、事前に定まったメモリ領域を確保し、管理するという手間は生じない。<br>
                これは、C++におけるstd::vectorやJavaにおけるjava.util.ArrayListなどのライブラリが実装するものと同様の機能である。
            </Paragraph>

            <SubHeader>配列とfor文</SubHeader>
            <Paragraph>
                配列の全ての要素を順に取り出したい場合には、for文を用いる。
                listはイテラブルオブジェクト(<span class="text-yellow-400">iterable object</span>)であるため、単にfor文のin句の直後に渡せば良い。  

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    for i in food:
                        print(i)
                </PyScript>

                また、配列の全ての要素に対して変更を加えたい場合にもfor文を用いる。
                ただし、上記の方法ではうまくいかない。
                なぜなら、indexのiには配列の各要素のコピー(<span class="text-yellow-400">deep copy</span>)が渡されているため、参照元のデータを書き換えることができないからである。これについては、次項で詳説する。
            </Paragraph>

            <Paragraph>
                したがって、配列の全ての要素を書き換えるためには次のようにすること。

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    for i in range(len(food)):
                        food[i] = 'Meat'
                    
                    print(food)
                </PyScript>

                <span class="text-yellow-400">len()</span>関数は、与えられたlistの要素の個数を返す。
                つまり上の例では、foodの要素の個数は5個のため、len(food)はint型の5を返す。
                これをrange()関数に渡すことで、listのindexに対応した数列が得られる。
            </Paragraph>

            <SubHeader>要素の追加、削除</SubHeader>
            <Paragraph>
                配列に要素を追加するためには、いくつかの方法がある。代表的なものを以下に挙げる。

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    food.append('Fennel')
                    print(food)
                </PyScript>

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    food += ['Fennel']
                    print(food)
                </PyScript>

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    food.insert(5, 'Fennel')
                    print(food)
                </PyScript>
            </Paragraph>

            <Paragraph>
                配列の要素を削除するためにも、いくつかの方法がある。代表的なものを以下に挙げる。

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    food.remove('Eggplant')
                    print(food)
                </PyScript>

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    food.pop(4)
                    print(food)
                </PyScript>

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    del food[4]
                    print(food)
                </PyScript>

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']

                    food = [i for i in food if i != 'Eggplant']
                    print(food)
                </PyScript>
            </Paragraph>

            <SubHeader>配列のコピー</SubHeader>
            <Paragraph>
                データのコピーには、<span class="text-yellow-400">deep copy</span>と<span class="text-yellow-400">shallow copy</span>の2種類が存在する。
                一般の変数のコピーはdeep copyであるが、配列の場合にはshallow copyとなるため、注意を要する。
                まずは、例をそれぞれ見てみよう。

                <PyScript>
                    a = 2
                    b = a       # deep copy
                    
                    print(f'Before: a = {a}')
                    print(f'Before: b = {b}')

                    a = 3       # コピーの参照元を書き換え

                    print(f'After: a = {a}')
                    print(f'After: b = {b}')
                </PyScript>

                上のコードでは、一般の変数のコピー、すなわちdeep copyの例を示した。
                deep copyは、コピー元のオブジェクト(変数)のみならず、メモリ上にある参照先の実体(インスタンス)ごとコピーを行うため、一方を書き換えても、他方のインスタンスが書き換わることはない。
            </Paragraph>

            <Paragraph>
                では次に、shallow copyの例を見てみよう。

                <PyScript>
                    sequence1 = [0, 1, 2, 3, 5, 8, 13]
                    sequence2 = sequence1           # shallow copy

                    print(f'Before: seq1 = {sequence1}')
                    print(f'Before: seq2 = {sequence2}')

                    sequence1[0] = 1                # コピーの参照元を書き換え

                    print(f'After: seq1 = {sequence1}')
                    print(f'After: seq2 = {sequence2}')
                </PyScript>

                上の例は、コピーの参照元を書き換えると、コピーした配列の値も書き変わることを示している。
                shallow copyでは、コピーされるのはコピー元のオブジェクト(変数)のみであり、実体となるメモリ上のデータは共有される。
                つまり、同じデータに対して異なる変数名を使ってアクセスしていることに相違ない。
            </Paragraph>

            <Paragraph>
                配列をdeep copyしたい場合には、copyモジュールの<span class="text-yellow-400">deepcopy()</span>関数を使うか、以下のように、for文で各要素を一つずつコピーする方法がある。

                <PyScript>
                    food = ['Apple', 'Banana', 'Cherry', 'Durian', 'Eggplant']
                    new_food = [i for i in food]

                    food[0] = 'Avocado'

                    print(f'food: {food}')
                    print(f'new_food: {new_food}')
                </PyScript>
            </Paragraph>

            <SubHeader>おまけ: 多次元配列</SubHeader>
            <Paragraph>
                これまで見てきた配列は、すべて<span class="text-yellow-400">1次元配列</span>(one-dimensional array, linear array)であった。
                つまり、要素が一方向のみに増えていく1次元的な配列である。
                これに対し、要素がn方向に向かって増えていくものを一般に<span class="text-yellow-400">n次元配列</span>(n-dimensional array)と呼ぶ。
                以下は、2次元配列の例である。

                <PyScript>
                    matrix = [
                                [1, 2, 3],
                                [4, 5, 6],
                                [7, 8, 9]
                            ]

                    print(matrix[1][2])
                </PyScript>

                ここで、配列が数学のベクトルや行列と似た概念であることに気づいた人がいるかもしれない。
                実際、計算科学の文脈では配列のことをベクトルと呼ぶ場合もあるようである。
            </Paragraph>
        </Paragraph>


        <Header>2.辞書</Header>
        <Paragraph>
            配列について理解できれば、辞書(<span class="text-yellow-400">dictionary</span>)を理解するのはそれほど難しいことではない。なぜなら辞書は、配列のindexにスカラー値以外のデータ型も用いられるに過ぎないからである。また、辞書のindexをキー(<span class="text-yellow-400">key</span>)と呼ぶ。

            <SubHeader>辞書の基本</SubHeader>
            <Paragraph>
                とりあえず、辞書の実例を見てみるとしよう。

                <PyScript>
                    me = {"name": "John", "age": 10, "hobby": "video game"}

                    print(me["name"])
                </PyScript>

                このようにして、辞書は各要素にkey("name", "age", "hobby")を割り当てることができる。
                要素にアクセスする際は、indexの代わりにkeyを用いる。
                要素の順番を意識する必要がある配列とは異なり、keyを使用することで、任意の要素を絶対的に取り出すことができる。
                当然ながら、keyは一意でなければならない。
            </Paragraph>

            <Paragraph>
                keyを使うということ以外は、ほとんど配列と変わらない。
                以下は、辞書の要素を書き換える例である。

                <PyScript>
                    me = {"name": "John", "age": 10, "hobby": "video game"}

                    me["name"] = "Tom"
                    print(me["name"])
                </PyScript>
            </Paragraph>

            <SubHeader>辞書とfor文</SubHeader>
            <Paragraph>
                辞書には、要素の他にkeyが存在するため、for文ではその両方を取り出すことが可能である。
                以下の例を見てみよう。

                <PyScript>
                    me = {"name": "John", "age": 10, "hobby": "video game"}

                    for k, v in me.items():
                        print(f'{k}: {v}')
                </PyScript>

                配列のように、辞書単体をfor文に渡すことはできず、辞書のクラスメソッドである<span class="text-yellow-400">items()</span>関数を先に呼び出さねばならない。
                すると、for文の1番目のindex(k)にはkeyが、2番目のindex(v)には要素の値(value)が渡される。
            </Paragraph>

            <SubHeader>要素の追加、削除</SubHeader>
            <Paragraph>
                辞書に要素を追加するためには、keyを指定する必要があることに注意しよう。
                以下に代表的な方法を示す。

                <PyScript>
                    desktop = {"os": "Windows 11", "cpu": "Core i9-14900K"}

                    desktop["gpu"] = "RTX 4090"
                    print(desktop)
                </PyScript>

                <PyScript>
                    desktop = {"os": "Windows 11", "cpu": "Core i9-14900K"}

                    desktop.update(gpu="RTX 4090")
                    print(desktop)
                </PyScript>

                <PyScript>
                    desktop = {"os": "Windows 11", "cpu": "Core i9-14900K"}

                    desktop.update({"gpu": "RTX 4090"})
                    print(desktop)
                </PyScript>
            </Paragraph>

            <Paragraph>
                次に、要素を削除する方法を示す。

                <PyScript>
                    desktop = {"os": "Windows 11", "cpu": "Xeon", "gpu": "RTX 4090"}

                    desktop.pop("cpu")
                    print(desktop)
                </PyScript>
                
                <PyScript>
                    desktop = {"os": "Windows 11", "cpu": "Xeon", "gpu": "RTX 4090"}

                    del desktop["cpu"]
                    print(desktop)
                </PyScript>

                <PyScript>
                    desktop = {"os": "Windows 11", "cpu": "Xeon", "gpu": "RTX 4090"}

                    desktop = {k: v for k, v in desktop.items() if k != "cpu"}
                    print(desktop)
                </PyScript>

                このように、配列とほぼ同様にして値を操作することができる。
            </Paragraph>

            <SubHeader>配列と辞書</SubHeader>
            <Paragraph>
                配列と辞書は、互いに入れ子にすることが可能である。
                具体例を見てみるとしよう。

                <PyScript>
                    arr1 = [1, 2, 3]
                    arr2 = [4, 5, 6]
                    dict = {"array1": arr1, "array2": arr2}

                    print(dict)
                </PyScript>

                <PyScript>
                    person1 = {"name": "Mike", "age": 50}
                    person2 = {"name": "Josh", "age": 20}
                    arr = [person1, person2]
                    
                    print(arr)
                </PyScript>

                このようにして、多重なる階層を持つ配列・辞書を作ることも可能である。
            </Paragraph>
        </Paragraph>


        <Header>3.練習問題</header>
        <Paragraph>
            作問者からのお願い: numpyを使わないで下さい。全問、一瞬で易化します。最後の問題のみ、あらかじめmathライブラリをインポートしておきました。
        </Paragraph>
        <Paragraph>
            <SubHeader class="mb-3">[<span class="text-green-600 font-bold">Easy</span>] 次のデータを正規化せよ。</SubHeader>
            <Hint unique_id="easy">グレイスケール(8bit)ですね。</Hint>

            <PyScript>
                data = [
                            [133, 254,   0, 233,  45],
                            [230,  43, 198, 172,  85],
                            [ 75, 189,  34, 210, 222],
                            [ 43, 240,  65, 184, 202],
                            [139,  14,  78, 255, 164]
                        ]
                # 続きを書いてね

                print(normalized_data)
            </PyScript>
        </Paragraph>

        <Paragraph>
            <SubHeader class="mb-3">[<span class="text-yellow-400 font-bold">Medium</span>] 次のデータを昇順(ascending order)に並べ替えよ。ただし、バブルソートを使うこと。</SubHeader>
            <Hint unique_id="med1">各データが泡のように浮かび上がってきます。</Hint>

            <PyScript>
                data = [5, 9, 3, 6, 2, 1, 3, 7, 8, 0]
                # 続きを書いてね

                print(sorted_data)
            </PyScript>
        </Paragraph>

        <Paragraph>
            <SubHeader class="mb-3">[<span class="text-yellow-400 font-bold">Medium</span>] 行列Aと行列Bの積を求めよ。</SubHeader>
            <Hint unique_id="med2">定義通りに計算しよう。(※手計算しないでね)</Hint>

            <PyScript>
                A = [
                        [1, 2, 3],
                        [4, 5, 6],
                        [7, 8, 9]
                    ]
                B = [
                        [-1,  0,  1],
                        [ 0,  1, -1],
                        [ 1, -1, -1]
                    ]
                # 続きを書いてね

                print(C)
            </PyScript>
        </Paragraph>

        <Paragraph>
            <SubHeader class="mb-3">[<span class="text-yellow-400 font-bold">Medium</span>] データAとデータBの相関係数を求めよ。</SubHeader>
            <Hint unique_id="med3">平方根は、<span class="text-yellow-400">math.sqrt()</span>関数で計算できる。</Hint>

            <PyScript>
                import math

                data_A = [ 0,  3,  5,  9, -2,  1,  2, -4,  7,  8]
                data_B = [ 3, 12,  6,  5,  4, -2,  3, -1,  9,  7]
                # 続きを書いてね

                print(cor_AB)
            </PyScript>
        </Paragraph>

        <ProgressFooter :progress="completion" class="mt-16" />
        <div class="text-center my-5">
            <a href="#" class="p-2 rounded-md bg-green-700 hover:bg-green-600">ページ上部へ!</a>
        </div>
    </div>
</template>

<style scoped>
</style>
### HAVING句の力

<div style="padding: 10px; margin-bottom: 10px; border: 5px double;">
    <h5>学習のポイント</h5>
    <ul>
        <li>テーブルはファイルではなく、行や順序も持たない。そのためSQLでは原則ソートを記述しない。</li>
        <li>GROUP BY句は過不足ない部分集合を作る。</li>
        <li>WHERE句は集合の要素の性質を調べる道具、HAVING句は集合自身の性質を調べる道具。</li>
        <li>SQLで検索条件を設定するときは検索対象を見極めることが肝要</li>
        <ul>
            <li>実体1つにつき<font color=red>1行</font>が対応している→要素なので<font color=red>WHERE句</font>を使う。</li>
            <li>実体1つにつき<font color=red>複数行</font>が対応している→集合なので<font color=red>HAVING句</font>を使う。</li>
        </ul>
    </ul>
</div>

<table>
    <caption>集合の性質を調べるための条件の使い方一覧</caption>
	<tbody>
		<tr>
			<th>条件式</th>
			<th>用途</th>
		</tr>
		<tr>
			<td>COUNT(DISTINCT col) = COUNT(col)</td>
			<td>colの値が一意である</td>
		</tr>
		<tr>
			<td>COUNT(*) = COUNT(col)</td>
			<td>colにNULLが存在しない</td>
		</tr>
		<tr>
			<td>COUNT(*) = MAX(col)</td>
			<td>colは歯抜けのない連番(開始値は1)</td>
		</tr>
		<tr>
			<td>COUNT(*) = MAX(col) - MIN(col) + 1</td>
			<td>colは歯抜けのない連番(開始値は任意の整数)<br>※3の一般化</td>
		</tr>
		<tr>
			<td>MIN(col) = MAX(col)</td>
			<td>colが一つだけの値を持つか、またはNULLである</td>
		</tr>
		<tr>
			<td>MIN(col) * MAX(col) > 0</td>
			<td>全てのcolの符号が同じである</td>
		</tr>
		<tr>
			<td>MIN(col) * MAX(col) < 0</td>
			<td>最大値の符号が正で最小値の符号が負</td>
		</tr>
		<tr>
			<td>MIN(ABS(col)) = 0</td>
			<td>colは少なくとも1つの0を含む</td>
		</tr>
		<tr>
			<td>MIN(col - 定数) = -MAX(col - 定数)</td>
			<td>colの最大値と最小値が<br>指定した定数から同じ幅の距離にある。</td>
		</tr>
	</tbody>
</table>

<div style="page-break-before:always"></div>

#### データの歯抜けを探す

<table>
    <caption>SeqTbl</caption>
    <thead>
    <tr>
        <th><u>seq</th>
        <th>name</th>
    </tr>
    </thead>
    <tbody>
        <tr><td>1</td><td>ディック</td></tr>
        <tr><td>2</td><td>アン</td></tr>
        <tr><td>3</td><td>ライル</td></tr>
        <tr><td>5</td><td>カー</td></tr>
        <tr><td>6</td><td>マリー</td></tr>
        <tr><td>8</td><td>ベン</td></tr>
    </tbody>
</table>

上記テーブルを用いて、欠番の有無を調べるクエリと欠番の最小値を返すクエリを考える。

```sql
-- 歯抜けの有無を返すクエリ
SELECT '歯抜けあり' AS gap FROM seqtbl
HAVING COUNT(*) <> MAX(seq);

-- 欠番を返すクエリ
SELECT MIN(seq + 1) FROM seqtbl
WHERE seq + 1 NOT IN(SELECT seq FROM seqtbl);
```

上記クエリは「開始値が1」を前提とており、例えば「3, 4, 5, 6, 7」といった連番の場合でも「歯抜けあり」「欠番値8」という結果が得られてしまう。そこで、改善後のクエリを以下に示す。

```sql
-- 【改善版】歯抜けの有無を返すクエリ
SELECT '歯抜けあり' AS gap FROM seqtbl
HAVING COUNT(*) <> MAX(seq) - MIN(seq) + 1; -- 上限値 - 下限値 + 1

-- 【改善版】欠番を返すクエリ
SELECT CASE 
	WHEN COUNT(*) = 0 OR MIN(seq) > 1 THEN 1 -- 下限が1の場合は1を返す
	ELSE ( -- 下限が1の場合は最小の欠番を返す
		SELECT MIN(seq + 1) FROM seqtbl S1
		WHERE NOT EXISTS(SELECT * FROM seqtbl S2 WHERE S1.seq + 1 = S2.seq)
	)
END FROM seqtbl;
```

<div style="page-break-before:always"></div>

#### HAVING句でサブクエリ〜最頻値を求める〜

<table>
    <caption>Graduates(卒業生テーブル)</caption>
    <thead>
        <tr>
            <th><u>name</th>
            <th>income</th>
        </tr>
    </thead>
    <tbody>
        <tr><td>サンプソン</td><td>400000</td></tr>
        <tr><td>マイク</td><td>30000</td></tr>
        <tr><td>ホワイト</td><td>20000</td></tr>
        <tr><td>アーノルド</td><td>20000</td></tr>
        <tr><td>スミス</td><td>20000</td></tr>
        <tr><td>ロレンス</td><td>15000</td></tr>
        <tr><td>ハドソン</td><td>15000</td></tr>
        <tr><td>ケント</td><td>10000</td></tr>
        <tr><td>ベッカー</td><td>10000</td></tr>
        <tr><td>スコット</td><td>10000</td></tr>
    </tbody>
</table>


<div style="page-break-before:always"></div>

#### NULLを含まない集合を探す
<table>
	<tbody>
		<tr>
			<td>
                <table>
                    <caption>NullTbl</caption>
                    <thead>
                        <tr>
                            <th>col_1</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>NULL</td></tr>
                        <tr><td>NULL</td></tr>
                        <tr><td>NULL</td></tr>
                    </tbody>
                </table>
            </td>
			<td>
                <table>
                    <caption>Students</caption>
                    <thead>
                        <tr>
                            <th>学生ID</th>
                            <th>学部</th>
                            <th>入学日</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>100</td><td>理学部</td><td>2018-10-10</td></tr>
                        <tr><td>101</td><td>理学部</td><td>2018-09-22</td></tr>
                        <tr><td>102</td><td>文学部</td><td></td></tr>
                        <tr><td>103</td><td>文学部</td><td>2018-09-10</td></tr>
                        <tr><td>200</td><td>文学部</td><td>2018-09-22</td></tr>
                        <tr><td>201</td><td>工学部</td><td></td></tr>
                        <tr><td>202</td><td>経済学部</td><td>2018-09-25</td></tr>
                    </tbody>
                </table>
            </td>
		</tr>
	</tbody>
</table>

<div style="page-break-before:always"></div>

#### HAVING句で全称量化

<table>
	<tbody>
		<tr>
			<td>
                <table>
                    <caption>Items</caption>
                    <thead>
                        <tr>
                            <th>商品名</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>ビール</td></tr>
                        <tr><td>紙オムツ</td></tr>
                        <tr><td>自転車</td></tr>
                    </tbody>
                </table>
            </td>
			<td>
                <table>
                    <caption>ShopItems</caption>
                    <thead>
                        <tr>
                            <th>地域</th>
                            <th>商品名</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>仙台</td><td>ビール</td></tr>
                        <tr><td>仙台</td><td>紙オムツ</td></tr>
                        <tr><td>仙台</td><td>自転車</td></tr>
                        <tr><td>仙台</td><td>カーテン</td></tr>
                        <tr><td>東京</td><td>ビール</td></tr>
                        <tr><td>東京</td><td>紙オムツ</td></tr>
                        <tr><td>東京</td><td>自転車</td></tr>
                        <tr><td>大阪</td><td>テレビ</td></tr>
                        <tr><td>大阪</td><td>紙オムツ</td></tr>
                        <tr><td>大阪</td><td>自転車</td></tr>
                    </tbody>
                </table>
            </td>
		</tr>
	</tbody>
</table>



<div style="page-break-before:always"></div>

#### 一意集合と多重集合



<div style="page-break-before:always"></div>

#### 関係除算でバスケット解析
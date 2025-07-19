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



<div style="page-break-before:always"></div>

#### HAVING句でサブクエリ〜最頻値を求める〜



<div style="page-break-before:always"></div>

#### NULLを含まない集合を探す



<div style="page-break-before:always"></div>

#### HAVING句で全称量化



<div style="page-break-before:always"></div>

#### 一意集合と多重集合



<div style="page-break-before:always"></div>

#### 関係除算でバスケット解析
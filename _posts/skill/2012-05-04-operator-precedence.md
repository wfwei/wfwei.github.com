---
layout: post
title: "operator precedence"
category: skill
---

<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="68">
<p align="center">优先级</p>
</td>
<td width="64">
<p align="center">运算符</p>
</td>
<td width="126">
<p align="center">名称或含义</p>
</td>
<td width="144">
<p align="center">使用形式</p>
</td>
<td width="72">
<p align="center">结合方向</p>
</td>
<td width="86">
<p align="center">说明</p>
</td>
</tr>
<tr>
<td rowspan="4" width="68">
<p align="center">1</p>
</td>
<td width="64">
<p align="center">[]</p>
</td>
<td width="126">
<p align="center">数组下标</p>
</td>
<td width="144">
<p align="center">数组名[常量表达式]</p>
</td>
<td rowspan="4" width="72">
<p align="center">左到右</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">()</p>
</td>
<td width="126">
<p align="center">圆括号</p>
</td>
<td width="144">
<p align="center">（表达式）/函数名(形参表)</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">.</p>
</td>
<td width="126">
<p align="center">成员选择（对象）</p>
</td>
<td width="144">
<p align="center">对象.成员名</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">-&gt;</p>
</td>
<td width="126">
<p align="center">成员选择（指针）</p>
</td>
<td width="144">
<p align="center">对象指针-&gt;成员名</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td rowspan="9" width="68">
<p align="center">2</p>
</td>
<td width="64">
<p align="center">-</p>
</td>
<td width="126">
<p align="center">负号运算符</p>
</td>
<td width="144">
<p align="center">-表达式</p>
</td>
<td rowspan="9" width="72">
<p align="center">右到左</p>
</td>
<td width="86">
<p align="center">单目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">(类型)</p>
</td>
<td width="126">
<p align="center">强制类型转换</p>
</td>
<td width="144">
<p align="center">(数据类型)表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">++</p>
</td>
<td width="126">
<p align="center">自增运算符</p>
</td>
<td width="144">
<p align="center">++变量名/变量名++</p>
</td>
<td width="86">
<p align="center">单目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">--</p>
</td>
<td width="126">
<p align="center">自减运算符</p>
</td>
<td width="144">
<p align="center">--变量名/变量名--</p>
</td>
<td width="86">
<p align="center">单目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">*</p>
</td>
<td width="126">
<p align="center">取值运算符</p>
</td>
<td width="144">
<p align="center">*指针变量</p>
</td>
<td width="86">
<p align="center">单目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">&amp;</p>
</td>
<td width="126">
<p align="center">取地址运算符</p>
</td>
<td width="144">
<p align="center">&amp;变量名</p>
</td>
<td width="86">
<p align="center">单目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">!</p>
</td>
<td width="126">
<p align="center">逻辑非运算符</p>
</td>
<td width="144">
<p align="center">!表达式</p>
</td>
<td width="86">
<p align="center">单目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">~</p>
</td>
<td width="126">
<p align="center">按位取反运算符</p>
</td>
<td width="144">
<p align="center">~表达式</p>
</td>
<td width="86">
<p align="center">单目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">sizeof</p>
</td>
<td width="126">
<p align="center">长度运算符</p>
</td>
<td width="144">
<p align="center">sizeof(表达式)</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td rowspan="3" width="68">
<p align="center">3</p>
</td>
<td width="64">
<p align="center">/</p>
</td>
<td width="126">
<p align="center">除</p>
</td>
<td width="144">
<p align="center">表达式/表达式</p>
</td>
<td rowspan="3" width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">*</p>
</td>
<td width="126">
<p align="center">乘</p>
</td>
<td width="144">
<p align="center">表达式*表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">%</p>
</td>
<td width="126">
<p align="center">余数（取模）</p>
</td>
<td width="144">
<p align="center">整型表达式/整型表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td rowspan="2" width="68">
<p align="center">4</p>
</td>
<td width="64">
<p align="center">+</p>
</td>
<td width="126">
<p align="center">加</p>
</td>
<td width="144">
<p align="center">表达式+表达式</p>
</td>
<td rowspan="2" width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">-</p>
</td>
<td width="126">
<p align="center">减</p>
</td>
<td width="144">
<p align="center">表达式-表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td rowspan="2" width="68">
<p align="center">5</p>
</td>
<td width="64">
<p align="center">&lt;&lt;</p>
</td>
<td width="126">
<p align="center">左移</p>
</td>
<td width="144">
<p align="center">变量&lt;&lt;表达式</p>
</td>
<td rowspan="2" width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">&gt;&gt;</p>
</td>
<td width="126">
<p align="center">右移</p>
</td>
<td width="144">
<p align="center">变量&gt;&gt;表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td rowspan="4" width="68">
<p align="center">6</p>
</td>
<td width="64">
<p align="center">&gt;</p>
</td>
<td width="126">
<p align="center">大于</p>
</td>
<td width="144">
<p align="center">表达式&gt;表达式</p>
</td>
<td rowspan="4" width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">&gt;=</p>
</td>
<td width="126">
<p align="center">大于等于</p>
</td>
<td width="144">
<p align="center">表达式&gt;=表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">&lt;</p>
</td>
<td width="126">
<p align="center">小于</p>
</td>
<td width="144">
<p align="center">表达式&lt;表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">&lt;=</p>
</td>
<td width="126">
<p align="center">小于等于</p>
</td>
<td width="144">
<p align="center">表达式&lt;=表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td rowspan="2" width="68">
<p align="center">7</p>
</td>
<td width="64">
<p align="center">==</p>
</td>
<td width="126">
<p align="center">等于</p>
</td>
<td width="144">
<p align="center">表达式==表达式</p>
</td>
<td rowspan="2" width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="64">
<p align="center">!=</p>
</td>
<td width="126">
<p align="center">不等于</p>
</td>
<td width="144">
<p align="center">表达式!= 表达式</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="68">
<p align="center">8</p>
</td>
<td width="64">
<p align="center">&amp;</p>
</td>
<td width="126">
<p align="center">按位与</p>
</td>
<td width="144">
<p align="center">表达式&amp;表达式</p>
</td>
<td width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="68">
<p align="center">9</p>
</td>
<td width="64">
<p align="center">^</p>
</td>
<td width="126">
<p align="center">按位异或</p>
</td>
<td width="144">
<p align="center">表达式^表达式</p>
</td>
<td width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="68">
<p align="center">10</p>
</td>
<td width="64">
<p align="center">|</p>
</td>
<td width="126">
<p align="center">按位或</p>
</td>
<td width="144">
<p align="center">表达式|表达式</p>
</td>
<td width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="68">
<p align="center">11</p>
</td>
<td width="64">
<p align="center">&amp;&amp;</p>
</td>
<td width="126">
<p align="center">逻辑与</p>
</td>
<td width="144">
<p align="center">表达式&amp;&amp;表达式</p>
</td>
<td width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="68">
<p align="center">12</p>
</td>
<td width="64">
<p align="center">||</p>
</td>
<td width="126">
<p align="center">逻辑或</p>
</td>
<td width="144">
<p align="center">表达式||表达式</p>
</td>
<td width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">双目运算符</p>
</td>
</tr>
<tr>
<td width="68">
<p align="center">13</p>
</td>
<td width="64">
<p align="center">?:</p>
</td>
<td width="126">
<p align="center">条件运算符</p>
</td>
<td width="144">
<p align="center">表达式1? 表达式2: 表达式3</p>
</td>
<td width="72">
<p align="center">右到左</p>
</td>
<td width="86">
<p align="center">三目运算符</p>
</td>
</tr>
<tr>
<td rowspan="11" width="68">
<p align="center">14</p>
</td>
<td width="64">
<p align="center">=</p>
</td>
<td width="126">
<p align="center">赋值运算符</p>
</td>
<td width="144">
<p align="center">变量=表达式</p>
</td>
<td rowspan="11" width="72">
<p align="center">右到左</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">/=</p>
</td>
<td width="126">
<p align="center">除后赋值</p>
</td>
<td width="144">
<p align="center">变量/=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">*=</p>
</td>
<td width="126">
<p align="center">乘后赋值</p>
</td>
<td width="144">
<p align="center">变量*=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">%=</p>
</td>
<td width="126">
<p align="center">取模后赋值</p>
</td>
<td width="144">
<p align="center">变量%=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">+=</p>
</td>
<td width="126">
<p align="center">加后赋值</p>
</td>
<td width="144">
<p align="center">变量+=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">-=</p>
</td>
<td width="126">
<p align="center">减后赋值</p>
</td>
<td width="144">
<p align="center">变量-=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">&lt;&lt;=</p>
</td>
<td width="126">
<p align="center">左移后赋值</p>
</td>
<td width="144">
<p align="center">变量&lt;&lt;=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">&gt;&gt;=</p>
</td>
<td width="126">
<p align="center">右移后赋值</p>
</td>
<td width="144">
<p align="center">变量&gt;&gt;=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">&amp;=</p>
</td>
<td width="126">
<p align="center">按位与后赋值</p>
</td>
<td width="144">
<p align="center">变量&amp;=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">^=</p>
</td>
<td width="126">
<p align="center">按位异或后赋值</p>
</td>
<td width="144">
<p align="center">变量^=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="64">
<p align="center">|=</p>
</td>
<td width="126">
<p align="center">按位或后赋值</p>
</td>
<td width="144">
<p align="center">变量|=表达式</p>
</td>
<td width="86"></td>
</tr>
<tr>
<td width="68">
<p align="center">15</p>
</td>
<td width="64">
<p align="center">,</p>
</td>
<td width="126">
<p align="center">逗号运算符</p>
</td>
<td width="144">
<p align="center">表达式,表达式,…</p>
</td>
<td width="72">
<p align="center">左到右</p>
</td>
<td width="86">
<p align="center">从左向右顺序运算</p>
</td>
</tr>
</tbody>
</table>

#  运算符优先级表
<table>
    <tr>
        <th> 优先级 </th>
        <th> 结合律 </th>
        <th> 运算符 </th>
        <th> 功能 </th>
        <th> 用法 </th>
    </tr>
    <tr >
        <td rowspan="3">1</td>
        <td>左 </td>
        <td> <code> :: </code> </td>
        <td> 全局作用域 </td>
        <td> <code> ::name </code> </td>
    </tr>
    <tr>
        <td>左 </td>
        <td> <code> :: </code> </td>
        <td>类作用域 </td>
        <td> <code> class::name </code> </td>
    </tr>
    <tr>
        <td>左 </td>
        <td> <code> :: </code> </td>
        <td>命名空间作用域 </td>
        <td> <code> namespace::name </code> </td>
    </tr>
    <tr >
        <td rowspan="5">2</td>
        <td>左 </td>
        <td> <code> . </code> </td>
        <td>成员选择</td>
        <td> <code> object.member </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code> -> </code> </td>
        <td>成员选择</td>
        <td> <code> pointer->member </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code> [] </code> </td>
        <td> 下标 </td>
        <td> <code> expr[expr] </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>() </code> </td>
        <td> 函数调用 </td>
        <td> <code> name(expr_list) </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>() </code> </td>
        <td> 类型构造 </td>
        <td> <code> type(expr_list) </code> </td>
    </tr>
    <tr >
        <td rowspan="5">3</td>
        <td>右 </td>
        <td> <code> ++ </code> </td>
        <td>后置递增运算符</td>
        <td> <code> lvlaue++ </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code> -- </code> </td>
        <td>后置递减运算符</td>
        <td> <code> lvalue-- </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code> typeid </code> </td>
        <td> 类型ID </td>
        <td> <code> typeid[expr] </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>typeid </code> </td>
        <td> 运行时类型ID </td>
        <td> <code> typeid(expr_list) </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>explicit cast </code> </td>
        <td> 类型转换 </td>
        <td> <code> cast_name<type>(expr) </code> </td>
    </tr>
    <tr >
        <td rowspan="17">4</td>
        <td>右 </td>
        <td> <code>++ </code> </td>
        <td> 前置递增运算符</td>
        <td> <code> ++lvalue </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>-- </code> </td>
        <td> 前置递减运算符 </td>
        <td> <code> --lvalue </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>~ </code> </td>
        <td> 位求反 </td>
        <td> <code> ~expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>! </code> </td>
        <td> 逻辑非 </td>
        <td> <code> !expr</code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>- </code> </td>
        <td> 一元负号 </td>
        <td> <code> -expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>+ </code> </td>
        <td> 一元正号 </td>
        <td> <code> +expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>* </code> </td>
        <td> 解引用 </td>
        <td> <code> *expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>& </code> </td>
        <td> 取地址 </td>
        <td> <code> &lvalue </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>() </code> </td>
        <td> 类型转换 </td>
        <td> <code> (type)expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>sizeof </code> </td>
        <td> 对象的大小</td>
        <td> <code> sizeof expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>sizeof </code> </td>
        <td> 类型大小 </td>
        <td> <code> sizeof(type) </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>Sizeof... </code> </td>
        <td> 参数包的大小 </td>
        <td> <code> sizeof...(name) </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>new</code> </td>
        <td> 创建对象 </td>
        <td> <code> new type </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>new [] </code> </td>
        <td> 创建数组 </td>
        <td> <code> new type[size] </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>delete </code> </td>
        <td> 释放对象 </td>
        <td> <code> delete expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>delete [] </code> </td>
        <td> 释放数组 </td>
        <td> <code> delete[] expr </code> </td>
    </tr>
    <tr >
        <td>右 </td>
        <td> <code>noexcept</code> </td>
        <td> 能否抛出异常 </td>
        <td> <code> noexcept(expr) </code> </td>
    </tr>
    <tr >
        <td rowspan="2">5</td>
        <td>左 </td>
        <td> <code>->* </code> </td>
        <td> 指向成员选择的指针 </td>
        <td> <code> ptr->*ptr_to_member </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>.*</code> </td>
        <td> 指向成员选择的指针 </td>
        <td> <code> obj.*ptr_to_member </code> </td>
    </tr>
    <tr >
        <td rowspan="3">6</td>
        <td>左 </td>
        <td> <code> * </code> </td>
        <td> 乘法 </td>
        <td> <code> expr * expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>/</code> </td>
        <td> 除法 </td>
        <td> <code> expr / expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>%</code> </td>
        <td> 取模（取余） </td>
        <td> <code> expr % expr </code> </td>
    </tr>
    <tr >
        <td rowspan="2">7</td>
        <td>左 </td>
        <td> <code> + </code> </td>
        <td> 加法</td>
        <td> <code>expr + expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>-</code> </td>
        <td> 减法 </td>
        <td> <code> expr - expr </code> </td>
    </tr>
    <tr >
        <td rowspan="2">8</td>
        <td>左 </td>
        <td> <code> << </code> </td>
        <td> 向左移位 </td>
        <td> <code> expr << expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>>></code> </td>
        <td> 向右移位 </td>
        <td> <code> expr >> expr </code> </td>
    </tr>
    <tr >
        <td rowspan="4">9</td>
        <td>左 </td>
        <td> <code> < </code> </td>
        <td> 小于 </td>
        <td> <code> expr < expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code><=</code> </td>
        <td> 小于等于 </td>
        <td> <code> expr <= expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>></code> </td>
        <td> 大于 </td>
        <td> <code> expr > expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>>=</code> </td>
        <td> 大于等于 </td>
        <td> <code> expr >= expr </code> </td>
    </tr>
    <tr >
        <td rowspan="2">10</td>
        <td>左 </td>
        <td> <code> == </code> </td>
        <td> 相等 </td>
        <td> <code> expr == expr </code> </td>
    </tr>
    <tr >
        <td>左 </td>
        <td> <code>!=</code> </td>
        <td> 不相等 </td>
        <td> <code> expr != expr </code> </td>
    </tr>
    <tr >
        <td>11</td>
        <td>左 </td>
        <td> <code> & </code> </td>
        <td> 位与 </td>
        <td> <code> expr & expr </code> </td>
    </tr>
    <tr >
        <td>12</td>
        <td>左 </td>
        <td> <code> ^ </code> </td>
        <td> 位异或 </td>
        <td> <code> expr ^ expr </code> </td>
    </tr>
    <tr >
        <td>13</td>
        <td>左 </td>
        <td> <code> | </code> </td>
        <td> 位或 </td>
        <td> <code> expr | expr </code> </td>
    </tr>
    <tr >
        <td>14</td>
        <td>左 </td>
        <td> <code> && </code> </td>
        <td> 逻辑与 </td>
        <td> <code> expr && expr </code> </td>
    </tr>
    <tr >
        <td>15</td>
        <td>左 </td>
        <td> <code> || </code> </td>
        <td> 逻辑或 </td>
        <td> <code> expr || expr </code> </td>
    </tr>
    <tr >
        <td>16</td>
        <td>右 </td>
        <td> <code> ? : </code> </td>
        <td> 条件 </td>
        <td> <code> expr ? expr : expr</code> </td>
    </tr>
    <tr >
        <td>17</td>
        <td>右 </td>
        <td> <code> = </code> </td>
        <td> 赋值 </td>
        <td> <code> lvalue = expr </code> </td>
    </tr>
    <tr >
        <td rowspan="4">18</td>
        <td>右 </td>
        <td> <code> *=, /=, %= </code> </td>
        <td rowspan="4"> 复合赋值 </td>
        <td rowspan="4"> <code> lvalue += expr 等</code> </td>
    </tr>
    <tr>
        <td>右</td>
        <td> <code> +=, -= </code> </td>
    </tr>
    <tr>
        <td>右</td>
        <td> <code> <<=, >>= </code> </td>
    </tr>
    <tr>
        <td>右</td>
        <td> <code> &=, |=, ^=</code> </td>
    </tr>
    <tr >
        <td>19</td>
        <td>右 </td>
        <td> <code> throw </code> </td>
        <td> 抛出异常 </td>
        <td> <code> throw expr</code> </td>
    </tr>
    <tr >
        <td>20</td>
        <td>左 </td>
        <td> <code> , </code> </td>
        <td> 逗号 </td>
        <td> <code> expr , expr </code> </td>
    </tr>
</table>
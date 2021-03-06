# 2-1 Entry

entry是配置模块的入口，可抽象成输入，Webpack 执行构建的第一步将从入口开始搜寻及递归解析出所有入口依赖的模块。

entry 配置是必填的，若不填则将导致 Webpack 报错退出。

context
Webpack 在寻找相对路径的文件时会以 context 为根目录，context 默认为执行启动 Webpack 时所在的当前工作目录。 如果想改变 context 的默认配置，则可以在配置文件里这样设置它：

```js
module.exports = {
  context: path.resolve(__dirname, 'app')
}
```

注意， context 必须是一个绝对路径的字符串。 除此之外，还可以通过在启动 Webpack 时带上参数 webpack --context 来设置 context。

之所以在这里先介绍 context，是因为 Entry 的路径和其依赖的模块的路径可能采用相对于 context 的路径来描述，context 会影响到这些相对路径所指向的真实文件。

Entry 类型
Entry 类型可以是以下三种中的一种或者相互组合：

<table>
<thead>
<tr>
<th>类型</th>
<th>例子</th>
<th>含义</th>
</tr>
</thead>
<tbody>
<tr>
<td>string</td>
<td><code>'./app/entry'</code></td>
<td>入口模块的文件路径，可以是相对路径。</td>
</tr>
<tr>
<td>array</td>
<td><code>['./app/entry1', './app/entry2']</code></td>
<td>入口模块的文件路径，可以是相对路径。</td>
</tr>
<tr>
<td>object</td>
<td><code>{ a: './app/entry-a', b: ['./app/entry-b1', './app/entry-b2']}</code></td>
<td>配置多个入口，每个入口生成一个 Chunk</td>
</tr>
</tbody>
</table>

Chunk 名称
Webpack 会为每个生成的 Chunk 取一个名称，Chunk 的名称和 Entry 的配置有关：

如果 entry 是一个 string 或 array，就只会生成一个 Chunk，这时 Chunk 的名称是 main；
如果 entry 是一个 object，就可能会出现多个 Chunk，这时 Chunk 的名称是 object 键值对里键的名称。

配置动态 Entry

假如项目里有多个页面需要为每个页面的入口配置一个 Entry ，但这些页面的数量可能会不断增长，则这时 Entry 的配置会受到到其他因素的影响导致不能写成静态的值。其解决方法是把 Entry 设置成一个函数去动态返回上面所说的配置，代码如下：

```js
// 同步函数
entry: () => {
  return {
    a:'./pages/a',
    b:'./pages/b',
  }
};
// 异步函数
entry: () => {
  return new Promise((resolve)=>{
    resolve({
       a:'./pages/a',
       b:'./pages/b',
    });
  });
};
```
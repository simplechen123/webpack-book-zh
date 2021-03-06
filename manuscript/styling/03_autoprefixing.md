# Autoprefixing

# 自动前缀

It can be difficult to remember which vendor prefixes you have to use for specific CSS rules to support a large variety of users. **Autoprefixing** solves this problem. It can be enabled through PostCSS and the [autoprefixer](https://www.npmjs.com/package/autoprefixer) plugin. *autoprefixer* uses [Can I Use](http://caniuse.com/) service to figure out which rules should be prefixed and its behavior can be tuned further.

您可能很难记住，必须为特定的CSS规则使用哪些供应商前缀，来支持各种各样的用户。 **Autoprefixing**解决了这个问题。 它可以通过PostCSS和[autoprefixer](https://www.npmjs.com/package/autoprefixer)插件启用。 *autoprefixer*使用[Can I Use](http://caniuse.com/)服务来确定应该添加哪些规则，并且可以进一步调整它的行为。

## Setting Up Autoprefixing

## 设置自动前缀

Achieving autoprefixing takes a small addition to the current setup. Install *postcss-loader* and *autoprefixer* first:

实现自动前缀只需要对当前设置稍加修改。首先安装*postcss-loader*和*autoprefixer*：

```bash
npm install postcss-loader autoprefixer --save-dev
```

Add a fragment enabling autoprefixing like this:

添加一个启用自动前缀功能的片段，如下所示：

**webpack.parts.js**

```javascript
...

exports.autoprefix = function() {
  return {
    loader: 'postcss-loader',
    options: {
      plugins: () => ([
        require('autoprefixer'),
      ]),
    },
  };
};
```

To connect the loader with `ExtractTextPlugin`, hook it up as follows:

要使用`ExtractTextPlugin`连接加载器，请按如下方式将其关联起来：

**webpack.config.js**

```javascript
...

const productionConfig = merge([
  ...
leanpub-start-delete
  parts.extractCSS({ use: 'css-loader' }),
leanpub-end-delete
leanpub-start-insert
  parts.extractCSS({
    use: ['css-loader', parts.autoprefix()],
  }),
leanpub-end-insert
]);

...
```

To confirm that the setup works, we should have something to autoprefix. Adjust the CSS like this:

要确认安装是否正常，我们应该需要一些CSS规则来使其自动补全前缀。 像这样调整CSS：

**app/main.css**

```css
body {
  background: cornsilk;
leanpub-start-insert
  display: flex;
leanpub-end-insert
}
```

If you build the application now (`npm run build`) and examine the built CSS, you should be able to find a declaration like this there:

如果你现在构建应用程序，执行`npm run build`并检查构建的CSS，你应该能够找到下面这样的CSS规则：

```css
body {
  background: cornsilk;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
}
```

As you can see, autoprefixing expands the rules, so we don’t have to remember to do that.

正如你所看到的，自动前缀扩展了规则，所以我们不必记住要编写哪些前缀。

If you know what browsers you support, it is possible to set up a [browserslist](https://www.npmjs.com/package/browserslist) file. Different tools pick up this definition, *autoprefixer* included. Consider the example below where we select only specific browsers:

如果您明确需要支持哪些浏览器，则可以设置一个[browserslist](https://www.npmjs.com/package/browserslist)文件。 不同的工具都可以识别这个定义，包括*autoprefixer*。 考虑下面的示例，我们只选择特定的浏览器：

**browserslist**

```
> 1% # Browser usage over 1%
Last 2 versions # Last two versions too
IE 8 # And IE 8
```

W> It is important to note that *browserslist* rules are evaluated as **or**. If any of the rules matches, then the given browser will be supported by the definition!

W> 重要的是注意*browserslist*规则被评估为**或**。 如果任何规则匹配，则给定的浏览器将被支持！

## Conclusion

## 小结

Autoprefixing is a convenient technique as it decreases the amount of work needed while crafting CSS. You can maintain minimum browser requirements within a *browserslist* file. The tooling can then use that information to generate optimal output.

自动前缀是一种方便的技术，因为它减少了编写CSS所需的工作量。 您可以在*browserslist*文件中维护最低的浏览器要求。 然后，工具可以使用该信息来产生最佳输出。

To recap:
回顾：

* Autoprefixing can be enabled through the *autoprefixer* PostCSS plugin.
* 自动前缀可以通过PostCSS的插件*autoprefixer*来启用。

* Autoprefixing will write missing CSS definitions based on your minimum browser definition.
* 自动前缀插件将会根据最小的浏览器支持定义编写缺少的CSS定义。

* *browserslist* is a standard file that works with tooling beyond *autoprefixer*
* *browserslist*是一个标准文件，除了*autoprefixer*之外，还可以同其它工具一起使用。

In the next chapter, we will learn to eliminate unused CSS from the project.

下一章节，我们将学习如何从我们的项目中去掉无用的CSS。

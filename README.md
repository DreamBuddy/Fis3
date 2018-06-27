# Fis3 平时积累

## 使用PostCSS + postcss-preset-env + Async
主要是利用'promise-synchronizer'来实现Promise异步返回变为同步输出
```javascript
postprocessor: function (content, file, conf) {
    var sync = require('promise-synchronizer')

    var postcss = require('postcss');

    var addPlugins = function () {
      var plugins = [];

      // https://github.com/csstools/postcss-preset-env
      var env = require('postcss-preset-env');
      plugins.push(env({
        // https://github.com/ai/browserslist#queries
        browsers: browsers,
        stage: 0
      }));
      return plugins;
    }
    if (file.isCssLike && typeof addPlugins == 'function') {
      var plugins = addPlugins();

      if (Array.isArray(plugins) && plugins.length) {
        try {
          content = sync(postcss(plugins).process(content, {
            from: file.realpath
          }).then(function (data) {
            return data.css;
          }))
        } catch (err) {
          log.warn('%s might not processed due to:\n %s', file.id, err)
          process.exit(1)
        }

        return content;
      }
    }
    return content;
  }
  ```

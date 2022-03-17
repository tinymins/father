# father-build-universal

## Extends

See our [main repo](https://github.com/umijs/father) for more information.

## New Feature Options

### hookRollupPluginOptions

**可用于对 rollup 插件设置项进行自定义修改，实现特殊需求。**

在构建中，需要在插件 `replace` 设置项中增加一些自定义替换内容，如该场景对应配置为：

```js
export default {
  hookRollupPluginOptions: (pluginName, pluginOptions, environment) => {
    if (pluginName === 'replace') {
      pluginOptions = [{
        ...pluginOptions[0] || {},
        'process.env.BUILD_UNIX_TIME': Date.now(),
      }, ...pluginOptions.slice(1)];
    }
    return pluginOptions;
  },
}
```

### hookRollupConfig

**可用于对生成的 rollup 设置项进行自定义修改，实现特殊需求。**

在构建中，需要在插件设置项 `babel` 前方插入 `eslint` 插件才能保证其正常工作，`hookRollupConfig` 允许你自定义修改最终的 `rollup` 设置项，如该场景对应配置为：

```js
import eslint from '@rollup/plugin-eslint';

export default {
  hookRollupConfig: (rollupOptions, environment) => {
    return rollupOptions.map((rollupOption) => ({
      ...rollupOption,
      plugins: {
        eslint(),
        ...rollupOption.plugin,
      },
    }));
  },
}
```

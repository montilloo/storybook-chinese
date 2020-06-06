# 选项参数

Storybook UI可以使用选项API进行配置，该API允许您针对每个story全局地调整其外观。

## 全局options

{% embed url="https://在manager.js文件中导入并使用setConfig。" %}

```text
import { addons } from '@storybook/addons';

addons.setConfig({
  /**
   * show story component as full screen
   * @type {Boolean}
   */
  isFullscreen: false,

  /**
   * display panel that shows a list of stories
   * @type {Boolean}
   */
  showNav: true,

  /**
   * display panel that shows addon configurations
   * @type {Boolean}
   */
  showPanel: true,

  /**
   * where to show the addon panel
   * @type {('bottom'|'right')}
   */
  panelPosition: 'bottom',

  /**
   * sidebar tree animations
   * @type {Boolean}
   */
  sidebarAnimations: true,

  /**
   * enable/disable shortcuts
   * @type {Boolean}
   */
  enableShortcuts: true,

  /**
   * show/hide tool bar
   * @type {Boolean}
   */
  isToolshown: true,

  /**
   * theme storybook, see link below
   */
  theme: undefined,

  /**   
   * id to select an addon panel    
   * @type {String} 
   */   
  selectedPanel: undefined,
});
```

#### showRoots <a id="showroots"></a>

在`preview.js`文件中使用`options`导入并使用`addParameters`。

```text
import { addParameters } from '@storybook/react';

addParameters({
  options: {
    /**
     * display the top-level grouping as a "root" in the sidebar
     * @type {Boolean}
     */
    showRoots: false,
  },
});
```

#### Sorting stories <a id="sorting-stories"></a>

在`preview.js`文件中使用`options`导入并使用`addParameters`。

```text
import { addParameters } from '@storybook/react';

addParameters({
  options: {
    storySort: (a, b) =>
      a[1].kind === b[1].kind ? 0 : a[1].id.localeCompare(b[1].id, undefined, { numeric: true }),
  },
});
```

有关配置主题的更多信息，请参见[主题](https://storybook.js.org/docs/configurations/theming/)。

**单个story配置**

`options-addon`接受`options`上的参数：

```text
import MyComponent from './my-component';

export default {
  title: 'Options',
  parameters: {
    options: { selectedPanel: 'storybook/a11y/panel' },
  },
};
```






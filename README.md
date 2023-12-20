## 介绍

基于 [薄荷拼音](https://github.com/Mintimate/oh-my-rime) 配置方案, 修改的输入法配置文件, 只留下了薄荷拼音,
删除了其他的输入方案配置文件: 如五笔,双拼,月球拼音等

<p style="color:red">注:修改配置文件后,必须重新部署才能知道是否修改成功</p>

## 主要文件和目录介绍

```txt
- build:     部署后的配置文件缓存(重新部署时会自动生成)
- *.userdb:  用户自定义词库
- dicts:     词库文件
- lua:       lua 插件
- opencc:    emoji 插件增强
- *.yaml:    配置文件
```

## 词库管理

修改 `custom_dict.all.dict.yaml`, 如果不需要哪个直接注释掉即可

```yaml
import_tables:
  - dicts/rime_ice.8105   # 霧凇拼音 常用字集合
  - dicts/rime_ice.41448  # 霧凇拼音 完整字集合
  - dicts/custom_simple   # 自定义
  - dicts/rime_ice.base   # 雾凇拼音 https://github.com/iDvel/rime-ice
  - dicts/se_words        # 互联网网络词汇
  - dicts/luna_pinyin.biaoqing # 表情
  - dicts/luna_pinyin.emoji    # emoji Ext
```

## 增加输入到词库

修改 `dicts/custom_simple.dict.yaml` 

```yaml
# 如: 结果 拼音(简写)
哈哈  hh
```

## 修改默认皮肤

[这里可以找到更多的皮肤主题](https://github.com/NavisLab/rime-pifu)

修改 `squirrel.custom.yaml` 中的 `style/color_scheme` 字段, 但是注意, 你修改的配色主题必须要在 `squirrel.custom.yaml` 存在, 否则部署报错

```yaml
patch:
  # 会根据系统的主题自动变化
  "style/color_scheme_dark": macos_dark
  "style/color_scheme": macos_light
```

## 修改快捷键

### 首先如何知道有哪些快捷键?

参考编译部署后的结果: `build/default.yaml`

```yaml
# 切换中英文
ascii_composer:
  switch_key:
    Caps_Lock: commit_code
    Control_L: noop
    Control_R: noop
    Shift_L: noop
    Shift_R: commit_code

# 切换输入方案(默认值是这样的)
switcher:
  hotkeys:
    - "Control+grave"
    - "Control+Shift+grave"
    - F4

# 其他快捷键
key_binder:
  bindings:
    - {accept: "Control+p", send: Up, when: composing}
    # ...
```

### 修改快捷键

由于现在只有薄荷拼音这一个输入方案, 那么就不需要切换输入方案, 修改 `switcher/hotkeys` 字段, 改为空数组就行

```yaml
patch:
  "switcher/hotkeys": []
```

## 手动备份

```sh
cd ~/Library/Rime
tar -cjvf "$(date +%Y-%m-%d)-backup.bz2" --exclude="build/*" .
```

## 如果是从 github 下载配置, 需要恢复

复制所有配置文件覆盖原来的配置文件, 然后重新部署


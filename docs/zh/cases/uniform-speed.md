# 匀速运动

## 描述

由于本弹幕库对弹幕的运动速度控制默认不是匀速的，所以对于有匀速需求的用户来说，可以通过以下的方式来实现。

> [!NOTE] 注意事项
>
> 1. 当为弹幕设置速度后，`config.timeRange` 和 `config.duration` 这俩配置会失效。
> 2. 本弹幕库在匀速模式下只会保证弹幕的速度是一致的，但是弹幕之间的间距是不保证是一样的，虽然可以设置 `gap` 配置，但是这只是让弹幕之间的间距不小于 `gap` 设置的值，而不是等于。
> 3. 如果你有强诉求保证间距是一样的，你可以减小速度来达到近似的值，原理见此 [issue](https://github.com/imtaotao/danmu/issues/34)。

### 设置匀速运动

有以下两种方式来设置为匀速运动模式。

> [!NOTE] 原理
> 当设置弹幕速度后，弹幕的运动时间计算如下，你可以根据想要的时间推导出速度：
>
> 1. $FacileDuration = (containter.width + danmaku.width) / v$
> 2. $FlexibleDuration = (danmaku.position.x + danmaku.width) / v$

```ts {3-4}
// 1. 在初始化的时候设置速度
const manager = create({
  speed: 0.1,
  speed: '100% / 1000', // duration 约等于 1000ms
});
```

```ts {2-3}
// 2. 通过 `setSpeed` api 来设置
manager.setSpeed(0.1);
manager.setSpeed('100% / 2000'); // duration 约等于 2000ms
```

### 取消匀速运动

设置为 `null` 则取消匀速运动，恢复为默认的行为。

```ts
manager.setSpeed(null);
```

### 单个弹幕不跟随匀速模式

如果你需要让大部分弹幕是匀速运动，但是一些特殊的弹幕不是匀速运动，可以在发送这些弹幕的时候传递 `speed` 配置为 `null`。当然你可以给一个不同于全局 `speed` 的值。

```ts {3}
// 1. 普通弹幕
manager.push('弹幕内容', {
  speed: null，
});
```

```ts {3}
// 2. 高级弹幕
manager.pushFlexibleDanmaku('弹幕内容', {
  speed: null，
});
```

### 和 [**`config.mode`**](../reference/manager-configuration.md#config-mode) 的关系

当设置弹幕的速度后，还是会受到 `config.mode` 的限制。

- **`node`** 弹幕的发送不会有任何碰撞检测，弹幕会立即渲染。
- **`strict`** 会进行严格的碰撞检测，如果不满足条件则会推迟渲染。
- **`adaptive`** 在满足立即渲染的前提下，会尽力进行碰撞检测。

# Uniform Speed

## Description

Since the default behavior of this danmaku library does not ensure uniform speed for danmaku movements, users with uniform motion requirements can achieve this by using the following methods.

> [!NOTE] Important Notes
>
> 1. When you set the `speed` for danmaku, the configurations `config.timeRange` and `config.duration` will become invalid.
> 2. In uniform motion mode, this library only ensures that the speed of danmaku is consistent, but it does not guarantee that the spacing between danmaku is uniform. You can set the `gap` configuration, but this only ensures that the spacing between danmaku is not less than the value of `gap`, not that it is equal to it.
> 3. If you strongly require uniform spacing, you can reduce the speed to achieve an approximate result. See the explanation in this [issue](https://github.com/imtaotao/danmu/issues/34).

### Setting Uniform Speed

There are two ways to enable uniform speed mode.

> [!NOTE] Principle
> When the speed of danmaku is set, the motion duration of danmaku is calculated as follows, you can adjust the launching speed according to the desired duration:
>
> 1. $FacileDuration = (containter.width + danmaku.width) / v$
> 2. $FlexibleDuration = (danmaku.position.x + danmaku.width) / v$

```ts {3-4}
// 1. Configure speed during initialization
const manager = create({
  speed: 0.1,
  speed: '100% / 1000', // The `duration` is approximately 1000ms
});
```

```ts {2-3}
// 2. Use the `setSpeed` API to set speed
manager.setSpeed(0.1);
manager.setSpeed('100% / 2000'); // The `duration` is approximately 2000ms
```

### Cancel Uniform Speed

Set speed to `null` to cancel uniform speed mode and revert to the default behavior.

```ts
manager.setSpeed(null);
```

### Excluding Specific Danmaku from Uniform Speed Mode

If you want most danmaku to move at a uniform speed but exclude certain danmaku from this mode, you can specify the `speed` configuration as `null` when sending those danmaku. Alternatively, you can assign a speed value different from the global `speed`.

```ts {3}
// 1. Facile danmaku
manager.push('content', {
  speed: null，
});
```

```ts {3}
// 2. Flexible danmaku
manager.pushFlexibleDanmaku('content', {
  speed: null，
});
```

### Relationship with [**`config.mode`**](../reference/manager-configuration.md#config-mode)

hen speed is set for danmaku, it will still be subject to the restrictions of `config.mode`.

- **`node`** No collision detection, danmaku will render immediately.
- **`strict`** Strict collision detection, rendering will be delayed if conditions are not met.
- **`adaptive`** Attempts collision detection while ensuring immediate rendering.

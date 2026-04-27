# 定义 Modifier 逻辑

在上一篇中已经完成了 `Modifier` 的注册, 但此时仅具备结构, 尚未包含任何实际行为
本篇将说明如何为 `Modifier` 添加一个最基础的功能实现

从实现方式上看, `Modifier` 子类与原版的 `Item` 或 `Block` 类类似, 都是通过方法或事件驱动逻辑

需要注意:

* `Forge` 提供通用事件系统, 适合快速实现逻辑
* `TConstruct` 提供专用 Hook, 更推荐用于正式开发
* 本文以 `ForgeEvents` 作为入门示例

---

## 示例目标

实现如下效果:

> 当攻击目标身上存在以下任意 debuff 时, 且武器具有该 Modifier, 攻击必定暴击
>
> * 中毒
> * 凋零
> * 流血

---

## Step 1: 判定 debuff

先定义一个工具方法, 用于检测目标是否具有指定效果

```java
public class AbuserModifier extends Modifier {
	/**
	 * 判断触发对象身上是否有三种对应的 debuff
	 *
	 * @param entity 存活的实体
	 * @return
	 */
	private boolean hasEffect(LivingEntity entity) {
		// 判断实体是否有 中毒 效果
		return entity.hasEffect(MobEffects.POISON)
				// 判断实体是否有 凋零 效果
				|| entity.hasEffect(MobEffects.WITHER)
				// 判断实体是否有 流血 效果(来自匠魂)
				|| entity.hasEffect(TinkerEffects.bleeding.get());
	}
}
```

---

## Step 2: 注入攻击事件

通过 `CriticalHitEvent` 控制暴击逻辑, 并增加 Modifier 校验

```java
// 注册事件让事件生效
@Mod.EventBusSubscriber(modid = YourMod.MODID, bus = Mod.EventBusSubscriber.Bus.FORGE)
public class AbuserModifier extends Modifier {
	/**
	 * 订阅事件
	 * 
	 * @param event 事件
	 */
	@SubscribeEvent
	public static void onCriticalHitEvent(CriticalHitEvent event) {
		// 定义常量用于在Event中获取玩家
		Player player = event.getEntity();
		// 获取玩家主手上的物品
		ItemStack stack = player.getMainHandItem();

		// 仅对存活实体生效
		if (!(event.getTarget() instanceof LivingEntity target)) {
			return;
		}
		
		// 判断工具是否具有该 Modifier
		boolean hasModifier = ModifierUtil.getModifierLevel(
				stack,
				new ModifierId(ModModifier.ABUSER.getId())
		);
		
		// 满足条件时强制暴击
		if (hasEffect(target) && hasModifier) {
			event.setResult(Event.Result.ALLOW);
		}
	}

	/**
	 * 判断触发对象身上是否有三种里其中之一的对应 debuff
	 *
	 * @param entity 存活的实体
	 * @return
	 */
	private static boolean hasEffect(LivingEntity entity) {
		// 判断实体是否有 中毒 效果
		return entity.hasEffect(MobEffects.POISON)
				// 判断实体是否有 凋零 效果
				|| entity.hasEffect(MobEffects.WITHER)
				// 判断实体是否有 流血 效果(来自匠魂)
				|| entity.hasEffect(TinkerEffects.bleeding.get());
	}
}
```

---

## 关键点

* 使用 `ModifierUtil.getModifierLevel` 精确判断 Modifier 是否存在
* 事件中优先进行类型过滤, 减少不必要开销
* 逻辑条件应尽量集中, 提高可读性

---

## 结尾

至此, 已完成一个具备实际效果的 Modifier 实现:

* 状态检测
* Modifier 校验
* 行为控制

这种基于事件的方式适合快速实现和测试功能
在复杂场景或正式项目中, 建议逐步迁移到 TConstruct 的 Hook 体系, 以获得更清晰的结构与更高的可维护性
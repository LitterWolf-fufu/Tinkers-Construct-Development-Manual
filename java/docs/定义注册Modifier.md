# 定义和注册一个 Modifier 类

TConstruct 提供了 `Modifier(slimeknights.tconstruct.library.modifiers.Modifier)` 作为基类, 用于自定义 Modifier 时继承

## 基本流程

1. 在 Mod 包下创建 `modifier` 或 `common.modifier` 包
2. 新建类并继承 `Modifier`
3. 创建注册类统一注册所有 Modifier
4. 在主类中挂载注册
5. 进游戏使用指令验证

---

## 示例

在项目中创建自定义 Modifier 类

```java
import slimeknights.tconstruct.library.modifiers.Modifier;

public class AbuserModifier extends Modifier {
}
```

---

## 注册 Modifier

在 `common.register` 下创建注册类, 用于集中管理 Modifier

```java
import slimeknights.tconstruct.library.modifiers.util.ModifierDeferredRegister;
import slimeknights.tconstruct.library.modifiers.util.StaticModifier;

public class ModModifier {
	// 注册器
	public static final ModifierDeferredRegister MODIFIER;

	// 自定义 Modifier 实例
	public static final StaticModifier<AbuserModifier> ABUSER;

	static {
		MODIFIER = ModifierDeferredRegister.create(YourMod.MODID);

		// 参数1: 注册 id
		// 参数2: 构造函数引用
		ABUSER = MODIFIER.register("abuser", AbuserModifier::new);
	}

	/**
	 * 主类调用
	 *
	 * @param bus 事件总线
	 */
	public static void register(IEventBus bus) {
		MODIFIERS.register(bus);
	}
}
```

---

## 在主类中注册

将注册类接入 Mod 事件总线

```java
@Mod(YourMod.MODID)
public class YourMod {
	public static final String MODID = "your_modid";

	public YourMod(FMLJavaModLoadingContext context) {
		IEventBus bus = context.getModEventBus();

		// 注册 Modifier
		ModModifier.register(bus);
	}
}
```

---

## 测试

进入游戏后, 手持任意匠魂工具, 执行指令

```
tconstruct modifiers @s add your_modid:abuser(或其它注册id)
```

---

## 备注

* 建议统一在一个注册类中管理所有 Modifier, 避免分散
* 注册 id 需全局唯一, 且与资源路径保持一致
* 若未生效, 优先检查:

	* 是否正确注册到事件总线
	* MODID 是否一致
	* 指令中的 id 是否拼写正确

---

# [下一篇](/java/docs/定义Modifier逻辑.md)